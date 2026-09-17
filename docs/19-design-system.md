# 19 · Hệ thống thiết kế — "Nghi thức bản đồ"

> Thẩm mỹ: **bản đồ hàng hải cổ gặp bia đá**. Không phải giao diện ứng dụng học tập.

## Hai chủ đề

| | **Night** | **Field** |
|---|---|---|
| Dùng cho | Hành trình, bản đồ, thử thách | Hồ sơ, chứng nhận, in giấy, dùng ngoài nắng |
| Nền | Mực đen xanh sâu | Giấy dó ngà |
| Cảm giác | Đang đi trong đêm với ngọn đèn | Bản khắc đã hoàn thành |

Cả hai đều phải đạt WCAG AA. Field phải in ra giấy đen trắng vẫn đọc được — chứng nhận phải tồn tại ngoài màn hình.

## Token

Chuẩn **W3C Design Tokens**. Một nguồn sự thật, xuất ra mọi đích.

```
color.tokens.json ─┐
typography  ───────┼─► @hsv/tokens ─┬─► variables.css
space       ───────┤                ├─► tokens.ts
motion      ───────┘                ├─► three.palette.json
                                    └─► print.css
```

### Nhóm token

```
color.ink.{900,800,700,…}        mực — nền tối, nét
color.paper.{100,200,300}        giấy — nền sáng
color.axis.{nen,chan,dung,tham,nguoi,truyen}
color.state.{locked,available,in-progress,passed,failed,dormant,memorial}
color.fog.{learner,world}        HAI loại sương, phải phân biệt được
color.money.{escrow,paid,pending}
space.{0..12}                    thang 4px
type.display / type.ui / type.data
motion.{instant,quick,considered,ritual}
```

**Quy tắc token:**
- Không màu nào xuất hiện trong code ngoài token.
- Token trục và token trạng thái **không bao giờ trùng nhau** — người học phải phân biệt "đây là trục CHẨN" với "đây là đã qua".
- Mọi cặp nền/chữ phải qua kiểm tương phản trong CI.

### Chữ

| Vai | Đặc điểm | Dùng |
|---|---|---|
| `display` | Có chân, gợi bia khắc | Tên ngưỡng, ghi danh, nghi thức |
| `ui` | Không chân, dấu tiếng Việt chuẩn | Toàn bộ giao diện |
| `data` | Đều nét | Code, vết, số liệu, tiền |

Bắt buộc: dấu tiếng Việt phải đúng ở **mọi** trọng lượng. Kiểm tra thật với `ề ữ ỵ ẫ ộ` — rất nhiều font nổi tiếng hỏng ở đây.

### Chuyển động

Chuyển động mang nghĩa nghi thức, không phải trang trí:

| Token | Thời lượng | Dùng cho |
|---|---|---|
| `instant` | 0ms | Phản hồi trực tiếp |
| `quick` | 120ms | Trạng thái giao diện |
| `considered` | 400ms | Di chuyển trên bản đồ, sương tan |
| `ritual` | 1200ms | Nhập môn, ghi danh, lên đỉnh |

`ritual` chỉ dùng cho khoảnh khắc thật sự quan trọng. Dùng nhiều thì mất thiêng.
Tôn trọng `prefers-reduced-motion` tuyệt đối.

---

## Thành phần

### Nguyên tử
`Nút` · `Nhãn trục` · `Chấm trạng thái` · `Thanh bất định` (hiện σ) · `Số tiền` · `Nhãn nội dung` (`[sử liệu]`/`[truyền thuyết]`/`[hư cấu]`)

### Phân tử
- **Thẻ ngưỡng cửa** — thành phần quan trọng thứ hai sau nguyên tử. Dựng nó sớm: nó ép bạn quyết định gần như mọi thứ quan trọng của sản phẩm (độ khó hiện thế nào, tiền hiện thế nào, gác cổng hiện thế nào, chưa đạt hiện thế nào).
- **Chân dung gác cổng** — mặt, nhiệm kỳ, số người đã dẫn qua
- **Bảng rubric** — tiêu chí + trọng số + mức, đọc được trước khi nhập môn
- **Dòng thời gian vết** — sự kiện theo trục thời gian, ngõ cụt hiện rõ
- **Bia ghi danh** — danh sách tên khắc đá
- **Sổ cái** — dòng tiền

### Cơ quan
`Bản đồ vĩ mô` · `Bản đồ vùng` · `Phòng kín` · `Phiên bảo vệ` · `Trang phán quyết` · `Trang hồ sơ`

### Nghi thức (không phải "màn hình")
`Nhập môn` · `Nộp bài` · `Bảo vệ` · `Ghi danh` · `Vinh quy`

Nghi thức khác màn hình ở chỗ: có mở đầu, có ngắt quãng, có kết thúc, và không bỏ dở giữa chừng được bằng nút Back.

---

## Luật thiết kế

1. **Bất định luôn nhìn thấy được.** Không con số năng lực nào hiện mà không kèm σ.
2. **Không huy hiệu tách rời năng lực.** Mọi thứ hiển thị đều suy ra từ bằng chứng đã kiểm chứng.
3. **Tiền hiện rõ ràng, không rực rỡ.** Không pháo hoa, không tiếng xu rơi.
4. **Chưa đạt trông giống một ngã rẽ, không giống một cánh cửa đóng.**
5. **Bản đồ 2D là mặc định.** 3D là công tắc tuỳ chọn.
6. **Không thông báo đẩy về thành tích của người khác.**
7. **Không đếm ngược trừ khi giới hạn thời gian là một phần của thử thách.**
8. **Mọi thứ đọc được bằng trình đọc màn hình**, kể cả bản đồ (có mô tả văn bản đầy đủ).
9. **Thẩm mỹ khắc gỗ**: viền dày, mảng dẹt, không gradient — bản sắc và nhẹ cùng lúc.
10. **Chủ đề Field phải in được** trên máy in đen trắng.

---

## Cấu trúc mã

```
packages/tokens/          # nguồn token + bộ xuất (tự viết, ~600 dòng)
packages/ui/              # thành phần React, không phụ thuộc thư viện UI ngoài
  primitives/ molecules/ organisms/ rites/
packages/ui/stories/      # trang sống, render thật mọi thành phần
```

**Không dùng thư viện UI có thẩm mỹ riêng** (Material, Ant, Chakra) — chúng mang sẵn một thế giới quan sẽ chống lại hệ thống này ở mọi bước. Nếu cần nền tảng về khả năng truy cập, dùng thư viện *không kiểu dáng* và bọc sau lớp của mình.

## Kiểm tra trong CI

- [ ] Tương phản AA cho mọi cặp nền/chữ ở cả hai chủ đề
- [ ] Không màu cứng ngoài token
- [ ] Dấu tiếng Việt đúng ở mọi trọng lượng font
- [ ] Mọi thành phần có trạng thái: rỗng · đang tải · lỗi · offline
- [ ] `prefers-reduced-motion` được tôn trọng
- [ ] Chủ đề Field in đen trắng đọc được
- [ ] Bản 2D của bản đồ chứa mọi thông tin có ở bản 3D
