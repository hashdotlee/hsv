# 26 · Dữ liệu &amp; Lưu trữ

## Bốn loại dữ liệu, bốn chế độ đối xử

| Loại | Ví dụ | Chế độ | Nơi |
|---|---|---|---|
| **Nội dung** | genome, ngưỡng cửa, lore, culture pack | Git, có review, phiên bản hoá | Kho mã |
| **Sự kiện** | mọi thứ đã xảy ra | Chỉ ghi thêm, bất biến | Kho sự kiện |
| **Bản chiếu** | vị trí, địa hình, bảng xếp việc | Suy ra được, xoá và dựng lại được | CSDL quan hệ |
| **Cá nhân** | danh tính, liên hệ, video bảo vệ | Mã hoá, hạn lưu, xoá được | Kho riêng, khoá riêng |

**Nguyên tắc:** nội dung nằm trong git để người và agent cùng đi qua một cửa review. Sự kiện không bao giờ sửa. Bản chiếu luôn dựng lại được. Dữ liệu cá nhân được đối xử như chất độc hại — giữ ít nhất có thể, xoá sớm nhất có thể.

---

## Nội dung trong Git

```
content/
  genome/<TRUC>/*.yaml
  genome/_graph.lock.json        # sinh tự động; CI kiểm
  challenges/<TRUC>/*.yaml
  challenges/_rubrics/*.yaml
  challenges/_seeds/<id>/
  lore/*.md
cultures/vi-VN/<version>/
```

Mọi thay đổi nội dung là một PR — **kể cả của agent**. Agent nộp đề xuất, Lõi tạo PR, người duyệt ở vùng cần duyệt. Một cửa duy nhất, một lịch sử duy nhất.

CI kiểm: schema, bất biến hiến pháp, không chu trình, không trùng ngữ nghĩa, đủ xuất xứ.

---

## Kho sự kiện

```sql
CREATE TABLE world_events (
  seq         BIGSERIAL PRIMARY KEY,
  at          TIMESTAMPTZ NOT NULL,
  kind        TEXT        NOT NULL,
  actor_kind  TEXT        NOT NULL CHECK (actor_kind IN ('human','agent')),
  actor_id    TEXT        NOT NULL,
  actor_model TEXT,
  payload     JSONB       NOT NULL,
  prev_hash   BYTEA       NOT NULL,
  hash        BYTEA       NOT NULL
);
-- KHÔNG có UPDATE, KHÔNG có DELETE. Cưỡng chế bằng quyền của CSDL.
```

Chuỗi băm: mỗi sự kiện băm cùng băm của sự kiện trước. Sửa lịch sử là phá chuỗi, và việc đó phát hiện được.

**Ràng buộc cưỡng chế ở tầng ghi:** `kind = 'VerdictRecorded'` ⇒ `actor_kind = 'human'`.

---

## Bản chiếu

Sinh từ sự kiện, luôn xoá và dựng lại được:

```
learner_positions   · challenge_states  · terrain_cache
gatekeeper_load     · ledger_balances   · telemetry_aggregates
semantic_index      (vector, cho INV-06)
```

```
hsv projections rebuild --all      # phải ra kết quả y hệt
```

Kiểm thử bắt buộc trong CI: dựng lại toàn bộ bản chiếu từ sự kiện và so với bản đang chạy. Lệch một trường là lỗi nghiêm trọng.

---

## Vết quá trình

Khối lượng lớn nhất trong hệ thống.

| | |
|---|---|
| Định dạng | JSONL nén, ký ở cuối |
| Cỡ điển hình | 50KB–2MB một lần thử |
| Lưu | Kho đối tượng, con trỏ trong CSDL |
| Hạn lưu | 2 năm sau khi ngưỡng đóng, rồi hỏi người học |
| Truy cập | Người học (của mình) · gác cổng đang chấm · hội đồng kháng nghị |
| Agent | **Không bao giờ** (INV-08) — kể cả `scribe` cũng chỉ đọc bản sao đã lọc |

Lọc tại nguồn: biến môi trường, token, khoá, đường dẫn chứa tên thật bị loại trước khi vết rời máy.

---

## Dữ liệu cá nhân

| Trường | Cần cho | Hạn lưu | Mã hoá |
|---|---|---|---|
| Email | Đăng nhập, thông báo | Tới khi xoá tài khoản | ✅ |
| Tên hiển thị | Ghi danh | Vĩnh viễn nếu người học chọn công khai | — |
| Tuổi/ngày sinh | Chế độ vị thành niên | Tới khi đủ 18 + 1 năm | ✅ |
| Danh tính thật | Chỉ khi vượt hạn mức tiền | 5 năm (nghĩa vụ kế toán) | ✅ khoá riêng |
| Video phiên bảo vệ | Kháng nghị, an toàn vị thành niên | 90 ngày (18+) / 1 năm (vị thành niên) | ✅ |
| Liên hệ người giám hộ | Chế độ vị thành niên | Tới khi đủ 18 | ✅ |

**Không thu thập:** vị trí, danh bạ, lịch sử duyệt web, nội dung ngoài hệ thống, dữ liệu sinh trắc.

### Xuất và xoá

```
hsv export --learner <id>     # JSON + Markdown mở, đọc được không cần phần mềm của chúng tôi
hsv erase  --learner <id>
```

Xoá thì: dữ liệu cá nhân bị xoá thật, video bị xoá, vết bị xoá. **Giữ lại:** ghi danh đã công khai (nếu người học chọn công khai) và số liệu thống kê đã ẩn danh — hai thứ này không truy ngược được về cá nhân.

Sự kiện không xoá được, nhưng dữ liệu cá nhân trong payload sự kiện được thay bằng con trỏ tới kho riêng; xoá kho riêng là dữ liệu biến mất, chuỗi băm vẫn nguyên. Đây là cách dung hoà "sự kiện bất biến" với "quyền được xoá".

---

## Sổ cái

```sql
CREATE TABLE ledger_entries (
  id          BIGSERIAL PRIMARY KEY,
  at          TIMESTAMPTZ NOT NULL,
  kind        TEXT NOT NULL,            -- escrow_in | payout | reversal | fee | fund
  amount      BIGINT NOT NULL,          -- số nguyên, đơn vị nhỏ nhất. KHÔNG dùng số thực
  currency    TEXT NOT NULL,
  verdict_id  TEXT,                     -- BẮT BUỘC với kind='payout'
  counterparty TEXT NOT NULL,
  reverses    BIGINT REFERENCES ledger_entries(id),
  memo        TEXT NOT NULL
);
```

Luật: chỉ ghi thêm · tiền là số nguyên · mọi `payout` có `verdict_id` · sửa sai bằng bút toán đảo · đối chiếu hàng ngày với cổng thanh toán, lệch thì báo động ngay.

---

## Sao lưu

| Dữ liệu | Tần suất | Giữ | Thử khôi phục |
|---|---|---|---|
| Kho sự kiện | Liên tục + hàng ngày | 7 năm | Hàng tháng |
| Nội dung (git) | Mỗi commit, nhiều bản sao | Vĩnh viễn | — |
| Vết | Hàng ngày | Theo hạn lưu | Hàng quý |
| Cá nhân | Hàng ngày, khoá riêng | Theo hạn lưu | Hàng quý |
| Bản chiếu | Không sao lưu | — | Dựng lại |

**Bản sao lưu chưa từng thử khôi phục thì không phải bản sao lưu.** Ghi lịch thử vào [`52-runbooks.md`](52-runbooks.md).

---

## Khi rời khỏi hệ thống

Cả người học và cả dự án đều phải có đường ra:

- Người học: xuất toàn bộ, định dạng mở, bất cứ lúc nào (Điều lệ §2)
- Dự án: kho sự kiện + nội dung git + culture pack là toàn bộ hệ thống. Không có gì bị khoá trong dịch vụ của bên thứ ba.
