# 51 · Giao diện dòng lệnh `hsv`

> Một bề mặt duy nhất cho mọi việc vận hành. Các tài liệu khác gọi lệnh `hsv` rải rác; file này là nơi chúng được định nghĩa.
>
> **Trạng thái: đặc tả.** Chưa gói nào được cài đặt. Lệnh nào chưa tới giai đoạn của nó thì chưa tồn tại — cột "Từ" nói rõ điều đó.

## Nguyên tắc

1. **CLI là bề mặt đầy đủ, không phải phụ kiện.** Mọi việc ở G0 làm được bằng YAML + terminal, không cần giao diện ([`40-roadmap.md`](40-roadmap.md) § G0).
2. **Lệnh đọc thì tự do; lệnh ghi thì đi qua Lõi Thế Giới.** CLI không bao giờ viết thẳng vào kho sự kiện ([`22-world-kernel.md`](22-world-kernel.md)).
3. **Mặc định an toàn.** Lệnh có hậu quả (phát hành, xoá, quay lui) hỏi xác nhận, trừ khi có `--yes`.
4. **Ra máy đọc được khi cần.** `--json` cho mọi lệnh có kết quả, để CI dùng lại.

## Cờ chung

```
--json          in kết quả dạng JSON
--yes           bỏ qua hỏi xác nhận
--dry-run       tính toán và in ra, không ghi gì
--config <path> đường dẫn cấu hình (mặc định: ./hsv.config.yaml)
```

Mã thoát: `0` thành công · `1` lỗi vận hành · `2` vi phạm kiểm định (schema, bất biến, xuất xứ).

---

## Nội dung

| Lệnh | Việc | Từ |
|---|---|---|
| `hsv validate <path…>` | Kiểm schema, bất biến hiến pháp, không chu trình, không trùng ngữ nghĩa | G0 |
| `hsv assay <challengeId>` | Chạy `assayer`: đo `ai_solo_pass_rate` (INV-02) | G0 |
| `hsv seed` | Nạp genome + các ngưỡng cửa đầu tiên vào một cài đặt trống | G0 |

`hsv validate` là cùng một bộ kiểm mà CI chạy — khác nhau thì CI thắng. Xem [`50-contributing.md`](50-contributing.md).

## Culture pack &amp; asset

| Lệnh | Việc | Từ |
|---|---|---|
| `hsv ingest <thư mục>` | Đưa nguồn vào pipeline; bắt buộc có `meta.yaml` hợp lệ | G3 |
| `hsv build <packId>` | Chạy mười giai đoạn, sinh token và dẫn xuất | G3 |
| `hsv pack validate <packId>` | Đủ trường · đủ xuất xứ · tương phản · không `unclear` (INV-12) | G2 |
| `hsv pack release <packId> --version <x.y.z>` | Gắn thẻ bất biến và phát hành | G4 |
| `hsv pack rollback <packId> --to <x.y.z>` | Đổi con trỏ về bản cũ; không build lại | G4 |

Chi tiết: [`25-asset-pipeline.md`](25-asset-pipeline.md), [`18-culture-pack.md`](18-culture-pack.md). Quy trình phát hành: [`52-runbooks.md`](52-runbooks.md) RB-11.

## Vận hành

| Lệnh | Việc | Từ |
|---|---|---|
| `hsv projections rebuild --all` | Dựng lại toàn bộ bản chiếu từ kho sự kiện | G1 |
| `hsv events verify` | Kiểm chuỗi băm từ đầu tới cuối | G1 |
| `hsv kill --scope <single_agent\|single_provider\|all_generation\|adversary_mode> [--id <id>]` | Công tắc ngắt, hiệu lực < 60 giây, không làm gián đoạn người đang làm dở | G2 |

`hsv projections rebuild --all` phải cho kết quả **y hệt** bản đang chạy; lệch một trường là lỗi nghiêm trọng ([`26-data-and-storage.md`](26-data-and-storage.md), RB-10).

## Dữ liệu người học

| Lệnh | Việc | Từ |
|---|---|---|
| `hsv export --learner <id>` | Xuất toàn bộ hồ sơ: JSON + Markdown mở (Điều lệ §2) | G1 |
| `hsv erase --learner <id>` | Xoá dữ liệu cá nhân thật; giữ ghi danh đã công khai và thống kê ẩn danh | G1 |

Hai lệnh này là cách Điều lệ §2 được cưỡng chế bằng công cụ chứ không bằng lời hứa. Chúng nằm trong danh mục kiểm tra trước khi mở cho người ngoài ([`28-security-safety.md`](28-security-safety.md)).

---

## Chạy tối thiểu

```
docker compose up     # api + world-kernel + postgres + crucible
hsv seed              # genome + 5 ngưỡng đầu
```

Một máy, không internet ra ngoài (trừ cổng thanh toán), không agent, không AI: người tự viết nội dung, người tự chấm. Đây là cấu hình G0 và cũng là cấu hình dự phòng nếu mọi nhà cung cấp mô hình biến mất ([`20-architecture.md`](20-architecture.md), Điều lệ §24).
