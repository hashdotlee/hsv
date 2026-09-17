# Human Skill Vault — Kho Năng Lực Người

> Nơi duy nhất chứng nhận rằng bạn **hiểu**, chứ không phải bạn **có quyền truy cập vào một mô hình**.

Một hệ thống bảo tồn và phát triển năng lực con người dưới dạng **thử thách thực tế**, vận hành như một **nghi thức** có bản sắc Việt Nam, với **gác cổng là người đi trước**, **tiền thật trả cho năng lực đã kiểm chứng**, và một **thế giới tự nở** do nhiều AI agent cùng dựng dưới sự điều tiết của con người.

Nghề mở màn: **kỹ sư phần mềm / AI** — chính nghề đang bị AI thay thế nhanh nhất.

---

## Tài liệu này dùng thế nào

Đây là **kim chỉ nam**, không phải tài liệu tham khảo. Đọc theo thứ tự nếu bạn mới vào; tra theo mục nếu bạn đang làm.

| Nếu bạn định… | Đọc |
|---|---|
| Hiểu vì sao dự án tồn tại | [`docs/01-vision.md`](docs/01-vision.md), [`CHARTER.md`](CHARTER.md) |
| Bắt đầu viết code tuần này | [`docs/40-roadmap.md`](docs/40-roadmap.md) § G0, [`docs/21-packages.md`](docs/21-packages.md) |
| Thiết kế một thử thách | [`docs/12-challenge.md`](docs/12-challenge.md), [`docs/13-assessment.md`](docs/13-assessment.md) |
| Cắm một AI agent vào | [`docs/23-agent-platform.md`](docs/23-agent-platform.md), [`WORLD-CONSTITUTION.yaml`](WORLD-CONSTITUTION.yaml) |
| Thêm một nền văn hoá | [`docs/18-culture-pack.md`](docs/18-culture-pack.md) |
| Đưa ảnh/tài liệu vào hệ thống | [`docs/25-asset-pipeline.md`](docs/25-asset-pipeline.md) |
| Quyết định có nên thêm thư viện ngoài | [`DEPENDENCY-CONSTITUTION.md`](DEPENDENCY-CONSTITUTION.md) |
| Hiểu ai quyết định cái gì | [`docs/16-governance.md`](docs/16-governance.md), [`docs/42-loops-human.md`](docs/42-loops-human.md) |

---

## Mục lục đầy đủ

### Gốc — luật bất biến
- [`CHARTER.md`](CHARTER.md) — Điều lệ: cam kết với người học. Sửa cần đồng thuận, không sửa theo sprint.
- [`DEPENDENCY-CONSTITUTION.md`](DEPENDENCY-CONSTITUTION.md) — Hiến pháp phụ thuộc: tự viết gì, đi thuê gì.
- [`WORLD-CONSTITUTION.yaml`](WORLD-CONSTITUTION.yaml) — Bất biến máy đọc được; mọi đề xuất của agent bị kiểm tra ngược lại nó.
- [`GLOSSARY.md`](GLOSSARY.md) — Từ điển thuật ngữ VI/EN. **Đọc trước mọi thứ khác.**

### Nền tảng
- [`docs/01-vision.md`](docs/01-vision.md) — Tầm nhìn, định vị, nghịch lý trung tâm
- [`docs/02-principles.md`](docs/02-principles.md) — 12 nguyên tắc thiết kế bất biến

### Mô hình miền
- [`docs/10-domain-model.md`](docs/10-domain-model.md) — Bản đồ khái niệm và quan hệ
- [`docs/11-skill-genome.md`](docs/11-skill-genome.md) — Skill atom, đồ thị, sáu trục, suy giảm
- [`docs/12-challenge.md`](docs/12-challenge.md) — Ngưỡng cửa: cấu trúc, vòng đời, spec
- [`docs/13-assessment.md`](docs/13-assessment.md) — Bằng chứng, rubric, phiên bảo vệ, AI đối kháng
- [`docs/14-onboarding.md`](docs/14-onboarding.md) — Định vị người học khi vào cửa
- [`docs/15-map-and-progression.md`](docs/15-map-and-progression.md) — Bản đồ, địa hình, sương mù, lộ trình
- [`docs/16-governance.md`](docs/16-governance.md) — Gác cổng, hội đồng, kháng nghị
- [`docs/17-economy.md`](docs/17-economy.md) — Dòng tiền, ký quỹ, tài trợ, chống gian lận
- [`docs/18-culture-pack.md`](docs/18-culture-pack.md) — Hợp đồng culture pack, vi-VN
- [`docs/19-design-system.md`](docs/19-design-system.md) — Cartographic Ritual

### Kỹ thuật
- [`docs/20-architecture.md`](docs/20-architecture.md) — Kiến trúc tổng thể, monorepo, dịch vụ
- [`docs/21-packages.md`](docs/21-packages.md) — 16 gói tự xây, đặc tả từng gói
- [`docs/22-world-kernel.md`](docs/22-world-kernel.md) — Event sourcing, đề xuất, trọng tài
- [`docs/23-agent-platform.md`](docs/23-agent-platform.md) — Gateway, manifest, giao thức phối hợp
- [`docs/24-agent-catalog.md`](docs/24-agent-catalog.md) — Danh mục 14 agent, phạm vi và quyền hạn
- [`docs/25-asset-pipeline.md`](docs/25-asset-pipeline.md) — Ảnh/tài liệu → asset import được
- [`docs/26-data-and-storage.md`](docs/26-data-and-storage.md) — Lưu trữ, sự kiện, xuất dữ liệu
- [`docs/27-api-contracts.md`](docs/27-api-contracts.md) — Hợp đồng API

### Ràng buộc
- [`docs/28-security-safety.md`](docs/28-security-safety.md) — An ninh, riêng tư, sandbox
- [`docs/29-minors.md`](docs/29-minors.md) — Chế độ vị thành niên (bắt buộc từ G1)
- [`docs/30-accessibility-i18n.md`](docs/30-accessibility-i18n.md) — Tiếp cận, băng thông thấp, đa ngôn ngữ

### Vận hành & lộ trình
- [`docs/40-roadmap.md`](docs/40-roadmap.md) — Sáu giai đoạn G0→G5, tiêu chí hoàn thành
- [`docs/41-loops-agent.md`](docs/41-loops-agent.md) — Agent loop
- [`docs/42-loops-human.md`](docs/42-loops-human.md) — Human loop
- [`docs/43-loops-user.md`](docs/43-loops-user.md) — User loop
- [`docs/44-metrics.md`](docs/44-metrics.md) — Chỉ số: cái nào thật, cái nào dối
- [`docs/45-risks.md`](docs/45-risks.md) — Sổ rủi ro
- [`docs/50-contributing.md`](docs/50-contributing.md) — Đóng góp: code, ngưỡng cửa, văn hoá
- [`docs/51-cli.md`](docs/51-cli.md) — Giao diện dòng lệnh `hsv` (đặc tả)
- [`docs/52-runbooks.md`](docs/52-runbooks.md) — Quy trình vận hành khi có sự cố
- [`docs/adr/`](docs/adr/README.md) — Nhật ký quyết định kiến trúc (ADR-0001…0010)

---

## Sửa tài liệu này thế nào

| Loại | Quy trình |
|---|---|
| Điều lệ, bất biến, an toàn vị thành niên | Thảo luận công khai 30 ngày + 2/3 hội đồng gác cổng ([`docs/52-runbooks.md`](docs/52-runbooks.md) RB-12) |
| Quyết định kiến trúc | ADR mới trong [`docs/adr/`](docs/adr/README.md); ADR cũ không xoá, đánh dấu `Superseded` |
| Mọi thứ khác | PR thường ([`docs/50-contributing.md`](docs/50-contributing.md)) |

**Phân biệt hai loại nội dung trong tài liệu này:** những gì nằm trong `CHARTER.md`, `WORLD-CONSTITUTION.yaml` và `docs/02-principles.md` là **nguyên tắc bất biến**; mọi thứ còn lại — stack, tên gói, schema, ngưỡng số — là **lựa chọn triển khai**, sửa được khi có bằng chứng tốt hơn.

---

## Trạng thái hiện tại

| Giai đoạn | Trạng thái |
|---|---|
| **G0 — Một người (chính bạn)** | ⬜ chưa bắt đầu |
| G1 — Mười người | ⬜ |
| G2 — Vòng tự tái tạo | ⬜ |
| G3 — Tiền thật | ⬜ |
| G4 — Thế giới | ⬜ |
| G5 — Mở nguồn & cộng đồng | ⬜ |

## Việc kế tiếp

Xem [`docs/40-roadmap.md`](docs/40-roadmap.md) § G0. Tóm tắt:

1. Viết 5 ngưỡng cửa đầu tiên (YAML) — một cho mỗi trục trừ TRUYỀN
2. Dựng `@hsv/trace` — ghi vết quá trình
3. Dựng `@hsv/genome` + `@hsv/rubric`
4. Tự đi ngưỡng đầu tiên
5. Chạy kiểm tra "AI một mình có qua được không" cho cả 5 ngưỡng

## Giấy phép

- `packages/` — mở nguồn (đề xuất Apache-2.0)
- `content/` — CC-BY-SA, trừ nội dung có ràng buộc riêng ghi trong `PROVENANCE.json`
- `cultures/` — theo giấy phép từng asset; xem [`docs/25-asset-pipeline.md`](docs/25-asset-pipeline.md)
