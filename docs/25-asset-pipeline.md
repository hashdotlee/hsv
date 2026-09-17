# 25 · Asset Pipeline

> Bạn thả ảnh/tài liệu vào một thư mục → hệ thống tự trích màu, hoa văn, sprite, token → xuất ra đúng định dạng `cultures/vi-VN/` import thẳng được.

## Nguyên tắc

1. **Nguồn là bất biến.** Không bao giờ sửa file gốc. Mọi thứ sinh ra đều dẫn xuất và tái tạo được.
2. **Thiếu quyền ⇒ cách ly.** Không có ngoại lệ, kể cả khi chậm tiến độ (Điều lệ §22).
3. **Người duyệt ở đúng chỗ người cần duyệt:** quyền, độ chính xác văn hoá, an toàn. Không duyệt những thứ máy kiểm được.
4. **Mọi thứ quy về chuẩn W3C Design Tokens** rồi xuất đi khắp nơi. Đây là thứ làm cho "hoàn toàn tương thích" thành hiện thực thay vì lời hứa.

---

## Đầu vào

```
content-inbox/
  2026-03-12_tranh-dong-ho/
    source/
      ga-dan.tif            # gốc, không đụng vào
      lon-am-duong.tif
    meta.yaml               # BẮT BUỘC
```

```yaml
# meta.yaml
title: "Tranh Đông Hồ — Gà đàn, Lợn âm dương"
kind: artwork               # artwork | photo | document | audio | video | map | 3d
culture: vi-VN
region: "Bắc Ninh"
period: "thế kỷ 20"

provenance:
  collected_by: "…"
  collected_at: "2026-03-10"
  source_type: "chụp trực tiếp tại xưởng"   # | sách | bảo tàng | internet
  source_ref: "…"

rights:
  license: "CC BY-SA 4.0"     # bắt buộc; "unclear" ⇒ cách ly (INV-12)
  holder: "…"
  permission_doc: "docs/permissions/2026-03-10.pdf"
  commercial_use: true
  attribution_required: true

council:
  required: true
  reviewers: ["nghệ nhân …"]

labels: [ "sử liệu" ]         # sử liệu | truyền thuyết | hư cấu
notes: "…"
```

Không có `meta.yaml` hợp lệ ⇒ pipeline **không chạy**. Đây là cửa chặn quan trọng nhất: với tài sản văn hoá, rủi ro pháp lý và đạo đức lớn hơn mọi lỗi kỹ thuật.

---

## Mười giai đoạn

```
1  INGEST       kiểm meta.yaml · băm nội dung · lưu bất biến · gán ID
2  NORMALIZE    không gian màu → sRGB · xoay theo EXIF · gỡ siêu dữ liệu riêng tư
3  PALETTE      trích màu chủ đạo · gộp cụm · đối chiếu với bột màu đã biết
4  MOTIF        tách nền · vector hoá đường nét · tách hoa văn lặp
5  TEXTURE      sinh texture liền mạch từ mảng hoa văn
6  SPRITE       sprite nhân vật, atlas, khung hình
7  EXTRACT      OCR / nhận dạng tiếng nói → văn bản → trích thực thể, lore
8  TOKENIZE     màu + khoảng cách + chữ → W3C Design Tokens
9  VALIDATE     tương phản · đủ trường · đủ xuất xứ · kích thước · trùng lặp
10 PACKAGE      đóng gói `cultures/<id>/<version>/` · sinh PROVENANCE.json
```

### Điểm dừng cho người

| Sau giai đoạn | Ai duyệt | Duyệt cái gì |
|---|---|---|
| 1 | Người phụ trách quyền | Giấy phép rõ ràng? Có văn bản cho phép? |
| 4 | Hội đồng văn hoá | Hoa văn tách ra có đúng không, có bị cắt mất ý nghĩa không? |
| 7 | Hội đồng văn hoá | Lore trích ra có đúng không? Nhãn `[sử liệu]`/`[truyền thuyết]`/`[hư cấu]` đúng chưa? |
| 9 | Thiết kế | Token có dùng được không? Tương phản có đạt không? |

Máy làm hết phần còn lại. **Người chỉ duyệt thứ máy không quyết được.**

---

## Đầu ra

```
cultures/vi-VN/1.4.0/
  pack.json
  PROVENANCE.json              # xuất xứ + giấy phép từng asset, máy đọc được
  tokens/
    color.tokens.json          # chuẩn W3C
    typography.tokens.json
    space.tokens.json
    motion.tokens.json
    _generated/                # KHÔNG sửa tay
      variables.css
      tokens.ts
      three.palette.json
      print.css
  motifs/*.svg
  textures/*.{webp,ktx2}
  characters/
  terrain/biomes.json
  lore/*.md                    # có nhãn nguồn
  lexicon.json
  calendar.json
  audio/
```

### `PROVENANCE.json`

```json
{
  "assets": [
    {
      "id": "motif.dong-ho.ga-dan.001",
      "derivedFrom": "src.2026-03-12.ga-dan.tif",
      "sourceHash": "sha256:…",
      "pipeline": ["normalize@1.2", "motif@0.9"],
      "rights": { "license": "CC BY-SA 4.0", "holder": "…", "attribution": "…" },
      "council": { "approvedBy": ["…"], "at": "2026-03-14" },
      "label": "sử liệu"
    }
  ]
}
```

Mọi asset trong bản phát hành phải có mục ở đây. Không có mục ⇒ build thất bại.

---

## Công cụ

Theo Hiến pháp phụ thuộc: **logic quyết định là của mình, công cụ xử lý ảnh thì thuê.**

| Giai đoạn | Thuê | Tự viết |
|---|---|---|
| Xử lý ảnh | thư viện ảnh chuẩn (Pillow/OpenCV) | Chính sách chuẩn hoá, quy tắc đặt tên |
| Không gian màu | thư viện khoa học màu | Đối chiếu với bột màu truyền thống, quy tắc đặt tên token |
| Gộp cụm màu | thư viện học máy cơ bản | Số cụm, quy tắc chọn màu đại diện |
| Tách nền | mô hình phân đoạn | Ngưỡng, hậu xử lý |
| Vector hoá | công cụ raster→vector | Tham số, làm sạch, tham số hoá hoa văn |
| OCR / ASR | công cụ chuẩn | Trích thực thể, gán nhãn nguồn |
| **Xuất token** | — | **Tự viết** (`@hsv/tokens`, ~600 dòng) |
| Điều phối | Makefile / script | Toàn bộ |

**Vì sao tự viết bộ xuất token:** nó chạm trực tiếp vào bản sắc, và bạn sẽ cần xuất ra những đích lạ (bảng màu Three.js, culture pack, CSS in giấy) mà công cụ có sẵn không lo. 600 dòng đổi lấy quyền kiểm soát hoàn toàn tầng bản sắc — đúng định nghĩa tầng 0.

**Điều phối:** dùng Makefile và script ở G3–G4. Chỉ cân nhắc công cụ điều phối luồng dữ liệu khi thật sự có nhiều nguồn chạy song song hàng ngày.

---

## Lưu trữ

| Loại | Nơi | Vì sao |
|---|---|---|
| Nguồn gốc (lớn) | Kho đối tượng + con trỏ trong git | Không phình repo |
| `meta.yaml`, token, SVG | Git thường | Cần diff và review |
| Dẫn xuất (texture, atlas) | Sinh lại được từ nguồn, lưu đệm | Không cần phiên bản hoá |
| Bản phát hành gói | Có gắn thẻ, bất biến | Quay lui được |

---

## Lệnh

```
hsv ingest content-inbox/2026-03-12_tranh-dong-ho
hsv build vi-VN
hsv pack validate vi-VN
hsv pack release vi-VN --version 1.4.0
hsv pack rollback vi-VN --to 1.3.2
```

## Quay lui

Mỗi bản phát hành gói là bất biến và có thẻ. Quay lui là đổi con trỏ, không phải build lại. Người học đang dùng gói cũ không bị gián đoạn giữa chừng.
