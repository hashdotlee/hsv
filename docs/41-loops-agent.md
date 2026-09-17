# 41 · Vòng lặp Agent

> Máy làm gì, bao lâu một lần, ra cái gì, ai duyệt. Mỗi vòng lặp có tần suất, đầu ra, và cổng kiểm định.

## Bảng tổng

| # | Vòng lặp | Agent | Tần suất | Xuất hiện |
|---|---|---|---|---|
| A1 | Khai thác kỹ năng | `miner` | hàng ngày | G2 |
| A2 | Rèn ngưỡng cửa | `smith` | hàng ngày | G2 |
| A3 | Thử vàng | `assayer` | mỗi đề xuất + 90 ngày/lần | G2 |
| A4 | Khảo hạch | `redteam` | mỗi đề xuất | G2 |
| A5 | Tiền thẩm | `scribe` | mỗi lần nộp bài | G2 |
| A6 | Trọng tài | `arbiter` | liên tục + rà tuần | G2 |
| A7 | Vẽ lại bản đồ | `cartographer` | hàng đêm | G3 |
| A8 | Coi kho | `curator` | hàng tuần | G3 |
| A9 | Mai mối | `matchmaker` | mỗi lần nhập môn | G3 |
| A10 | Gác an ninh | `sentinel` | liên tục | G3 |
| A11 | Quản gia | `steward` | hàng ngày + báo cáo tuần | G2 |
| A12 | Giữ chuyện | `lorekeeper` | hàng tuần | G4 |
| A13 | Xưởng vẽ | `atelier` | theo yêu cầu | G4 |

---

## A1 · Khai thác kỹ năng

```
đọc nguồn mới (postmortem, issue, tài liệu, bản ghi phỏng vấn, KHO LÝ DO CHẾT)
  → tìm năng lực chưa có trong genome
  → viết mastery_signal quan sát được
  → suy ra tiên quyết và lỗi điển hình
  → trích dẫn ≥ 2 nguồn độc lập
  → nộp đề xuất
```

**Cổng:** INV-03 (quan sát được) · INV-05 (không chu trình) · INV-06 (không trùng) · có nguồn thật.
**Hay hỏng ở:** bịa nguồn. Kiểm tra nguồn tồn tại thật là bắt buộc, không phải tuỳ chọn.

## A2 · Rèn ngưỡng cửa

```
nhận atom + vết chuyên gia + mẫu thiết kế
  → viết kịch bản với BIẾN ẨN
  → viết rubric có ít nhất một điều kiện veto
  → viết thang gợi ý theo chữ ký lỗi
  → viết on_fail.next_step
  → gọi A3 tự đo
  → nộp
```

**Hay hỏng ở:** viết đề bài quá đầy đủ và rõ ràng — đúng thứ làm ngưỡng cửa mất giá trị. Prompt phải ép ngược xu hướng này.

## A3 · Thử vàng

```
lấy ngưỡng cửa → cho mô hình mạnh nhất tự giải N lần, không người can thiệp
  → đo tỉ lệ qua, thời gian, kiểu thất bại
  → ≥ 0.25 ⇒ trả về cho A2 kèm chẩn đoán "vì sao máy qua được"
```

Chạy lại mỗi 90 ngày cho ngưỡng `live`. **Đây là cơ chế giữ hệ thống không lạc hậu** — mô hình mạnh lên thì ngưỡng cũ tự chết và chuẩn tự nâng.

## A4 · Khảo hạch

```
với mỗi đề xuất: tìm đường tắt · thử qua bằng lối không định trước
  · tìm mâu thuẫn rubric · kiểm an toàn vị thành niên
  → phá được ⇒ loại, uy tín agent sinh giảm, uy tín redteam tăng
```

Cần **≥ 2 `redteam` dùng mô hình của 2 nhà cung cấp khác nhau** — cùng mô hình sinh và kiểm thì cùng điểm mù.

## A5 · Tiền thẩm

```
vết được nộp → dựng dòng thời gian · đánh dấu ngõ cụt
  · đếm tương tác AI và phân loại (tin ngay / bác bỏ / kiểm chứng)
  · ghi quan sát theo từng tiêu chí rubric, KÈM TRÍCH DẪN
  · soạn câu hỏi gợi ý cho phiên bảo vệ
  → KHÔNG điểm, KHÔNG kết luận
```

**Vòng học ngược:** gác cổng đánh dấu chỗ không đồng ý → dữ liệu hiệu chỉnh cho A5.

## A6 · Trọng tài

```
liên tục: xử mâu thuẫn từng đề xuất
hàng tuần: rà toàn cục
  → đo đa dạng ngữ nghĩa
  → tìm cụm nội dung na ná nhau
  → tìm vùng phình nhanh hơn lượng người thật
  → đề xuất gộp / vạch ranh giới / tạm đóng sinh ở vùng đó
```

**Chỉ phán xử, không sinh.** Tách quyền sinh khỏi quyền duyệt.

## A7 · Vẽ lại bản đồ

```
hàng đêm: đọc telemetry → tính lại height/roughness/traffic/fog/biome
  → hàm thuần (INV-13), cùng dữ liệu cho cùng địa hình
  → so với bản cũ; thay đổi lớn thì báo cho người
```

Địa hình đổi đột ngột là tín hiệu — hoặc dữ liệu hỏng, hoặc có gì đó thật sự thay đổi.

## A8 · Coi kho

```
hàng tuần: quét mọi ngưỡng live
  → so với tín hiệu suy tàn
  → đề xuất chuyển declining, KÈM LÝ DO
  → lý do vào kho lý do chết → đầu vào bắt buộc cho A1, A2 lứa sau
```

Đây là cách dàn agent **học** thay vì chỉ chạy.

## A9 · Mai mối

```
người học nhập môn → tìm người khác đang làm cùng ngưỡng
  → ghép theo chữ ký lỗi KHÁC NHAU (bổ sung nhau)
  → tránh ghép người giống hệt (cùng bí một chỗ)
  → tránh ghép vị thành niên với người lớn không qua sàng lọc
```

## A10 · Gác an ninh

```
liên tục: cụm gác cổng–người học lặp lại · tỉ lệ đạt vọt sau khi có tiền
  · nhiều tài khoản cùng thiết bị · vết có dấu hiệu dán ghép
  → BÁO ĐỘNG CHO NGƯỜI. Không bao giờ tự xử lý.
```

Cáo buộc gian lận là quyết định có hậu quả với đời thật của một con người.

## A11 · Quản gia

```
hàng ngày: chi phí · hạn ngạch · độ sâu hàng đợi duyệt · tỉ lệ chấp nhận theo agent
hàng tuần: báo cáo cho người điều hành
  → chỉ số then chốt: chi phí mỗi đề xuất được chấp nhận VÀ còn sống sau 30 ngày
  → chỉ số này phải GIẢM theo thời gian
```

Agent cần sớm nhất sau `arbiter`. Không có nó, bạn không biết dàn agent đang đốt tiền tạo rác cho tới khi hoá đơn về.

## A12 · Giữ chuyện

```
hàng tuần: sinh lore, NPC, câu đố ven đường cho vùng mới
  → BẮT BUỘC nhãn [sử liệu] / [truyền thuyết] / [hư cấu] (INV-07)
  → nội dung văn hoá ⇒ hội đồng văn hoá duyệt
```

## A13 · Xưởng vẽ

```
theo yêu cầu: từ asset ĐÃ DUYỆT → biến thể hoa văn, texture, sprite
  → không bao giờ dùng nguồn chưa rõ quyền (INV-12)
  → mọi đầu ra ghi vào PROVENANCE.json
```

---

## Cổng chung cho mọi vòng lặp

Không vòng lặp nào bỏ qua được:

```
1. đề xuất qua Agent Gateway (không ghi thẳng)
2. kiểm bất biến hiến pháp
3. kiểm trùng ngữ nghĩa
4. kiểm hạn ngạch
5. vùng trưởng thành quyết định mức duyệt của người
6. ghi sự kiện với Actor đầy đủ (kể cả mô hình và phiên bản prompt)
```

## Nhịp

```
liên tục   A6(xử) · A10
mỗi sự kiện A3 · A4 · A5 · A9
hàng ngày   A1 · A2 · A11
hàng đêm    A7
hàng tuần   A6(rà) · A8 · A12 · báo cáo A11
90 ngày     A3 đo lại toàn bộ ngưỡng live
```
