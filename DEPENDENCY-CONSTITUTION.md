# HIẾN PHÁP PHỤ THUỘC

> Mục tiêu: hệ thống **độc lập về bản chất**, không độc lập một cách giáo điều.

Tự viết mọi thứ là cách chắc chắn nhất để sau 18 tháng có một hạ tầng đẹp và không có người học nào. Phụ thuộc bừa bãi là cách chắc chắn nhất để một ngày nào đó có người khác quyết định số phận dự án của bạn. Hiến pháp này chia đường giữa hai cái chết đó.

---

## Ba tầng

### Tầng 0 — Tự sở hữu 100%

Mọi thứ định nghĩa **bản sắc và tính chính danh** của hệ thống. Không được import thư viện bên thứ ba ngoài thư viện chuẩn của ngôn ngữ.

| Thuộc tầng 0 | Vì sao |
|---|---|
| Skill genome, đồ thị kỹ năng | Đây *là* sản phẩm |
| Rubric & chấm điểm | Chạm vào tính công bằng; không được phụ thuộc ai |
| Vết bằng chứng (`trace`) | Chứng cứ pháp lý về năng lực của một con người |
| Máy trạng thái nghi thức (`rite`) | Là trải nghiệm cốt lõi |
| Sổ cái tiền (`ledger`) | Tiền của người khác |
| Sinh địa hình (`cartograph`) | Cách chúng ta nhìn năng lực — không ai làm thay được |
| Culture pack & design token | Bản sắc |
| Hiến pháp thế giới & trọng tài | Quyền lực trong hệ thống |

### Tầng 1 — Thuê, nhưng sở hữu giao diện

Được dùng, nhưng **luôn nằm sau một adapter của mình**, và phải có **kế hoạch thoát viết sẵn**.

| Thuê gì | Ví dụ hiện tại | Adapter | Kế hoạch thoát |
|---|---|---|---|
| Kết xuất 3D | `three.js` | `@hsv/cartograph/renderer` | Chế độ bản đồ 2D SVG luôn tồn tại song song và đủ dùng |
| CSDL | Postgres / SQLite | `@hsv/chronicle/store` | Event store là append-only; chuyển sang bất kỳ kho nào |
| Sandbox thi hành | container runtime | `@hsv/crucible/host` | Đổi runtime không đụng rubric |
| Mô hình AI | bất kỳ | `@hsv/oracle` | Bắt buộc chạy được với mô hình mở tự host |
| Thanh toán | cổng nội địa | `@hsv/ledger/rail` | Sổ cái nội bộ là nguồn sự thật; cổng chỉ là ống dẫn |
| Vận chuyển agent | MCP / HTTP | `@hsv/oracle/transport` | Đổi giao thức không đụng logic agent |

**Điều kiện bắt buộc cho mỗi phụ thuộc tầng 1:**
1. Có adapter, lõi không import trực tiếp
2. Có ít nhất một cài đặt thay thế **đã từng chạy thử** (không chỉ trên giấy)
3. Có mục trong bảng trên, ghi rõ kế hoạch thoát
4. Thêm mới phải qua PR riêng + một ADR trong [`docs/adr/`](docs/adr/)

### Tầng 2 — Tự do

Tiện ích thay thế được trong một buổi chiều (định dạng ngày tháng, tiện ích chuỗi…). Không cần adapter. **Nhưng không được chạm vào tiền, danh tính, phán quyết, hay vết bằng chứng.**

---

## Bốn điều luật

**L1 — Không SaaS trên đường tới hạn.**
Hệ thống phải chạy được trên một máy chủ duy nhất, không internet ra ngoài, trừ cổng thanh toán. Nếu một tính năng chỉ hoạt động khi có dịch vụ đám mây của bên thứ ba, tính năng đó là tuỳ chọn, không phải cốt lõi.

**L2 — Trung lập mô hình.**
Mọi lời gọi AI đi qua `@hsv/oracle`. Cấm gọi thẳng SDK của nhà cung cấp ở bất kỳ đâu khác — CI cưỡng chế bằng luật lint. Phải có cấu hình chạy được hoàn toàn bằng mô hình mở tự host, kể cả nếu chất lượng thấp hơn.

**L3 — Dữ liệu là của người học.**
Mọi dữ liệu người học xuất được ra định dạng mở, có tài liệu, đọc được không cần phần mềm của chúng tôi.

**L4 — Chi phí thoát phải biết trước.**
Không được thêm phụ thuộc tầng 1 mà không trả lời được: *nếu ngày mai thứ này biến mất, mất bao lâu để thay?* Nếu câu trả lời &gt; 4 tuần thì nó phải là tầng 0, hoặc không dùng.

---

## Quy trình thêm một phụ thuộc

```
đề xuất (issue) → trả lời 5 câu hỏi dưới → ADR → PR riêng → duyệt
```

1. Vì sao không tự viết? Ước lượng công sức tự viết là bao nhiêu?
2. Thứ này chạm vào tầng 0 nào không?
3. Chạy offline được không?
4. Kế hoạch thoát là gì, mất bao lâu?
5. Giấy phép có tương thích với kế hoạch mở nguồn không?

---

## Danh sách cấm

| Cấm | Vì sao |
|---|---|
| SDK nhà cung cấp AI gọi trực tiếp ngoài `@hsv/oracle` | Vi phạm L2 |
| Thư viện UI có thẩm mỹ riêng (Material, Ant, Chakra) | Chúng mang sẵn một thế giới quan sẽ chống lại design system của bạn |
| Dịch vụ phân tích của bên thứ ba trong ứng dụng người học | Riêng tư; xem [`docs/28-security-safety.md`](docs/28-security-safety.md) |
| Dịch vụ xác thực đóng làm nguồn danh tính duy nhất | Danh tính là tầng 0 |
| Bất kỳ thứ gì yêu cầu cửa hàng ứng dụng để cập nhật nội dung | Cửa hàng ứng dụng cũng là một dạng phụ thuộc bên thứ ba — đây là lý do chọn PWA |

---

## Mâu thuẫn thường gặp và cách xử

> *"Tự viết engine 3D cho độc lập?"* — Không. Kết xuất là tầng 1 và đã có đường thoát (bản đồ 2D). Bản đồ 2D SVG **phải luôn dùng được**, không phải bản dự phòng suy biến.

> *"Tự viết mô hình ngôn ngữ?"* — Không. Nhưng phải chạy được với mô hình mở, và chất lượng hệ thống không được sụp khi dùng mô hình yếu.

> *"Tự viết Style Dictionary?"* — Có. Khoảng 600 dòng, chạm trực tiếp vào bản sắc, và bạn sẽ muốn xuất ra những đích lạ (Three.js, culture pack) mà công cụ có sẵn không lo.

> *"Tự viết cổng thanh toán?"* — Không, không được phép về pháp lý. Nhưng **sổ cái nội bộ là nguồn sự thật**, cổng chỉ là ống dẫn; đổi cổng không được làm mất lịch sử tiền.
