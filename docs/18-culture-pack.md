# 18 · Gói văn hoá

> **Lõi không được biết tên của bất kỳ nền văn hoá nào.** Chi phí tách bây giờ gần bằng 0; sau hai năm là viết lại toàn bộ.

## Ranh giới

| Thuộc lõi (trung lập văn hoá) | Thuộc gói văn hoá |
|---|---|
| `SkillAtom`, `SkillGenome`, `Challenge`, `Rubric` | Tên gọi hiển thị của sáu trục |
| Máy trạng thái nghi thức: `initiation → attempt → defense → verdict → inscription` | Tên và hình thức của từng bước nghi thức |
| Thuật toán sinh địa hình | Bảng màu, hoa văn, hình dạng biome |
| Cơ chế ghi danh | Hình thức bia đề danh |
| Vai trò: người học, gác cổng, hội đồng | Tên gọi: "thợ cả", "tổ nghề", "hương ước" |
| Lịch: mốc thời gian, chu kỳ | Ngày lễ, giỗ tổ nghề, tết |
| Cơ chế phòng kín | Tên gọi và không gian hình ảnh |

**Kiểm tra đơn giản:** nếu xoá thư mục `cultures/` thì hệ thống vẫn phải chạy — xấu, không tên, nhưng đúng chức năng.

---

## Cấu trúc

```
cultures/
  vi-VN/
    1.4.0/
      pack.json              # manifest: phiên bản, tương thích lõi, hội đồng
      PROVENANCE.json        # xuất xứ + giấy phép từng asset
      tokens/
        color.tokens.json    # chuẩn W3C Design Tokens
        typography.tokens.json
        space.tokens.json
        motion.tokens.json
        _generated/          # sinh tự động, không sửa tay
          variables.css
          tokens.ts
          three.palette.json
      motifs/*.svg
      terrain/biomes.json
      characters/
      lore/
      rites.json
      lexicon.json
      calendar.json
      audio/
      council.md
  ja-JP/ ...
```

### `pack.json`

```json
{
  "id": "vi-VN",
  "version": "1.4.0",
  "coreCompat": ">=0.8.0 <2.0.0",
  "displayName": "Việt Nam",
  "council": ["…"],
  "axisNames": {
    "NEN": "Nền", "CHAN": "Chẩn", "DUNG": "Dựng",
    "THAM": "Thẩm", "NGUOI": "Người", "TRUYEN": "Truyền"
  },
  "riteNames": {
    "initiation": "Lễ nhập môn",
    "defense": "Phiên vấn đáp",
    "inscription": "Ghi danh bia đá",
    "summit": "Vinh quy"
  },
  "macroMapGeometry": "dong-son-drum",
  "requiresCulturalReview": true
}
```

---

## Gói `vi-VN`

### Cấu trúc xã hội làm cơ chế

Bạn không cần phát minh cơ chế. Việt Nam đã vận hành chúng hàng trăm năm:

| Cơ chế hệ thống | Nguồn Việt Nam | Cách dùng |
|---|---|---|
| Ba cấp ngưỡng cửa | Thi Hương → thi Hội → thi Đình | Ba cấp độ ngưỡng, mỗi cấp một hình thức bảo vệ khác nhau |
| Ghi danh vĩnh viễn | Bia đề danh tiến sĩ, Văn Miếu | Trang ghi danh công khai, khắc đá hình ảnh |
| Nhập môn | Lễ bái sư | Gặp gác cổng, nghe rubric, cam kết |
| Cộng đồng tự quản | Phường hội, hương ước tự viết | Mỗi vùng bản đồ có quy ước riêng do gác cổng vùng đó viết |
| Tổ tiên nghề | Tổ nghề, giỗ tổ nghề | Vết chuyên gia được tôn vinh; ngày giỗ tổ nghề trong lịch |
| Cộng đồng chứng kiến | Vinh quy bái tổ, đình làng | Nghi thức khi lên đỉnh; không gian chung của vùng |
| Học nghề dài hạn | Quan hệ thầy–thợ | Trục TRUYỀN |

### Bản đồ vĩ mô: mặt trống đồng Đông Sơn

```
        tâm (ngôi sao)      = đỉnh tối cao
        vòng đồng tâm       = cấp độ
        cánh sao (thường 12)= ánh xạ về 6 trục, mỗi trục 2 cánh
        chim Lạc bay vòng   = người học đang đi (ẩn danh)
        hoa văn răng lược   = ranh giới vùng
```

Tổ tiên đã thiết kế xong bản đồ. Nó vừa đúng cấu trúc cần có, vừa là biểu tượng văn hoá mạnh nhất, vừa render cực nhẹ ở dạng SVG.

### Bảng màu: bột màu tranh Đông Hồ

Năm màu từ vật liệu tự nhiên, thay vì màu tuỳ ý:

| Tên | Vật liệu | Dùng cho |
|---|---|---|
| Than lá tre | tro lá tre | Nét, chữ, nền tối |
| Vỏ điệp | vỏ sò nghiền | Nền sáng, ánh xà cừ |
| Hoa hoè | hoa hoè | Vàng — trục NỀN, ánh sáng |
| Sỏi son | đá son | Đỏ — cảnh báo, ngưỡng cửa |
| Lá chàm | cây chàm | Xanh — sương mù, chiều sâu |

> **Mã hex hiện tại là diễn giải từ mô tả vật liệu, chưa đối chiếu hiện vật.** Cần ảnh gốc độ phân giải cao chạy qua asset pipeline ([`25-asset-pipeline.md`](25-asset-pipeline.md)) để có màu chuẩn thật. Đánh dấu `provisional: true` trong token cho tới khi đối chiếu xong.

Nét khắc gỗ: viền dày, mảng màu dẹt, không đổ bóng gradient — vừa đúng bản sắc, vừa render nhẹ trên máy yếu.

### Địa lý

Biome dựa trên địa hình Việt Nam thật: núi phía bắc, đồng bằng châu thổ, dải duyên hải hẹp, cao nguyên, đồng bằng sông nước phía nam. Ánh xạ trục năng lực về biome cần hội đồng văn hoá duyệt — không gán bừa để tránh ngụ ý vùng miền không mong muốn.

### Lịch

```yaml
calendar:
  - { id: gio-to-nghe,  recurring: yearly, effect: "tôn vinh vết chuyên gia, trả tô nhượng bổ sung" }
  - { id: tet,          recurring: yearly, effect: "nghỉ nghi thức; không ngưỡng nào hết hạn" }
  - { id: khoa-thi,     recurring: quarterly, effect: "mùa ngưỡng cấp cao, nhiều gác cổng trực" }
```

Thời gian trong thế giới chạy song song thời gian thật.

---

## Ranh giới đạo đức

| Luật | Vì sao |
|---|---|
| Không biến nhân vật đang được thờ phụng thành NPC giao nhiệm vụ | Xúc phạm tín ngưỡng sống |
| Nội dung về cộng đồng dân tộc thiểu số phải có người của cộng đồng đó **đồng tác giả và hưởng lợi** | Không khai thác văn hoá |
| Ba nhãn bắt buộc: `[sử liệu]` · `[truyền thuyết]` · `[hư cấu của chúng tôi]` | Không làm mờ ranh giới để câu chuyện hay hơn |
| Không dùng asset khi chưa rõ quyền | Rủi ro lớn hơn mọi lỗi kỹ thuật |
| Hội đồng văn hoá có quyền **phủ quyết**, không chỉ góp ý | Quyền phủ quyết mới là quyền thật |
| Không dùng biểu tượng tôn giáo làm phần thưởng chơi game | — |

---

## Thêm một nền văn hoá mới

```
1. Lập hội đồng văn hoá cho nền đó — TRƯỚC khi viết dòng code nào
2. fork cấu trúc thư mục từ cultures/_template/
3. Điền tokens, motifs, lexicon, rites, calendar
4. Chạy `hsv pack validate <id>` — kiểm tra đủ trường, đủ xuất xứ, đủ tương phản
5. Hội đồng duyệt từng nhóm asset
6. Phát hành phiên bản, người học chọn được gói
```

**Lõi không bao giờ phải sửa để thêm một nền văn hoá.** Nếu phải sửa lõi, đó là lỗi kiến trúc — ghi ADR và sửa lõi cho tổng quát hơn, đừng đặc biệt hoá.

## Kiểm định gói

```
hsv pack validate vi-VN
  ✓ pack.json hợp lệ, tương thích lõi
  ✓ mọi token bắt buộc có mặt
  ✓ tương phản màu đạt WCAG AA cho mọi cặp nền/chữ
  ✓ mọi asset có mục trong PROVENANCE.json
  ✓ mọi lore có nguồn hoặc nhãn [hư cấu]   (INV-07)
  ✓ mọi asset đã có chữ ký duyệt của hội đồng
  ⚠ 3 token đánh dấu provisional — chưa đối chiếu hiện vật
```
