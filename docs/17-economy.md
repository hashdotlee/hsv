# 17 · Kinh tế

> Tiền chỉ chảy theo **năng lực đã kiểm chứng**. Không theo giờ học, lượt đăng nhập, hay bất kỳ chỉ số tương tác nào.

## Dòng tiền

```
NGUỒN VÀO                           PHÂN BỔ (ví dụ một ngưỡng 4.000.000đ)
──────────────────────────          ─────────────────────────────────────
Nhà tài trợ doanh nghiệp      →     Người học đạt          2.400.000đ  (60%)
Ngân sách nhà nước/địa phương →     Người học chưa đạt       200.000đ  ( 5%)
Gia đình đầu tư cho con em    →     Gác cổng                 600.000đ  (15%)
Kho bạc hệ thống              →     Tô nhượng di sản         120.000đ  ( 3%)
Quỹ Bảo tồn                   →     Kho bạc hệ thống         680.000đ  (17%)
                                        └─ ≥50% vào Quỹ Bảo tồn
```

## Bốn loại chi trả

| Loại | Kích hoạt | Ghi chú |
|---|---|---|
| **Trả ngưỡng** `thresholdPayout` | `Verdict.result == pass` | Chính |
| **Trả nỗ lực** `effortPayout` | `fail` + vết cho thấy nỗ lực thật | 5–10% của trả ngưỡng. **Để người nghèo dám thử** |
| **Trợ cấp lộ trình** `pathStipend` | Tiến bộ kiểm chứng được liên tục qua nhiều ngưỡng | Cho người cần thu nhập để tiếp tục học |
| **Tô nhượng di sản** `legacyRoyalty` | Vết chuyên gia của người A giúp người B qua ngưỡng | Trả lâu dài. Đây là thứ khiến chuyên gia chịu ngồi kể |

Cộng thêm: **trả công gác cổng** cho mỗi phiên bảo vệ, bất kể kết quả.

---

## Ký quỹ

```
nhà tài trợ chuyển tiền
  → vào ký quỹ, gắn với một ngưỡng cụ thể     (tiền đã cam kết, chưa thuộc về ai)
  → người học nộp bài
  → phán quyết của người
  → giải ngân sau 72 giờ                       (cửa sổ kháng nghị)
  → nếu có kháng nghị: giữ tới khi xử xong
```

**Luật:**
1. Không có tiền nào không gắn với một `Verdict` (bất biến mô hình miền #6).
2. Nhà tài trợ **không rút lại được** tiền đã ký quỹ vì không thích kết quả.
3. Sổ cái nội bộ là **nguồn sự thật**; cổng thanh toán chỉ là ống dẫn (Hiến pháp phụ thuộc).
4. Sổ cái chỉ ghi thêm, không sửa. Sửa sai bằng bút toán đảo, không bằng xoá.

---

## Nhà tài trợ

### Được phép

- Đặt bài toán thật của doanh nghiệp làm ngưỡng cửa
- Tài trợ một ngưỡng hoặc một vùng bản đồ (có ghi tên, kín đáo)
- Cấp học bổng / trợ cấp lộ trình
- Tài trợ công cụ, thiết bị, tài khoản dịch vụ cho người học
- Nhận báo cáo **tổng hợp** về số người đạt năng lực họ cần
- Liên hệ tuyển dụng **sau khi** người học đồng ý, thông qua hệ thống

### Không được phép

| Cấm | Vì sao |
|---|---|
| Sửa rubric hoặc chạm vào phán quyết | Điều lệ §9 |
| Biết danh tính người học trước khi có kết quả | Chống thiên vị |
| Độc quyền với người đã đào tạo | Người học không phải hàng hoá |
| Video quảng cáo xen ngang | Phá nghi thức |
| Mua tiến độ, phán quyết, hoặc ngoại hình nhân vật | Điều lệ §8 |
| Yêu cầu ngưỡng chỉ dùng công nghệ của họ | Biến hệ thống thành kênh bán hàng |

### Bán theo CPQ, không bán theo CPM

**Đơn vị bán hàng là "một người đạt năng lực"** (`costPerQualified`), không phải lượt hiển thị.

Nếu bán lượt hiển thị: bạn thua Facebook về quy mô, và tự ép mình tăng quảng cáo cho tới khi giết chết tính thiêng. Nếu bán năng lực: bạn bán thứ không ai khác có, và động cơ của bạn thẳng hàng với động cơ của người học.

```
CPQ = tổng chi của nhà tài trợ / số người đạt ngưỡng họ tài trợ
```

So sánh với chi phí tuyển dụng thật (phí headhunter 15–25% lương năm, chi phí tuyển sai còn cao hơn nhiều) — đây là luận điểm bán hàng.

---

## Quảng cáo trong thế giới

| Mức | Hình thức | Trạng thái |
|---|---|---|
| Tốt | Ngưỡng do nhà tài trợ đặt, công cụ tặng, học bổng, thiết bị | ✅ khuyến khích |
| Được | Tên nhà tài trợ trên một vùng bản đồ, biển hiệu trong thế giới (kín đáo, đúng thẩm mỹ) | ⚠️ có hạn mức |
| Cấm | Video xen ngang, banner, theo dõi hành vi để nhắm quảng cáo, pay-to-progress | ❌ |

Hạn mức: không quá 1 yếu tố thương hiệu hiển thị trên một màn hình; không bao giờ trong phiên bảo vệ, lễ nhập môn, hay trang ghi danh.

---

## Quỹ Bảo tồn

**≥50% phí nền tảng, khoá bằng điều lệ pháp lý** (Điều lệ §10).

Dùng cho: kỹ năng quan trọng nhưng không có nhà tài trợ thương mại — nghề truyền thống, kỹ năng đang mai một, vùng dân tộc thiểu số, và các trục NGƯỜI/TRUYỀN mà doanh nghiệp ít chịu trả tiền.

Nếu không khoá bằng điều lệ, quỹ này sẽ bị cắt trong quý khó khăn đầu tiên. Đây là dự đoán chắc chắn, không phải lo xa.

---

## Chống gian lận tiền

| Kiểu | Phát hiện | Chặn |
|---|---|---|
| Thông đồng gác cổng–người học | Cụm lặp lại, tỉ lệ đạt lệch, thời gian phiên ngắn bất thường | Chấm chéo bắt buộc trên ngưỡng tiền lớn; xoay gác cổng |
| Nhiều tài khoản | Trùng thiết bị, vết, nhịp gõ | Xác minh danh tính khi vượt hạn mức tiền tích luỹ |
| Nông trại tài khoản | Đăng ký hàng loạt, hành vi đồng nhất | Giới hạn tốc độ; ngưỡng đầu tiên luôn có phiên bảo vệ |
| Nhà tài trợ rửa tiền | Dòng tiền bất thường | Hội đồng tiền + hạn mức + kiểm tra danh tính pháp nhân |
| Vị thành niên bị lợi dụng tiền | Tiền vào quỹ giám hộ, không vào tay trực tiếp | [`29-minors.md`](29-minors.md) |

**Chỉ số cảnh báo sớm quan trọng nhất:** tỉ lệ đạt **tăng vọt** sau khi một ngưỡng có tiền. Nghĩa là gác cổng đang mềm tay vì tiền, hoặc có thông đồng.

---

## Sổ cái công khai

Công khai ở mức tổng hợp, đối chiếu được:

```
tháng 3/2026
  Vào:   nhà tài trợ 48.000.000đ · kho bạc 12.000.000đ
  Ra:    người học đạt 31.200.000đ (26 người)
         người học chưa đạt 2.400.000đ (16 người)
         gác cổng 7.800.000đ (42 phiên)
         tô nhượng di sản 1.560.000đ (9 chuyên gia)
  Còn:   kho bạc 17.040.000đ → Quỹ Bảo tồn 8.520.000đ
  Đang ký quỹ: 22.000.000đ (14 ngưỡng)
```

Không công khai: danh tính người nhận, số tiền của từng cá nhân.

---

## Kinh tế đơn vị cần đạt

| Chỉ số | Mục tiêu G3 | Mục tiêu G5 |
|---|---|---|
| CPQ | < 6.000.000đ | < 3.500.000đ |
| Tỉ lệ tiền tới tay người học | > 60% | > 65% |
| Chi phí vận hành agent / người học tháng | < 40.000đ | < 15.000đ |
| Tỉ lệ nhà tài trợ quay lại | — | > 60% |
| Quỹ Bảo tồn / phí nền tảng | ≥ 50% | ≥ 50% |

---

## Ba điều không bao giờ mua được

**Tiến độ · Phán quyết · Ngoại hình nhân vật.**

Ngoại hình nhân vật phải luôn là **bản kể trung thực về việc người đó đã làm được**. Đây vừa là điều lệ vừa là luật kỹ thuật: `@hsv/atlas` sinh ngoại hình *từ* dữ liệu năng lực đã kiểm chứng, và không có đường nào khác để thay đổi nó.
