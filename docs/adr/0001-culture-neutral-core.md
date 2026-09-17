# ADR-0001 · Lõi trung lập văn hoá + culture pack tách rời

**Trạng thái:** Accepted
**Liên quan:** [`18-culture-pack.md`](../18-culture-pack.md), [`21-packages.md`](../21-packages.md)

## Bối cảnh

Sản phẩm lấy văn hoá Việt Nam làm chính và có ý định mở ra các nền văn hoá khác về sau. Hiện tại chỉ có **một** nền văn hoá.

Cám dỗ rõ ràng: nhúng thẳng tên trục, màu sắc, nghi thức, hình học bản đồ Việt Nam vào lõi cho nhanh.

## Quyết định

Lõi kỹ thuật **trung lập văn hoá hoàn toàn**. Mọi thứ mang bản sắc nằm trong `cultures/<locale>/<version>/`, tải lúc chạy qua `@hsv/pack`.

Phép thử: **xoá thư mục `cultures/` thì hệ thống vẫn phải chạy** — xấu, không tên, nhưng đúng chức năng.

## Phương án đã cân nhắc

| Phương án | Vì sao không |
|---|---|
| Nhúng thẳng vào lõi | Sau 2 năm là viết lại toàn bộ |
| Tách sau khi có nền văn hoá thứ hai | Tách sau tốn gấp 50 lần tách bây giờ |
| Chỉ tách phần thị giác | Văn hoá là **cấu trúc**, không phải lớp sơn — khoa cử, tổ nghề, bia đề danh là *cơ chế* |

## Hệ quả

**Được:** thêm nền văn hoá không cần sửa lõi · cộng đồng khác tự làm gói của họ · kiểm thử lõi không phụ thuộc nội dung · hội đồng văn hoá có quyền phủ quyết thật vì gói tách rời được.

**Mất:** thêm một lớp gián tiếp ngay từ đầu · giao diện trần trụi khi phát triển (tên trục là `NEN`, `CHAN`, không phải "Nền", "Chẩn") · phải quyết định ranh giới core/pack cho từng khái niệm, và đôi khi ranh giới đó không hiển nhiên.

**Chi phí lúc này gần bằng 0.** Đó là toàn bộ lý do làm ngay.

## Khi nào nên xem lại

Nếu sau 3 năm vẫn chỉ có `vi-VN` **và** lớp gián tiếp gây ra lỗi thật lặp lại — cân nhắc gộp. Nhưng nhiều khả năng lúc đó chi phí gộp lại lớn hơn lợi ích.
