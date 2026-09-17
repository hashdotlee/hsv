# 50 · Đóng góp

> Mã nguồn mở theo mặc định. Nhưng "mở" không có nghĩa là mọi thứ đều bỏ phiếu — có những thứ không thương lượng.

## Giấy phép

| Phần | Giấy phép |
|---|---|
| Mã (`packages/`, `apps/`) | Apache-2.0 |
| Nội dung (`content/`, `docs/`) | CC BY-SA 4.0 |
| Culture pack | CC BY-SA 4.0 + điều kiện của cộng đồng nguồn |
| Asset bên thứ ba | Theo giấy phép gốc, ghi trong `PROVENANCE.json` |

Apache-2.0 cho mã (có điều khoản bằng sáng chế). CC BY-SA cho nội dung (ai dùng lại phải mở lại).

---

## Bốn loại đóng góp

### 1. Nội dung — dễ vào nhất, giá trị cao nhất

Ngưỡng cửa, atom, vết chuyên gia. **Đây là thứ dự án cần nhất.**

```
1. Đọc 11-skill-genome.md và 12-challenge.md
2. Viết YAML theo schema
3. Tự chạy: hsv validate content/challenges/<file>.yaml
4. Chạy assayer: hsv assay <id>     → ai_solo_pass_rate < 0.25
5. Mở PR, gán nhãn `content`
6. Một gác cổng của vùng đó duyệt
```

Ngưỡng cửa không có rubric, không có biến ẩn, hoặc chưa đo `ai_solo_pass_rate` sẽ bị đóng ngay, không tranh luận.

**Vết chuyên gia** là đóng góp quý nhất và hiếm nhất: bạn làm một việc khó, ghi lại đầy đủ quá trình, **kể cả những ngõ cụt và những thứ bạn đã cân nhắc rồi bỏ**. Điểm quyết định và lý do từ chối phương án khác quan trọng hơn lời giải đúng.

### 2. Mã

```
packages/  → cần test tính chất cho bất biến
apps/      → cần đạt ngân sách hiệu năng
agents/    → cần manifest hợp lệ + self-test
```

Tiêu chuẩn: TypeScript nghiêm ngặt, không `any` · tầng 0 không phụ thuộc thời gian chạy · lint và typecheck sạch · có test.

### 3. Culture pack

Gói văn hoá mới **phải do người thuộc nền văn hoá đó dẫn dắt.** Không phải quy tắc lịch sự — nó là điều kiện để gói được chấp nhận.

```
1. Lập hội đồng văn hoá (≥ 3 người thuộc cộng đồng đó)
2. Đọc 18-culture-pack.md
3. Bắt đầu từ cấu trúc, không phải màu sắc:
   nền văn hoá này công nhận năng lực bằng cách nào?
4. Mọi asset có xuất xứ và quyền rõ ràng
5. Hội đồng của bạn có quyền phủ quyết với gói của chính mình, vĩnh viễn
```

### 4. Agent

```
1. Viết dịch vụ nói giao thức đề xuất (23-agent-platform.md)
2. Nộp agent.yaml — scope.write PHẢI rỗng
3. Chạy thử 2 tuần: mọi đề xuất qua Trọng tài, hạn ngạch thấp
4. Đạt uy tín → chế độ bình thường
```

Lõi không sửa một dòng nào để nhận agent của bạn.

---

## Không thương lượng

Bốn nhóm này không thay đổi bằng PR:

1. **Điều lệ** ([`CHARTER.md`](../CHARTER.md)) — sửa qua quy trình H10: thảo luận công khai 30 ngày + 2/3 hội đồng gác cổng
2. **Bất biến** (`WORLD-CONSTITUTION.yaml` INV-01…14) — như trên
3. **An toàn vị thành niên** — chỉ siết chặt hơn, không nới ra
4. **`scope.write` của agent luôn rỗng** — không có ngoại lệ, kể cả agent của người sáng lập

PR đụng vào bốn nhóm này sẽ bị đóng kèm liên kết tới quy trình sửa điều lệ.

---

## Quy trình PR

```
1. Vấn đề trước, mã sau — mở issue để thống nhất hướng
2. Một PR một việc
3. Mô tả: VÌ SAO, không chỉ CÁI GÌ
4. CI phải xanh: lint · typecheck · test · schema · bất biến · ngân sách hiệu năng
5. Review: mã cần 1 người bảo trì · nội dung cần 1 gác cổng vùng
              văn hoá cần hội đồng · điều lệ cần 2/3 hội đồng
```

## ADR

Quyết định có tác động dài hạn cần một ADR trong [`adr/`](adr/):

```
Cần ADR khi:      thêm phụ thuộc tầng 1 · đổi mô hình miền
                  · đổi mô hình đánh giá · đổi cơ chế tiền
                  · đổi quyền của agent · đổi mô hình lưu trữ
Không cần ADR:    sửa lỗi · thêm nội dung · sửa giao diện · tài liệu
```

Thêm phụ thuộc tầng 1 cần **ADR riêng và PR riêng** kèm kế hoạch thoát ([`DEPENDENCY-CONSTITUTION.md`](../DEPENDENCY-CONSTITUTION.md)).

---

## Quản trị

| Vai | Quyền | Chọn ra sao |
|---|---|---|
| Người bảo trì | Gộp PR mã | Đóng góp liên tục, hội đồng mời |
| Gác cổng vùng | Duyệt nội dung vùng đó | Đã qua ngưỡng + trục TRUYỀN, nhiệm kỳ 6 tháng |
| Hội đồng văn hoá | Phủ quyết nội dung văn hoá | Do cộng đồng nguồn chọn |
| Hội đồng an toàn | Xử sự cố | Hội đồng bổ nhiệm |
| Hội đồng gác cổng | Sửa điều lệ (2/3) | Gác cổng đang tại nhiệm |
| Người sáng lập | Dừng hệ thống, dừng chi tiêu, ngắt agent | — |

**Người sáng lập không có:** quyền lật phán quyết chuyên môn · quyền ép nội dung văn hoá · quyền bỏ qua hội đồng an toàn · quyền phủ quyết sửa điều lệ.

---

## Quy tắc ứng xử

Hệ thống tồn tại để **mở rộng năng lực con người**. Hành vi làm người khác thấy mình nhỏ bé đi thì đi ngược lại mục đích đó.

Cụ thể: phê bình sản phẩm, không phê bình người · giả định người kia đang học · nhớ có người 14 tuổi trong không gian này · không ai phải chứng minh mình xứng đáng được tôn trọng.

Vi phạm: cảnh cáo → tạm đình chỉ → loại. Có đường kháng nghị.

---

## Bắt đầu từ đâu

| Bạn là | Bắt đầu ở |
|---|---|
| Kỹ sư có kinh nghiệm | Viết một vết chuyên gia cho việc bạn giỏi nhất |
| Kỹ sư mới | Đi một ngưỡng, rồi báo cáo chỗ nào trải nghiệm dở |
| Nhà thiết kế | `@hsv/ui`, hoặc trạng thái "chưa đạt" (màn hình quan trọng nhất sản phẩm) |
| Người làm văn hoá | Xuất xứ asset, hoặc gói văn hoá mới |
| Nhà nghiên cứu giáo dục | Phản biện mô hình đánh giá trong [`13-assessment.md`](13-assessment.md) |
| Người làm AI | Viết một agent, hoặc phá ngưỡng cửa đang có (Red Team) |

Đóng góp có giá trị nhất mà **không cần viết dòng mã nào**: đi một ngưỡng cửa, và nói thật chỗ nào nó không đo đúng thứ nó tưởng nó đang đo.
