# ADR-0005 · Định tuyến mô hình trung lập nhà cung cấp

**Trạng thái:** Accepted
**Liên quan:** [`DEPENDENCY-CONSTITUTION.md`](../../DEPENDENCY-CONSTITUTION.md), [`23-agent-platform.md`](../23-agent-platform.md)

## Bối cảnh

Hệ thống dùng AI ở nhiều chỗ: sinh nội dung, tiền thẩm, nhúng, đối kháng, phân tích. Yêu cầu của người sáng lập: hệ thống phải **độc lập**, và **admin chọn được nhà cung cấp AI**.

Thị trường mô hình biến động nhanh: giá đổi, API đổi, chính sách đổi, nhà cung cấp biến mất.

## Quyết định

Mọi lời gọi mô hình đi qua `@hsv/oracle`. **Không package nào khác được import SDK của nhà cung cấp** — cưỡng chế bằng luật lint trong CI.

Định tuyến theo **loại tác vụ**, cấu hình được lúc chạy, không cần triển khai lại:

```ts
oracle.policy({
  'proposal.generate': { prefer: 'provider-a', fallback: ['local-70b'], maxCostPerCall: 0.15 },
  'preassess':         { prefer: 'provider-b', requireCitations: true },
  'embed':             { prefer: 'local-embed' },
});
```

**Bắt buộc:** phải có một cấu hình chạy **hoàn toàn bằng mô hình mở tự host**, được diễn tập mỗi quý.

## Phương án đã cân nhắc

| Phương án | Vì sao không |
|---|---|
| Gọi thẳng SDK một nhà cung cấp | Nhanh hơn lúc đầu, nhưng khoá chặt; vi phạm yêu cầu độc lập |
| Dùng thư viện trừu tượng hoá LLM của bên thứ ba | Đổi một phụ thuộc lấy một phụ thuộc khác, và thêm bề mặt API không kiểm soát được |
| Chỉ dùng mô hình mở tự host | Chất lượng chưa đủ cho sinh nội dung ở G2; và tự vận hành hạ tầng suy luận là một dự án riêng |

## Hệ quả

**Được:** đổi nhà cung cấp trong giao diện quản trị · định tuyến theo chi phí (nhúng chạy cục bộ, rẻ và riêng tư) · so sánh chất lượng giữa nhà cung cấp trên cùng bộ tác vụ · Red Team dùng mô hình khác với mô hình sinh (tránh cùng điểm mù) · sống sót khi một nhà cung cấp biến mất.

**Mất:** mẫu số chung nhỏ nhất — không dùng được tính năng độc quyền của một nhà cung cấp · thêm một lớp phải bảo trì · chất lượng đầu ra khác nhau giữa các nhà cung cấp, cần bộ kiểm thử hồi quy.

## Bộ kiểm thử hồi quy

Một bộ đề xuất chuẩn có kết quả kỳ vọng. Chạy khi: đổi nhà cung cấp · nhà cung cấp cập nhật mô hình · đổi prompt. Chất lượng tụt ⇒ tăng mức duyệt của người tạm thời (RB-08).

## Khi nào nên xem lại

Nếu chi phí của lớp trừu tượng (bỏ lỡ tính năng quan trọng) vượt giá trị của nó — nhưng lúc đó hãy xem lại Điều lệ §24 trước.
