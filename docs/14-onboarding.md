# 14 · Định vị người học

> Bài toán: *không thể bắt một người trưởng thành học lại tiếng Việt như đứa trẻ.* Nhưng cũng không thể bắt họ làm bài kiểm tra 2 tiếng trước khi được vào.

## Nguyên tắc nền

**Định vị ban đầu không cần chính xác. Nó chỉ cần đủ đúng để không xúc phạm, và đủ khiêm tốn để tự sửa.**

Vị trí thật hình thành trong 2–3 tuần đầu từ hành vi thật. Đây là khác biệt căn bản so với bài kiểm tra xếp lớp truyền thống: chúng ta không cố đo đúng ngay, chúng ta cố **giảm bất định nhanh**.

---

## Bốn nguồn tín hiệu

| Nguồn | Thu thế nào | Độ tin | Chi phí với người dùng |
|---|---|---|---|
| **Tự khai báo** | "Tôi cần gì" + tự đánh giá từng mảng + số năm kinh nghiệm | Thấp, nhưng **đắt giá về ý định** | 2 phút |
| **Bằng chứng có sẵn** | GitHub, repo, CV, sản phẩm đã làm — tuỳ chọn | Cao nếu có | 0 phút |
| **Thăm dò thích ứng** | 12–20 mục, mỗi mục chọn theo kết quả mục trước | Trung bình–cao | 10–15 phút |
| **Ngưỡng hiệu chỉnh** | Một thử thách thật, ngắn (45–90 phút), nhắm đúng vùng nghi ngờ | **Cao nhất** | tuần đầu |

Ý định (người ta *muốn* gì) quan trọng ngang năng lực. Một người `DUNG=0.7` muốn chuyển sang bảo mật thì nên bắt đầu ở chỗ khác hoàn toàn so với người cùng chỉ số muốn lên kiến trúc sư.

---

## Luồng onboarding

```
1. Ý ĐỊNH        "Bạn muốn làm được gì mà hiện chưa làm được?"
                 → văn bản tự do, phân loại về vùng bản đồ
                 → và "Vì sao bây giờ?" — ràng buộc thời gian, động lực

2. TỰ KHAI       6 thanh trượt (sáu trục) + lớp nền + số năm + hoàn cảnh
                 (giờ rảnh, thiết bị, mạng, tuổi → chế độ vị thành niên)

3. BẰNG CHỨNG    (tuỳ chọn, bỏ qua được) liên kết GitHub / tải CV / dán link
                 → trích tự động, luôn hiện cho người dùng xác nhận

4. THĂM DÒ       12–20 mục thích ứng. Bỏ qua bất cứ lúc nào.
                 → θ và σ theo từng trục

5. ĐẶT LÊN BẢN ĐỒ  Hiện vị trí + sương mù theo σ
                 → "Bản đồ đang còn học về bạn. Hai tuần nữa nó sẽ rõ hơn nhiều."

6. NGƯỠNG ĐẦU    Một ngưỡng hiệu chỉnh ngắn, nhắm vào trục có σ lớn nhất
                 mà người học quan tâm nhất
```

Toàn bộ bỏ qua được trừ bước 1 và bước 6. Người bỏ qua hết thì bắt đầu với σ rất lớn và bản đồ gần như toàn sương mù — **và điều đó hoàn toàn ổn**, nó sẽ tan trong hai tuần.

---

## Thăm dò thích ứng

Nguyên lý mượn từ *computerized adaptive testing* (nền tảng của các kỳ thi máy tính hoá và các hệ xếp lớp ngôn ngữ hiện đại): mỗi mục được chọn để **giảm bất định nhiều nhất**, không phải để đi tuần tự từ dễ tới khó.

```
khởi tạo θ từ tự khai + bằng chứng      # KHÔNG bắt đầu từ 0 — đó là sự xúc phạm
lặp:
  chọn mục có thông tin kỳ vọng cao nhất quanh θ hiện tại
  ghi nhận kết quả (đúng/sai/bỏ qua/thời gian trả lời)
  cập nhật θ và σ
  dừng khi: σ < ngưỡng  HOẶC  đã 20 mục  HOẶC  người dùng bấm "đủ rồi"
xuất: θ và σ theo từng trục và từng lớp nền
```

### Ba điều khiến nó không giống bài thi

1. **Bỏ qua được bất cứ lúc nào** — và bỏ qua cũng là tín hiệu (bỏ qua nhanh ≠ bỏ qua sau 2 phút suy nghĩ).
2. **Xen kẽ câu hỏi phán đoán**, không chỉ câu hỏi kiến thức:
   - "Đoạn code này sai ở đâu?"
   - "Bạn sẽ hỏi gì thêm trước khi bắt đầu làm việc này?"
   - "Hai cách này, bạn chọn cách nào và đánh đổi là gì?"
   - "Câu trả lời này của AI, bạn tin bao nhiêu phần? Vì sao?"
3. **Luôn hiện tiến trình và lý do**: "còn 4 câu nữa là đủ để định vị mảng đồng thời".

### Hiệu chỉnh mục hỏi

Mỗi mục cần tham số độ khó `b` và độ phân biệt `a`. Hai cách lấy:

| Giai đoạn | Cách |
|---|---|
| G1–G2 | Chuyên gia ước lượng bằng tay. Thô nhưng đủ dùng. |
| G3+ | Hiệu chỉnh từ dữ liệu thật — **cần ~200 người trả lời mỗi mục**. Làm sớm hơn là làm bừa. |

Mục nào có độ phân biệt thấp (`a < 0.3`) thì loại khỏi kho.

---

## Cấu trúc vị trí

```ts
interface Position {
  axes: Record<Axis, { theta: number; sigma: number; lastEvidenceAt: number }>;
  foundations: Record<string, { theta: number; sigma: number }>;
  intent: { goal: string; targetRegion: RegionId; why: string };
  constraints: {
    hoursPerWeek: number;
    device: 'phone' | 'low_laptop' | 'laptop';
    network: 'stable' | 'intermittent' | 'metered';
    isMinor: boolean;
  };
  calibrating: boolean;          // true trong 2 tuần đầu
  updatedAt: number;
}
```

Ví dụ thật:

```yaml
axes:
  NEN:    { theta: 0.62, sigma: 0.31 }
  CHAN:   { theta: 0.41, sigma: 0.44 }
  DUNG:   { theta: 0.70, sigma: 0.28 }
  THAM:   { theta: 0.22, sigma: 0.52 }    # σ lớn ⇒ sương mù dày ở vùng này
  NGUOI:  { theta: 0.55, sigma: 0.40 }
  TRUYEN: { theta: 0.10, sigma: 0.60 }
foundations:
  tieng_anh_ky_thuat_doc:  { theta: 0.55, sigma: 0.25 }
  tieng_anh_ky_thuat_viet: { theta: 0.20, sigma: 0.35 }
intent:
  goal: "đọc hiểu tài liệu kỹ thuật tiếng Anh không cần dịch"
constraints: { hoursPerWeek: 6, device: low_laptop, network: intermittent, isMinor: false }
```

**Bán kính bất định `σ` được vẽ thẳng lên bản đồ thành sương mù.** Hệ thống thú nhận nó chưa biết rõ bạn ở đâu, thay vì giả vờ chính xác. Và **sương tan dần chính là phần thưởng đầu tiên** người học cảm nhận được — mạnh hơn mọi huy hiệu.

---

## Ví dụ đầy đủ: "cần học tiếng Anh cho lập trình"

**Tự khai:** mục tiêu = đọc tài liệu kỹ thuật; tự đánh giá tiếng Anh 3/10; lập trình 6/10.

**Thăm dò (14 mục, 11 phút):**
- Cho đọc một đoạn tài liệu API thật → hỏi điều **suy ra được** từ nó, không hỏi từ vựng
- Cho một issue GitHub tiếng Anh thật → hỏi người báo lỗi thực sự muốn gì
- Cho một đoạn changelog → hỏi phiên bản nào phá vỡ tương thích
- Tăng dần mật độ thuật ngữ và độ phức tạp ngữ pháp

**Kết quả:** đọc hiểu kỹ thuật **0.55** — cao hơn tự đánh giá rất nhiều (người Việt làm kỹ thuật thường đánh giá thấp bản thân ở mảng này). Nhưng *viết* 0.20, *nghe* 0.15.

**Định vị:** không phải "học tiếng Anh từ đầu" — mà đặt ở rìa vùng **Giao tiếp kỹ thuật**.

**Ngưỡng đầu tiên:** *viết một issue tiếng Anh cho một dự án mã nguồn mở thật và được người bảo trì trả lời.*

> Vừa là bài kiểm tra, vừa là việc thật, vừa có kết quả ngoài đời, vừa tạo ra một đóng góp tồn tại vĩnh viễn trên internet mang tên người học. **Đây là hình dạng đúng của một thử thách trong hệ thống này.**

---

## Ba luật của định vị

**Luật 1 — Không bao giờ đặt người thấp hơn mức họ tự khai quá một bậc nếu không có bằng chứng trực tiếp.**
Nhục mạ ở phút thứ 10 là mất người vĩnh viễn. Khi nghi ngờ, đặt cao hơn và để ngưỡng đầu tiên nói sự thật — thất bại ở một thử thách thật có phẩm giá hơn nhiều so với bị một bài trắc nghiệm xếp thấp.

**Luật 2 — Hai tuần đầu là thời kỳ hiệu chỉnh.**
Hệ số cập nhật cao, rồi giảm dần. Nói rõ điều này với người học: *"bản đồ đang còn học về bạn"*. Trong thời kỳ này, mọi phán quyết ảnh hưởng vị trí mạnh, và không có hậu quả tiền bạc.

**Luật 3 — Định vị lại được bất cứ lúc nào**, và **bắt buộc** định vị lại sau một kỳ ngủ đông dài (&gt; 90 ngày).

---

## Chống các kiểu hỏng thường gặp

| Kiểu hỏng | Chặn bằng |
|---|---|
| Người khiêm tốn bị đặt quá thấp | Luật 1 + trọng số cho bằng chứng có sẵn |
| Người tự tin thái quá bị đặt quá cao rồi thất bại liên tiếp | Ngưỡng đầu tiên luôn ngắn và có thể "chưa đạt" mà không mất gì |
| Bỏ giữa chừng onboarding | Mọi bước bỏ qua được; chỉ ý định là bắt buộc |
| Thăm dò cảm giác như bị phán xét | Dùng ngôn ngữ "để bản đồ hiểu bạn", hiện tiến trình, không bao giờ hiện "sai rồi" |
| Cùng một ngưỡng đầu cho mọi người | Ngưỡng đầu chọn theo σ lớn nhất **giao với** ý định |
| Vị thành niên vào nhầm luồng | Hỏi tuổi ở bước 2, chuyển chế độ ngay lập tức |

---

## Định vị lại và sương mù

```
σ tăng khi: thời gian trôi không có bằng chứng · ngủ đông · đổi ý định · genome đổi quanh vùng đó
σ giảm khi: có phán quyết mới · có bằng chứng độc lập · gác cổng khác xác nhận
```

Sương mù trên bản đồ = `f(σ)`. Vùng chưa ai đi qua cũng có sương mù riêng (bất định của *thế giới*, không phải của người học) — hai loại sương này hiển thị khác nhau và người học phải phân biệt được.
