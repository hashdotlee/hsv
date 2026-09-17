# ADR-0002 · Một người ghi duy nhất (World Kernel)

**Trạng thái:** Accepted
**Liên quan:** [`22-world-kernel.md`](../22-world-kernel.md), [`23-agent-platform.md`](../23-agent-platform.md)

## Bối cảnh

Nhiều AI agent độc lập cùng dựng thế giới: sinh atom, ngưỡng cửa, lore, asset. Yêu cầu rõ ràng của người sáng lập: **không agent nào xung đột với agent nào**.

Phản xạ kỹ thuật thông thường là khoá phân tán, giao dịch, hoà giải xung đột. Nhưng vấn đề thật ở đây không phải xung đột ghi — mà là **trôi dạt ngữ nghĩa**: sau ba tháng thế giới đầy nội dung na ná nhau và mâu thuẫn nhau, dù không lần ghi nào xung đột.

## Quyết định

**Một tiến trình duy nhất — World Kernel — được ghi vào trạng thái thế giới.** Mọi thứ khác (agent, giao diện quản trị, công cụ nhập liệu, kể cả người sáng lập) đều nộp **đề xuất**.

`agent.scope.write` luôn rỗng. Không có ngoại lệ.

## Phương án đã cân nhắc

| Phương án | Vì sao không |
|---|---|
| Khoá phân tán theo vùng | Giải đúng vấn đề dễ (xung đột ghi), không giải vấn đề khó (trôi dạt) |
| CRDT / hợp nhất tự động | Hợp nhất được cú pháp, không hợp nhất được ngữ nghĩa. Hai ngưỡng dạy mâu thuẫn nhau vẫn hợp nhất "thành công" |
| Mỗi agent một nhánh git, gộp sau | Gộp thủ công không mở rộng được; và vẫn cần một người quyết |

## Hệ quả

**Được:** toàn bộ lớp bài toán xung đột ghi biến mất ở mức **thiết kế**, không phải mức khoá · một chỗ duy nhất cưỡng chế hiến pháp · một chỗ duy nhất kiểm trùng ngữ nghĩa · vết kiểm toán đầy đủ · công tắc ngắt thật sự hiệu quả · thêm agent mới không cần nghĩ lại về đồng bộ.

**Mất:** Kernel là điểm lỗi đơn · **cố tình không mở rộng ngang** · độ trễ giữa đề xuất và nội dung xuất hiện · agent phải chịu được việc bị từ chối.

Về mở rộng: Kernel chỉ xử lý đề xuất và phán quyết — vài trăm mỗi ngày ở G4. Nếu nó thành nút thắt, dự án đã thành công vượt mọi mong đợi; lúc đó phân mảnh theo vùng bản đồ.

## Khi nào nên xem lại

Khi Kernel xử lý > 10.000 đề xuất/ngày, hoặc khi có nhu cầu vận hành nhiều thế giới độc lập hoàn toàn (ví dụ mỗi tổ chức một thế giới riêng).
