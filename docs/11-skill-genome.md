# 11 · Bộ gen kỹ năng

## Sáu trục

Sáu trục của mặt trống đồng, diễn giải cho nghề kỹ sư phần mềm/AI. Culture pack đổi được tên hiển thị; mã trục thì cố định.

| Mã | Tên | Nội dung | Mức AI thay thế |
|---|---|---|---|
| `NEN` | **Nền** | Nguyên lý máy tính: bộ nhớ, đồng thời, mạng, dữ liệu, độ phức tạp, hệ điều hành. Thứ phải nằm sẵn trong đầu để **nghi ngờ đúng chỗ** | cao — nhưng cần để dùng được AI |
| `CHAN` | **Chẩn** | Đọc hệ thống lạ, truy nguyên nhân gốc, tái hiện lỗi khó, thu hẹp không gian giả thuyết | trung bình |
| `DUNG` | **Dựng** | Thiết kế, đánh đổi, quyết định không đảo ngược được, giao hàng thật, vận hành | trung bình |
| `THAM` | **Thẩm** | Đánh giá đầu ra của người khác *và của máy*: review, kiểm thử, bảo mật, phát hiện điều bịa, dám nói không | **thấp — trục đặc trưng của thời đại AI** |
| `NGUOI` | **Người** | Moi ra yêu cầu thật sau yêu cầu được nói ra, bất đồng kỹ thuật, ước lượng trung thực, viết cho người khác hiểu | thấp |
| `TRUYEN` | **Truyền** | Đưa người khác qua ngưỡng | thấp nhất |

Ba trục đầu là cái thị trường *tưởng* nó cần. Ba trục sau là cái nó *thật sự* thiếu. Đường lên đỉnh bắt buộc qua cả sáu.

### Lớp nền cắt ngang

Một số năng lực không thuộc trục nào mà cắt ngang tất cả. Chúng được mô hình hoá riêng vì chúng quyết định *trần* của mọi trục khác:

```yaml
foundation_layers:
  - tieng_anh_ky_thuat    # đọc / viết / nghe tách riêng — rất khác nhau
  - toan_roi_rac
  - doc_hieu_tai_lieu
  - ky_luat_thoi_gian
```

Ví dụ: một người `DUNG=0.70` nhưng `tieng_anh_ky_thuat.doc=0.30` sẽ bị chặn ở mọi ngưỡng cần đọc tài liệu gốc. Hệ thống phải **nhìn ra điều đó và đề xuất đi xuống vá nền**, thay vì để người đó thất bại liên tục mà không hiểu vì sao.

---

## Schema `SkillAtom`

```yaml
# content/genome/CHAN/race-condition-duoi-tai.yaml
id: atom.CHAN.race-condition-duoi-tai
version: 1.2.0
name: "Phát hiện lỗi tranh chấp chỉ lộ dưới tải"
axis: CHAN
type: DIAGNOSTIC          # PROCEDURAL | DIAGNOSTIC | GENERATIVE | SOCIAL | PERCEPTUAL

# BẮT BUỘC (INV-03) — nếu không viết được câu này thì đây không phải atom
mastery_signal: >
  Khi đứng trước một hệ thống có hành vi không tất định dưới tải, người này
  dựng được một giả thuyết về tranh chấp tài nguyên, thiết kế được cách tái
  hiện có kiểm soát, và phân biệt được tranh chấp thật với nhiễu đo lường.

prerequisites:
  - { id: atom.NEN.mo-hinh-bo-nho, min_mastery: 0.5 }
  - { id: atom.NEN.dong-thoi-co-ban, min_mastery: 0.6 }
  - { id: atom.CHAN.tai-hien-loi, min_mastery: 0.4 }

unlocks: [atom.CHAN.debug-he-phan-tan, atom.DUNG.thiet-ke-idempotent]

# Lỗi điển hình — nguồn dữ liệu cho chữ ký lỗi và cho việc sinh gợi ý
common_errors:
  - id: err.sua-trieu-chung
    desc: "Thêm sleep/retry cho hết lỗi mà không hiểu nguyên nhân"
    hint_ladder: [1, 2, 3]
  - id: err.tin-log
    desc: "Tin vào thứ tự dòng log trong môi trường đa luồng"
  - id: err.khong-tai-hien
    desc: "Kết luận nguyên nhân mà chưa tái hiện được lỗi một lần nào"

decay_rate: 0.05          # tỉ lệ mất mỗi tháng nếu không dùng
embodiment: LOW           # LOW | MEDIUM | HIGH — mức cần thao tác thân thể
ai_substitutability:
  value: 0.45
  measured_at: "2026-01-15"
  measured_with: "reasoning-tier-2"
  note: "mô hình đề xuất giả thuyết tốt nhưng không thiết kế được cách tái hiện"

evidence_modality: [process_trace, defense_session, artifact]
estimated_hours: { p25: 6, p50: 14, p75: 40 }   # cập nhật từ dữ liệu thật

sources:
  - { type: postmortem, ref: "…", quote: "…" }
  - { type: interview, ref: "…", at: "12:30" }

created_by: { kind: agent, id: "miner-backend", reviewed_by: "human:…" }
```

### Kiểm định khi ghi

| Kiểm tra | Bất biến |
|---|---|
| `mastery_signal` mô tả hành vi quan sát được | INV-03 |
| Đồ thị vẫn không chu trình sau khi thêm | INV-05 |
| Có ít nhất một đường đi từ atom nhập môn tới atom này | INV-04 |
| Không trùng ngữ nghĩa với atom đã có (cosine < 0.88) | INV-06 |
| Có ít nhất một nguồn trích dẫn | — |

---

## Đồ thị

```ts
interface SkillGraph {
  atoms: Map<AtomId, SkillAtom>;
  edges: Array<{ from: AtomId; to: AtomId; minMastery: number; kind: 'prereq' | 'soft' }>;
}
```

- **Cạnh cứng (`prereq`)** — chặn thật. Không đủ thì không mở được ngưỡng.
- **Cạnh mềm (`soft`)** — chỉ gợi ý. Người học vẫn đi được, nhưng hệ thống cảnh báo.

**Nhiều đường tới một đích là tính chất bắt buộc, không phải hiệu ứng phụ.** Nếu chỉ có một đường tới một atom, đó là dấu hiệu genome chưa đủ giàu — đánh dấu để agent W2 bổ sung.

---

## Ước lượng mức thành thạo

```
mastery(learner, atom, t) =
    evidence_component      # từ các phán quyết đã có, trọng số theo độ mới
  + transfer_component      # lan từ atom lân cận trong đồ thị
  - decay(atom, Δt)         # suy giảm theo thời gian thật
  → kẹp vào [0,1], kèm σ
```

Quy tắc:

- **Bằng chứng trực tiếp lấn át lan truyền.** Lan truyền chỉ dùng khi chưa có bằng chứng, và luôn kèm `σ` lớn.
- **Suy giảm là thật, không phải cơ chế ép quay lại.** Suy giảm chỉ ảnh hưởng độ rõ trên bản đồ và gợi ý ôn lại — **không thu hồi ghi danh**.
- **σ giảm khi có thêm bằng chứng độc lập**, không giảm chỉ vì thời gian trôi.
- Hai tuần đầu là **thời kỳ hiệu chỉnh**: hệ số cập nhật cao, rồi giảm dần. Xem [`14-onboarding.md`](14-onboarding.md).

---

## Chữ ký lỗi

Cốt lõi của cá nhân hoá. **Hai người giống nhau khi họ sai giống nhau**, không phải khi họ cùng tuổi, cùng ngành, hay cùng làm một bài kiểm tra tính cách.

```ts
type ErrorSignature = Map<ErrorId, { frequency: number; recency: number; severity: number }>;
```

Dùng để:
1. **Chọn ngưỡng tiếp theo** — ưu tiên ngưỡng tấn công đúng lỗi hay mắc nhất.
2. **Chọn gợi ý** — thang gợi ý mở theo lỗi *của riêng người này*, không theo đồng hồ.
3. **Truyền dạy giữa các thế hệ** — vết của người đi trước có chữ ký lỗi tương tự thì hữu ích hơn nhiều.
4. **Ghép phòng kín** — chữ ký lỗi khác nhau thì bổ sung nhau; giống hệt nhau thì cùng bí một chỗ.

> Đây là câu trả lời kỹ thuật cho yêu cầu ban đầu: *"dữ liệu được truyền qua thế hệ sau, phụ thuộc vào độ tương đồng"*.

---

## Tổ chức file

```
content/genome/
  NEN/*.yaml
  CHAN/*.yaml
  DUNG/*.yaml
  THAM/*.yaml
  NGUOI/*.yaml
  TRUYEN/*.yaml
  _foundation/tieng-anh-ky-thuat.yaml
  _graph.lock.json        # sinh tự động; CI kiểm tính hợp lệ
```

Mỗi atom là một PR. Người và agent đi cùng một cửa. CI chạy toàn bộ kiểm định trên.

---

## Quy mô mong đợi

| Giai đoạn | Số atom | Ghi chú |
|---|---|---|
| G0 | 15–25 | Bạn viết tay, chỉ quanh 5 ngưỡng đầu |
| G1 | 40–60 | Vẫn chủ yếu viết tay |
| G2 | 100–200 | Agent W2 bắt đầu đóng góp |
| G4 | 500–1500 | Agent sinh, người duyệt |

Trên 2000 atom cho một nghề là dấu hiệu chia quá nhỏ. Nếu một atom không có ngưỡng nào đo được nó, nó không nên tồn tại.
