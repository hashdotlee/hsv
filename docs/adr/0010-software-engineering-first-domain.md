# ADR-0010 · Nghề mở màn: kỹ sư phần mềm / AI

**Trạng thái:** Accepted
**Liên quan:** [`01-vision.md`](../01-vision.md), [`11-skill-genome.md`](../11-skill-genome.md)

## Bối cảnh

Ý tưởng gốc hướng tới bảo tồn kỹ năng của các thế hệ trước, và phương án mặc định ban đầu là một nghề công nghiệp thiếu nhân lực cộng một làng nghề truyền thống. Người sáng lập chọn khác: **nghề kỹ sư phần mềm / AI**.

## Quyết định

Nghề mở màn là kỹ sư phần mềm / AI, với sáu trục năng lực:

```
NEN     nền tảng — máy tính, hệ thống, thuật toán
CHAN    chẩn đoán — gỡ lỗi, điều tra, tìm nguyên nhân gốc
DUNG    dựng — thiết kế, đánh đổi, giao hàng, vận hành
THAM    thẩm — đánh giá đầu ra của người và máy, phát hiện ảo giác
NGUOI   người — giao tiếp, khai thác yêu cầu, bất đồng, ước lượng
TRUYEN  truyền — dạy và dẫn người đi sau
```

**Tiếng Anh kỹ thuật** là **lớp nền cắt ngang**, không phải trục thứ bảy — nó nâng đỡ cả sáu trục.

## Vì sao lựa chọn này mạnh hơn

Nó biến sản phẩm thành câu trả lời trực tiếp cho nỗi lo khởi nguồn: **bảo tồn năng lực tư duy kỹ thuật ngay tại nghề mà AI đang thay thế nhanh nhất.** Một nền tảng dạy nghề gốm mà lo về AI thì lời lo đó ở ngoài sản phẩm; ở đây nó nằm trong lõi.

Ba lợi thế thực tế:
- Người sáng lập là chuyên gia trong nghề này ⇒ tự viết được vết chuyên gia đầu tiên và tự làm gác cổng đầu tiên
- Bằng chứng số hoá sẵn: vết quá trình, sandbox, kiểm thử tự động — không cần thiết bị vật lý
- Người học có sẵn thiết bị và kết nối

## Bài toán khó nó tạo ra

**Khi máy làm được bài tập thì đo cái gì mới là năng lực thật?**

Đây là câu hỏi trung tâm của tài liệu, không né. Trả lời bằng:

- **Trục THẨM** — phát hiện khi máy nói sai. Cơ chế **AI đối kháng** cố tình trả lời sai có kiểm soát. Chưa nền tảng học lập trình nào làm điều này
- **Ba chế độ kiểm tra:** cấm AI · cho dùng AI · AI đối kháng
- **Đo quá trình, không đo sản phẩm cuối** — vết + phiên bảo vệ
- **INV-02:** ngưỡng nào mô hình mạnh nhất tự giải được > 25% thì chết

## Phương án đã cân nhắc

| Phương án | Vì sao không (bây giờ) |
|---|---|
| Nghề công nghiệp thiếu nhân lực (điện, cơ khí) | Bằng chứng cần hiện diện vật lý; người sáng lập không phải chuyên gia; G0 không tự đi được |
| Làng nghề truyền thống (gốm Bát Tràng) | Đúng tinh thần bảo tồn nhất, nhưng cần nghệ nhân đồng hành và quan sát tại chỗ ngay từ đầu |
| Nhiều nghề cùng lúc | Chưa chứng minh được một nghề thì mở rộng chỉ nhân lên cái sai |

Hai phương án đầu **không bị loại vĩnh viễn** — chúng là ứng viên cho nghề thứ hai sau G4, khi cơ chế đã được chứng minh. Kiến trúc genome không có gì riêng cho phần mềm.

## Hệ quả

**Được:** người sáng lập tự đi G0 được ngay · bằng chứng hoàn toàn số hoá · trùng khít với nỗi lo khởi nguồn · thu hút được người đóng góp kỹ thuật.

**Mất:** tạm rời xa ý tưởng "kỹ năng ông cha để lại" theo nghĩa đen · cạnh tranh trong một thị trường đông đúc (nhưng không ai đang đo trục THẨM) · rủi ro R2 lớn nhất ở đúng nghề này — mô hình mạnh lên sẽ giết ngưỡng cửa nhanh nhất ở đây.

## Khi nào nên xem lại

Sau G4. Nghề thứ hai nên chọn để **kiểm chứng tính tổng quát của genome** — lý tưởng là một nghề có bằng chứng vật lý, để buộc mô hình bằng chứng phải rộng ra.
