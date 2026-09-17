# 45 · Sổ rủi ro

Xếp theo **mức nguy hiểm với dự án**, không theo xác suất.

Ký hiệu: 🔴 có thể kết thúc dự án · 🟠 nghiêm trọng · 🟡 cần theo dõi

---

## 🔴 R1 · Ngưỡng cửa không đo được năng lực thật

**Dấu hiệu:** người đạt ngưỡng không làm được việc thật; nhà tài trợ không thấy giá trị.
**Vì sao chết:** toàn bộ luận điểm sụp. Không có gì cứu được.
**Chặn:** chỉ số Bắc Đẩu #1 (năng lực chuyển giao được) · phiên bảo vệ · biến ẩn · theo dõi người học sau khi rời hệ thống.
**Kiểm ở G0:** nếu chính bạn qua ngưỡng mà biết mình chưa hiểu, ngưỡng đó sai.

## 🔴 R2 · Mô hình mạnh lên và qua được mọi ngưỡng

**Dấu hiệu:** `ai_solo_pass_rate` tăng đều ở mọi ngưỡng.
**Vì sao chết:** sản phẩm hết lý do tồn tại.
**Chặn:** đo lại mỗi 90 ngày · chết tự động khi vượt 0,25 · trục THẨM và trục NGƯỜI khó bị thay thế hơn · chuyển trọng tâm sang phán đoán, giao tiếp, đánh giá đầu ra máy.
**Sự thật cần chấp nhận:** đây là rủi ro **không loại bỏ được**, chỉ chạy nhanh hơn được. Thiết kế phải giả định các ngưỡng thuần kỹ thuật sẽ chết dần và trọng tâm dịch sang THẨM/NGƯỜI/TRUYỀN.

## 🔴 R3 · Sự cố an toàn với người vị thành niên

**Vì sao chết:** kết thúc dự án, và đúng như vậy.
**Chặn:** toàn bộ [`29-minors.md`](29-minors.md) từ G1 · INV-09 cưỡng chế ở tầng ghi · phiên bảo vệ luôn ghi hình và có người thứ ba · phòng kín kiểm duyệt · phản ứng P0.
**Không bao giờ:** hoãn chế độ vị thành niên để kịp tiến độ.

## 🔴 R4 · Tiền làm hỏng đánh giá

**Dấu hiệu:** tỉ lệ đạt tăng sau khi ngưỡng có tiền; cụm gác cổng–người học lặp lại.
**Chặn:** ký quỹ · chấm chéo trên ngưỡng có tiền · xoay gác cổng · `sentinel` · cửa sổ kháng nghị 72h · sổ cái công khai · **kiểm định thống kê tỉ lệ đạt trước/sau**.
**Phản ứng:** dừng chi trả ngay khi phát hiện, không chờ hết quý.

## 🟠 R5 · Gác cổng là nút thắt

**Dấu hiệu:** thời gian chờ > 7 ngày; gác cổng kiệt sức và bỏ.
**Vì sao nguy hiểm:** 1 gác cổng ≈ 6 người học. Đây là giới hạn tăng trưởng cứng.
**Chặn:** trục TRUYỀN từ G2 · trả công thật · nhiệm kỳ 6 tháng có nghỉ · `matchmaker` giảm tải · tiền thẩm rút ngắn thời gian chấm.
**Kiểm ở G1:** nếu 2 gác cổng kiệt sức với 10 người học, đừng mở rộng — giải bài này trước.

## 🟠 R6 · Trôi dạt ngữ nghĩa

**Dấu hiệu:** đa dạng ngữ nghĩa giảm; nhiều ngưỡng na ná nhau; người học không biết chọn cái nào.
**Chặn:** INV-06 (cosine < 0,88) · `arbiter` rà tuần · hạn ngạch · `curator` cho chết bớt · chỉ số đa dạng theo dõi hàng tuần.
**Sự thật:** khoá ghi là chuyện một buổi chiều; **trôi dạt mới là thứ giết thế giới**.

## 🟠 R7 · Chi phí AI vượt kiểm soát

**Chặn:** hạn ngạch cứng trong hiến pháp · dừng ở 100% · cảnh báo 70% · `steward` hàng ngày · định tuyến mô hình rẻ cho việc rẻ (nhúng chạy cục bộ) · cấu hình chạy hoàn toàn bằng mô hình mở.

## 🟠 R8 · Xây hạ tầng 18 tháng, không có người học

**Dấu hiệu:** ba tháng trôi qua, chưa ai ngoài bạn đi một ngưỡng nào.
**Vì sao nguy hiểm:** đây là **chế độ thất bại tự nhiên nhất** của một người sáng lập kỹ thuật muốn tự viết mọi thứ.
**Chặn:** G0 dùng YAML và terminal, không giao diện · không xây gói nào trước khi có thứ cần dùng nó · tiêu chí hoàn thành từng giai đoạn · cấm bản đồ 3D trước G4.

## 🟠 R9 · Hệ thống thành cult độc hại

**Dấu hiệu:** người học cảm thấy tội lỗi khi nghỉ; ngôn ngữ nội bộ loại trừ; người rời đi bị coi là phản bội.
**Chặn:** Điều lệ §1–6 · ngủ đông không phạt · không chuỗi ngày · không xếp hạng · rời đi không ma sát · quyền của người sáng lập bị giới hạn · kháng nghị bắt buộc.
**Kiểm định:** một người học nghỉ 3 tháng rồi quay lại — họ có cảm thấy bị phán xét không? Hỏi thật, không đoán.

## 🟠 R10 · Chiếm dụng và xúc phạm văn hoá

**Chặn:** hội đồng văn hoá có quyền phủ quyết · xuất xứ bắt buộc (INV-12) · nhãn nguồn (INV-07) · cộng đồng thiểu số đồng tác giả và hưởng lợi · không nhân vật đang được thờ phụng làm NPC · không biểu tượng tôn giáo làm phần thưởng.

## 🟡 R11 · Tái tạo bất bình đẳng hiện có

**Dấu hiệu:** người có bằng đại học đạt cao hơn rõ rệt; thiết bị yếu đạt thấp hơn.
**Chặn:** INV-14 (rubric không nhắc đặc điểm cá nhân) · chấm mù ở ngưỡng có tiền · ngân sách hiệu năng cho máy yếu · theo dõi chỉ số công bằng hàng tháng · trợ cấp thiết bị nếu cần.
**Đây là mục tiêu định danh**, không phải một rủi ro phụ.

## 🟡 R12 · Nhà tài trợ đòi quyền kiểm soát

**Chặn:** điều lệ nói rõ từ trước · cam kết ký · sổ cái công khai · hội đồng duyệt · sẵn sàng từ chối tiền.
**Chuẩn bị tinh thần:** bạn sẽ phải từ chối một khoản tiền lớn ít nhất một lần. Nếu không bao giờ phải từ chối, có thể bạn đang nhượng bộ mà không nhận ra.

## 🟡 R13 · Agent bị chiếm hoặc sinh nội dung độc

**Chặn:** `scope.write` rỗng · kiểm `forbidden` ba lớp · mTLS · `redteam` đa nhà cung cấp · công tắc ngắt < 60s · vết kiểm toán đầy đủ trong `Actor`.

## 🟡 R14 · Người duyệt lười, cổng duyệt thành hình thức

**Dấu hiệu:** tỉ lệ duyệt > 0,9; thời gian duyệt trung bình dưới 30 giây.
**Chặn:** tối đa 20 lượt duyệt một phiên · lấy mẫu kiểm tra ngược · theo dõi tỉ lệ chấp nhận theo người duyệt · `redteam` kiểm cả nội dung đã được người duyệt.

## 🟡 R15 · Phụ thuộc một nhà cung cấp mô hình

**Chặn:** `@hsv/oracle` trung lập · **bắt buộc có cấu hình chạy hoàn toàn bằng mô hình mở tự host** · kiểm thử hồi quy trên bộ đề xuất chuẩn khi đổi nhà cung cấp · Điều lệ §24 (hệ thống chạy được khi không có AI).

## 🟡 R16 · Người sáng lập kiệt sức

**Thật:** bạn là người sáng lập, kỹ sư, người dùng đầu tiên, và gác cổng đầu tiên. Bốn vai.
**Chặn:** trục TRUYỀN từ G2 để chuyển giao vai gác cổng · tiêu chí G5 là "hệ thống chạy 30 ngày không có bạn" · quyền của người sáng lập bị giới hạn ngay từ đầu (nghĩa là hệ thống được thiết kế để không cần bạn) · mã nguồn mở để người khác tiếp quản được.

---

## Ba rủi ro cần xem lại mỗi tháng

1. **R2** — đo lại `ai_solo_pass_rate` toàn bộ ngưỡng `live`
2. **R5** — thời gian chờ và số gác cổng bỏ cuộc
3. **R8** — tuần này có người học thật nào đi một ngưỡng không?

Câu hỏi thứ ba là câu hỏi quan trọng nhất trong 12 tháng đầu.
