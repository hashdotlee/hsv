# 44 · Chỉ số

> Chọn sai chỉ số thì sản phẩm sẽ tối ưu đúng theo chỉ số sai đó. File này liệt kê cả những chỉ số **cấm đo**.

## Chỉ số cấm

| Không đo | Vì sao |
|---|---|
| Thời lượng sử dụng hàng ngày | Khuyến khích giữ chân, không khuyến khích học |
| Chuỗi ngày | Vi phạm Điều lệ §1 |
| Số ngưỡng hoàn thành như thành tích | Khuyến khích ngưỡng dễ |
| Tỉ lệ đạt thô của gác cổng | Khuyến khích cho qua |
| Tổng số người dùng như chỉ số thành công | Không nói gì về giá trị |
| Bất kỳ xếp hạng toàn cục nào | Vi phạm Điều lệ §5 |

Không xây bảng điều khiển nào hiện các số này — kể cả "chỉ để tham khảo". Số hiện lên là số sẽ được tối ưu.

---

## Bốn chỉ số Bắc Đẩu

### 1. Năng lực chuyển giao được

```
tỉ lệ người học đạt ngưỡng X rồi qua ngưỡng CHƯA TỪNG THẤY cùng trục ở lần thử đầu
```

Đo cái duy nhất đáng đo: người ta có thật sự học được không, hay chỉ qua được đúng bài đó.
**Mục tiêu:** > 0,6 · **Báo động:** < 0,4

### 2. Khoảng cách với AI

```
tỉ lệ đạt của người  −  tỉ lệ đạt của mô hình mạnh nhất tự giải
```

Đo lý do tồn tại của sản phẩm. Thu hẹp dần nghĩa là ngưỡng cửa đang lạc hậu.
**Mục tiêu:** > 0,45 · **Báo động:** < 0,25 (INV-02)

### 3. Tỉ lệ tự tái tạo

```
số gác cổng xuất thân từ người học / tổng số gác cổng
```

Đo khả năng hệ thống sống mà không có bạn.
**Mục tiêu:** > 0,8 ở G4 · **Báo động:** < 0,4 ở G3

### 4. Giá trị hiển nhiên

```
tỉ lệ người học quay lại làm ngưỡng thứ hai mà KHÔNG được nhắc
```

Không thông báo đẩy, không email, không khuyến mãi. Đo giá trị thật, không đo kỹ thuật giữ chân.
**Mục tiêu:** > 0,55 · **Báo động:** < 0,35

---

## Sức khoẻ đánh giá

| Chỉ số | Mục tiêu | Báo động |
|---|---|---|
| Độ nhất quán giữa gác cổng (chấm chéo) | > 0,75 | < 0,6 |
| Tỉ lệ kháng nghị | 0,03–0,10 | < 0,01 hoặc > 0,20 |
| Tỉ lệ kháng nghị được lật | 0,15–0,35 | > 0,5 |
| Tỉ lệ gác cổng không đồng ý với tiền thẩm | 0,10–0,30 | < 0,05 |
| Thời gian từ nộp tới phán quyết | < 72h | > 168h |

Ba chỉ số này đáng chú ý ở **cả hai đầu**:

- **Kháng nghị quá thấp** ⇒ người học không biết mình có quyền, hoặc sợ. Nguy hiểm hơn kháng nghị cao.
- **Lật quá thấp** ⇒ hội đồng kháng nghị chỉ đóng dấu.
- **Không đồng ý với máy quá thấp** ⇒ gác cổng đang neo vào tóm tắt của máy thay vì đọc bằng chứng. Đây là dấu hiệu sớm của việc con người rút lui khỏi vai trò phán xét.

---

## Sức khoẻ thế giới

| Chỉ số | Mục tiêu | Báo động |
|---|---|---|
| Đa dạng ngữ nghĩa (khoảng cách nhúng trung bình) | ổn định hoặc tăng | giảm 3 tuần liền |
| Ngưỡng `live` mỗi atom | 2–6 | > 8 |
| Tỉ lệ nội dung có người đi qua trong 60 ngày | > 0,5 | < 0,25 |
| Tỉ lệ chết đúng quy trình / tổng nghỉ | > 0,8 | < 0,5 |
| Độ phủ bản đồ (vùng có ≥ 1 người đi) | tăng | đứng yên 8 tuần |

**Đa dạng ngữ nghĩa giảm là tín hiệu trôi dạt sớm nhất.** Nó xuất hiện trước khi bạn nhìn thấy sự loãng bằng mắt.

---

## Sức khoẻ dàn agent

| Chỉ số | Mục tiêu | Báo động |
|---|---|---|
| **Chi phí / đề xuất được chấp nhận & sống 30 ngày** | **giảm theo thời gian** | **tăng 3 tuần liền** |
| Tỉ lệ chấp nhận theo agent | 0,3–0,7 | > 0,9 (kiểm định quá dễ) |
| Tỉ lệ sống sót 30 ngày | > 0,6 | < 0,4 |
| Tỉ lệ bị Red Team phá | 0,1–0,3 | < 0,05 (Red Team đang lười) |
| Độ sâu hàng đợi duyệt của người | < 40 | > 120 |
| % ngân sách đã dùng | < 70% | > 90% |

Chỉ số in đậm là **chỉ số duy nhất phải xem hằng ngày** khi dàn agent đang chạy. Nếu nó không giảm theo thời gian, dàn agent đang chạy mà không học — bạn đang trả tiền để tạo rác.

Tỉ lệ chấp nhận **quá cao** cũng là báo động: nghĩa là bộ kiểm định không còn kiểm gì.

---

## Kinh tế

| Chỉ số | Mục tiêu |
|---|---|
| CPQ (chi phí cho một người đạt) | < 6.000.000đ |
| % tiền tới tay người học | > 60% |
| % phí nền tảng vào Quỹ Bảo tồn | ≥ 50% (khoá bằng điều lệ) |
| Tỉ lệ nhà tài trợ quay lại | > 0,5 |
| Sai lệch đối chiếu sổ cái | **0** |
| Tỉ lệ đạt trước/sau khi ngưỡng có tiền | **không đổi có ý nghĩa thống kê** |

Dòng cuối là **kiểm định tính chính danh của toàn hệ thống**. Tỉ lệ đạt tăng sau khi có tiền ⇒ dừng chi trả và điều tra ngay, không chờ hết quý.

---

## Công bằng & an toàn

| Chỉ số | Mục tiêu |
|---|---|
| Chênh lệch tỉ lệ đạt giữa các nhóm nhân khẩu | < 0,1 |
| Chênh lệch giữa người có bằng và không bằng | **≈ 0** |
| Chênh lệch giữa thiết bị mạnh và yếu | **≈ 0** |
| Sự cố an toàn vị thành niên | **0** |
| Thời gian xử lý báo cáo an toàn | < 24h |
| Tỉ lệ hoàn thành phiên bảo vệ có phương án thay thế | ≈ tỉ lệ chung |

Chênh lệch giữa người có bằng và không có bằng bằng 0 là **mục tiêu định danh của dự án**. Nếu hệ thống tái tạo lại đúng bất bình đẳng của hệ thống bằng cấp hiện có, nó không có lý do tồn tại.

---

## Nhịp xem

```
hàng ngày   chi phí/đề xuất-sống-30-ngày · báo động an toàn · sai lệch sổ cái
hàng tuần   hàng đợi duyệt · đa dạng ngữ nghĩa · năng suất gác cổng · ngân sách
hàng tháng  bốn chỉ số Bắc Đẩu · sức khoẻ đánh giá · công bằng
hàng quý    kinh tế · độ phủ bản đồ · rà nội dung chính điển
```

## Công khai

Công khai (Điều lệ §10): sổ cái tổng hợp · tỉ lệ đạt theo ngưỡng · tỉ lệ kháng nghị và lật · danh sách nhà tài trợ và số tiền · chi phí dàn agent.

Không công khai: dữ liệu cá nhân · vết · phán quyết cá nhân · video · bất kỳ thứ gì cho phép xếp hạng người học với nhau.
