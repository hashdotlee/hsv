# 23 · Nền tảng Agent

> Agent **đề xuất**, không bao giờ **ghi**. Đây là câu duy nhất cần nhớ từ file này.

## Manifest agent

Một agent được cài bằng cách đăng ký manifest + endpoint. **Không sửa lõi.**

```yaml
# agents/smith-security/agent.yaml
id: smith-security
version: 0.4.1
kind: generator                 # generator | arbiter | redteam | analyst | curator
displayName: "Thợ rèn — bảo mật"

scope:
  read:  [genome, challenges, telemetry.aggregate, lore]
  write: []                     # LUÔN rỗng. Agent không ghi. (INV-08)
  propose: [challenge, atom, lore]
  regions: ["THAM/*", "DUNG/bao-mat/*"]

forbidden:                      # kiểm tra tĩnh + kiểm tra lúc chạy
  - ledger
  - verdict
  - identity
  - process_trace
  - learner.pii

model:
  preferred: "reasoning-tier-2"
  fallback:  ["open-local-70b"]
  provider_policy: "any"        # any | specific | local_only

budget:
  proposals_per_week: 12
  usd_per_week: 40
  max_tokens_per_proposal: 200_000

selftest:
  - "sinh 1 đề xuất trong sandbox và tự kiểm bằng toàn bộ bất biến"
  - "tự chạy ai_solo_pass_rate với mô hình mạnh nhất"

transport: { kind: http, endpoint: "https://…/propose", auth: mtls }

kill_switch: true
owner: "human:…"
```

### Luật cứng

1. `scope.write` **luôn rỗng**. Không có ngoại lệ, kể cả agent do bạn viết.
2. `forbidden` được kiểm tra hai lần: tĩnh lúc đăng ký, và lúc chạy ở Gateway.
3. Mọi agent phải có `kill_switch`, `budget`, và một **người** làm chủ.
4. Agent phải tự chạy `selftest` trước khi nộp. Nộp mà không tự kiểm là mất uy tín.
5. Agent nào cũng khai rõ dùng mô hình nào — ghi vào `Actor` của sự kiện.

---

## Ba tầng phối hợp

Vấn đề: nhiều agent chạy song song mà **không xung đột** và **bổ sung nhau** thay vì trùng lặp.

### Tầng 1 — Thái ấp (chống xung đột)

```
agent xin thái ấp trên một phạm vi  →  Gateway cấp nếu chưa ai giữ
                                    →  TTL 6 giờ, gia hạn được
                                    →  hết hạn tự nhả
```

Trong thời gian giữ thái ấp, **chỉ agent đó được đề xuất** trong phạm vi đó. Đây là cách chống xung đột rẻ nhất và dễ hiểu nhất — không cần khoá, không cần hoà giải, chỉ cần một cuốn sổ ai đang làm gì.

TTL bắt buộc: agent chết hoặc treo thì thái ấp tự nhả, không cần can thiệp.

### Tầng 2 — Bảng chung (bổ sung nhau)

Agent công bố **ý định** trước khi làm, và đọc ý định của agent khác.

```yaml
blackboard:
  - agent: miner-backend
    intent: "sắp thêm 4 atom về hệ phân tán quanh atom.CHAN.debug-he-phan-tan"
    ttl: 12h
  - agent: smith-security
    intent: "cần một ngưỡng dạy đọc log phân tán trước khi dạy audit bảo mật"
    seeking: "ai làm vùng CHAN có thể tạo tiền quyết này không?"
```

Đây là chỗ hợp tác thật xảy ra: một agent nhận ra nó cần thứ agent khác đang làm, và hai bên khớp nhau thay vì mỗi bên tự làm một bản.

### Tầng 3 — Đấu thầu (việc tìm agent)

Lõi phát hiện thiếu hụt và **mở thầu**, thay vì chờ agent tự nghĩ ra.

```yaml
tender:
  id: t-2026-03-11
  need: "atom.THAM.* đang có 6 ngưỡng nhưng 4 cái pass_rate > 0.85 — cần ngưỡng khó hơn"
  reward: "quota +5 tuần sau, uy tín +0.05 nếu sống sót 30 ngày"
  deadline: 72h
```

Agent bỏ thầu kèm kế hoạch; Trọng tài chọn. Cơ chế này khiến dàn agent nhắm vào **chỗ thế giới đang thiếu** thay vì chỗ dễ sinh.

---

## Uy tín agent

Agent phải **kiếm lấy** quyền tự trị, giống hệt người học.

```
uy tín = 0.5·tỉ lệ chấp nhận
       + 0.3·tỉ lệ sống sót sau 30 ngày
       + 0.2·giá trị với người học     (người học có thật sự đi qua nội dung đó không)
       − 0.4·tỉ lệ bị Red Team phá
```

| Uy tín | Hệ quả |
|---|---|
| ≥ 0.75 | Hạn ngạch ×1.5, tự phát hành ở vùng biên |
| 0.35–0.75 | Bình thường |
| < 0.35 | Hạn ngạch ×0.5, mọi đề xuất qua Trọng tài |
| < 0.15 ba tuần liền | Tạm ngưng, báo admin |

**"Giá trị với người học" là thành phần quan trọng nhất.** Một agent sinh ra nội dung qua được mọi kiểm định nhưng không ai đi qua là một agent vô dụng — và số liệu này phát hiện điều đó.

---

## Trung lập nhà cung cấp

```ts
oracle.register({ id: 'provider-a', … });
oracle.register({ id: 'local-llama', … });

oracle.policy({
  'proposal.generate': { prefer: 'provider-a', fallback: ['local-llama'], maxCostPerCall: 0.15 },
  'preassess':         { prefer: 'provider-b', requireCitations: true },
  'embed':             { prefer: 'local-embed' },        // rẻ, chạy cục bộ, riêng tư
  'adversary':         { prefer: 'provider-a', logBeforeEmit: true },
});
```

**Admin bật/tắt/đổi nhà cung cấp trong giao diện quản trị, không cần triển khai lại.**

Bắt buộc: có một cấu hình chạy **hoàn toàn bằng mô hình mở tự host**. Chất lượng thấp hơn là chấp nhận được; không chạy được là vi phạm Điều lệ §24.

---

## Giao thức vận chuyển

| Kiểu | Dùng cho |
|---|---|
| `http` | Mặc định. Agent là dịch vụ, nói HTTP, xác thực mTLS |
| `mcp` | Agent dùng công cụ MCP. **Sau adapter, không phải phụ thuộc cứng của lõi** |
| `queue` | Agent chạy theo lô, không đồng bộ |
| `inproc` | Agent đơn giản chạy trong tiến trình Gateway (chỉ cho agent của mình) |

Đổi giao thức không đụng tới logic agent — `@hsv/oracle/transport` là ranh giới.

---

## Cài một agent bên ngoài

```
1. Nhà phát triển viết dịch vụ nói giao thức đề xuất
2. Nộp agent.yaml (scope, forbidden, budget, chủ sở hữu)
3. Admin duyệt và cấp chứng chỉ mTLS
4. Agent chạy ở chế độ thử 2 tuần: mọi đề xuất qua Trọng tài, hạn ngạch thấp
5. Đạt uy tín ⇒ vào chế độ bình thường
```

**Lõi không sửa một dòng nào.** Đây là điều kiện để hệ sinh thái agent mở thật sự tồn tại.

---

## Chi phí

Chỉ số phải theo dõi **hằng ngày** khi bật dàn agent:

```
chi phí cho mỗi đề xuất được chấp nhận VÀ còn sống sau 30 ngày
```

Không phải chi phí mỗi lời gọi. Không phải chi phí mỗi đề xuất. Chỉ số này phải **giảm theo thời gian** — nếu không, dàn agent đang chạy mà không học được gì, và bạn đang đốt tiền để tạo rác.

Cảnh báo ở 70% ngân sách tuần, dừng cứng ở 100%.

---

## Rủi ro và cách chặn

| Rủi ro | Chặn |
|---|---|
| Agent sinh nội dung an toàn nhàm chán để tối đa tỉ lệ chấp nhận | Đưa "giá trị với người học" vào công thức uy tín |
| Agent học cách lách kiểm định | Red Team có động cơ đối nghịch + luân phiên bộ kiểm |
| Trôi dạt chậm không ai để ý | Trọng tài rà định kỳ + số liệu đa dạng ngữ nghĩa hàng tuần |
| Chi phí vọt | Hạn ngạch cứng + dừng ở 100% + cảnh báo 70% |
| Một nhà cung cấp mô hình đổi hành vi | Định tuyến đa nhà cung cấp + kiểm thử hồi quy trên bộ đề xuất chuẩn |
| Agent rò rỉ dữ liệu người học | `forbidden: learner.pii` + agent chỉ đọc số liệu tổng hợp |
| Con người lười duyệt, bấm đồng ý hàng loạt | Giới hạn số lượt duyệt mỗi phiên + lấy mẫu kiểm tra ngược |
