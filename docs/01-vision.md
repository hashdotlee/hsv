# 01 · Tầm nhìn

## Vì sao dự án này tồn tại

Các cuộc cách mạng công nghiệp trước lấy đi cơ bắp của con người. Chúng ta không tuyệt chủng — chúng ta lui về văn phòng. Nhưng phần lớn người hiện đại yếu hơn tổ tiên họ rất nhiều, và không ai gọi đó là mất mát vì năng suất vẫn tăng.

Lần này thứ bị lấy đi là trí não. Tính toán, giải quyết vấn đề cụ thể, viết, tổng hợp, lập luận trung bình — tất cả đang được chuyển giao. Và chúng ta đang *trích xuất kỹ năng từ những người được đào tạo tốt nhất để huấn luyện AI*, thay vì dùng AI để phát triển kỹ năng mới và bổ sung lại cho con người.

Xã hội luôn có hai phe: một phe tiến về phía trước, một phe giữ lại. Cả hai đều cần thiết. Dự án này đứng ở phe thứ hai — không phải để chống lại AI, mà để **giữ đường quay về**. Khi loài người tìm cách trở lại với chính mình, phải còn thứ gì đó để quay về.

## Nghịch lý trung tâm

> **Nếu AI giải được bài tập của bạn trong 8 giây, bài tập đó không đo năng lực người nữa. Nó đo việc người đó có mở AI ra hay không.**

Mọi hệ thống đánh giá lập trình dựa trên "viết hàm này cho đúng" đã chết về mặt đo lường, dù chưa ai thừa nhận. Bằng cấp, chứng chỉ, bài phỏng vấn thuật toán — tất cả đang mất giá cùng lúc, và thị trường lao động kỹ thuật chưa có gì thay thế.

Đây vừa là vấn đề vừa là cơ hội của dự án. Nếu bạn giải được bài toán *"làm sao chứng nhận rằng một người thật sự hiểu"*, bạn đang tạo ra thứ khan hiếm nhất trong 5 năm tới.

## Ba tư thế trước AI

Hệ thống cần cả ba, dùng đúng chỗ:

| Tư thế | Cách làm | Đo được gì | Dùng ở đâu |
|---|---|---|---|
| **Cấm AI** | Sandbox ngắt mạng, có ghi vết | Thứ thật sự nằm trong đầu | Chỉ trục NỀN. Ít thôi — lạm dụng là đang huấn luyện một kỹ năng sắp tuyệt chủng |
| **Cho phép AI** | AI thoải mái, độ khó nâng tới mức AI một mình không qua nổi | Điều phối, phân rã, kiểm chứng, biết khi nào dừng | Mặc định |
| **AI đối kháng** | Hệ thống cố tình đưa câu trả lời sai tinh vi, code chạy được nhưng sai ngữ nghĩa | **Phát hiện khi cỗ máy nói sai** | Trục THẨM — lợi thế cạnh tranh thật |

## Định vị sản phẩm

**Nơi duy nhất chứng nhận rằng bạn hiểu, chứ không phải bạn có quyền truy cập vào một mô hình.**

Không phải nền tảng học trực tuyến. Không phải trường. Không phải sàn tuyển dụng. Gần nhất với: **một phường hội nghề có nghi thức nhập môn, có thợ cả, và có bia ghi danh** — chỉ khác là nó chạy trên internet và nội dung là kỹ thuật hiện đại.

## Cấu trúc đã có sẵn: Việt Nam

Bạn không cần phát minh cơ chế. Việt Nam đã vận hành chính xác cỗ máy này hàng trăm năm:

| Thành phần cần | Đã có trong văn hoá Việt |
|---|---|
| Ngưỡng cửa ba cấp | Thi Hương → thi Hội → thi Đình |
| Ghi danh vĩnh viễn | Bia đề danh tiến sĩ ở Văn Miếu |
| Nghi thức nhập môn | Lễ bái sư |
| Cộng đồng tự quản | Phường hội, hương ước tự viết |
| Tổ tiên nghề | Tổ nghề, giỗ tổ nghề |
| Cộng đồng chứng kiến | Vinh quy bái tổ, đình làng |
| Bản đồ của tổ tiên | Mặt trống đồng Đông Sơn: tâm = đỉnh, vòng đồng tâm = cấp độ, cánh sao = sáu trục |

Định vị mạnh nhất: **khôi phục một cỗ máy xã hội Việt Nam, thay nội dung Nho học bằng kỹ năng nghề hiện đại.**

Xem [`18-culture-pack.md`](18-culture-pack.md). Lõi kỹ thuật vẫn phải trung lập văn hoá để sau này thêm nền văn hoá khác mà không viết lại.

## Hình dạng của hệ thống

```
Người học được ĐỊNH VỊ trên một bản đồ năng lực sinh từ dữ liệu
   → chọn một NGƯỠNG CỬA phù hợp
   → NHẬP MÔN trước một GÁC CỔNG là người đã đi qua
   → làm việc thật, trong PHÒNG KÍN cùng người khác
   → nộp SẢN PHẨM + VẾT QUÁ TRÌNH
   → AI TIỀN THẨM (chỉ đọc) tóm tắt cho gác cổng
   → PHIÊN BẢO VỆ: "vì sao anh làm thế này?"
   → PHÁN QUYẾT của người, kèm lý do viết
   → TIỀN THẬT + GHI DANH vĩnh viễn
   → PHẢN TƯ → trở thành gợi ý cho người đi sau
   → một ngày, chính họ làm GÁC CỔNG
```

Thế giới chứa các ngưỡng cửa đó **không do người viết**, mà do nhiều AI agent cùng dựng và liên tục tái sinh, dưới sự điều tiết của con người. Xem [`22-world-kernel.md`](22-world-kernel.md).

## Điều gì khiến hệ thống không thể sao chép

Mã nguồn mở hết — vẫn không ai sao chép được, vì tài sản thật là những thứ tích luỹ theo thời gian:

1. **Kho vết chuyên gia** — cách những người giỏi thật sự *quyết định*. Không mô hình nào sinh ra được.
2. **Mạng lưới gác cổng** — người đã qua và biết dẫn người khác qua.
3. **Kho lý do chết của thử thách** — một năm dữ liệu về việc cái gì không hiệu quả và vì sao.
4. **Ghi danh** — uy tín của chứng nhận tăng theo số người đã qua và chất lượng của họ.

## Điều gì sẽ giết dự án này

Theo thứ tự khả năng xảy ra:

1. **Tự viết quá nhiều hạ tầng** trước khi có người học. 18 tháng code, 0 người dùng.
2. **Nút thắt gác cổng** — phiên bảo vệ không nhân bản được.
3. **Trôi dạt nội dung** — agent sinh ra một thế giới loãng và tự mâu thuẫn.
4. **Trượt thành tổ chức độc hại** — nghi thức + tiền + thứ bậc + người trẻ.
5. **Tiền vào làm hỏng đánh giá** — gác cổng mềm tay khi có tiền.

Mỗi cái có cơ chế chặn cụ thể; xem [`45-risks.md`](45-risks.md) và [`../CHARTER.md`](../CHARTER.md).

## Không phải là gì

- Không phải nền tảng khoá học. Không có "bài giảng".
- Không phải mạng xã hội. Không bảng tin, không theo dõi, không lượt thích.
- Không phải game giải trí. Thử thách là việc thật, hậu quả thật.
- Không phải sàn việc làm, dù nhà tuyển dụng sẽ muốn dùng nó như vậy.
- Không phải công cụ đo lường cho nhà tuyển dụng. Nếu nó trở thành thế, người học sẽ tối ưu để qua bài thay vì để giỏi lên, và toàn bộ giá trị biến mất.
