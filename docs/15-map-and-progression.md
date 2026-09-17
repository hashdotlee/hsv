# 15 · Bản đồ &amp; Lộ trình

> Bản đồ không phải trang trí. Nó là **giao diện chính** để một người hiểu mình đang ở đâu — thứ mà không hệ thống học tập nào hiện nay làm được.

## Địa hình sinh từ dữ liệu

```
height(x,y)    = độ sâu thành thạo cần có      # f(số giờ p50, mức trừu tượng)
roughness(x,y) = phương sai thời gian giữa những người học
                 # gồ ghề = người khác nhau mất thời gian rất khác nhau
traffic(x,y)   = số người đã đi qua            # thành lối mòn
fog(x,y)       = 1 - độ tin cậy dữ liệu        # BẤT ĐỊNH, không phải phần thưởng
biome(x,y)     = trục năng lực chủ đạo
water(x,y)     = vùng bị AI thay thế cao → đang "ngập", thu hẹp dần theo thời gian
```

**INV-13: `terrain = f(genome, telemetry, seed)` — hàm thuần.** Không vẽ tay, không ý kiến của mô hình, chạy lại được. Cùng dữ liệu cho cùng địa hình.

### Vì sao điều này quan trọng

Địa hình vẽ tay sẽ mang định kiến của người vẽ về cái gì khó. Địa hình sinh từ dữ liệu **nói sự thật**: nếu 40 người mất trung bình 3 tuần ở một chỗ, chỗ đó là một ngọn núi — dù bạn nghĩ nó dễ.

### Hai loại sương mù

| Loại | Nghĩa | Hiển thị |
|---|---|---|
| **Sương của người học** | `σ` của bạn cao ở vùng này — hệ thống chưa biết bạn ở đâu | Sương xám, tan khi bạn đi |
| **Sương của thế giới** | Vùng chưa đủ dữ liệu — hệ thống chưa biết vùng này | Sương xanh lạnh + nhãn "vùng mới, do máy dựng, chưa ai kiểm chứng" |

Người học phải phân biệt được hai loại. Trung thực về độ tin cậy là điều kiện để dám cho agent tự do sinh ở vùng biên.

### Ngã ba nơi các thế hệ bất đồng

Khi dữ liệu từ nhiều nguồn mâu thuẫn về con đường tốt nhất tới một atom, địa hình sinh ra **ngã ba** thay vì chọn bừa một bên. Người học chọn, và dữ liệu về sau cho biết phái nào đúng. Đây là cách hệ thống *học* mà không cần ai phán xử.

---

## Ba lớp bản đồ

| Lớp | Nội dung | Công nghệ |
|---|---|---|
| **Vĩ mô** | Mặt trống đồng: tâm = đỉnh tối cao, vòng đồng tâm = cấp độ, cánh sao = sáu trục, chim Lạc = người đang đi | SVG, cực nhẹ |
| **Vùng** | Địa hình một lĩnh vực: đồi núi, lối mòn, sương mù, ngưỡng cửa | SVG (mặc định) hoặc 3D |
| **Cục bộ** | Một ngưỡng cửa và lân cận: tiên quyết, gác cổng, người đang làm | SVG |

**Bản đồ 2D SVG là mặc định, không phải bản dự phòng suy biến** (Nguyên tắc 10). 3D là tuỳ chọn cho máy khoẻ, bật bằng một công tắc. Mọi thông tin phải đọc được ở chế độ 2D.

---

## Định vị người học trên bản đồ

```
vị trí hiển thị = chiếu (θ theo 6 trục) xuống 2D
bán kính ánh sáng = f(1/σ)       # biết rõ thì sáng rõ, không biết thì mờ
```

Một người không phải một điểm — họ là một **vùng sáng có hình dạng**. Người mạnh một trục và yếu trục khác có vùng sáng méo; người cân bằng có vùng tròn. Hình dạng đó tự nó đã là chân dung năng lực.

---

## Chọn ngưỡng tiếp theo

Hệ thống **đề xuất**, người học **chọn**. Không bao giờ ép một con đường.

```
điểm ưu tiên(ngưỡng) =
    khớp_ý_định        × 0.30    # có đưa họ tới nơi họ muốn không
  + giảm_bất_định      × 0.20    # có làm sương tan không
  + tấn_công_chữ_ký_lỗi× 0.20    # có nhắm đúng lỗi họ hay mắc không
  + vừa_tầm            × 0.20    # vùng phát triển gần: khó vừa đủ
  + đa_dạng_trục       × 0.10    # có kéo họ về phía trục đang yếu không
  − quá_tải_gác_cổng
  − mới_thất_bại_gần_đây ở vùng này
```

**Luôn hiện ít nhất 3 lựa chọn, thuộc 3 hướng khác nhau:**

| Hướng | Nghĩa |
|---|---|
| **Lên** | Sâu hơn trong trục mạnh nhất |
| **Ngang** | Trục khác, cùng độ khó — mở rộng |
| **Xuống** | Vá nền. Thường là thứ họ *cần* nhất và *muốn* ít nhất |

Và luôn có lựa chọn thứ tư: **"cho tôi xem cái gì đó bất ngờ"** — một ngưỡng ngẫu nhiên có trọng số, ở vùng biên. Đây là nguồn khám phá, và là cách dữ liệu đến với những vùng chưa ai đi.

---

## Vùng phát triển gần

Độ khó mục tiêu: xác suất thành công ước lượng **0.55–0.75**.

- Trên 0.85 → quá dễ, không học được gì
- Dưới 0.40 → thất bại nản, và bằng chứng thu được ít giá trị

Dễ dàng hơn với người mới (0.7–0.8) và khó dần khi người học đã quen với việc thất bại có phẩm giá.

---

## Đỉnh tối cao

Một đích chung cho mọi con đường. Điều kiện:

1. Mức thành thạo tối thiểu trên **cả sáu trục** (không được bỏ trục nào)
2. Ít nhất một ngưỡng `REAL_STAKES` đã qua
3. **Đã đưa ít nhất 3 người qua một ngưỡng** (trục TRUYỀN — Điều lệ §17)
4. Được hội đồng những người đã lên đỉnh chấp nhận

Người lên đỉnh trở thành gác cổng của chính đỉnh đó. Đây là cơ chế tái tạo cuối cùng.

**Không có gì sau đỉnh.** Không "đỉnh 2", không mùa giải, không cấp huyền thoại. Sau đỉnh là dạy người khác — và nếu hệ thống làm đúng, đó là điều người ta muốn làm.

---

## Lối mòn và dấu chân người đi trước

Mỗi lần ai đó qua một ngưỡng, họ để lại:

- **Vết chân** — đường họ đã đi trên bản đồ (ẩn danh, chỉ thấy khi có đủ nhiều người)
- **Phản tư** — đoạn họ viết sau khi qua: chỗ nào suýt sai, cái gì đã cứu họ
- **Chữ ký lỗi** — dùng để ghép với người sau có kiểu sai tương tự

Người học sau thấy lối mòn và thấy phản tư của **những người có chữ ký lỗi giống mình nhất** — không phải của người giỏi nhất. Người giỏi nhất thường không hiểu vì sao bạn bí.

> Đây là hiện thân kỹ thuật của "dữ liệu được truyền qua thế hệ sau, phụ thuộc vào độ tương đồng".

---

## Ngủ đông và quay lại

```
vắng mặt → KHÔNG phạt, KHÔNG khoá
         → mastery suy giảm theo decay_rate thật
         → σ tăng dần → sương mù dày lại ở vùng lâu không đi
         → quay lại bất cứ lúc nào
         → nếu vắng > 90 ngày: một ngưỡng hiệu chỉnh ngắn để định vị lại
         → ghi danh cũ KHÔNG BAO GIỜ bị thu hồi
```

Bản đồ khi quay lại phải nói: *"Chào mừng trở lại. Vùng này đã mờ đi một chút — nhưng đường bạn từng đi vẫn còn."*

---

## Kết xuất

| | 2D SVG | 3D |
|---|---|---|
| Mặc định | ✅ | ❌ (tuỳ chọn) |
| Kích thước tải | < 200KB một vùng | 2–8MB |
| Máy yếu | ✅ | ❌ |
| Đọc màn hình | ✅ (có mô tả văn bản đầy đủ) | ⚠️ luôn kèm bản 2D |
| In ra giấy | ✅ (chủ đề Field) | ❌ |
| Offline | ✅ | ⚠️ cần tải trước |

**Bắt buộc:** mọi thông tin trên bản đồ 3D phải có mặt ở bản 2D và ở bản mô tả văn bản. Bản đồ không bao giờ là cách duy nhất để lấy một thông tin.
