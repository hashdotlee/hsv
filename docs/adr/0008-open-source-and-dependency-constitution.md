# ADR-0008 · Mã nguồn mở + hiến pháp phụ thuộc ba tầng

**Trạng thái:** Accepted
**Liên quan:** [`DEPENDENCY-CONSTITUTION.md`](../../DEPENDENCY-CONSTITUTION.md), [`50-contributing.md`](../50-contributing.md)

## Bối cảnh

Hai yêu cầu của người sáng lập:
1. Mã nguồn mở theo mặc định để cộng đồng đóng góp
2. Hệ thống phải **độc lập, không xây dựng nhiều trên phần mềm thứ ba**

Yêu cầu thứ hai hiểu tuyệt đối sẽ giết dự án: tự viết engine 3D, trình biên dịch, cơ sở dữ liệu là nhiều năm. Chế độ thất bại tự nhiên nhất của một người sáng lập kỹ thuật là **18 tháng xây hạ tầng mà không có người học nào**.

## Quyết định

### Mã nguồn mở

Apache-2.0 cho mã, CC BY-SA 4.0 cho nội dung. Mở **toàn bộ**, kể cả path engine và culture pack — không giữ lại phần nào.

Lý do: giá trị của dự án nằm ở **cộng đồng, kho vết chuyên gia, và tính chính danh của chứng nhận**, không nằm ở mã. Mã bị sao chép không làm ai mất gì; một hệ thống chứng nhận không ai tin thì mã không cứu được.

### Ba tầng phụ thuộc

| Tầng | Nội dung | Luật |
|---|---|---|
| **0 · Tự sở hữu** | skill genome · rubric · trace · rite · ledger · cartograph · culture pack · design token · hiến pháp · trọng tài | **Không phụ thuộc thời gian chạy nào.** Tự viết, không ngoại lệ |
| **1 · Ngoài, qua adapter** | render 3D · CSDL · sandbox · mô hình AI · cổng thanh toán · vận chuyển MCP/HTTP | Phải qua adapter của mình. Thêm mới cần ADR riêng + **kế hoạch thoát** |
| **2 · Tiện ích thay thế được** | build tool · test runner · lint | Dùng thoải mái, thay lúc nào cũng được |

Tầng 0 là **nơi bản sắc sản phẩm cư trú**. Nếu phần đó do người khác viết, sản phẩm không phải của bạn.

## Bốn luật

```
L1  Không SaaS trên đường tới hạn — chạy được trên một máy chủ,
    không internet ra ngoài, trừ cổng thanh toán
L2  Trung lập mô hình — mọi lời gọi AI qua @hsv/oracle (ADR-0005)
L3  Dữ liệu là của người học — xuất ra định dạng mở bất cứ lúc nào
L4  Chi phí thoát phải biết TRƯỚC — không thêm phụ thuộc tầng 1
    nếu chưa biết cách thay và mất bao lâu
```

## Phương án đã cân nhắc

| Phương án | Vì sao không |
|---|---|
| Tự viết mọi thứ | 18 tháng hạ tầng, không người học. Rủi ro R8 |
| Dùng thoải mái mọi thứ | Mất bản sắc; khoá vào SaaS; vi phạm yêu cầu độc lập |
| Lõi mở, culture pack và path engine đóng | Giữ lại đúng phần mà cộng đồng cần sửa nhất; và mâu thuẫn với việc mời cộng đồng khác làm gói văn hoá của họ |

## Hệ quả

**Được:** đóng góp từ ngoài · người khác tiếp quản được nếu người sáng lập dừng · tự host được hoàn toàn · minh bạch làm nên tính chính danh · không bị khoá nhà cung cấp trên đường tới hạn.

**Mất:** đối thủ sao chép được · phải bảo trì API công khai · review PR tốn thời gian · tầng 0 tốn công tự viết · adapter là mã thêm không tạo tính năng mới.

## Khi nào nên xem lại

Xem lại **ranh giới tầng 0/1** cho từng thành phần cụ thể qua ADR riêng. Không xem lại mô hình ba tầng và bốn luật.
