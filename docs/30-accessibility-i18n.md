# 30 · Khả năng truy cập &amp; Đa ngôn ngữ

> Người bạn muốn phục vụ nhất có điện thoại tầm trung và mạng chập chờn. Khả năng truy cập ở đây không phải tuân thủ quy định — nó là điều kiện để dự án có ý nghĩa.

## Ngân sách hiệu năng

| Chỉ số | Mục tiêu | Tối đa |
|---|---|---|
| Tải lần đầu (nén) | 180 KB | 300 KB |
| Hiển thị nội dung đầu tiên (3G) | < 2,5s | 4s |
| Tương tác được (điện thoại tầm trung) | < 4s | 6s |
| Một vùng bản đồ 2D | 120 KB | 200 KB |
| RAM khi chạy (2D) | 80 MB | 150 MB |

CI chặn merge khi vượt ngân sách. Không có ngoại lệ "tạm thời" — tạm thời sẽ thành vĩnh viễn.

## Thiết bị mục tiêu

Kiểm thử thật trên: điện thoại Android tầm trung 4 năm tuổi · laptop cũ 4GB RAM · mạng 3G chập chờn · màn hình 360px.

**Không** kiểm thử chỉ trên máy của người phát triển. Nếu không có thiết bị thật thì bóp băng thông và CPU trong trình duyệt — nhưng thiết bị thật vẫn tốt hơn.

---

## Khả năng truy cập

Mục tiêu: **WCAG 2.2 AA** cho toàn bộ ứng dụng.

### Bản đồ — chỗ khó nhất

Bản đồ là giao diện chính, và bản đồ trực quan loại trừ người khiếm thị. Bắt buộc:

```
Mọi thông tin trên bản đồ phải có ở cả ba dạng:
  1. Bản đồ trực quan (SVG, mặc định)
  2. Danh sách phân cấp có cấu trúc, điều hướng bằng bàn phím
  3. Mô tả văn bản: "Bạn đang ở rìa vùng Chẩn đoán. Ba hướng: …"
```

Không bao giờ bản đồ là **cách duy nhất** để lấy một thông tin. Dạng 2 và 3 không phải bản thay thế hạng hai — chúng là bề mặt đầy đủ.

### Danh mục

- [ ] Bàn phím đi được toàn bộ; thứ tự tiêu điểm hợp lý; vòng tiêu điểm nhìn thấy rõ
- [ ] Trình đọc màn hình: nhãn ARIA đúng, vùng động thông báo đúng lúc
- [ ] Tương phản AA ở cả hai chủ đề, mọi trạng thái
- [ ] Không truyền đạt thông tin **chỉ** bằng màu (trạng thái luôn có chữ hoặc hình)
- [ ] `prefers-reduced-motion` tôn trọng tuyệt đối
- [ ] Vùng chạm ≥ 44×44px
- [ ] Phóng tới 200% không vỡ bố cục
- [ ] Phiên bảo vệ: có phụ đề; có phương án chỉ âm thanh; có phương án văn bản không đồng bộ

### Phiên bảo vệ và khả năng truy cập

Đây là chỗ va chạm thật: phiên bảo vệ bằng lời nói loại trừ người khiếm thính và người nói khó. Phương án thay thế **phải có giá trị tương đương**, không phải bản rút gọn:

| Phương án | Hình thức |
|---|---|
| Nói trực tiếp | Mặc định |
| Có phiên dịch ngôn ngữ ký hiệu | Theo yêu cầu |
| Hỏi đáp viết, không đồng bộ | Gác cổng hỏi, người học trả lời bằng văn bản trong 24h |
| Trình bày viết + hỏi đáp | Cho người cần thêm thời gian xử lý |

Rubric **không được** thưởng cho sự lưu loát bằng lời — chỉ thưởng cho chất lượng lập luận (INV-14).

---

## Đa ngôn ngữ

### Ba lớp tách biệt

| Lớp | Nội dung | Ai dịch |
|---|---|---|
| **Giao diện** | Nút, nhãn, thông báo | File dịch, cộng đồng đóng góp |
| **Nội dung** | Ngưỡng cửa, atom, rubric | Người, có review — **không dịch máy tự động** |
| **Văn hoá** | Từ vựng nghi thức, lore | Gói văn hoá, không phải bản dịch |

Ba lớp này **không được trộn**. Dịch giao diện sang tiếng Nhật ≠ có gói văn hoá `ja-JP`.

### Tiếng Việt là ngôn ngữ nguồn

Nội dung viết bằng tiếng Việt trước, dịch sang tiếng Anh sau. Ngược lại sẽ tạo ra thứ tiếng Việt dịch máy.

**Yêu cầu kỹ thuật với tiếng Việt:**
- Dấu đúng ở mọi trọng lượng font (kiểm thật với `ề ữ ỵ ẫ ộ ỡ`)
- Sắp xếp theo thứ tự tiếng Việt, không theo mã Unicode
- Tìm kiếm không dấu vẫn ra kết quả có dấu
- Chuẩn hoá Unicode NFC lúc lưu
- Xuống dòng đúng với từ ghép

### Tiếng Anh kỹ thuật là nội dung, không phải giao diện

Nhiều người học đến để **học đọc tài liệu kỹ thuật tiếng Anh**. Nghĩa là:

- Giao diện tiếng Việt, nhưng nội dung kỹ thuật giữ nguyên tiếng Anh khi đó là điều người học cần gặp
- Có công tắc "hiện bản dịch" nhưng **mặc định tắt** ở ngưỡng nhắm vào lớp nền tiếng Anh
- Thuật ngữ hiển thị song ngữ ở lần xuất hiện đầu: "tranh chấp tài nguyên (race condition)"

Giấu tiếng Anh đi là làm hại chính người cần học nó.

---

## Mạng yếu

| Kỹ thuật | |
|---|---|
| Ngoại tuyến trước | Service worker đệm bản đồ, ngưỡng cửa, rubric |
| Vết xếp hàng | Ghi cục bộ, đồng bộ khi có mạng |
| Tải tăng dần | Bản đồ tải theo vùng, không tải cả thế giới |
| Ảnh | SVG là chính (nhẹ, phóng to không vỡ); raster chỉ khi cần |
| Không tự động phát | Không video, không âm thanh tự chạy |
| Chế độ tiết kiệm dữ liệu | Công tắc: chỉ văn bản, không 3D, không video |
| Thử lại | Lùi dần theo cấp số nhân, không mất dữ liệu |

Kiểm thử bắt buộc: **hoàn thành một lần thử với mạng bị ngắt giữa chừng 10 phút**.

---

## Chi phí dữ liệu

Với nhiều người học, dung lượng di động là chi phí thật. Hiện rõ:

```
Vùng này: 118 KB  ·  Ngưỡng này: 34 KB  ·  Phiên bảo vệ: ~40 MB (20 phút)
```

Và cho phép: tải trước một vùng khi có wifi, rồi làm ngoại tuyến.
