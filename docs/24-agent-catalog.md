# 24 · Danh mục Agent

> Đặt tên theo nghề trong phường hội: mỗi agent là một người thợ có việc riêng.

## Bảng tổng

| Agent | Nghề | Việc | Xuất hiện |
|---|---|---|---|
| `miner` | **Thợ đào** | Đọc nguồn thật (postmortem, issue, tài liệu, phỏng vấn) → đề xuất atom | G2 |
| `smith` | **Thợ rèn** | Atom → ngưỡng cửa có rubric và biến ẩn | G2 |
| `assayer` | **Người thử vàng** | Đo `ai_solo_pass_rate`, độ khó, thời gian ước lượng | G2 |
| `arbiter` | **Trọng tài** | Xử mâu thuẫn ngữ nghĩa. **Không sinh nội dung** | G2 |
| `redteam` | **Khảo hạch** | Phá sản phẩm của agent khác. Thưởng khi phá được | G2 |
| `cartographer` | **Người vẽ bản đồ** | Tính lại địa hình, sương mù, lối mòn từ dữ liệu | G3 |
| `scribe` | **Người chép sử** | Tóm tắt vết cho gác cổng (tiền thẩm, chỉ đọc) | G2 |
| `lorekeeper` | **Người giữ chuyện** | Lore, NPC, câu đố ven đường — có nhãn nguồn | G4 |
| `curator` | **Người coi kho** | Phát hiện nội dung nên chết; đề xuất `declining` | G3 |
| `matchmaker` | **Người mai mối** | Ghép phòng kín theo chữ ký lỗi bổ sung nhau | G3 |
| `sentinel` | **Người gác** | Phát hiện thông đồng, gian lận, bất thường tiền | G3 |
| `envoy` | **Người đưa tin** | Tìm nhà tài trợ tiềm năng, soạn đề xuất bài toán | G4 |
| `steward` | **Quản gia** | Theo dõi chi phí, hạn ngạch, sức khoẻ dàn agent | G2 |
| `atelier` | **Xưởng vẽ** | Asset: hoa văn, texture, sprite từ nguồn đã duyệt | G4 |

---

## Chi tiết các agent then chốt

### `miner` — Thợ đào

**Đầu vào:** postmortem công khai, issue GitHub, tài liệu kỹ thuật, bản ghi phỏng vấn chuyên gia, kho lý do chết của ngưỡng cửa.

**Đầu ra:** đề xuất `SkillAtom` kèm `mastery_signal` quan sát được, tiên quyết, lỗi điển hình, và **trích dẫn nguồn bắt buộc**.

**Kiểm định riêng:** mỗi atom phải trích dẫn ít nhất 2 nguồn độc lập. Atom không có nguồn thật thì bị loại — đây là chỗ mô hình dễ bịa nhất.

### `smith` — Thợ rèn

**Đầu vào:** atom, vết chuyên gia, mẫu thiết kế ngưỡng cửa ([`12-challenge.md`](12-challenge.md)).

**Đầu ra:** ngưỡng cửa đầy đủ: kịch bản, ràng buộc, **biến ẩn**, rubric, thang gợi ý, `on_fail.next_step`.

**Kiểm định riêng:** phải tự gọi `assayer` đo `ai_solo_pass_rate` trước khi nộp. Không đo thì từ chối ngay ở bước 1.

Biến ẩn là chỗ khó nhất với mô hình — nó có xu hướng viết đề bài đầy đủ và rõ ràng, đúng thứ làm ngưỡng cửa mất giá trị. Prompt của `smith` phải ép ngược điều đó.

### `assayer` — Người thử vàng

Chạy mô hình mạnh nhất hiện có giải ngưỡng cửa **nhiều lần, không có người can thiệp**, đo tỉ lệ qua.

```
ai_solo_pass_rate ≥ 0.25  →  ngưỡng bị loại (INV-02)
```

Đo lại mỗi 90 ngày cho ngưỡng đang `live`. **Đây là cơ chế giữ cho hệ thống không lạc hậu**: khi mô hình mạnh lên, ngưỡng cũ tự chết và thế giới tự nâng chuẩn. Cái chết vì lý do này không phải thất bại — nó là bằng chứng hệ thống đang theo kịp thực tại.

### `arbiter` — Trọng tài

Quyền: **phán xử, không sinh**. Tách quyền sinh khỏi quyền duyệt.

Việc:
- Hai ngưỡng dạy cùng thứ theo hai cách mâu thuẫn → chọn một, hoặc biến thành ngã ba có chủ đích
- Hai atom chồng lấn → gộp hoặc vạch ranh giới
- Vùng phình quá nhanh so với số người thật đi qua → tạm đóng sinh ở vùng đó
- Rà định kỳ: đo đa dạng ngữ nghĩa toàn cục, báo động khi tụt

### `redteam` — Khảo hạch

**Được thưởng khi phá được.** Động cơ đối nghịch là cách duy nhất khiến kiểm định không thành hình thức.

Tìm: đường tắt, ngưỡng mà mô hình tự qua, rubric mâu thuẫn, tiên quyết sai, trùng lặp lọt lưới, ngưỡng vi phạm an toàn vị thành niên.

Phá được ⇒ đề xuất bị loại, uy tín agent sinh giảm, uy tín `redteam` tăng.

**Cần ít nhất hai `redteam` dùng mô hình của hai nhà cung cấp khác nhau** — cùng một mô hình sinh và kiểm thì cùng điểm mù.

### `scribe` — Người chép sử

Chạy tiền thẩm. Ràng buộc nghiêm ngặt nhất trong cả danh mục:

- Mọi quan sát phải có `evidenceRef` trỏ tới bằng chứng gốc
- **Không điểm số, không kết luận, không khuyến nghị** (Điều lệ §12)
- Chỉ đọc **bản sao đã lọc** của vết, qua Agent Gateway — không thấy vết gốc, không bao giờ ghi (INV-08)
- Gác cổng ghi lại chỗ không đồng ý → dữ liệu hiệu chỉnh cho `scribe`

### `curator` — Người coi kho

Phát hiện nội dung nên chết theo tín hiệu suy tàn ([`12-challenge.md`](12-challenge.md)) và đề xuất chuyển `declining`.

**Mỗi đề xuất chết phải kèm lý do.** Kho lý do chết là đầu vào bắt buộc cho `miner` và `smith` ở lứa sau — đây là cách dàn agent *học* thay vì chỉ chạy.

### `sentinel` — Người gác

Phát hiện: cụm gác cổng–người học lặp lại bất thường, tỉ lệ đạt vọt sau khi có tiền, nhiều tài khoản cùng thiết bị, vết có dấu hiệu dán ghép.

**Chỉ báo động cho hội đồng người. Không bao giờ tự xử lý.** Cáo buộc gian lận là quyết định có hậu quả với đời thật của một con người.

### `steward` — Quản gia

Theo dõi chi phí mỗi đề xuất được chấp nhận và còn sống sau 30 ngày, hạn ngạch, tỉ lệ chấp nhận theo agent, độ trễ hàng đợi duyệt. Báo cáo hàng tuần cho người điều hành.

Đây là agent bạn cần **sớm nhất** sau `arbiter` — không có nó, bạn sẽ không biết dàn agent đang đốt tiền tạo rác cho tới khi hoá đơn về.

---

## Hợp tác: một ví dụ thật

```
tender: "vùng THAM thiếu ngưỡng khó — 4/6 ngưỡng có pass_rate > 0.85"

miner       bỏ thầu: "tôi có 3 postmortem về sự cố do tin đầu ra AI"
smith       bỏ thầu: "tôi làm được ngưỡng đối kháng, nhưng cần atom tiền quyết"
            → đăng lên bảng chung: "cần atom về đọc log phân tán"
miner       thấy ý định, điều chỉnh: thêm atom.CHAN.doc-log-phan-tan trước
arbiter     chọn: miner làm atom trước, smith làm ngưỡng sau, cấp thái ấp tuần tự
assayer     đo ngưỡng của smith: ai_solo_pass_rate = 0.31  →  QUÁ CAO
smith       sửa: thêm biến ẩn, tăng độ mơ hồ  →  đo lại 0.12  →  qua
redteam     tìm được đường tắt (một dòng trong đề bài lộ đáp án)  →  trả về
smith       sửa  →  qua
kernel      chấp nhận, vào trạng thái seed
người       gác cổng vùng THAM duyệt (vùng settled)  →  trial
30 ngày sau steward: đề xuất này còn sống, chi phí 4,2 USD  →  uy tín smith +0.03
```

Đây là hình dạng của "nhiều agent hợp tác không xung đột": **thái ấp chống đè nhau, bảng chung để bổ sung nhau, đấu thầu để nhắm đúng chỗ thiếu, trọng tài và khảo hạch để giữ chất lượng.**

---

## Bảng quyền

| Agent | Đọc | Đề xuất | Cấm tuyệt đối |
|---|---|---|---|
| `miner` | genome, nguồn ngoài | atom | ledger, verdict, identity, trace |
| `smith` | genome, vết chuyên gia | challenge, rubric | ↑ |
| `assayer` | challenge | telemetry | ↑ |
| `arbiter` | tất cả nội dung | phán xử | ↑ + không sinh nội dung |
| `redteam` | tất cả nội dung | báo cáo phá | ↑ |
| `scribe` | vết (chỉ đọc), rubric | tiền thẩm | ↑ + không điểm số |
| `sentinel` | telemetry tổng hợp | cảnh báo | ↑ + không tự xử lý |
| `atelier` | asset đã duyệt | asset | ↑ + không asset thiếu quyền |

Cột "cấm tuyệt đối" giống nhau ở mọi agent vì đó là INV-08 — không có ngoại lệ nào.
