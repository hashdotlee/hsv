# 27 · Hợp đồng API

> Bốn bề mặt: người học · gác cổng · agent · quản trị. Giao thức của mình, mỏng, chạy được trên mạng 2G.

## Quy ước chung

```
Base:   /v1
Định dạng: JSON
Xác thực: Bearer (người) · mTLS + manifest (agent)
Lỗi:    { "error": { "code": "…", "message": "…", "detail": {…} } }
Phân trang: con trỏ (cursor), không dùng offset
Idempotency-Key: bắt buộc cho mọi POST có tác dụng phụ
```

Mọi phản hồi chứa số năng lực **phải kèm σ**. Không có ngoại lệ — Nguyên tắc 2 được cưỡng chế ở tầng hợp đồng.

---

## API người học

```http
POST /v1/onboarding/intent          { goal, why }
POST /v1/onboarding/selfreport      { axes, foundations, years, constraints }
POST /v1/onboarding/evidence        { github?, cv?, links[] }
GET  /v1/onboarding/probe/next      → { itemId, prompt, kind, progress }
POST /v1/onboarding/probe/answer    { itemId, answer | skipped, elapsedMs }
GET  /v1/onboarding/position        → Position   (θ và σ theo từng trục)

GET  /v1/map/macro                  → SVG + dữ liệu vùng
GET  /v1/map/region/:id             → { terrain, fog{learner,world}, challenges[], trails[] }
GET  /v1/me/position                → Position
GET  /v1/me/suggestions?n=4         → [{ challenge, direction: up|across|down|surprise, why }]

GET  /v1/challenges/:id             → Challenge   (rubric công khai — INV-10)
POST /v1/challenges/:id/initiate    → { attemptId, chamberId?, gatekeeper, sandbox }
GET  /v1/attempts/:id               → Attempt
POST /v1/attempts/:id/trace         { events[] }          # xếp hàng được khi ngoại tuyến
POST /v1/attempts/:id/hint          → { level, text, recorded: boolean }
POST /v1/attempts/:id/submit        { artifacts[] }
GET  /v1/attempts/:id/verdict       → Verdict   (kèm reasoning — Điều lệ §13)
POST /v1/attempts/:id/appeal        { reason }

GET  /v1/chambers/:id/messages      (SSE)
POST /v1/chambers/:id/messages      { text }

GET  /v1/me/inscriptions            → Inscription[]
GET  /v1/me/wallet                  → { pending, escrowed, paid, history[] }
GET  /v1/me/export                  → tải toàn bộ, định dạng mở (Điều lệ §2)
POST /v1/me/erase                   → xoá tài khoản
```

### Chế độ ngoại tuyến

`POST /v1/attempts/:id/trace` chấp nhận lô sự kiện có dấu thời gian cục bộ và `Idempotency-Key`. Máy khách xếp hàng khi mất mạng, gửi lại khi có. Chênh lệch đồng hồ được **ghi nhận, không sửa**.

---

## API gác cổng

```http
GET  /v1/gk/queue                   → [{ attemptId, challenge, waitingSince, isMinor }]
GET  /v1/gk/attempts/:id/evidence   → bằng chứng GỐC       # trả về TRƯỚC
GET  /v1/gk/attempts/:id/preassess  → PreAssessment        # chỉ lấy được SAU khi đã gọi /evidence
POST /v1/gk/attempts/:id/defense    { scheduledAt, recording: boolean }
POST /v1/gk/attempts/:id/verdict    { criteriaScores, reasoning, nextStep, confidence }
POST /v1/gk/attempts/:id/disagree   { preassessField, note }   # dữ liệu hiệu chỉnh cho scribe

GET  /v1/gk/me/stats                → { learnerProgress, agreement, appealOverturnRate, responseTime }
```

**Thứ tự cưỡng chế:** `/preassess` trả `409` nếu chưa gọi `/evidence` cho lần thử đó. Gác cổng phải nhìn bằng chứng gốc trước khi đọc tóm tắt của máy — nếu không, họ sẽ neo vào tóm tắt.

**`POST /verdict` từ chối nếu:**
- `reasoning` rỗng hoặc dưới 80 ký tự (Điều lệ §13)
- Người gọi không phải gác cổng đủ điều kiện của ngưỡng đó
- Ngưỡng yêu cầu chấm chéo mà chưa đủ hai phán quyết độc lập

---

## API agent

```http
POST /v1/agents/register            { manifest }              → { agentId, cert }
POST /v1/agents/:id/claim           { region, ttl }           → { claimId, expiresAt } | 409
POST /v1/agents/:id/claim/:cid/renew
DELETE /v1/agents/:id/claim/:cid

GET  /v1/blackboard                 → Intent[]
POST /v1/blackboard                 { intent, ttl, seeking? }
GET  /v1/tenders                    → Tender[]
POST /v1/tenders/:id/bid            { plan, estimatedCost }

POST /v1/agents/:id/propose         { proposal }              → { proposalId, stage }
GET  /v1/proposals/:id              → { state, stage, rejectionReason? }

GET  /v1/agents/:id/budget          → { used, remaining, resetsAt }
GET  /v1/agents/:id/reputation      → { score, components }
```

### Phản hồi từ chối

Máy đọc được, để agent học:

```json
{
  "state": "rejected",
  "stage": 3,
  "reason": { "code": "INV-06", "message": "trùng ngữ nghĩa 0.91 với chal.THAM.doc-diff-co-ban",
              "suggestion": "nộp lại dưới dạng đề xuất SỬA cho ngưỡng đó" }
}
```

### Cưỡng chế phạm vi

Mọi lời gọi của agent bị kiểm với manifest **tại Gateway**, không tin agent tự giới hạn:

```
scope.write không rỗng           → 403 tại lúc đăng ký
chạm vào forbidden               → 403 + cảnh báo + uy tín giảm
ngoài regions đã khai            → 403
vượt hạn ngạch                   → 429 kèm resetsAt
đề xuất ngoài thái ấp đang giữ   → 409
```

---

## API quản trị

```http
GET  /v1/admin/providers            → ModelProvider[]
POST /v1/admin/providers            { id, kind, config }
PUT  /v1/admin/routing              { taskKind: { prefer, fallback[], maxCostPerCall } }

POST /v1/admin/kill                 { scope: agent|provider|generation|adversary, id? }
PUT  /v1/admin/budgets              { … }                    # ghi đè hạn ngạch hiến pháp

GET  /v1/admin/health               → { queueDepth, costPerAcceptedAlive30d, driftIndex, gkCapacityRatio }
GET  /v1/admin/review-queue         → Proposal[]
POST /v1/admin/review/:id           { decision, note }

GET  /v1/admin/ledger/public        → PublicLedger           # công khai, không cần xác thực
```

`POST /v1/admin/kill` phải có hiệu lực **< 60 giây** và không được làm gián đoạn lần thử đang chạy.

---

## Sự kiện SSE

```
GET /v1/stream        (theo vai trò)

learner:    attempt.state · verdict.ready · chamber.message · payout.released · map.updated
gatekeeper: queue.new · defense.reminder · appeal.filed
admin:      budget.warning · drift.alert · sentinel.flag · provider.degraded
```

---

## Phiên bản

- Đường dẫn: `/v1`. Thay đổi phá vỡ tương thích ⇒ `/v2`, chạy song song ≥ 12 tháng.
- Thêm trường là tương thích; xoá hoặc đổi nghĩa thì không.
- Máy khách phải bỏ qua trường lạ.

## Giới hạn tốc độ

| Bề mặt | Giới hạn |
|---|---|
| Người học | 120 req/phút; `trace` 600 sự kiện/phút |
| Gác cổng | 60 req/phút |
| Agent | Theo hạn ngạch manifest, không theo thời gian |
| Quản trị | 30 req/phút |
| Sổ cái công khai | 10 req/phút, có bộ nhớ đệm |
