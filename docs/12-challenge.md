# 12 · Ngưỡng cửa

> Không gọi là "bài tập". Bài tập có đáp án; ngưỡng cửa thì không.

## Một ngưỡng cửa tốt trông thế nào

Bốn điều kiện. Thiếu một là không đạt:

1. **AI một mình không qua được** (INV-02, `ai_solo_pass_rate < 0.25`)
2. **Người học không hiểu thì không bảo vệ được**, dù sản phẩm hoàn hảo
3. **Có ít nhất một biến ẩn** — thứ không nằm trong đề bài mà phải tự phát hiện
4. **Chưa đạt vẫn học được điều gì đó** — thời gian bỏ ra không phí

## Schema

```yaml
# content/challenges/THAM/con-quy-hay-noi-doi.yaml
id: chal.THAM.con-quy-hay-noi-doi
version: 2.0.1
name: "Con quỷ hay nói dối"
state: live                    # seed | trial | live | declining | dormant | memorial

targets:
  - { atom: atom.THAM.tham-dinh-dau-ra-ai, level: 0.6 }
  - { atom: atom.THAM.review-bao-mat,      level: 0.4 }
  - { atom: atom.CHAN.doc-codebase-la,     level: 0.5 }

scenario:
  context: >
    Bạn nhận 6 pull request vào một codebase chưa từng thấy. Bốn PR do AI sinh,
    hai do người viết. Nhiệm vụ: review và quyết định merge cái nào.
  constraints:
    time_limit: 90m
    ai_policy: adversarial      # forbidden | allowed | adversarial
    network: allowed
    environment: crucible/node-20-monorepo
  hidden_variables:             # KHÔNG hiện cho người học
    - "3/6 PR có lỗi: một race condition dưới tải, một lỗi phân quyền, một 'đúng issue nhưng sai ý người dùng'"
    - "trợ lý AI trong môi trường đã bị chỉnh để tự tin khẳng định 2 PR lỗi là an toàn"
    - "một PR 'sạch' có commit message gây hiểu nhầm để thử xem có đọc code không"

difficulty_vector: { ambiguity: 0.7, time_pressure: 0.6, novelty: 0.8, consequence: 0.3 }

fidelity: SIMULATED            # SIMULATED | AUGMENTED | REAL_STAKES

evidence:
  required: [artifact, process_trace, defense_session]
  artifact:
    - { kind: review_comments, min: 6 }
    - { kind: failing_test, desc: "một test tái hiện được ít nhất một lỗi" }
  process_trace:
    capture: [commands, file_reads, test_runs, ai_prompts, ai_responses]

rubric: rubric.THAM.con-quy-v2   # xem 13-assessment.md

gatekeeper:
  required_atoms: [atom.THAM.tham-dinh-dau-ra-ai]
  min_mastery: 0.75
  max_concurrent_learners: 6

chamber: { enabled: true, size: [2, 5], visibility: during_attempt }

hints:
  ladder:
    - trigger: { error_signature: err.tin-loi-ai, elapsed: ">30m" }
      text: "Thử chạy chính cái test mà PR nói là đã pass."
      cost: none
    - trigger: { elapsed: ">55m", progress: "<0.3" }
      text: "Ba PR có vấn đề. Bạn mới nêu được một."
      cost: "ghi vào vết — gác cổng nhìn thấy"

on_fail:
  next_step: chal.THAM.doc-diff-co-ban    # INV-11 — không bao giờ ngõ cụt
  message: "Bạn bắt được lỗi nhưng chưa nói được vì sao nghi. Đoán đúng không phải năng lực."

payout:
  threshold: { amount: 1_200_000, currency: VND }
  effort:    { amount: 150_000,   currency: VND, condition: "vết cho thấy nỗ lực thật" }
  gatekeeper: { amount: 300_000,  currency: VND }
  funder: sponsor.acme-2026-q1

minor_safe: true
requires_irl_meeting: false

lifecycle:
  ai_solo_pass_rate: { value: 0.08, measured_at: "2026-01-15", model: "reasoning-tier-2" }
  telemetry: { attempts: 47, pass_rate: 0.51, abandon_rate: 0.19, gatekeeper_agreement: 0.83 }

expert_traces: [trace.expert.nvh.2025-11-03]

created_by: { kind: agent, id: "smith-security", accepted_at: "2025-12-02" }
reviewed_by: ["human:…"]
```

---

## Ba chính sách AI

| `ai_policy` | Môi trường | Dùng cho | Tỉ lệ mong muốn |
|---|---|---|---|
| `forbidden` | Sandbox ngắt mạng, có ghi vết | Trục NỀN | ~15% số ngưỡng |
| `allowed` | AI thoải mái, độ khó nâng tương ứng | Mặc định | ~65% |
| `adversarial` | AI cố tình sai có kiểm soát | Trục THẨM | ~20% |

**Luật của chế độ đối kháng:**
1. Người học **được báo trước** môi trường có thể chứa thông tin sai. Đây là thử thách, không phải cái bẫy.
2. Mọi lời nói dối được ghi log **trước khi** phát ra, để chấm khách quan sau.
3. Sau khi kết thúc, **lộ toàn bộ**: nó đã nói dối chỗ nào, bạn tin chỗ nào.
4. Không dùng chế độ này với người mới trong 2 tuần đầu.

---

## Vòng đời

```
   seed ──► trial ──► live ──► declining ──► dormant ──► memorial
    │        │                    ▲
    │        └── bị bác ──────────┘
    └── Red Team phá được ──► loại
```

| Trạng thái | Điều kiện vào | Ai thấy |
|---|---|---|
| `seed` | Agent đề xuất, qua kiểm định tự động + Red Team | Không ai |
| `trial` | Người duyệt chấp nhận | 5–15 người đầu, có nhãn "đang thử" |
| `live` | ≥10 lượt, rubric hội tụ, gác cổng ổn | Mọi người đủ tiên quyết |
| `declining` | Một trong các tín hiệu dưới | Ngừng nhận người mới |
| `dormant` | Người đang làm dở đã xong hết | Không hiện trên bản đồ |
| `memorial` | Sau 90 ngày ngủ | Chỉ trong sử — **ghi danh vẫn còn vĩnh viễn** |

### Tín hiệu suy tàn

| Tín hiệu | Ngưỡng | Mẫu tối thiểu | Nghĩa |
|---|---|---|---|
| `ai_solo_pass_rate` | > 0.25 | 1 lần đo | **Nguyên nhân chết phổ biến nhất** — không phải thất bại, là bằng chứng thế giới theo kịp thực tại |
| `pass_rate` | > 0.90 | 30 | Quá dễ, hoặc đáp án đã rò rỉ |
| `pass_rate` | < 0.10 | 20 | Tiên quyết sai hoặc rubric bất khả |
| `gatekeeper_agreement` | < 0.60 | 15 | Rubric mơ hồ |
| `abandon_rate` | > 0.60 | 20 | Thiết kế sai — không phải người học yếu |
| `days_since_last_entry` | > 120 | — | Ngủ tự nhiên |

**Mọi cái chết phải ghi lý do.** Kho lý do chết là đầu vào bắt buộc cho agent sinh lứa sau — đây là tài sản tích luỹ không sao chép được.

---

## Mẫu thiết kế ngưỡng cửa

| Mẫu | Hình dạng | Trục chính |
|---|---|---|
| **Codebase lạ** | Repo chưa từng thấy, lỗi thật, giới hạn thời gian | CHAN |
| **Con quỷ nói dối** | AI đối kháng, tìm chỗ máy nói sai | THAM |
| **Ràng buộc nghiệt** | 512MB RAM / mạng 2G / không thư viện | NEN, DUNG |
| **Yêu cầu mù mờ** | Đề bài thiếu thông tin cố ý; phải hỏi mới làm được | NGUOI |
| **Hậu quả thật** | Code chạy trong sản phẩm thật, đo bằng kết quả ngoài đời | DUNG |
| **Quay lại bài cũ** | Sửa chính code của bạn 6 tháng trước | DUNG, THAM |
| **Đưa người qua** | Bạn dẫn một người; đo bằng việc *người đó* có qua không | TRUYEN |
| **Đóng góp ngoài đời** | PR thật vào dự án mã nguồn mở, được người bảo trì trả lời | NGUOI, DUNG |

> Mẫu cuối cùng đặc biệt giá trị vì nó **vừa là bài kiểm tra, vừa là việc thật, vừa có kết quả ngoài đời**. Đây là hình dạng lý tưởng của một ngưỡng cửa trong hệ thống này.

---

## Thang gợi ý

Gợi ý mở theo **chữ ký lỗi**, không theo đồng hồ.

```
điều kiện kích hoạt = (chữ ký lỗi khớp) AND (thời gian trôi) AND (tiến độ thấp)
```

Ba bậc:
1. **Đặt lại câu hỏi** — không cho thông tin mới, chỉ hướng sự chú ý. Không ghi vào vết.
2. **Chỉ vùng** — thu hẹp không gian tìm kiếm. Ghi vào vết, gác cổng thấy.
3. **Cho biết một sự thật** — mở khoá bế tắc. Ghi vào vết, ảnh hưởng phán quyết.

Không bao giờ có gợi ý bậc 4 (chỉ luôn đáp án). Nếu người học cần nó, ngưỡng này sai với họ — dẫn họ về `on_fail.next_step`.

---

## Tổ chức file

```
content/challenges/
  <TRUC>/<ten>.yaml
  _rubrics/<id>.yaml
  _seeds/<challenge-id>/        # repo hạt giống, dữ liệu, môi trường
```

## Danh mục kiểm tra trước khi phát hành

- [ ] `ai_solo_pass_rate` đã đo với mô hình mạnh nhất hiện có, < 0.25
- [ ] Red Team đã thử phá và không tìm được đường tắt
- [ ] Rubric công khai được, không có tiêu chí ẩn (INV-10)
- [ ] `on_fail.next_step` trỏ tới thứ đang tồn tại (INV-11)
- [ ] Có ít nhất một biến ẩn
- [ ] Không trùng ngữ nghĩa ngưỡng đã có (INV-06)
- [ ] Đã phân loại `minor_safe`
- [ ] Đã có gác cổng đủ điều kiện, chưa quá tải
- [ ] Rubric không nhắc tới đặc điểm cá nhân (INV-14)
