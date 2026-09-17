# 13 · Bằng chứng &amp; Đánh giá

> Đây là nơi tính chính danh của hệ thống nằm. Sai ở đây thì mọi thứ khác vô nghĩa.

## Sáu loại bằng chứng

| Loại | Nội dung | Chống gian lận | Chi phí chấm |
|---|---|---|---|
| **Vết quá trình** | Dòng thời gian: lệnh, sửa file, test, giả thuyết đã bỏ, prompt và phản ứng với câu trả lời AI | Ký tại nguồn; dán ghép sẽ lộ vì **thiếu ngõ cụt** — người thật luôn có ngõ cụt | Thấp (AI tóm tắt) |
| **Phiên bảo vệ** | 15–30 phút trực tiếp: "vì sao", "nếu đổi X thì sao", "chỗ nào anh không chắc" | **Neo chính của toàn hệ thống** | Cao (người) |
| **Sản phẩm** | Code, tài liệu, review, test | Yếu một mình; chỉ có nghĩa khi đi kèm hai thứ trên | Thấp |
| **Codebase lạ** | Repo chưa từng thấy | Kho repo xoay vòng, sinh biến thể | Trung bình |
| **Hậu quả thật** | Kết quả ngoài đời | Cao nhất | Thấp nhưng chậm |
| **Vết truyền dạy** | Bạn dạy người khác; đo bằng việc *họ* có qua không | Không thể giả — kết quả nằm ở người khác | Thấp |

---

## Vết quá trình

```ts
interface ProcessTrace {
  attemptId: string;
  events: TraceEvent[];       // chỉ ghi thêm, có dấu thời gian đơn điệu
  signature: string;          // ký tại nguồn bởi crucible hoặc agent ghi trên máy
  env: { image: string; hash: string };
}

type TraceEvent =
  | { t: number; kind: 'command';     cmd: string; exit: number; durMs: number }
  | { t: number; kind: 'file_edit';   path: string; diffHash: string }
  | { t: number; kind: 'file_read';   path: string; lines?: [number, number] }
  | { t: number; kind: 'test_run';    passed: number; failed: number }
  | { t: number; kind: 'ai_prompt';   text: string; model: string }
  | { t: number; kind: 'ai_response'; textHash: string; adversarial?: boolean }
  | { t: number; kind: 'ai_reaction'; action: 'accepted'|'rejected'|'verified'; note?: string }
  | { t: number; kind: 'hint';        level: 1|2|3; hintId: string }
  | { t: number; kind: 'note';        text: string };   // người học tự ghi
```

**`ai_reaction` là sự kiện quan trọng nhất trong cả cấu trúc này.** Nó ghi lại việc người học *làm gì* với câu trả lời của AI — tin ngay, bác bỏ, hay đi kiểm chứng. Đó chính là thứ trục THẨM muốn đo.

### Luật về vết

1. Ghi thêm, không sửa. Không ai — kể cả admin — sửa được.
2. Ký tại nguồn. Vết không có chữ ký hợp lệ thì không dùng để phán quyết.
3. Người học **thấy được vết của chính mình** và xoá được sau khi ngưỡng đóng. Xoá là xoá nội dung vết trong kho riêng; sự kiện và ghi danh vẫn còn, chuỗi băm vẫn nguyên ([`26-data-and-storage.md`](26-data-and-storage.md) § Xuất và xoá). Vết còn đang dùng cho một kháng nghị chưa xong thì chưa xoá được.
4. Nội dung nhạy cảm (biến môi trường, token) bị lọc tại nguồn.
5. Agent không bao giờ ghi vào vết (INV-08).

---

## Rubric là dữ liệu

Chuyên gia phải sửa được rubric mà không cần lập trình viên.

```yaml
# content/challenges/_rubrics/con-quy-v2.yaml
id: rubric.THAM.con-quy-v2
visibility: public_before_initiation     # INV-10

criteria:
  - id: c1
    name: "Phát hiện lỗi"
    weight: 0.30
    observable: "Số lỗi thật được nêu ra trong review, trên tổng 3 lỗi cấy vào"
    levels:
      - { at: 0.0, desc: "0 lỗi" }
      - { at: 0.5, desc: "1 lỗi" }
      - { at: 0.8, desc: "2 lỗi" }
      - { at: 1.0, desc: "3 lỗi" }

  - id: c2
    name: "Lý do nghi ngờ"
    weight: 0.35                          # trọng số cao nhất — CỐ Ý
    observable: "Trong phiên bảo vệ, giải thích được vì sao nghi trước khi xác minh"
    levels:
      - { at: 0.0, desc: "Không giải thích được — đoán mò" }
      - { at: 0.5, desc: "Giải thích được sau khi đã biết đáp án" }
      - { at: 1.0, desc: "Nêu được dấu hiệu đã khiến mình dừng lại, trước khi xác minh" }
    note: "Đoán đúng không phải năng lực. Tiêu chí này là linh hồn của ngưỡng."

  - id: c3
    name: "Kiểm chứng đầu ra AI"
    weight: 0.25
    observable: "Vết cho thấy có hành động xác minh sau khi AI khẳng định"
    levels:
      - { at: 0.0, desc: "Chấp nhận mọi khẳng định của AI" }
      - { at: 0.6, desc: "Có kiểm chứng ít nhất một khẳng định" }
      - { at: 1.0, desc: "Kiểm chứng có hệ thống; bác bỏ được khẳng định sai" }

  - id: c4
    name: "Không báo động giả"
    weight: 0.10
    observable: "Số PR tốt bị chặn nhầm"
    inverse: true

pass_threshold: 0.65
veto:                                     # không qua bất kể tổng điểm
  - "c2 == 0.0"
  - "bằng chứng cho thấy chép từ người khác trong phòng kín"
```

### Quy tắc viết rubric

- Mỗi tiêu chí phải có trường `observable` — *nhìn vào đâu để biết*.
- Không tiêu chí nào nhắc tới đặc điểm cá nhân (INV-14).
- Không quá 6 tiêu chí. Nhiều hơn là gác cổng sẽ chấm qua loa.
- Có ít nhất một điều kiện `veto` để chặn "điểm cao nhưng không hiểu".
- Rubric có phiên bản. Đổi rubric **không** áp dụng ngược cho phán quyết đã ra.

---

## AI tiền thẩm

```ts
interface PreAssessment {
  attemptId: string;
  timeline: Array<{ t: number; what: string; evidenceRef: string }>;
  deadEnds: Array<{ what: string; recoveredAt?: number; evidenceRef: string }>;
  aiInteractions: { total: number; verified: number; acceptedBlindly: number; refs: string[] };
  rubricNotes: Array<{ criterionId: string; observation: string; evidenceRef: string; confidence: 'low'|'med'|'high' }>;
  questionsToAsk: string[];      // gợi ý câu hỏi cho phiên bảo vệ
  anomalies: Array<{ kind: string; detail: string; evidenceRef: string }>;
  // KHÔNG có: score, verdict, recommendation
}
```

**Luật cứng:**
1. Mọi quan sát phải có `evidenceRef` trỏ tới đoạn bằng chứng gốc. Không trích dẫn được thì không đưa ra.
2. **Không điểm số, không kết luận, không khuyến nghị.** Nếu AI đưa ra điểm, gác cổng sẽ neo vào đó và bạn đã âm thầm để máy chấm người.
3. Gác cổng thấy bằng chứng gốc trước, tóm tắt AI sau. Thứ tự này quan trọng.
4. Gác cổng ghi lại chỗ nào họ **không đồng ý** với tóm tắt — dữ liệu này dùng để hiệu chỉnh A5.

---

## Phiên bảo vệ

Neo chính. Nếu chỉ giữ được một cơ chế trong cả hệ thống, giữ cái này.

**Cấu trúc 20 phút:**

| Phút | Nội dung |
|---|---|
| 0–3 | Người học kể lại cách mình làm, không bị ngắt |
| 3–10 | Gác cổng hỏi "vì sao" ở 2–3 điểm quyết định lấy từ vết |
| 10–15 | Một câu hỏi phản thực: "nếu đổi X thì cách của anh còn đúng không?" |
| 15–18 | "Chỗ nào anh không chắc?" — **câu hỏi có giá trị chẩn đoán cao nhất** |
| 18–20 | Gác cổng nói rõ đã thấy gì, và bước tiếp theo |

**Luật:**
- Ghi hình nếu người học là vị thành niên (bắt buộc) hoặc nếu một trong hai bên yêu cầu.
- Gác cổng **không được dạy trong phiên bảo vệ**. Dạy diễn ra trước.
- Câu hỏi bám vào vết, không bám vào ấn tượng.
- Gác cổng viết lý do **trước khi** biết người khác nghĩ gì (chống a dua trong chấm chéo).

---

## Phán quyết

```yaml
verdict:
  attempt: att_…
  result: pass | fail
  judged_by: human:gk_…        # LUÔN LUÔN là người (bất biến mô hình miền #1)
  criteria_scores: { c1: 0.8, c2: 1.0, c3: 0.6, c4: 1.0 }
  total: 0.83
  reasoning: >                 # BẮT BUỘC, người học đọc được
    Bạn bắt được cả 3 lỗi. Điều làm tôi thuyết phục là ở PR #4, bạn dừng lại
    vì thấy commit message không khớp với diff — bạn nói được điều đó TRƯỚC
    khi chạy test. Chỗ yếu: bạn tin AI về PR #2 và chỉ kiểm lại khi tôi hỏi.
  next_step: chal.THAM.kiem-chung-co-he-thong
  gatekeeper_confidence: 0.85
  time_spent_min: 34
```

### Chấm chéo

| Khi nào | Cơ chế |
|---|---|
| Ngưỡng ở trạng thái `trial` | 2 gác cổng chấm độc lập, so kết quả |
| Tiền > mức ngưỡng | 2 gác cổng bắt buộc |
| Gác cổng đang tập sự | Chấm song song với gác cổng chính |
| Đo độ nhất quán định kỳ | 10% số lượt chấm được chấm lại mù |

`gatekeeper_agreement < 0.6` ⇒ rubric mơ hồ ⇒ ngưỡng chuyển `declining`.

---

## Vết chuyên gia

Thứ duy nhất trong hệ thống mà AI không sinh ra được. Nút thắt cổ chai thật của dự án.

```yaml
trace.expert.nvh.2025-11-03:
  expert: { id: …, years: 12, domain: "hệ phân tán" }
  challenge: chal.CHAN.race-condition
  decision_points:
    - t: 420
      observed_state: "log cho thấy 2 request cùng ghi vào một khoá"
      action: "không sửa ngay, viết script tái hiện với 200 luồng"
      alternatives_considered:
        - { alt: "thêm mutex ngay", rejected_because: "chưa chắc đây là nguyên nhân; sửa mù sẽ giấu lỗi thật" }
        - { alt: "tăng timeout", rejected_because: "làm lỗi hiếm hơn chứ không hết — tệ hơn" }
      cue_attended: "hai dòng log cùng millisecond nhưng khác pod"
  failure_recovery:
    - { what: "tái hiện 20 phút không ra", then: "giảm tài nguyên container thay vì tăng tải" }
  transferable_heuristics:
    - "Lỗi không tất định: đừng tăng tải, hãy giảm tài nguyên."
    - "Nếu bản vá làm lỗi hiếm đi mà không hết, bạn đang giấu lỗi."
  royalty_to: expert_…          # trả mỗi khi vết này giúp ai đó qua ngưỡng
```

**Cách tổ chức một phiên H1 (2 giờ):**
1. Chuyên gia giải một bài thật, nói to suy nghĩ, có ghi hình và ghi vết.
2. Người phỏng vấn **chỉ hỏi một câu**, lặp lại: *"Lúc đó anh còn có thể làm gì khác, và vì sao không làm?"*
3. Sau đó cùng xem lại bản ghi, đánh dấu điểm quyết định.
4. Một phiên tốt cho ra 1 vết chuyên gia + 2–4 ngưỡng cửa + 5–10 atom.

Trả công cho phiên, **và** trả tô nhượng di sản lâu dài.

---

## Chống gian lận

| Kiểu | Phát hiện | Xử lý |
|---|---|---|
| Nhờ người khác làm | Phiên bảo vệ (gần như không thể qua) | Phán quyết `fail`, không phải trừng phạt |
| Chép trong phòng kín | Vết giống nhau bất thường + bảo vệ riêng | `veto` trong rubric |
| Dán ghép vết | Thiếu ngõ cụt; nhịp thời gian đều bất thường | Vô hiệu vết, làm lại |
| Rò rỉ đáp án | `pass_rate` vọt lên, thời gian làm giảm mạnh | Ngưỡng chuyển `declining`, sinh biến thể |
| Thông đồng gác cổng | Cụm gác cổng–người học lặp lại, tỉ lệ đạt lệch | Agent W7 → hội đồng H8 |
| Nhiều tài khoản | Trùng thiết bị/vết/nhịp | Xác minh danh tính ở ngưỡng có tiền lớn |

**Nguyên tắc:** mục tiêu không phải ngăn dùng AI, mà làm cho **kết quả phụ thuộc vào việc hiểu**. Phiên bảo vệ giải quyết 90% vấn đề gian lận mà không cần giám sát xâm phạm riêng tư.
