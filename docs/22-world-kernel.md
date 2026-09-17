# 22 · Lõi Thế Giới

> **Kẻ thù không phải xung đột ghi. Kẻ thù là trôi dạt ngữ nghĩa.**
>
> Khoá ghi là chuyện một buổi chiều. Thứ giết thế giới là 400 thử thách na ná nhau sau ba tháng, mâu thuẫn nhau, không ai buồn đi.

## Vai trò

Lõi Thế Giới là **tiến trình duy nhất được ghi vào trạng thái thế giới**. Mọi thứ khác — agent, giao diện quản trị, công cụ nhập liệu, kể cả người sáng lập — đều đi qua nó dưới dạng đề xuất.

Điều này loại bỏ toàn bộ lớp bài toán "nhiều agent xung đột" ở mức thiết kế thay vì mức khoá.

---

## Đường ống xử lý đề xuất

```
                  ┌──────────────┐
  Proposal ──────►│ 1. CÚ PHÁP   │ schema hợp lệ? trường bắt buộc?
                  └──────┬───────┘
                  ┌──────▼───────┐
                  │ 2. BẤT BIẾN  │ WORLD-CONSTITUTION.yaml — INV-01..14
                  └──────┬───────┘
                  ┌──────▼───────┐
                  │ 3. TRÙNG LẶP │ cosine < 0.88 (INV-06)
                  └──────┬───────┘   → nếu trùng: chuyển thành đề xuất SỬA
                  ┌──────▼───────┐
                  │ 4. HẠN NGẠCH │ tuần này còn quota không? chi phí còn không?
                  └──────┬───────┘
                  ┌──────▼───────┐
                  │ 5. RED TEAM  │ có đường tắt không? AI một mình qua được không?
                  └──────┬───────┘
                  ┌──────▼───────┐
                  │ 6. TRỌNG TÀI │ mâu thuẫn với nội dung đang có? chọn bên nào?
                  └──────┬───────┘
                  ┌──────▼───────┐
                  │ 7. VÙNG      │ frontier: tự phát hành
                  │              │ settled:  chờ gác cổng vùng duyệt
                  │              │ canon:    chỉ ghi nhận làm gợi ý
                  └──────┬───────┘
                  ┌──────▼───────┐
                  │ 8. GHI SỰ KIỆN│ append-only, có nguồn gốc đầy đủ
                  └──────────────┘
```

Mỗi bước từ chối đều trả về **lý do máy đọc được** — agent dùng nó để học, và uy tín agent điều chỉnh theo.

---

## Bốn cơ chế chống trôi dạt

### 1. Chặn trùng lặp ngữ nghĩa (INV-06)

Quan trọng nhất. Mọi nội dung mới được nhúng và so với toàn bộ nội dung đang có. Cosine ≥ 0.88 ⇒ **không tạo mới, chuyển thành đề xuất sửa cái đã có**.

Không có điều luật này, ba tháng sau bạn có 400 biến thể của 30 ý tưởng, và người học không phân biệt được cái nào đáng làm.

### 2. Trọng tài (`Arbiter`)

Một agent **chỉ phán xử, không sinh nội dung**. Tách quyền sinh khỏi quyền duyệt — đúng nguyên lý tách gác cổng khỏi người học.

Việc của Trọng tài:
- Hai ngưỡng dạy cùng một thứ theo hai cách mâu thuẫn → chọn một, hoặc biến thành ngã ba có chủ đích
- Hai atom chồng lấn → gộp hoặc vạch ranh giới rõ
- Một vùng phình quá nhanh so với số người thật đi qua → tạm đóng sinh ở vùng đó

### 3. Khảo hạch (`Red Team`)

Agent **được thưởng khi phá được sản phẩm của agent khác**. Động cơ đối nghịch là cách duy nhất khiến kiểm định không thành hình thức.

Tìm: đường tắt, ngưỡng mà mô hình mạnh tự qua được, rubric mâu thuẫn, tiên quyết sai, nội dung trùng đã lọt lưới.

Phá được ⇒ đề xuất bị loại, uy tín agent sinh giảm, uy tín Red Team tăng.

### 4. Hạn ngạch

Giới hạn cứng trong hiến pháp. Đây là **van chính bạn nắm**: muốn thế giới nở nhanh thì mở van, thấy loãng thì siết. Không phải quyết định kỹ thuật, là quyết định điều hành.

```yaml
new_challenges_per_week: 40
max_live_challenges_per_atom: 6      # chặn phình theo chiều sâu
max_open_proposals: 200              # chặn hàng đợi duyệt vỡ trận
```

---

## Ba vùng trưởng thành

**Quyền tự trị tỉ lệ nghịch với mật độ người đi qua.** Đây là câu trả lời cho "sinh liên tục ở giai đoạn đầu, kiểm soát dần khi hệ thống đủ lớn" — và nó **tự động theo dữ liệu**, không cần bạn quyết từng vùng.

| Vùng | Điều kiện | Agent | Người |
|---|---|---|---|
| **Biên giới** | < 20 lượt | Tự phát hành | Xem tổng hợp hàng tuần |
| **Có người ở** | 20–200 lượt | Chỉ đề xuất | Một gác cổng vùng duyệt |
| **Chính điển** | > 200 lượt, gác cổng nhất trí ≥ 0.8 | Chỉ gợi ý | Hội đồng + phiên bản hoá như sửa luật |

Vùng biên hiển thị sương mù dày kèm nhãn trung thực: *"vùng mới, do máy dựng, chưa ai kiểm chứng"*. Trung thực về độ tin cậy là điều kiện để dám cho agent tự do.

Một vùng **tự nó chuyển** từ biên giới sang chính điển khi người học đi qua nhiều. Bạn không phải quản lý việc siết — hệ thống tự siết ở đúng chỗ người thật đang đi.

---

## Kho sự kiện

```ts
type WorldEvent =
  | { kind: 'AtomAdded';        atom: SkillAtom;   by: Actor }
  | { kind: 'ChallengeAdded';   challenge: Challenge; by: Actor }
  | { kind: 'ChallengeStateChanged'; id: string; from: State; to: State; reason: string }
  | { kind: 'ProposalRejected'; proposal: ProposalRef; stage: number; reason: string }
  | { kind: 'AttemptInitiated' | 'AttemptSubmitted'; … }
  | { kind: 'VerdictRecorded';  verdict: Verdict }      // by LUÔN là người
  | { kind: 'Inscribed';        inscription: Inscription }
  | { kind: 'PayoutReleased';   payout: Payout }
  | { kind: 'ClaimGranted' | 'ClaimExpired'; … }
  | { kind: 'ConstitutionAmended'; diff: string; approvedBy: string[] };

interface Actor {
  kind: 'human' | 'agent';
  id: string;
  model?: string;        // với agent: mô hình nào, phiên bản nào
  promptVersion?: string;
}
```

**Luật:**
1. Chỉ ghi thêm. Không sửa, không xoá.
2. `Actor` luôn ghi rõ người hay máy. Không bao giờ mơ hồ.
3. `VerdictRecorded` bị từ chối nếu `by.kind != 'human'` — cưỡng chế ở tầng ghi.
4. Trạng thái hiện tại là bản chiếu. Xoá bản chiếu, dựng lại, phải ra kết quả y hệt.
5. Sự kiện đủ để **tái dựng toàn bộ thế giới** — kể cả địa hình, vì địa hình là hàm thuần của genome + telemetry + seed.

---

## Tái lập

Cùng một hạt giống + cùng một chuỗi sự kiện ⇒ cùng một thế giới.

Điều này cho phép:
- Kiểm toán: "vì sao ngưỡng này tồn tại?" → truy ngược tới đề xuất, agent, mô hình, phiên bản prompt
- Thử nghiệm: phát lại lịch sử với một hạn ngạch khác để xem thế giới sẽ ra sao
- Khôi phục: quay lại một thời điểm và phát lại

Với nội dung do mô hình sinh, không tái lập được **nội dung** (mô hình không tất định), nhưng tái lập được **quyết định** — đề xuất nào được chấp nhận và vì sao. Đó mới là thứ cần kiểm toán.

---

## Công tắc ngắt

| Phạm vi | Hiệu lực |
|---|---|
| Một agent | < 60s |
| Một nhà cung cấp mô hình | < 60s, tự định tuyến sang nhà cung cấp khác |
| Toàn bộ sinh nội dung | < 60s — **ứng dụng vẫn chạy bình thường** |
| Chế độ AI đối kháng | < 60s |

**Ngắt không bao giờ làm gián đoạn người học đang làm dở.** Một lần thử đang chạy phải đi hết được, dù toàn bộ dàn agent đã tắt.
