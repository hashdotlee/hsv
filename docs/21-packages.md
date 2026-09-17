# 21 · Danh mục gói

> "Xây nhà cũng cần gạch vữa." Các gói này là gạch vữa — và chúng có giá trị độc lập với sản phẩm chính. Mỗi gói xuất bản riêng được.

## Bảng tổng

| Gói | Vai | Tầng phụ thuộc | Ưu tiên |
|---|---|---|---|
| `@hsv/genome` | Đồ thị kỹ năng: schema, kiểm định, truy vấn, ước lượng thành thạo | **0** | G0 |
| `@hsv/rubric` | Rubric dạng dữ liệu, chấm, veto, đo độ nhất quán | **0** | G0 |
| `@hsv/trace` | Ghi vết, ký, phát lại, phát hiện bất thường | **0** | G0 |
| `@hsv/rite` | Máy trạng thái nghi thức | **0** | G1 |
| `@hsv/ledger` | Sổ cái chỉ ghi thêm, ký quỹ, phân bổ | **0** | G3 |
| `@hsv/cartograph` | Sinh địa hình, định vị, sương mù, chọn đường | **0** | G2 |
| `@hsv/pack` | Culture pack: schema, tải, kiểm định, phiên bản | **0** | G2 |
| `@hsv/tokens` | Design token W3C → mọi đích | **0** | G1 |
| `@hsv/chronicle` | Kho sự kiện, bản chiếu, dựng lại | **0** | G1 |
| `@hsv/oracle` | **Adapter AI** — trung lập nhà cung cấp, tiền thẩm, chế độ đối kháng | 1 | G2 |
| `@hsv/crucible` | **Adapter sandbox** — môi trường cách ly có ghi vết | 1 | G1 |
| `@hsv/forge` | Asset pipeline: ingest → chuẩn hoá → trích xuất → đóng gói | 1 | G3 |
| `@hsv/glyph` | Hoa văn SVG: tham số hoá, lát, sinh biến thể | 2 | G4 |
| `@hsv/atlas` | Sinh ngoại hình nhân vật **từ dữ liệu năng lực** | 2 | G4 |
| `@hsv/ui` | Thành phần React, không phụ thuộc thư viện UI ngoài | 2 | G1 |
| `@hsv/net` | Giao thức: HTTP+SSE, xếp hàng ngoại tuyến, đồng bộ | 1 | G1 |

---

## Chi tiết các gói tầng 0

### `@hsv/genome`

```ts
class SkillGraph {
  add(atom: SkillAtom): Result<void, ValidationError>;   // kiểm không chu trình, không ngõ cụt, không trùng
  unlockedFor(position: Position): AtomId[];
  pathTo(target: AtomId, from: Position): AtomId[][];     // NHIỀU đường, không phải một
  mastery(learner: LearnerEvidence, atom: AtomId, at: Timestamp): { theta: number; sigma: number };
  errorSignature(learner: LearnerEvidence): ErrorSignature;
  similarity(a: ErrorSignature, b: ErrorSignature): number;
}
```

Thuần, không I/O. Đây là gói đầu tiên phải viết và là gói có giá trị dùng lại cao nhất cho cộng đồng.

### `@hsv/rubric`

```ts
function score(rubric: Rubric, observations: Observation[]): ScoreResult;
function checkVeto(rubric: Rubric, ctx: AttemptContext): VetoResult;
function agreement(verdicts: Verdict[]): number;       // độ nhất quán giữa gác cổng
function validate(rubric: Rubric): ValidationError[];   // INV-10, INV-14
```

Rubric là **dữ liệu**. Gói này không bao giờ tạo ra một `Verdict` — nó chỉ tính điểm từ quan sát mà **người** đã ghi nhận.

### `@hsv/trace`

```ts
class TraceRecorder { record(e: TraceEvent): void; seal(): SignedTrace; }
class TraceReader   { replay(t: SignedTrace): Iterable<TraceEvent>; }

function verify(t: SignedTrace): boolean;
function anomalies(t: SignedTrace): Anomaly[];   // thiếu ngõ cụt, nhịp đều bất thường, khoảng trống
function summarizeForHuman(t: SignedTrace): TraceSummary;   // không dùng AI
```

`summarizeForHuman` không dùng AI — đây là bản dự phòng khi không có nhà cung cấp mô hình nào (Điều lệ §24).

### `@hsv/cartograph`

```ts
function terrain(genome: SkillGraph, telemetry: Telemetry, seed: number): TerrainField;  // HÀM THUẦN (INV-13)
function project(position: Position, terrain: TerrainField): MapCoords;
function fog(position: Position, telemetry: Telemetry): FogField;   // hai lớp: của người học và của thế giới
function suggest(position: Position, genome: SkillGraph, n: number): ChallengeSuggestion[];
```

### `@hsv/ledger`

```ts
function commit(entry: LedgerEntry): Result<EntryId, LedgerError>;   // chỉ ghi thêm
function escrow(sponsorId, challengeId, amount): EscrowId;
function release(escrowId, verdictId): Result<PayoutId, LedgerError>; // BẮT BUỘC có verdictId
function reverse(entryId, reason): EntryId;                           // sửa sai bằng bút toán đảo
function publicSummary(period): PublicLedger;
```

`release` không nhận được lệnh nếu không có `verdictId` — bất biến mô hình miền #6 được cưỡng chế ở chữ ký hàm.

---

## Các adapter tầng 1

### `@hsv/oracle`

```ts
interface ModelProvider {
  id: string;
  complete(req: CompletionRequest): Promise<CompletionResponse>;
  capabilities: { maxContext: number; vision: boolean; cost: CostModel };
}

class Oracle {
  register(p: ModelProvider): void;
  route(task: TaskKind, policy: RoutingPolicy): ModelProvider;
  preAssess(trace: SignedTrace, rubric: Rubric): Promise<PreAssessment>;  // KHÔNG trả điểm
  adversary(ctx: ChallengeContext): AdversaryRun;                          // log lời nói dối TRƯỚC khi phát
}
```

**Cấm gọi SDK nhà cung cấp ở bất kỳ đâu khác** — luật lint trong CI (Hiến pháp phụ thuộc L2). Phải có cấu hình chạy hoàn toàn bằng mô hình mở tự host.

### `@hsv/crucible`

```ts
interface SandboxHost {
  spawn(image: ImageRef, policy: NetworkPolicy): Promise<Session>;
}
interface Session {
  exec(cmd: string): Promise<ExecResult>;
  trace: TraceRecorder;      // vết được ký TẠI ĐÂY, tại nguồn
  teardown(): Promise<void>;
}
```

Chính sách mạng là công dân hạng nhất: `forbidden` cần ngắt mạng thật, không chỉ yêu cầu người học đừng dùng.

### `@hsv/net`

Giao thức riêng, mỏng: HTTP + SSE, hàng đợi ngoại tuyến, đồng bộ lại, thử lại có lùi dần. Không dùng framework nặng — phần này phải chạy được trên mạng 2G.

---

## Các gói tầng 2

### `@hsv/atlas`

Sinh ngoại hình nhân vật **từ dữ liệu năng lực đã kiểm chứng**, và không có đường nào khác.

```ts
function appearance(position: Position, inscriptions: Inscription[], pack: CulturePack): Appearance;
```

Hàm thuần, không có tham số "mua thêm". Đây là cách Điều lệ §8 ("không mua được ngoại hình") được cưỡng chế ở mức kiểu dữ liệu chứ không phải ở mức chính sách.

### `@hsv/glyph`

Hoa văn SVG tham số hoá: lát hoa văn trống đồng, sinh biến thể theo hạt giống, xuất SVG/PNG/texture. Dùng cho bản đồ, nền, con dấu, chứng nhận.

---

## Thứ tự xây

```
G0  genome · rubric · trace                    ← đủ để bạn tự đi 5 ngưỡng đầu
G1  rite · chronicle · crucible · net · ui · tokens
G2  cartograph · oracle · pack
G3  ledger · forge
G4  atlas · glyph
```

**Đừng xây gói nào trước khi có thứ cần dùng nó.** Rủi ro lớn nhất của bản năng "tự viết hết" là 18 tháng hạ tầng đẹp và không có người học nào.

## Tiêu chuẩn chung cho mọi gói

- TypeScript nghiêm ngặt, không `any`
- Tầng 0: **không phụ thuộc thời gian chạy** ngoài thư viện chuẩn
- Kiểm thử tính chất cho bất biến (không chu trình, sổ cái cân bằng, phát lại vết)
- `README.md` + ví dụ chạy được
- Xuất bản độc lập, semver
- Giấy phép: Apache-2.0 cho mã, CC BY-SA 4.0 cho nội dung
