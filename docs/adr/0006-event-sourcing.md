# ADR-0006 · Event sourcing cho trạng thái thế giới

**Trạng thái:** Accepted
**Liên quan:** [`22-world-kernel.md`](../22-world-kernel.md), [`26-data-and-storage.md`](../26-data-and-storage.md)

## Bối cảnh

Hệ thống cấp chứng nhận năng lực và trả tiền thật dựa trên chứng nhận đó. Nội dung phần lớn do máy sinh. Người học, nhà tài trợ, và về sau là nhà tuyển dụng cần tin vào những chứng nhận này.

Ba câu hỏi phải trả lời được bất cứ lúc nào:
- Vì sao ngưỡng cửa này tồn tại, ai tạo ra, mô hình nào?
- Vì sao người này đạt?
- Vì sao khoản tiền này được trả?

Với trạng thái sửa tại chỗ, không câu nào trả lời được sau sáu tháng.

## Quyết định

Trạng thái thế giới là **nhật ký sự kiện chỉ ghi thêm, nối bằng chuỗi băm**. Trạng thái hiện tại là bản chiếu suy ra được, xoá và dựng lại được.

```
world_events: seq · at · kind · actor_kind · actor_id · actor_model
              · payload · prev_hash · hash
-- không UPDATE, không DELETE, cưỡng chế bằng quyền của CSDL
```

`actor_kind ∈ {human, agent}` là trường bắt buộc. `VerdictRecorded` bị từ chối nếu `actor_kind != 'human'`.

## Phương án đã cân nhắc

| Phương án | Vì sao không |
|---|---|
| CRUD + bảng audit | Bảng audit luôn trôi lệch khỏi sự thật, và lệch đúng lúc cần nhất |
| CRUD + lịch sử phiên bản từng bảng | Ghi được "cái gì đổi", không ghi được "vì sao" và "theo yêu cầu của ai" |
| Chỉ dùng git cho mọi thứ | Tốt cho nội dung, không hợp cho lần thử và giao dịch tiền |

Kết quả là mô hình lai: **nội dung trong git** (cần review và diff), **sự kiện trong nhật ký** (cần bất biến và kiểm toán).

## Hệ quả

**Được:** kiểm toán đầy đủ tới tận mô hình và phiên bản prompt đã sinh ra một ngưỡng cửa · phán quyết và chi trả truy ngược được · sửa lịch sử là phá chuỗi băm và việc đó phát hiện được · phát lại lịch sử để thử nghiệm ("nếu hạn ngạch khác thì thế giới sẽ ra sao") · bản chiếu mới thêm bất cứ lúc nào từ lịch sử cũ.

**Mất:** phức tạp hơn CRUD đáng kể · truy vấn phải qua bản chiếu · nhật ký lớn dần · **xoá dữ liệu cá nhân khó** — và đây là xung đột thật với quyền được xoá.

## Giải quyết xung đột với quyền được xoá

Dữ liệu cá nhân **không nằm trong payload sự kiện**. Payload chứa con trỏ tới kho riêng có khoá riêng. Xoá kho riêng ⇒ dữ liệu biến mất thật, chuỗi băm vẫn nguyên vẹn, kiểm toán vẫn hoạt động.

## Kiểm thử bắt buộc

CI dựng lại toàn bộ bản chiếu từ sự kiện và so với bản đang chạy. **Lệch một trường là lỗi nghiêm trọng.** Diễn tập khôi phục hàng tháng (RB-10).

## Khi nào nên xem lại

Nếu nhật ký vượt quá khả năng phát lại trong thời gian chấp nhận được — lúc đó dùng ảnh chụp định kỳ + phát lại từ ảnh chụp, không bỏ event sourcing.
