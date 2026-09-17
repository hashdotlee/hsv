# 52 · Runbook vận hành

> Quy trình cho lúc có chuyện. Viết trước khi cần, vì lúc cần thì không còn bình tĩnh để nghĩ.

## Mức sự cố

| Mức | Ví dụ | Phản ứng | Ai |
|---|---|---|---|
| **P0** | Vị thành niên gặp nguy hiểm · rò rỉ dữ liệu · mất tiền | Ngay lập tức | Người sáng lập + hội đồng an toàn |
| **P1** | Nội dung độc lọt lưới · gác cổng thông đồng · sandbox bị thoát | Trong ngày | Trực + hội đồng liên quan |
| **P2** | Lỗi chấm hệ thống · dịch vụ ngừng | Trong 72h | Trực |
| **P3** | Nội dung sai · lỗi hiển thị | Quy trình thường | Người bảo trì |

---

## RB-01 · P0: An toàn vị thành niên

```
1. ĐÌNH CHỈ NGAY tài khoản liên quan (cả hai phía nếu chưa rõ)
2. Giữ nguyên toàn bộ bằng chứng — KHÔNG xoá, KHÔNG sửa
3. Báo hội đồng an toàn trong 1 giờ
4. Liên hệ người giám hộ trong 24 giờ
5. Cân nhắc nghĩa vụ báo cơ quan chức năng theo luật sở tại
6. Điều tra: nghe cả hai phía, không kết luận trước
7. Quyết định + đường kháng nghị
8. Phân tích không đổ lỗi trong 7 ngày
9. Mỗi sự cố phải sinh ra một CƠ CHẾ CHẶN, không chỉ một lời hứa
```

**Không bao giờ:** xử lý riêng tư không qua hội đồng · trì hoãn vì sợ tai tiếng · xoá bằng chứng.

## RB-02 · P0: Rò rỉ dữ liệu

```
1. Cô lập: ngắt bề mặt bị lộ
2. Xác định phạm vi: dữ liệu gì, bao nhiêu người
3. Xoay toàn bộ khoá và chứng chỉ
4. Báo người bị ảnh hưởng trong 72 giờ — NÓI RÕ, không giảm nhẹ
5. Bảo toàn log để điều tra
6. Báo cơ quan chức năng theo luật
7. Công bố công khai sau khi đã báo người bị ảnh hưởng
```

## RB-03 · P0: Sai lệch sổ cái

```
1. TẠM DỪNG mọi giải ngân ngay
2. Đối chiếu sổ cái nội bộ với cổng thanh toán, từng bút toán
3. Tìm bút toán đầu tiên lệch
4. Sửa bằng BÚT TOÁN ĐẢO, không bao giờ sửa bản ghi cũ
5. Nếu người học bị thiệt: trả bù ngay, xin lỗi, không bắt họ chứng minh
6. Nếu người học được lợi do lỗi: KHÔNG đòi lại — đó là lỗi của hệ thống
7. Mở lại giải ngân chỉ sau khi đối chiếu khớp 100%
```

Nguyên tắc 6 là quyết định có chủ ý: chi phí đòi lại tiền — về niềm tin — lớn hơn số tiền.

## RB-04 · P1: Agent sinh nội dung độc

```
1. Ngắt agent đó (< 60s)
2. Liệt kê mọi đề xuất đã chấp nhận từ agent đó trong 30 ngày
3. Đưa toàn bộ về trạng thái quarantine
4. Với đề xuất có người học đã đi qua: KHÔNG xoá kết quả của họ
   → ghi danh của họ vẫn còn giá trị (Điều lệ §6)
5. Điều tra: prompt? mô hình? nguồn đầu vào? bị chiếm?
6. Sửa và chạy thử lại 2 tuần trước khi bật lại
7. Kiểm: vì sao redteam không bắt được? bổ sung ca kiểm thử
```

## RB-05 · P1: Gác cổng thông đồng

```
1. Sentinel báo động → hội đồng an toàn nhận, KHÔNG tự động xử lý
2. Tạm ngưng quyền chấm (không công bố lý do khi chưa kết luận)
3. Rà toàn bộ phán quyết của gác cổng đó: chấm chéo mù bởi 2 người khác
4. Nghe gác cổng trình bày
5. Kết luận:
   - Có thông đồng → thu hồi, xử lý phán quyết liên quan, báo người học
   - Không → phục hồi, xin lỗi công khai với chính họ
6. Người học bị ảnh hưởng: chấm lại miễn phí, giữ nguyên tiền đã nhận
```

## RB-06 · P1: Sandbox bị thoát

```
1. Ngắt toàn bộ crucible, dừng nhận lần thử mới
2. Bảo toàn container liên quan để pháp chứng
3. Đánh giá: chạm được gì? mạng nội bộ? dữ liệu khác?
4. Vá, thử lại bằng cùng kỹ thuật thoát
5. Mở lại theo từng phần
6. Nếu do người học cố ý: điều tra riêng — nhưng cũng xem đây là
   một báo cáo bảo mật có giá trị; cân nhắc mời họ làm redteam
```

## RB-07 · P2: Chi phí AI vọt

```
1. Tự động dừng cứng ở 100% ngân sách tuần
2. Xem steward: agent nào? tác vụ nào?
3. Tạm thời: định tuyến sang mô hình rẻ hơn / cục bộ
4. Nguyên nhân thường gặp: vòng lặp thử lại · ngữ cảnh phình
   · một agent gọi agent khác không giới hạn
5. Siết hạn ngạch agent đó
6. Bật lại dần
```

**Trong lúc này ứng dụng vẫn chạy bình thường.** Người học không bị ảnh hưởng — đó là toàn bộ ý nghĩa của Điều lệ §24.

## RB-08 · P2: Nhà cung cấp mô hình ngừng hoặc đổi hành vi

```
1. Oracle tự chuyển sang fallback
2. Chạy bộ đề xuất chuẩn để đo hồi quy chất lượng
3. Chất lượng tụt đáng kể → tăng mức duyệt của người tạm thời
4. Không fallback nào chạy → TẮT toàn bộ sinh nội dung
   → ứng dụng chuyển sang chế độ chỉ người: người tự viết, người tự chấm
5. Thông báo cho gác cổng về tải tăng
```

Chế độ chỉ-người phải được **diễn tập mỗi quý**, không phải chỉ tồn tại trên giấy.

## RB-09 · P2: Hàng đợi duyệt vỡ trận

```
Dấu hiệu: > 120 đề xuất chờ, hoặc người học chờ phán quyết > 7 ngày

1. Xác định: thiếu gác cổng, hay agent sinh quá nhanh?
2. Agent sinh quá nhanh → siết hạn ngạch ngay (van chính)
3. Thiếu gác cổng → tạm mở rộng điều kiện, tăng trả công,
   ưu tiên hàng đợi theo thời gian chờ
4. Người học chờ lâu: báo họ, xin lỗi, trả thêm tiền chờ
5. Dài hạn: đẩy mạnh trục TRUYỀN — đây là bài toán cấu trúc, không phải sự cố
```

## RB-10 · Khôi phục từ sao lưu

```
Diễn tập HÀNG THÁNG với kho sự kiện, HÀNG QUÝ với phần còn lại.

1. Khôi phục kho sự kiện vào môi trường tách biệt
2. Kiểm chuỗi băm từ đầu tới cuối
3. hsv projections rebuild --all
4. So bản chiếu dựng lại với bản đang chạy — lệch một trường là lỗi nghiêm trọng
5. Kiểm mẫu: 10 lần thử ngẫu nhiên, vết phát lại được, phán quyết khớp
6. Ghi lại thời gian khôi phục thực tế
```

**Bản sao lưu chưa từng thử khôi phục thì không phải bản sao lưu.**

## RB-11 · Phát hành culture pack

```
1. hsv pack validate <id>
2. Hội đồng văn hoá duyệt bản dựng thử
3. Kiểm PROVENANCE.json: mọi asset có mục, không có "unclear"
4. Gắn thẻ bất biến, phát hành
5. Theo dõi 48h
6. Có vấn đề → hsv pack rollback (đổi con trỏ, không build lại)
   → người đang dùng gói cũ KHÔNG bị gián đoạn giữa chừng
```

## RB-12 · Sửa điều lệ

```
1. Đề xuất công khai kèm lý do
2. Thảo luận ≥ 30 ngày, không rút ngắn
3. Hội đồng gác cổng bỏ phiếu, cần 2/3
4. Người sáng lập KHÔNG có quyền phủ quyết
5. Ghi sự kiện ConstitutionAmended kèm diff
6. Thông báo mọi người học
```

---

## Lịch định kỳ

```
hàng ngày   chi phí/đề xuất-sống-30-ngày · báo động an toàn · đối chiếu sổ cái
hàng tuần   hàng đợi duyệt · đa dạng ngữ nghĩa · ngân sách · báo cáo steward
hàng tháng  diễn tập khôi phục kho sự kiện · xét gác cổng · chỉ số Bắc Đẩu
            · thử công tắc ngắt THẬT
hàng quý    diễn tập chế độ chỉ-người · rà nội dung chính điển
            · rà rủi ro · diễn tập khôi phục toàn bộ
hàng năm    kiểm toán độc lập về đánh giá và tiền
```

## Liên hệ khẩn

Ghi vào `OPERATIONS.md` riêng (không công khai): trực chính · hội đồng an toàn · hội đồng văn hoá · cổng thanh toán · pháp lý.

## Sau mọi sự cố

```
1. Dòng thời gian: điều gì đã xảy ra, khi nào
2. Vì sao hệ thống cho phép điều đó
3. Vì sao không phát hiện sớm hơn
4. CƠ CHẾ CHẶN cụ thể (không phải "sẽ cẩn thận hơn")
5. Công khai nếu có ảnh hưởng tới người học
6. Cập nhật runbook này
```

Không đổ lỗi cá nhân. Nếu một người gây ra sự cố, câu hỏi là vì sao hệ thống cho phép một người làm được điều đó.
