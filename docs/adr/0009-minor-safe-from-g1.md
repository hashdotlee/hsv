# ADR-0009 · Chế độ vị thành niên đầy đủ từ G1

**Trạng thái:** Accepted
**Liên quan:** [`29-minors.md`](../29-minors.md), [`40-roadmap.md`](../40-roadmap.md)

## Bối cảnh

Người dùng mục tiêu bắt đầu từ **14 tuổi**. Hệ thống đồng thời có: **tiền thật**, **thử thách có thể yêu cầu gặp người ngoài đời**, **phòng kín để người lạ trao đổi**, và **phiên bảo vệ trực tiếp với người lớn**.

Bốn thứ đó cộng lại tạo ra rủi ro nghiêm trọng nhất của toàn dự án (R3). Cách làm thông thường — ra mắt trước, vá an toàn sau — ở đây là không chấp nhận được.

## Quyết định

Chế độ vị thành niên **đầy đủ, hoạt động, đã kiểm thử** từ G1 — trước khi có người dùng ngoài đầu tiên.

```
tuổi thu thập SỚM trong onboarding, trước khi thấy nội dung
< 18       → chế độ vị thành niên
14–15      → người giám hộ xác minh & tham gia TRƯỚC ngưỡng đầu tiên
16–17      → báo người giám hộ; cần đồng thuận cho ngưỡng có tiền
tiền       → quỹ uỷ thác do người giám hộ quản lý, không vào ví trực tiếp
gặp ngoài đời → KHÔNG hiển thị, ở bất kỳ mức tuổi nào dưới 18
bảo vệ     → luôn ghi hình + có người thứ ba
nhắn riêng người lớn ↔ vị thành niên → CẤM, không có ngoại lệ
ghi danh công khai → mặc định dùng bút danh
```

Cưỡng chế ở tầng dữ liệu, không ở tầng giao diện:

```ts
function visibleChallenges(learner: Learner, all: Challenge[]): Challenge[] {
  if (!learner.isMinor) return all;
  return all.filter(c =>
    c.minorSafe && !c.requiresIrlMeeting &&
    c.contentRating <= 'teen' && (c.timeLimit ?? 0) <= 90 * 60
  );
}
```

Kèm INV-09 trong hiến pháp: `requires_irl_meeting == true ⇒ minor_safe == false`.

## Phương án đã cân nhắc

| Phương án | Vì sao không |
|---|---|
| Giới hạn 18+ lúc đầu, mở cho vị thành niên sau | Nhóm 14–17 tuổi là nhóm **hưởng lợi nhiều nhất** — chưa bị hệ thống bằng cấp định đoạt. Bỏ họ là bỏ lý do tồn tại |
| Ra mắt trước, thêm bảo vệ sau | Chế độ vị thành niên quyết định **kiến trúc quyền hạn và luồng tiền** — không vá sau được |
| Chỉ cần điều khoản sử dụng ghi 18+ | Không ai đọc, và không bảo vệ được ai |

## Hệ quả

**Được:** phục vụ đúng nhóm cần nhất · kiến trúc quyền hạn và luồng tiền đúng từ đầu · tuân thủ pháp lý từ đầu · phụ huynh tin tưởng.

**Mất:** G1 tốn công hơn đáng kể · một số ngưỡng cửa không dùng được cho vị thành niên · luồng tiền phức tạp hơn (quỹ uỷ thác) · phiên bảo vệ cần người thứ ba, tốn thêm nguồn lực khan hiếm.

**Cái giá này được chấp nhận có chủ ý.** Một sự cố an toàn với người vị thành niên kết thúc dự án — và đúng là nên như vậy.

## Tiêu chí G1

Trong tiêu chí hoàn thành G1: **một người vị thành niên đi trọn luồng an toàn**, và **không sự cố an toàn nào**. Không đạt thì không qua G1.

## Khi nào nên xem lại

Chỉ theo hướng **siết chặt hơn**. Nới lỏng bất kỳ điều nào ở đây phải qua quy trình sửa điều lệ, không qua ADR.
