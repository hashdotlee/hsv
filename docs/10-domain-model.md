# 10 · Mô hình miền

> Bản đồ khái niệm. Mọi schema chi tiết nằm ở các file 11–19.

## Sơ đồ quan hệ

```
                          ┌──────────────┐
                          │  SkillGenome │  đồ thị có hướng, không chu trình
                          └──────┬───────┘
                                 │ chứa
                          ┌──────▼───────┐
              ┌──────────►│  SkillAtom   │◄──────────┐
              │           └──────┬───────┘           │
              │ targets          │ prerequisites     │ measures
              │                  ▼ (self-ref)        │
       ┌──────┴──────┐                        ┌──────┴───────┐
       │  Challenge  │───────has─────────────►│   Rubric     │
       └──┬───┬───┬──┘                        └──────────────┘
          │   │   └──────references──────────►┌──────────────┐
          │   │                               │ ExpertTrace  │ vết của người đi trước
          │   │                               └──────────────┘
          │   └──────guarded by───────────────►┌──────────────┐
          │                                    │  Gatekeeper  │ (là một Learner)
          │                                    └──────────────┘
          │ produces
          ▼
    ┌───────────┐   submits   ┌──────────────┐   summarised by  ┌──────────────┐
    │  Attempt  │◄────────────│   Learner    │                  │PreAssessment │
    └─────┬─────┘             └──────┬───────┘                  └──────────────┘
          │ contains                 │ has                            ▲ (AI, chỉ đọc)
          ▼                          ▼                                │
   ┌──────────────┐          ┌──────────────┐                         │
   │ProcessTrace  │──────────┴─►│  Position  │ θ và σ theo từng trục  │
   │ + Artifact   │             └──────────────┘                      │
   └──────┬───────┘                                                   │
          │ judged in                                                 │
          ▼                                                           │
   ┌──────────────┐    yields    ┌──────────┐   triggers   ┌──────────┴───┐
   │DefenseSession│─────────────►│ Verdict  │─────────────►│   Payout     │
   └──────────────┘              └────┬─────┘              └──────────────┘
                                      │ if pass
                                      ▼
                               ┌──────────────┐
                               │ Inscription  │ vĩnh viễn, sống lâu hơn Challenge
                               └──────────────┘
```

## Các thực thể và một câu định nghĩa

| Thực thể | Một câu | Chi tiết |
|---|---|---|
| `SkillAtom` | Đơn vị năng lực nhỏ nhất quan sát được bằng hành vi | [11](11-skill-genome.md) |
| `SkillGenome` | Đồ thị atom + tiên quyết của một lĩnh vực | [11](11-skill-genome.md) |
| `Challenge` | Một ngưỡng cửa: việc thật có rubric, gác cổng, bằng chứng | [12](12-challenge.md) |
| `Rubric` | Tiêu chí chấm dưới dạng **dữ liệu**, không phải code | [13](13-assessment.md) |
| `ExpertTrace` | Cách một người giỏi *quyết định* — không phải lời giải | [13](13-assessment.md) |
| `Attempt` | Một lần người học đi qua một ngưỡng | [12](12-challenge.md) |
| `ProcessTrace` | Dấu vết cách người học làm, ký số tại nguồn | [13](13-assessment.md) |
| `PreAssessment` | Tóm tắt của AI cho gác cổng — chỉ đọc, không điểm | [13](13-assessment.md) |
| `DefenseSession` | Phiên hỏi "vì sao" — neo chính của đánh giá | [13](13-assessment.md) |
| `Verdict` | Phán quyết của người, bắt buộc có lý do viết | [13](13-assessment.md) |
| `Inscription` | Ghi danh vĩnh viễn; sống lâu hơn cả ngưỡng cửa | [16](16-governance.md) |
| `Learner` | Người đang đi trên bản đồ | [14](14-onboarding.md) |
| `Position` | θ và σ trên từng trục — luôn kèm bất định | [14](14-onboarding.md) |
| `Gatekeeper` | Một `Learner` đang giữ nhiệm kỳ gác cổng | [16](16-governance.md) |
| `Chamber` | Phòng kín của những người cùng làm một ngưỡng | [43](43-loops-user.md) |
| `Payout` | Một lần chuyển tiền, luôn gắn với một `Verdict` | [17](17-economy.md) |
| `CulturePack` | Toàn bộ lớp văn hoá, tách khỏi lõi | [18](18-culture-pack.md) |
| `Proposal` | Đơn vị công việc của agent — chỉ đề xuất | [22](22-world-kernel.md) |

## Bảy bất biến của mô hình miền

1. **`Verdict` không bao giờ do máy tạo ra.** Trường `judgedBy` luôn trỏ tới một con người.
2. **`Inscription` là bất biến và sống lâu hơn `Challenge`.** Ngưỡng chết, tên người đã qua vẫn còn.
3. **`Position` không bao giờ là một con số.** Luôn là `{θ, σ}` theo từng trục.
4. **`ProcessTrace` chỉ ghi thêm và được ký tại nguồn.** Không ai — kể cả admin — sửa được.
5. **`Rubric` là dữ liệu.** Chuyên gia phải sửa được mà không cần lập trình viên.
6. **Mọi `Payout` truy ngược được tới đúng một `Verdict`.** Không có tiền nào không có phán quyết.
7. **`SkillGenome` không có chu trình.** Cưỡng chế lúc ghi, không phải lúc đọc.

## Vòng đời và trạng thái

| Thực thể | Các trạng thái |
|---|---|
| `Challenge` | `seed → trial → live → declining → dormant → memorial` |
| `Attempt` | `initiated → in_progress → submitted → pre_assessed → defended → judged → (appealed) → closed` |
| `Gatekeeper` | `candidate → probation → active → renewed \| expired \| revoked` |
| `Learner` | `onboarding → calibrating (2 tuần) → active → dormant → returned` |
| `Proposal` | `draft → validated → arbitrated → accepted \| rejected \| merged` |
| `Asset` | `ingested → processed → in_review → approved \| quarantined → published` |

## Quy ước đặt tên

- Định danh: `<loại>.<lĩnh vực>.<tên>` — ví dụ `atom.CHAN.race-condition-duoi-tai`
- Định danh không bao giờ đổi. Đổi tên hiển thị thì được; đổi ID thì không.
- Mọi thực thể nội dung có `version` (semver) và `supersedes`.
- Mọi thực thể có `createdBy` — con người hoặc agent, luôn ghi rõ cái nào.

## Những gì cố tình **không** có trong mô hình

| Không có | Vì sao |
|---|---|
| `Course`, `Lesson`, `Module` | Đây không phải trường học |
| `Score`, `Grade`, `Rank`, `Level` | Nguyên tắc 8 — không so sánh người với người |
| `Streak`, `DailyGoal` | Điều lệ §1 |
| `Badge` tách rời năng lực | Ngoại hình và danh hiệu luôn suy ra từ năng lực đã kiểm chứng |
| `Subscription` | Tiền chảy *tới* người học, không phải từ họ |
