# Nhật ký quyết định kiến trúc (ADR)

Mỗi ADR ghi lại **một quyết định**, **bối cảnh lúc quyết**, và **cái giá phải trả**. ADR không bị xoá hay sửa nội dung — khi đổi ý, viết ADR mới và đánh dấu cái cũ `Superseded`.

## Trạng thái

`Proposed` · `Accepted` · `Deprecated` · `Superseded by ADR-xxx`

## Danh sách

| # | Quyết định | Trạng thái |
|---|---|---|
| [0001](0001-culture-neutral-core.md) | Lõi trung lập văn hoá + culture pack tách rời | Accepted |
| [0002](0002-single-canonical-writer.md) | Một người ghi duy nhất (World Kernel) | Accepted |
| [0003](0003-read-only-ai-preassessment.md) | AI tiền thẩm chỉ đọc, không phán quyết | Accepted |
| [0004](0004-pwa-baseline-electron-showcase.md) | PWA là nền, Electron là bản showcase | Accepted |
| [0005](0005-provider-neutral-model-routing.md) | Định tuyến mô hình trung lập nhà cung cấp | Accepted |
| [0006](0006-event-sourcing.md) | Event sourcing cho trạng thái thế giới | Accepted |
| [0007](0007-2d-first-map.md) | Bản đồ 2D là mặc định, 3D là tuỳ chọn | Accepted |
| [0008](0008-open-source-and-dependency-constitution.md) | Mã nguồn mở + hiến pháp phụ thuộc ba tầng | Accepted |
| [0009](0009-minor-safe-from-g1.md) | Chế độ vị thành niên đầy đủ từ G1 | Accepted |
| [0010](0010-software-engineering-first-domain.md) | Nghề mở màn: kỹ sư phần mềm / AI | Accepted |

## Mẫu

```markdown
# ADR-xxxx · Tiêu đề

**Trạng thái:** Proposed | Accepted | …
**Ngày:**
**Liên quan:**

## Bối cảnh
Tình hình lúc quyết định. Ràng buộc gì, chưa biết gì.

## Quyết định
Một câu rõ ràng, ở thể chủ động.

## Phương án đã cân nhắc
Từng phương án, và vì sao không chọn.

## Hệ quả
Được gì. **Mất gì.** Cái giá phải trả là phần bắt buộc.

## Khi nào nên xem lại
Điều kiện cụ thể khiến quyết định này nên được xét lại.
```
