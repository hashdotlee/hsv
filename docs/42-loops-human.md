# 42 · Vòng lặp Người

> Người làm **phán xét**, máy làm **lao động**. File này liệt kê chính xác chỗ nào con người bắt buộc phải có mặt, và chỗ nào thì không.

## Bảng tổng

| # | Vòng lặp | Ai | Tần suất | Từ |
|---|---|---|---|---|
| H1 | Chấm và phán quyết | Gác cổng | Mỗi lần nộp | G0 |
| H2 | Phiên bảo vệ | Gác cổng | Mỗi lần nộp | G0 |
| H3 | Kháng nghị | Hội đồng kháng nghị | Theo yêu cầu | G1 |
| H4 | Duyệt đề xuất agent | Gác cổng vùng | Hàng ngày | G2 |
| H5 | Duyệt văn hoá | Hội đồng văn hoá | Hàng tuần | G2 |
| H6 | Tuyển và gia hạn gác cổng | Hội đồng gác cổng | Hàng tháng | G2 |
| H7 | Điều tra an toàn | Hội đồng an toàn | Theo báo động | G1 |
| H8 | Điều tiết hạn ngạch &amp; chi phí | Người điều hành | Hàng tuần | G2 |
| H9 | Duyệt nhà tài trợ | Hội đồng | Mỗi nhà tài trợ | G3 |
| H10 | Sửa điều lệ | Hội đồng + cộng đồng | Theo yêu cầu | G2 |
| H11 | Rà nội dung chính điển | Hội đồng | Hàng quý | G4 |

---

## H1 · Chấm và phán quyết

```
nhận việc  →  XEM BẰNG CHỨNG GỐC TRƯỚC  →  đọc tiền thẩm của máy
  →  đánh dấu chỗ không đồng ý với máy  →  tiến hành H2
  →  chấm từng tiêu chí rubric  →  viết LÝ DO nhìn thấy được
  →  viết bước tiếp theo (bắt buộc kể cả khi đạt)
  →  ghi phán quyết
```

**Thứ tự cưỡng chế bằng API:** `/preassess` trả `409` nếu chưa gọi `/evidence`. Không phải quy ước lịch sự — nếu đọc tóm tắt trước, gác cổng sẽ neo vào nó.

Thời gian: 30–60 phút một lần thử. Đây là ràng buộc quy mô thật của hệ thống, không phải một con số ước lượng có thể bỏ qua.

## H2 · Phiên bảo vệ

20 phút, dựa trên vết của chính người học:

```
0–3    người học kể lại quá trình của mình
3–10   gác cổng hỏi "vì sao" tại các điểm quyết định
10–15  câu hỏi phản thực: "nếu X khác đi thì sao?"
15–18  người học nói mình chưa chắc chắn ở đâu
18–20  gác cổng giải thích bằng chứng và bước tiếp theo
```

Vị thành niên: luôn ghi hình, luôn có người thứ ba ([`29-minors.md`](29-minors.md)).

Câu 15–18 quan trọng hơn vẻ ngoài của nó: người hiểu thật biết mình chưa chắc ở đâu; người copy thì không.

## H3 · Kháng nghị

```
người học không đồng ý  →  nộp trong 14 ngày
  →  gác cổng độc lập + một người học kỳ cựu
  →  GÁC CỔNG BAN ĐẦU KHÔNG THAM GIA QUYẾT ĐỊNH
  →  xem bằng chứng trước, xem lý do sau
  →  quyết trong 14 ngày, công khai lý do
  →  lật ⇒ tính vào chỉ số của gác cổng ban đầu
```

Tỉ lệ bị lật cao ở một gác cổng là tín hiệu quan trọng — nhưng tỉ lệ **bằng 0** cũng đáng ngờ (có thể không ai dám kháng nghị).

## H4 · Duyệt đề xuất agent

Chỉ ở vùng **có người ở** và **chính điển**. Vùng biên: máy tự phát hành, người chỉ xem tổng hợp tuần.

```
xem hàng đợi  →  với mỗi đề xuất:
  đây có phải năng lực THẬT không?
  ngưỡng này có đo được nó không?
  mình có muốn làm nó không?
  → duyệt / sửa / loại (KÈM LÝ DO máy đọc được)
```

**Giới hạn cứng: tối đa 20 lượt duyệt một phiên.** Quá số đó con người bắt đầu bấm đồng ý hàng loạt, và cổng duyệt thành hình thức. Kèm lấy mẫu kiểm tra ngược để phát hiện việc này.

## H5 · Duyệt văn hoá

```
hàng tuần: nội dung văn hoá mới + asset mới
  → đúng không? tôn trọng không? nhãn nguồn đúng chưa?
  → có quyền sử dụng không?
  → cộng đồng liên quan có được tham gia và hưởng lợi không?
  → phủ quyết được, không cần giải thích với ai ngoài hội đồng
```

## H6 · Tuyển và gia hạn gác cổng

```
hàng tháng: ai đủ điều kiện? (đã qua ngưỡng đó, mastery ≥ 0.75, ≥ 30 ngày)
  → mời làm ngưỡng chal.TRUYEN.dan-mot-nguoi-qua
  → thử việc: 3 lần chấm có người kèm
  → nhiệm kỳ 6 tháng
gia hạn: xét theo TIẾN BỘ CỦA NGƯỜI HỌC, độ nhất quán, chất lượng lý do,
         tỉ lệ bị lật, thời gian phản hồi — KHÔNG theo tỉ lệ đạt thô
```

## H7 · Điều tra an toàn

Theo báo động của `sentinel` hoặc báo cáo của người học.

```
P0 (vị thành niên gặp nguy hiểm / rò rỉ / mất tiền): ngay lập tức
  → đình chỉ tạm thời nếu cần
  → điều tra, nghe cả hai phía
  → quyết định + đường kháng nghị
```

Máy **không bao giờ** tự đưa ra kết luận gian lận.

## H8 · Điều tiết hạn ngạch &amp; chi phí

```
hàng tuần đọc báo cáo của steward:
  thế giới đang nở nhanh hơn hay chậm hơn số người thật?
  chi phí mỗi đề xuất sống-30-ngày đang tăng hay giảm?
  hàng đợi duyệt có đang vỡ không?
  → chỉnh hạn ngạch: mở van hoặc siết van
```

Đây là **van chính bạn nắm** trong một hệ thống phần lớn tự chạy. Một quyết định mỗi tuần, nhưng là quyết định quan trọng nhất trong tuần.

## H9 · Duyệt nhà tài trợ

```
nhà tài trợ đề nghị  →  kiểm: có xung đột giá trị không?
  → giải thích rõ điều họ KHÔNG mua được (rubric, phán quyết, danh tính sớm)
  → ký cam kết
  → công khai trên sổ cái
```

## H10 · Sửa điều lệ

```
đề xuất  →  thảo luận công khai ≥ 30 ngày
  →  hội đồng gác cổng bỏ phiếu, cần 2/3
  →  người sáng lập KHÔNG có quyền phủ quyết
  →  ghi thành sự kiện ConstitutionAmended, có diff
```

## H11 · Rà nội dung chính điển

Hàng quý, vùng > 200 lượt: nội dung này còn đúng không? Nghề đã đổi chưa? Mô hình đã qua được chưa? Sửa nội dung chính điển nặng như sửa luật.

---

## Ở đâu con người **không** cần có mặt

Nói rõ để khỏi tạo nút thắt giả:

- Kiểm định cú pháp và bất biến
- Phát hiện trùng lặp
- Đo `ai_solo_pass_rate`
- Tính lại địa hình
- Tóm tắt vết
- Nội dung ở vùng biên
- Theo dõi chi phí
- Ghép phòng kín

## Ràng buộc quy mô

Một gác cổng: **≤ 6 người học đồng thời**, 30–60 phút một lần chấm.

```
1 gác cổng ≈ 6 người học   ⇒   500 người học ≈ 85 gác cổng
```

**Đây là giới hạn tăng trưởng thật của hệ thống.** Nó chỉ nới ra bằng một con đường: người học trở thành gác cổng. Đó là lý do trục TRUYỀN không phải trang trí — nó là cơ chế mở rộng quy mô duy nhất.
