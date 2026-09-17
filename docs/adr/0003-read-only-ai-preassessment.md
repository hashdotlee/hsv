# ADR-0003 · AI tiền thẩm chỉ đọc, không phán quyết

**Trạng thái:** Accepted
**Liên quan:** [`13-assessment.md`](../13-assessment.md), [`CHARTER.md`](../../CHARTER.md) §12

## Bối cảnh

Chấm một lần thử tốn 30–60 phút của gác cổng. Gác cổng là nguồn lực khan hiếm nhất, và là giới hạn tăng trưởng cứng của hệ thống (1 gác cổng ≈ 6 người học). Cám dỗ để AI chấm là rất lớn và sẽ càng lớn khi hàng đợi dài ra.

Nhưng toàn bộ lý do tồn tại của sản phẩm là **chứng nhận rằng con người hiểu**. Một chứng nhận do máy cấp, trong một thế giới mà máy làm được bài, không có giá trị gì.

## Quyết định

AI chạy **tiền thẩm**: dựng dòng thời gian, đánh dấu ngõ cụt, phân loại tương tác với AI, ghi quan sát theo từng tiêu chí rubric **kèm trích dẫn bằng chứng**, soạn câu hỏi cho phiên bảo vệ.

AI **không** trả về: điểm số · kết luận đạt/chưa đạt · khuyến nghị.

AI **không bao giờ** ghi vào vết. Chỉ đọc.

`Verdict.judged_by` luôn trỏ tới một con người — cưỡng chế ở tầng ghi của Kernel.

## Phương án đã cân nhắc

| Phương án | Vì sao không |
|---|---|
| AI chấm hoàn toàn | Phá huỷ lý do tồn tại của sản phẩm |
| AI chấm, người duyệt | Người sẽ đóng dấu. Điểm số của máy neo phán quyết của người rất mạnh |
| AI chấm ngưỡng không có tiền | Tạo hai hạng chứng nhận, và biên giới sẽ trôi dần |
| Không dùng AI gì cả | Lãng phí: máy tóm tắt vết tốt và tiết kiệm thời gian thật cho người |

## Hệ quả

**Được:** tính chính danh của chứng nhận được giữ · gác cổng tiết kiệm thời gian mà không mất vai trò phán xét · chỗ gác cổng không đồng ý với máy trở thành dữ liệu hiệu chỉnh quý giá · hệ thống chạy được khi không có nhà cung cấp AI nào (`@hsv/trace.summarizeForHuman`).

**Mất:** không mở rộng được bằng cách bỏ người ra khỏi vòng lặp · chi phí mỗi lần chấm vẫn cao · sức ép sẽ quay lại mỗi lần hàng đợi dài.

Lối thoát duy nhất cho nút thắt là **nhiều gác cổng hơn** — tức là trục TRUYỀN, không phải tự động hoá.

## Cưỡng chế kỹ thuật

- Kiểu trả về của `Oracle.preAssess` **không có trường điểm số** — không phải chính sách, là kiểu dữ liệu
- `POST /v1/gk/.../preassess` trả `409` nếu chưa gọi `/evidence` — gác cổng phải nhìn bằng chứng gốc trước
- `VerdictRecorded` bị từ chối nếu `actor_kind != 'human'`

## Khi nào nên xem lại

Không xem lại. Đây là Điều lệ §12 — sửa phải qua quy trình sửa điều lệ (30 ngày thảo luận công khai + 2/3 hội đồng gác cổng), không phải qua ADR.
