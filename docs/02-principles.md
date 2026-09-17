# 02 · Mười hai nguyên tắc thiết kế

> Khi có tranh cãi về thiết kế, quay về đây. Nếu một tính năng vi phạm một nguyên tắc, tính năng đó sai — không phải nguyên tắc.

---

### 1. Đo quá trình, không đo sản phẩm

Sản phẩm cuối bây giờ ai cũng có thể có. Chỉ *cách một người đi tới đó* mới phân biệt được năng lực.
**Hệ quả kỹ thuật:** `ProcessTrace` là công dân hạng nhất trong mô hình dữ liệu, không phải tính năng phụ. Xây `@hsv/trace` ở G0.

### 2. Bất định phải nhìn thấy được

Hệ thống không giả vờ biết rõ. Sương mù trên bản đồ *là* độ bất định. Định vị luôn kèm bán kính `σ`. AI tiền thẩm luôn kèm mức độ không chắc.
**Hệ quả:** không API nào trả về một con số năng lực mà không kèm sai số.

### 3. Người ra phán quyết, máy làm việc nặng

Tự động hoá lao động, không tự động hoá phán quyết. Ranh giới này là nguồn tính chính danh của toàn hệ thống.
**Hệ quả:** `INV-08` cưỡng chế bằng CI.

### 4. Thế giới sinh ra từ dữ liệu, không vẽ tay

Địa hình, độ khó, lộ trình, vị trí — tất cả suy ra từ genome và hành vi thật. Cái gì vẽ tay được thì sẽ bị vẽ theo định kiến của người vẽ.
**Hệ quả:** `terrain = f(genome, telemetry, seed)` — hàm thuần, chạy lại được.

### 5. Quyền tự trị phải kiếm được

Người học mở khoá vùng mới bằng năng lực đã chứng minh. Gác cổng giữ chức bằng kết quả của người họ dẫn. Agent được hạn ngạch theo uy tín. **Không ai và không cái gì được tin sẵn.**

### 6. Cái chết là một phần của hệ thống

Ngưỡng cửa chết. Thái ấp hết hạn. Nhiệm kỳ gác cổng hết. Năng lực suy giảm. Một hệ thống chỉ tích luỹ mà không dọn sẽ tự nghẹt.
**Hệ quả:** mỗi thực thể nội dung phải có trạng thái vòng đời và điều kiện chết ghi rõ.

### 7. Thất bại phải rẻ và có phẩm giá

Chưa đạt: có lý do viết, có lối đi tiếp cụ thể, có khoản trả cho nỗ lực. Ngủ đông: không phạt. Bỏ đi: không mất gì.
**Vì sao:** người bạn muốn phục vụ nhất là người ít có khả năng chịu rủi ro nhất.

### 8. Không so sánh người với người

Không xếp hạng, không đường cong chuẩn, không phần trăm. Duy nhất một trục so sánh: bạn hôm nay với bạn trước đây.

### 9. Văn hoá là cấu trúc, không phải lớp sơn

Văn hoá định hình cách hệ thống vận hành (nghi thức, thứ bậc, ghi danh, tổ nghề), không chỉ màu sắc và hoa văn. Nhưng lõi kỹ thuật phải không biết tên của bất kỳ nền văn hoá nào.

### 10. Chạy được trên máy yếu, mạng yếu, không có AI

Người bạn muốn phục vụ nhất có điện thoại tầm trung và mạng chập chờn. Bản đồ 2D SVG phải **dùng được thật**, không phải bản suy biến. Hệ thống phải sống khi mọi API mô hình đóng cửa.

### 11. Minh bạch về nguồn gốc

Mọi asset có xuất xứ và giấy phép. Mọi lore có nhãn `[sử liệu]`/`[truyền thuyết]`/`[hư cấu]`. Mọi đồng tiền có đường đi công khai. Mọi phán quyết có lý do.
**Nguyên tắc chung:** nếu không truy nguyên được thì không đưa vào hệ thống.

### 12. Tối thiểu hoá quyền lực của người sáng lập

Mỗi khi bạn thêm một cơ chế, hỏi: *nếu tôi trở nên tệ hoặc biến mất, cơ chế này có tự bảo vệ người học không?* Điều lệ, kháng nghị, hội đồng độc lập, dữ liệu xuất được — tất cả tồn tại để trả lời "có".

---

## Bảng đánh đổi khi nguyên tắc xung đột

Nguyên tắc sẽ xung đột. Thứ tự ưu tiên khi phải chọn:

```
an toàn người học  >  tính chính danh của đánh giá  >  phẩm giá trải nghiệm
   >  bản sắc văn hoá  >  tính tự động  >  tốc độ phát triển  >  vẻ đẹp
```

Ví dụ áp dụng:

| Xung đột | Quyết định |
|---|---|
| AI đối kháng làm tăng độ chính xác đo lường nhưng có thể gây bực bội | Giữ, nhưng **báo trước** cho người học và lộ toàn bộ sau khi kết thúc — tính chính danh > phẩm giá trải nghiệm, nhưng không được lừa |
| Tự động hoá phán quyết giúp scale gấp 10 | Không. Tính chính danh > tính tự động |
| Chế độ vị thành niên làm chậm G1 | Làm. An toàn > tốc độ |
| Bản đồ 3D đẹp hơn nhưng loại máy yếu | Bản đồ 2D phải đủ dùng — nguyên tắc 10 thắng vẻ đẹp |
