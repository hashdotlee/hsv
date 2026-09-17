# 16 · Quản trị: Gác cổng, Hội đồng, Kháng nghị

> Nếu hệ thống này thất bại về mặt đạo đức, nó sẽ thất bại ở đây chứ không phải ở code.

## Gác cổng

**Chức vụ có nhiệm kỳ, không phải địa vị** (Điều lệ §15).

### Điều kiện

```yaml
gatekeeper_eligibility:
  must_have_passed: <chính ngưỡng đó>
  min_mastery_on_targets: 0.75
  min_time_since_passing: 30d      # đủ để lắng, chưa đủ để quên
  must_pass: chal.TRUYEN.dan-mot-nguoi-qua    # chọn gác cổng cũng là một thử thách
  max_concurrent_learners: 6
  term: 6 tháng, gia hạn được
```

### Vòng đời

```
ứng viên → tập sự (chấm song song với gác cổng chính, 3 lượt)
        → chính thức (nhiệm kỳ 6 tháng)
        → gia hạn | hết hạn | bị thu hồi
```

### Đánh giá gác cổng

Đánh giá dựa trên **tiến bộ của người học**, không dựa trên thành tích cá nhân của gác cổng.

| Chỉ số | Ý nghĩa | Ngưỡng cảnh báo |
|---|---|---|
| Tiến bộ của người học sau 60 ngày | Người họ dẫn có đi tiếp được không | thấp hơn trung bình 30% |
| Độ nhất quán với gác cổng khác | Chấm cùng bài có ra cùng kết quả không | < 0.6 |
| Chất lượng lý do phán quyết | Người học đọc và đánh giá có hữu ích không | < 3/5 |
| Tỉ lệ kháng nghị bị lật | Phán quyết bị hội đồng lật | > 20% |
| Thời gian phản hồi | Người học chờ bao lâu | > 7 ngày |
| **Tỉ lệ đạt** | *Không dùng để đánh giá* | — |

> Cố ý không dùng tỉ lệ đạt. Gác cổng chặt tay và gác cổng dễ dãi đều có vấn đề, nhưng tỉ lệ đạt không phân biệt được "chặt" với "ngưỡng khó".

### Thu hồi

Thu hồi ngay lập tức nếu: thông đồng, nhận tiền riêng của người học, vi phạm quy tắc vị thành niên, đánh giá dựa trên đặc điểm cá nhân. Quyết định bởi hội đồng 3 người, không phải một người.

### Trả công

Gác cổng được trả cho **mỗi phiên bảo vệ**, bất kể kết quả. Không bao giờ trả theo tỉ lệ đạt — đó là cách nhanh nhất để mua chuộc hệ thống đánh giá. Xem [`17-economy.md`](17-economy.md).

---

## Kháng nghị

**Không có đường kháng nghị thì hệ thống là độc tài, và người giỏi sẽ bỏ đi trước tiên.**

```
người học không đồng ý phán quyết
  → nộp kháng nghị trong 14 ngày, nêu lý do
  → hội đồng kháng nghị: 1 gác cổng khác (không cùng ngưỡng) + 1 người học kỳ cựu
  → gác cổng ban đầu KHÔNG tham gia, nhưng được đọc và trả lời bằng văn bản
  → hội đồng xem bằng chứng gốc trước, đọc cả hai lý do sau
  → quyết định trong 14 ngày: giữ nguyên | lật | làm lại
  → kết quả và lý do công khai (ẩn danh người học)
```

**Nếu kháng nghị thành công:** người học nhận đủ tiền như đã đạt, cộng khoản bồi thường thời gian chờ. Gác cổng không bị phạt cho một lần lật — chỉ khi tỉ lệ lật cao mới thành tín hiệu.

**Kháng nghị là dữ liệu.** Tỉ lệ kháng nghị cao ở một ngưỡng = rubric mơ hồ, không phải người học hay cãi.

---

## Các hội đồng

| Hội đồng | Thành phần | Quyền | Nhiệm kỳ |
|---|---|---|---|
| **Hội đồng gác cổng** | Gác cổng đang tại nhiệm | Chọn/gia hạn/thu hồi gác cổng; sửa điều lệ | luân phiên |
| **Hội đồng văn hoá** | Nghệ nhân, nhà nghiên cứu, người của cộng đồng sở hữu | **Phủ quyết** nội dung văn hoá | 1 năm |
| **Hội đồng an toàn** | Độc lập, ≥1 người ngoài dự án | Phủ quyết ngưỡng có rủi ro; giám sát quy tắc vị thành niên | 1 năm |
| **Hội đồng tiền** | 3 người, ≥1 không thuộc đội sản phẩm | Giữ tiền, hoàn tiền, cáo buộc gian lận | 6 tháng |
| **Hội đồng đỉnh** | Những người đã lên đỉnh tối cao | Chấp nhận người mới lên đỉnh | — |

**Người sáng lập không có quyền phủ quyết chuyên môn** (Điều lệ §16). Người sáng lập có quyền: dừng hệ thống, dừng chi tiền, dừng một agent. Không có quyền: lật một phán quyết, ép một nội dung văn hoá, bỏ qua hội đồng an toàn.

---

## Quy trình sửa điều lệ

```
đề xuất công khai → 14 ngày thảo luận mở
  → đồng thuận hội đồng gác cổng (≥ 2/3)
  → nếu chạm tới an toàn hoặc tiền: thêm hội đồng tương ứng
  → ghi vào nhật ký sửa đổi công khai trong CHARTER.md
```

Không sửa điều lệ để đạt chỉ tiêu tăng trưởng. Không sửa vì một nhà tài trợ.

---

## Nút thắt cổ chai: gác cổng không nhân bản được

Đây là **trần tăng trưởng thật** của hệ thống. Phiên bảo vệ cần một con người, 20–30 phút, không thể tự động hoá mà không phá tính chính danh.

| Biện pháp | Hiệu quả | Đánh đổi |
|---|---|---|
| Trục TRUYỀN bắt buộc để lên đỉnh | Cao — nguồn gác cổng tự sinh | Chậm ở đầu |
| Trả công gác cổng tử tế | Cao | Chi phí |
| Bảo vệ theo nhóm 3 người ở ngưỡng thấp | Trung bình | Kém sâu hơn |
| Bảo vệ không đồng bộ (ghi hình + hỏi đáp viết) ở ngưỡng thấp | Trung bình | Mất phần phản ứng tức thời |
| AI tiền thẩm tốt hơn | Trung bình — giảm thời gian chuẩn bị, không giảm thời gian phiên | — |
| **Tự động hoá phán quyết** | — | **Cấm** (Điều lệ §12) |

Chỉ số phải theo dõi: **số phiên bảo vệ khả dụng mỗi tuần / số lượt nộp mỗi tuần**. Dưới 1.0 kéo dài là hệ thống đang nghẹt; phải mở gác cổng mới hoặc tạm chậm nhận người học.

---

## Ghi danh

Lấy từ bia đề danh tiến sĩ ở Văn Miếu.

```yaml
inscription:
  learner: <tên hiển thị hoặc bút danh>
  challenge: chal.…
  challenge_name_at_time: "Con quỷ hay nói dối"   # tên lúc đó, không đổi theo sau
  gatekeeper: <tên>
  date: 2026-03-14
  cohort: [<những người cùng phòng kín>]
  permanent: true
```

**Luật:**
1. **Bất biến.** Không sửa, không xoá, kể cả khi ngưỡng cửa chết.
2. **Sống lâu hơn ngưỡng cửa.** Ngưỡng vào `memorial`, ghi danh vẫn còn.
3. **Không bao giờ thu hồi vì suy giảm năng lực.** Bạn *đã* làm được điều đó vào ngày đó — đó là sự thật lịch sử.
4. Chỉ thu hồi khi phát hiện gian lận, và việc thu hồi cũng được ghi công khai.
5. Người học chọn tên hiển thị hoặc bút danh; đổi được, nhưng ghi danh giữ liên kết.

---

## Phòng kín

| Quy tắc | Vì sao |
|---|---|
| Chỉ người đang làm cùng ngưỡng | Không biến thành diễn đàn chung |
| Đóng khi ngưỡng đóng, lưu trữ lại | Áp lực xã hội thấp |
| Ghép theo **chữ ký lỗi khác nhau** | Bổ sung nhau; giống hệt nhau thì cùng bí một chỗ |
| Gác cổng đọc được, không tham gia | Minh bạch, không can thiệp |
| Vị thành niên: kiểm duyệt, không tin nhắn riêng | [`29-minors.md`](29-minors.md) |
| Trao đổi được khuyến khích, chép thì `veto` | Ranh giới nằm ở phiên bảo vệ — chép thì không bảo vệ được |
