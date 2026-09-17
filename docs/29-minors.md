# 29 · Chế độ vị thành niên

> Người dùng từ **14 tuổi** + **tiền thật** + khả năng **gặp mặt ngoài đời** = đây là ràng buộc kiến trúc, không phải tính năng. **Không vá sau được.**

Phải có đầy đủ từ **G1** — thời điểm người ngoài đầu tiên vào hệ thống.

---

## Kích hoạt

Hỏi tuổi ở bước 2 của onboarding. Dưới 18 ⇒ chuyển chế độ vị thành niên **ngay lập tức**, trước khi thấy bất kỳ nội dung nào.

```ts
interface MinorMode {
  isMinor: true;
  ageBand: '14-15' | '16-17';
  guardian: { verified: boolean; contact: string; consentAt?: number };
  restrictions: MinorRestrictions;   // không tắt được từ phía người học
}
```

Không tự khai lên 18 để thoát chế độ: đổi ngày sinh cần xác minh người lớn. Khi đủ 18, hệ thống hỏi xác nhận và chuyển chế độ.

---

## Sáu ràng buộc cứng

### 1. Người giám hộ

| `14-15` | `16-17` |
|---|---|
| Phải có người giám hộ xác minh trước khi nhập môn ngưỡng đầu tiên | Thông báo cho giám hộ; đồng ý cần cho ngưỡng có tiền |
| Giám hộ nhận báo cáo tiến độ | Giám hộ nhận báo cáo nếu người học đồng ý |
| Giám hộ rút người học ra bất cứ lúc nào | — |

Xác minh giám hộ: liên hệ độc lập (email + gọi lại), không chỉ một ô tick.

### 2. Tiền vào quỹ giám hộ

```
người học vị thành niên đạt ngưỡng
  → tiền vào quỹ đứng tên người học, giám hộ quản
  → dưới hạn mức nhỏ: rút được với chấp thuận của giám hộ
  → trên hạn mức: giữ tới 18 tuổi
  → người học LUÔN thấy số dư của mình
```

Không bao giờ chuyển tiền trực tiếp vào tài khoản của một người dưới 18 tuổi.

### 3. Không gặp mặt người lớn lạ

```
INV-09: challenge.requires_irl_meeting == true ⇒ challenge.minor_safe == false
```

Cưỡng chế ở tầng ghi. Ngưỡng có gặp mặt **không hiển thị** cho người học vị thành niên — không phải hiện rồi chặn.

### 4. Phiên bảo vệ luôn ghi hình, không bao giờ một-một riêng tư

| | |
|---|---|
| Ghi hình | Bắt buộc, không tắt được |
| Người thứ ba | Bắt buộc: một gác cổng thứ hai **hoặc** giám hộ có mặt |
| Lưu | 1 năm |
| Truy cập | Hội đồng an toàn, hội đồng kháng nghị, giám hộ |

Gác cổng làm việc với người vị thành niên phải qua sàng lọc riêng và cam kết quy tắc ứng xử.

### 5. Phòng kín được kiểm duyệt

- Không tin nhắn riêng giữa người vị thành niên và người lớn
- Lọc tự động: thông tin liên hệ, liên kết ra ngoài, nội dung không phù hợp
- Người vị thành niên báo cáo được bằng một nút, luôn hiện
- Phòng kín toàn người vị thành niên vẫn bị kiểm duyệt

### 6. Không cơ chế gây áp lực

| Cấm với người vị thành niên | Vì sao |
|---|---|
| Thông báo đẩy ngoài giờ học | Giấc ngủ |
| Bất kỳ cơ chế chuỗi ngày nào | Đã cấm với mọi người (Điều lệ §1), nhấn mạnh lại ở đây |
| Hiện thành tích của người khác | So sánh xã hội ở tuổi này đặc biệt độc hại |
| Ngưỡng có giới hạn thời gian dài quá 90 phút | — |
| Số tiền lớn làm động lực chính | Bóp méo lý do học |

---

## Nội dung

| | |
|---|---|
| Ngưỡng bảo mật | Chỉ mức phòng thủ; không kỹ thuật tấn công |
| Ngưỡng `REAL_STAKES` | Cần đồng ý của giám hộ theo từng lần |
| Lore | Không bạo lực, không nội dung người lớn |
| AI đối kháng | Cho phép, nhưng **giải thích rõ trước** và lộ toàn bộ sau |

Chế độ AI đối kháng thật ra là kỹ năng an toàn mạng quan trọng cho lứa tuổi này — nhưng phải được đóng khung là *học cách không tin máy*, không phải bị lừa.

---

## Riêng tư

- Thu thập ít hơn nữa: không ảnh thật, không họ tên đầy đủ công khai
- Ghi danh dùng bút danh **theo mặc định**, không hỏi
- Video lưu riêng, khoá riêng, hạn 1 năm
- Đủ 18 tuổi: hệ thống hỏi người học muốn giữ hay xoá dữ liệu thời vị thành niên

---

## Cưỡng chế kỹ thuật

```ts
// Cổng bắt buộc — không có đường vòng
function visibleChallenges(learner: Learner, all: Challenge[]): Challenge[] {
  if (!learner.isMinor) return all;
  return all.filter(c =>
    c.minorSafe &&
    !c.requiresIrlMeeting &&
    c.contentRating <= 'teen' &&
    (c.timeLimit ?? 0) <= 90 * 60
  );
}
```

CI kiểm: không đường dẫn nào trong mã trả về ngưỡng cửa cho người học mà không đi qua cổng này.

### Kiểm thử bắt buộc

- [ ] Tài khoản vị thành niên không bao giờ thấy ngưỡng có `requiresIrlMeeting`
- [ ] Tiền không bao giờ tới thẳng tài khoản vị thành niên
- [ ] Phiên bảo vệ không bắt đầu được nếu chưa bật ghi hình
- [ ] Phiên bảo vệ không bắt đầu được nếu chỉ có hai người
- [ ] Tin nhắn riêng người lớn ↔ vị thành niên bị chặn ở tầng máy chủ
- [ ] Đổi ngày sinh cần xác minh
- [ ] Giám hộ rút người học ra được, và dữ liệu xuất/xoá được

---

## Vì sao làm ngay từ G1 dù nó làm chậm

Ba lý do:

1. **Nó quyết định kiến trúc quyền hạn và luồng tiền.** Vá sau nghĩa là viết lại hai hệ thống lõi.
2. **Rủi ro pháp lý và đạo đức lớn hơn mọi rủi ro kỹ thuật.** Một sự cố với trẻ vị thành niên là kết thúc dự án, không phải một sự cố cần khắc phục.
3. **Người dùng đầu tiên rất có thể là người trẻ.** Nhóm có nhiều thời gian nhất, ít bị ràng buộc bởi hệ thống bằng cấp hiện có nhất, và mất mát nhiều nhất nếu AI thay thế lao động trí óc ở mức đầu vào — chính là nhóm dự án này tồn tại vì họ.
