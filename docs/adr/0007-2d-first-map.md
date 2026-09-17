# ADR-0007 · Bản đồ 2D là mặc định, 3D là tuỳ chọn

**Trạng thái:** Accepted
**Liên quan:** [`15-map-and-progression.md`](../15-map-and-progression.md), [`30-accessibility-i18n.md`](../30-accessibility-i18n.md)

## Bối cảnh

Hình dung ban đầu là một bản đồ địa hình 3 chiều, nơi người học định vị năng lực của mình. Đây là ý tưởng đúng và mạnh — bản đồ là giao diện chính của sản phẩm.

Nhưng người dùng mục tiêu có điện thoại tầm trung và mạng chập chờn. Và một bản đồ trực quan thuần tuý loại trừ người khiếm thị hoàn toàn.

## Quyết định

**Bản đồ 2D (SVG) là chế độ mặc định và là bề mặt đầy đủ.** 3D là công tắc tuỳ chọn cho thiết bị đủ mạnh và cho trình diễn trên desktop.

Ràng buộc cứng: **mọi thông tin có trên bản 3D phải có trên bản 2D**, và mọi thông tin trên bản đồ phải có ở cả ba dạng:

```
1. Bản đồ trực quan (SVG, mặc định)
2. Danh sách phân cấp, điều hướng bằng bàn phím
3. Mô tả văn bản: "Bạn đang ở rìa vùng Chẩn đoán. Ba hướng: …"
```

Thêm nữa: **không làm bản đồ trước G2.** Địa hình là hàm thuần của genome + telemetry + seed (INV-13) — chưa có dữ liệu người học thật thì địa hình chỉ là hư cấu.

## Phương án đã cân nhắc

| Phương án | Vì sao không |
|---|---|
| 3D trước, 2D là bản rút gọn | Bản rút gọn luôn bị bỏ bê và thành hạng hai |
| Chỉ 3D | Loại bỏ máy yếu, mạng yếu, và người khiếm thị |
| Chỉ 2D | Bỏ mất sức mạnh trình diễn và cảm giác "địa hình" mà ý tưởng gốc mô tả |

## Hệ quả

**Được:** chạy trên điện thoại tầm trung · vùng bản đồ ~120 KB · ngoại tuyến được · trình đọc màn hình đọc được · thẩm mỹ khắc gỗ (viền dày, mảng dẹt, không gradient) vừa hợp bản sắc Đông Hồ vừa render cực nhẹ · phát triển nhanh hơn nhiều.

**Mất:** ít gây ấn tượng hơn trong 10 giây đầu · phải duy trì hai bộ hiển thị khi 3D xuất hiện ở G4 · "độ cao" phải biểu đạt bằng đường đồng mức thay vì hình khối.

Thẩm mỹ bản đồ hàng hải cổ + đường đồng mức thật ra **hợp chủ đề hơn** một thế giới mở 3D — và rẻ hơn khoảng 50 lần.

## Khi nào nên xem lại

Không xem lại việc 2D là mặc định. Có thể xem lại **mức đầu tư** vào 3D nếu dữ liệu ở G4 cho thấy nó ảnh hưởng thật tới việc người học hiểu vị trí của mình.
