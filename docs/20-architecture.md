# 20 · Kiến trúc

## Tổng quan

```
┌──────────────────────────────────────────────────────────────┐
│  VỎ                                                          │
│  PWA (mặc định)   │  Electron (showcase)  │  CLI (nhà phát triển) │
└─────────────────────────┬────────────────────────────────────┘
                          │  HTTP + SSE  (giao thức của mình)
┌─────────────────────────▼────────────────────────────────────┐
│  LỚP ỨNG DỤNG                                                │
│  API người học · API gác cổng · API quản trị · Agent Gateway │
└─────────────────────────┬────────────────────────────────────┘
┌─────────────────────────▼────────────────────────────────────┐
│  LÕI THẾ GIỚI  (tiến trình DUY NHẤT được ghi)                │
│  kiểm định → trọng tài → hàng đợi duyệt → ghi sự kiện        │
│  cưỡng chế WORLD-CONSTITUTION.yaml                           │
└──────┬──────────────────────────────────────────────┬────────┘
       │                                              │
┌──────▼─────────────────────────┐        ┌───────────▼────────┐
│  MIỀN  (thuần, không I/O)      │        │  KHO SỰ KIỆN       │
│  genome · rubric · trace       │        │  chỉ ghi thêm      │
│  rite · cartograph · ledger    │        │  + bản chiếu đọc   │
└────────────────────────────────┘        └────────────────────┘
       │
┌──────▼──────────────────────────────────────────────────────┐
│  CỔNG RA  (adapter — Hiến pháp phụ thuộc tầng 1)            │
│  oracle(AI) · crucible(sandbox) · store(DB) · rail(tiền)    │
└─────────────────────────────────────────────────────────────┘
```

## Bốn quy tắc kiến trúc

**QT1 — Miền thuần.** `packages/*` tầng miền không có I/O, không mạng, không `Date.now()` (thời gian là tham số truyền vào). Chạy được trong trình duyệt, trên máy chủ, trong test, không cần dàn dựng gì.

**QT2 — Một người ghi.** Chỉ Lõi Thế Giới ghi vào trạng thái thế giới. Mọi thứ khác đề xuất. Điều này loại bỏ toàn bộ lớp bài toán xung đột ghi giữa nhiều agent ở mức thiết kế, không phải mức khoá.

**QT3 — Sự kiện là sự thật.** Trạng thái hiện tại là bản chiếu suy ra được từ kho sự kiện. Xoá bản chiếu, dựng lại từ đầu, phải ra kết quả y hệt.

**QT4 — Hạ tầng nằm sau cổng ra.** Không package miền nào import SDK của nhà cung cấp. Cưỡng chế bằng luật lint trong CI.

---

## Vì sao PWA

| | PWA | App store | Electron |
|---|---|---|---|
| Cập nhật nội dung | tức thì | chờ duyệt | tự cập nhật |
| Phí và chính sách nền tảng | không | 15–30% + kiểm duyệt | không |
| Cài trên điện thoại | được (thêm vào màn hình chính) | được | không |
| Showcase trên desktop | được | — | tốt nhất |
| Rủi ro bị gỡ khỏi kho | không | **có** | không |

Cửa hàng ứng dụng cũng là một dạng phụ thuộc bên thứ ba — và là loại có thể xoá sản phẩm của bạn khỏi thị trường bằng một quyết định. **PWA là mặc định; Electron là cùng một mã nguồn đóng gói lại để showcase.**

Cùng một lõi TypeScript chạy trong cả ba vỏ.

---

## Đường đi một lần thử

```
người học bấm "nhập môn"
  → API tạo Attempt (sự kiện: AttemptInitiated)
  → crucible cấp môi trường sandbox, ghi vết bật lên
  → người học làm việc; vết chảy về theo luồng, ký tại nguồn
  → nộp bài (sự kiện: AttemptSubmitted)
  → oracle chạy tiền thẩm (CHỈ ĐỌC, không điểm)
  → gác cổng nhận thông báo, xem bằng chứng gốc rồi mới xem tóm tắt
  → phiên bảo vệ (video ngang hàng, ghi hình nếu cần)
  → gác cổng viết phán quyết (sự kiện: VerdictRecorded)
  → ledger giải ngân sau 72h (sự kiện: PayoutReleased)
  → ghi danh (sự kiện: Inscribed) — vĩnh viễn
  → cartograph tính lại vị trí và sương mù
```

## Đường đi một đề xuất của agent

```
agent nhận thái ấp → sinh đề xuất → nộp qua Agent Gateway
  → kiểm định bất biến (WORLD-CONSTITUTION.yaml)
  → kiểm trùng ngữ nghĩa (INV-06)
  → Red Team thử phá
  → theo vùng trưởng thành: tự phát hành | chờ người duyệt | chỉ gợi ý
  → Lõi ghi sự kiện ProposalAccepted
  → nội dung vào trạng thái seed
```

Chi tiết: [`22-world-kernel.md`](22-world-kernel.md), [`23-agent-platform.md`](23-agent-platform.md).

---

## Ngoại tuyến

Người dùng mục tiêu có mạng chập chờn. Ngoại tuyến không phải tính năng phụ.

| Hoạt động | Ngoại tuyến? |
|---|---|
| Xem bản đồ (2D) | ✅ có bộ nhớ đệm |
| Đọc đề ngưỡng cửa, rubric | ✅ |
| Làm việc, ghi vết | ✅ vết xếp hàng, đồng bộ sau |
| Nộp bài | ⏳ xếp hàng |
| Phiên bảo vệ | ❌ cần mạng |
| Xem hồ sơ, ghi danh | ✅ |

Vết ghi ngoại tuyến được ký tại nguồn với đồng hồ đơn điệu cục bộ; khi đồng bộ, dấu thời gian được đối chiếu và chênh lệch được ghi nhận, không sửa.

---

## Ranh giới triển khai

| Thành phần | Chạy ở đâu | Mở rộng |
|---|---|---|
| Vỏ PWA | Máy người dùng / CDN tĩnh | — |
| API ứng dụng | Máy chủ | ngang |
| **Lõi Thế Giới** | **Một tiến trình** | **dọc (cố ý)** |
| Agent Gateway | Máy chủ | ngang |
| Agent | Tiến trình riêng, có thể ở máy khác/bên ngoài | ngang |
| Crucible (sandbox) | Máy chuyên dụng, cách ly mạng | ngang |
| Kho sự kiện | CSDL | dọc rồi phân mảnh |

**Lõi Thế Giới cố tình không mở rộng ngang.** Nó chỉ xử lý đề xuất và phán quyết — vài trăm mỗi ngày ở G4. Nếu nó trở thành nút thắt, bạn đã thành công vượt mọi mong đợi; lúc đó hãy phân mảnh theo vùng bản đồ.

---

## Chạy tối thiểu

Điều kiện của Hiến pháp phụ thuộc L1: chạy được trên **một máy**, không internet ra ngoài (trừ cổng thanh toán):

```
docker compose up     # api + world-kernel + postgres + crucible
hsv seed              # genome + 5 ngưỡng đầu
# không agent, không AI: người tự viết nội dung, người tự chấm
```

Đây là cấu hình bạn dùng ở G0 và là cấu hình dự phòng nếu mọi nhà cung cấp AI biến mất.

---

## Kho mã

```
hsv/
  apps/        web/ desktop/ api/ world-kernel/ agent-gateway/ cli/
  packages/    genome/ rubric/ trace/ rite/ oracle/ crucible/ ledger/
               cartograph/ pack/ forge/ tokens/ glyph/ atlas/ chronicle/ ui/ net/
  agents/      miner/ smith/ arbiter/ redteam/ …
  content/     genome/ challenges/ lore/
  cultures/    vi-VN/
  docs/
```

Một kho, nhiều gói. Gói xuất bản độc lập được để cộng đồng dùng lại (Điều lệ: mã nguồn mở).
