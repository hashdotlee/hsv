# ADR-0004 · PWA là nền, Electron là bản showcase

**Trạng thái:** Accepted
**Liên quan:** [`20-architecture.md`](../20-architecture.md), [`30-accessibility-i18n.md`](../30-accessibility-i18n.md)

## Bối cảnh

Ý định ban đầu là Electron làm ứng dụng đa nền tảng. Người dùng mục tiêu từ 14 tuổi, có điện thoại **hoặc** máy tính. Người sáng lập nêu một ràng buộc thực tế: đưa ứng dụng lên các kho ứng dụng là việc khó, và desktop dễ trình diễn hơn.

## Quyết định

**PWA là nền tảng triển khai mặc định.** Hành vi mobile-first. **Electron là cùng một mã nguồn đóng gói lại** để trình diễn trên desktop.

Lõi TypeScript không biết gì về Electron.

## Phương án đã cân nhắc

| Phương án | Vì sao không |
|---|---|
| Chỉ Electron | Loại bỏ người chỉ có điện thoại — chính nhóm cần nhất |
| Ứng dụng native qua kho ứng dụng | Kho ứng dụng cũng là phụ thuộc bên thứ ba, và là loại **có thể xoá sản phẩm khỏi thị trường bằng một quyết định**. Thêm 15–30% phí và chờ duyệt mỗi lần cập nhật nội dung |
| React Native | Thêm một hệ sinh thái nữa để bảo trì, chưa giải quyết được desktop |

## Hệ quả

**Được:** cài từ trình duyệt, không qua kho · cập nhật nội dung tức thì · một mã nguồn cho mọi nền tảng · ngoại tuyến qua service worker · không rủi ro bị gỡ khỏi kho · Electron vẫn có cho nhu cầu trình diễn.

**Mất:** một số API thiết bị không dùng được · trải nghiệm cài đặt trên iOS kém trực quan hơn · không có kênh khám phá từ kho ứng dụng · phải tự làm marketing.

**Ràng buộc kèm theo:** ngân sách hiệu năng cứng (180 KB tải lần đầu, tương tác được < 4s trên điện thoại tầm trung), CI chặn merge khi vượt. PWA chỉ có ý nghĩa nếu nó thật sự nhẹ.

## Khi nào nên xem lại

Nếu có tính năng bắt buộc cần API chỉ native có (ví dụ ghi màn hình sâu cho phiên bảo vệ), hoặc nếu khả năng khám phá qua kho ứng dụng trở thành nút thắt tăng trưởng đã được chứng minh bằng dữ liệu.
