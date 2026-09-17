# 40 · Lộ trình G0 → G5

> Mỗi giai đoạn có **tiêu chí hoàn thành cứng**. Không sang giai đoạn sau khi chưa đạt — kể cả khi sốt ruột. Mỗi giai đoạn cũng có **tiêu chí dừng**: dấu hiệu nên dừng lại và nghĩ lại.

| | Giai đoạn | Câu hỏi cần trả lời | Người | Thời lượng ước tính |
|---|---|---|---|---|
| **G0** | Một người | Ngưỡng cửa có đo được năng lực thật không? | 1 (chính bạn) | 3–6 tuần |
| **G1** | Mười người | Người lạ có đi được không? | ~10 + 2 gác cổng | 2–3 tháng |
| **G2** | Vòng tự tái tạo | Hệ thống có tự sinh gác cổng và nội dung không? | ~50 | 3–4 tháng |
| **G3** | Tiền thật | Tiền có làm hỏng đánh giá không? | ~150 | 3–4 tháng |
| **G4** | Thế giới | Bản đồ và dàn agent có sống được không? | ~500 | 4–6 tháng |
| **G5** | Mở nguồn | Cộng đồng có tiếp quản được không? | mở | liên tục |

---

## G0 — Một người

**Bạn là người dùng đầu tiên.** Đây không phải khẩu hiệu — nó là phương pháp kiểm chứng rẻ nhất và trung thực nhất.

### Làm gì

1. Viết tay **5 ngưỡng cửa** ở 5 trục khác nhau (bỏ TRUYỀN — chưa có ai để dạy)
2. Xây `@hsv/trace` — ghi vết, ký, phát lại
3. Xây `@hsv/genome` — 15–25 atom quanh 5 ngưỡng đó
4. Xây `@hsv/rubric` — chấm từ quan sát
5. **Tự đi cả 5 ngưỡng**, ghi vết đầy đủ
6. Nhờ 1 người bạn kỹ sư làm gác cổng, bảo vệ thật trước họ
7. **Đưa cả 5 ngưỡng cho mô hình mạnh nhất tự giải**

### Hoàn thành khi

- [ ] 5 ngưỡng, mỗi ngưỡng có rubric đầy đủ và ít nhất 1 biến ẩn
- [ ] `ai_solo_pass_rate < 0.25` ở **cả 5**
- [ ] Bạn tự đi hết, có vết ký hợp lệ, phát lại được
- [ ] 5 phiên bảo vệ thật đã diễn ra
- [ ] Gác cổng nói được: "tôi biết người này hiểu hay không, từ bằng chứng, không từ cảm giác"
- [ ] Ít nhất 1 ngưỡng bạn **không qua** — và điều đó dạy bạn điều gì đó về thiết kế

### Dừng lại nếu

- Không viết nổi ngưỡng nào mà mô hình không tự qua ⇒ luận điểm cốt lõi sai, cần nghĩ lại toàn bộ
- Bạn thấy chán khi tự đi ⇒ người khác cũng sẽ chán
- Phiên bảo vệ không cho biết thêm gì so với đọc sản phẩm ⇒ neo chính không hoạt động

### Không làm ở G0

Bản đồ · 3D · thiết kế · tiền · agent · culture pack · onboarding · đăng ký tài khoản. Dùng file YAML và terminal.

---

## G1 — Mười người

Người lạ đầu tiên. **Chế độ vị thành niên bắt buộc có đầy đủ** ([`29-minors.md`](29-minors.md)).

### Làm gì

- Ứng dụng web tối thiểu: đăng nhập · danh sách ngưỡng · nộp bài · phiên bảo vệ · phán quyết
- `@hsv/rite`, `@hsv/chronicle`, `@hsv/crucible`, `@hsv/net`, `@hsv/ui`, `@hsv/tokens`
- Chế độ vị thành niên đầy đủ
- Onboarding thô: chỉ tự khai + ý định, chưa thăm dò thích ứng
- 10–15 ngưỡng, 2 gác cổng ngoài bạn
- Đường kháng nghị hoạt động

### Hoàn thành khi

- [ ] 10 người lạ hoàn thành ≥ 1 ngưỡng
- [ ] ≥ 6/10 quay lại làm ngưỡng thứ hai **không cần nhắc**
- [ ] 2 gác cổng chấm độc lập, độ nhất quán ≥ 0,7
- [ ] Ít nhất 1 người vị thành niên đi hết luồng an toàn
- [ ] Ít nhất 1 kháng nghị được nộp và xử
- [ ] Không sự cố an toàn nào
- [ ] Danh mục kiểm tra an ninh G1 đã qua ([`28-security-safety.md`](28-security-safety.md))

### Dừng lại nếu

- Dưới 4/10 quay lại ⇒ trải nghiệm chưa đủ giá trị, sửa trước khi mở rộng
- Gác cổng kiệt sức với 10 người ⇒ nút thắt xuất hiện quá sớm, phải giải trước

---

## G2 — Vòng tự tái tạo

Câu hỏi: hệ thống có tự sinh ra gác cổng và nội dung mới không, hay mọi thứ vẫn phụ thuộc vào bạn?

### Làm gì

- **Trục TRUYỀN**: ngưỡng "dẫn một người qua"; người học đầu tiên thành gác cổng
- Dàn agent đầu: `miner`, `smith`, `assayer`, `arbiter`, `redteam`, `scribe`, `steward`
- Lõi Thế Giới + Agent Gateway + hiến pháp cưỡng chế được
- `@hsv/oracle` — trung lập nhà cung cấp
- `@hsv/cartograph` — bản đồ 2D đầu tiên (dữ liệu bắt đầu đủ để địa hình có nghĩa)
- `@hsv/pack` + gói `vi-VN` tối thiểu
- Onboarding thăm dò thích ứng (mục hỏi hiệu chỉnh bằng tay)

### Hoàn thành khi

- [ ] ≥ 3 gác cổng **xuất thân từ người học của hệ thống**
- [ ] ≥ 30% ngưỡng đang `live` do agent đề xuất và người duyệt
- [ ] ≥ 1 ngưỡng đã chết đúng quy trình, có ghi lý do
- [ ] Chỉ số trôi dạt ổn định trong 8 tuần liên tiếp
- [ ] Chi phí mỗi đề xuất chấp nhận & sống 30 ngày **giảm** trong 3 tháng liên tiếp
- [ ] Bản đồ 2D phản ánh đúng trực giác của gác cổng về vùng nào khó

### Dừng lại nếu

- Agent sinh nhiều mà người học không đi qua ⇒ đang tạo rác; siết hạn ngạch, sửa công thức uy tín
- Không ai muốn làm gác cổng ⇒ trục TRUYỀN hoặc trả công có vấn đề

---

## G3 — Tiền thật

Câu hỏi nguy hiểm nhất: **tiền có làm hỏng đánh giá không?**

### Làm gì

- `@hsv/ledger`, ký quỹ, cổng thanh toán, sổ cái công khai
- Quỹ giám hộ cho vị thành niên
- 1–3 nhà tài trợ thật (bắt đầu bằng vốn của bạn để chứng minh cơ chế)
- `sentinel`, `curator`, `matchmaker`
- Chấm chéo bắt buộc trên ngưỡng có tiền
- `@hsv/forge` — asset pipeline

### Hoàn thành khi

- [ ] ≥ 100 lần chi trả thành công
- [ ] Tỉ lệ đạt **không tăng** sau khi ngưỡng có tiền (kiểm định thống kê, không cảm tính)
- [ ] Không vụ thông đồng nào được xác nhận, hoặc có và đã phát hiện + xử đúng quy trình
- [ ] Sổ cái công khai đối chiếu khớp 100% với cổng thanh toán
- [ ] CPQ < 6.000.000đ
- [ ] ≥ 60% tiền tới tay người học
- [ ] Quỹ Bảo tồn nhận đủ ≥ 50% phí nền tảng

### Dừng lại nếu

- **Tỉ lệ đạt tăng vọt sau khi có tiền** ⇒ dừng chi trả ngay, điều tra, không mở tiếp
- Người học bắt đầu nói về tiền nhiều hơn về năng lực ⇒ số tiền quá lớn so với ý nghĩa

---

## G4 — Thế giới

### Làm gì

- Bản đồ 3D (tuỳ chọn; 2D vẫn mặc định)
- `@hsv/atlas` — ngoại hình từ năng lực; `@hsv/glyph` — hoa văn
- `lorekeeper`, `atelier`, `envoy`
- Culture pack `vi-VN` đầy đủ: lore, nghi thức, lịch, âm thanh
- Nghi thức đỉnh tối cao; người đầu tiên lên đỉnh
- Agent bên ngoài cài được không cần sửa lõi

### Hoàn thành khi

- [ ] ≥ 1 người lên đỉnh tối cao qua đầy đủ điều kiện (gồm đã dẫn 3 người qua)
- [ ] ≥ 1 agent bên ngoài chạy ổn định
- [ ] Hội đồng văn hoá đã dùng quyền phủ quyết ít nhất một lần
- [ ] Bản đồ 2D vẫn chứa đầy đủ mọi thông tin của bản 3D
- [ ] Ngân sách hiệu năng vẫn đạt trên thiết bị mục tiêu

---

## G5 — Mở nguồn &amp; cộng đồng

### Làm gì

- Mở toàn bộ mã: Apache-2.0 (mã) + CC BY-SA 4.0 (nội dung)
- Quy trình đóng góp, quản trị, ADR công khai
- Gói văn hoá thứ hai do **cộng đồng khác** làm
- Bàn giao dần quyền cho các hội đồng

### Hoàn thành khi

- [ ] ≥ 10 người đóng góp ngoài đội gốc
- [ ] Gói văn hoá thứ hai do cộng đồng của nền văn hoá đó làm
- [ ] Hội đồng gác cổng đã sửa điều lệ ít nhất một lần **không cần bạn**
- [ ] Hệ thống chạy được 30 ngày không có bạn can thiệp

---

## Nguyên tắc xuyên suốt

1. **Đừng xây gói nào trước khi có thứ cần dùng nó.** Rủi ro lớn nhất là 18 tháng hạ tầng đẹp và không có người học nào.
2. **Không sang giai đoạn sau khi tiêu chí hoàn thành chưa đạt.** Tiêu chí tồn tại để chống chính sự sốt ruột của bạn.
3. **Chế độ vị thành niên và đường kháng nghị không bao giờ bị hoãn.**
4. **Mỗi giai đoạn phải có ít nhất một thứ bị vứt bỏ.** Không vứt gì nghĩa là không học được gì.
