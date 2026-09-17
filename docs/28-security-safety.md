# 28 · An ninh &amp; An toàn

## Mô hình đe doạ

| Kẻ tấn công | Muốn gì | Chặn bằng |
|---|---|---|
| Người học gian lận | Chứng nhận không xứng đáng, tiền | Phiên bảo vệ · bất thường vết · chấm chéo |
| Nhà tài trợ | Ảnh hưởng phán quyết, danh tính người học sớm | Ký quỹ · ẩn danh trước kết quả · điều lệ |
| Gác cổng biến chất | Bán phán quyết | Xoay gác cổng · chấm chéo · `sentinel` · nhiệm kỳ |
| Kẻ tấn công ngoài | Dữ liệu người học, tiền | Tiêu chuẩn an ninh thông thường |
| Agent lỗi/bị chiếm | Sinh nội dung độc, rò rỉ dữ liệu | Phạm vi · cấm ghi · hạn ngạch · công tắc ngắt |
| Người lớn xấu | Tiếp cận trẻ vị thành niên | [`29-minors.md`](29-minors.md) |
| Người trong nội bộ | Sửa lịch sử, lật phán quyết | Chuỗi băm · chỉ ghi thêm · tách quyền |

---

## Sandbox thi hành

Người học chạy code tuỳ ý. Đây là bề mặt tấn công lớn nhất.

```yaml
crucible_policy:
  isolation: container + seccomp + user namespace
  network:
    forbidden:  ngắt hoàn toàn, không DNS
    allowed:    danh sách trắng (gói, tài liệu, API mô hình)
    real_stakes: có kiểm toán, ghi log toàn bộ
  resources: { cpu: 2, memory: 2Gi, disk: 4Gi, pids: 256 }
  timeout: theo ngưỡng, cứng
  filesystem: chỉ đọc trừ /work
  egress: qua proxy, ghi log toàn bộ
  teardown: huỷ hoàn toàn, không dùng lại container
```

Chính sách `forbidden` phải **ngắt mạng thật**, không phải yêu cầu người học đừng dùng AI. Nếu không cưỡng chế được thì đừng tuyên bố cưỡng chế được.

---

## Cách ly agent

| Kiểm soát | Cưỡng chế ở đâu |
|---|---|
| `scope.write` rỗng | Lúc đăng ký (403) + lúc chạy tại Gateway |
| `forbidden` | Gateway, mọi lời gọi |
| Không truy cập PII người học | Agent chỉ thấy số liệu tổng hợp |
| Không truy cập vết thô | `scribe` chỉ đọc bản sao đã lọc |
| Hạn ngạch chi phí | Dừng cứng ở 100% |
| Chèn prompt qua nội dung người học | Vết đưa vào `scribe` được đánh dấu là dữ liệu không tin cậy; đầu ra bắt buộc có trích dẫn nên lời lạ không có nguồn sẽ lộ |
| Agent bị chiếm | mTLS, xoay chứng chỉ, công tắc ngắt < 60s |

**INV-08 được kiểm ba lần:** kiểm tĩnh manifest lúc đăng ký, kiểm mỗi lời gọi tại Gateway, và kiểm tại tầng ghi của Lõi (`VerdictRecorded` từ chối actor không phải người).

---

## Tính toàn vẹn của bằng chứng

1. Vết ký **tại nguồn** trong sandbox, không ký ở máy chủ sau khi nhận.
2. Chuỗi băm trong kho sự kiện; sửa lịch sử là phá chuỗi.
3. Không ai — kể cả admin — sửa được vết. Chỉ vô hiệu hoá được, và việc vô hiệu hoá cũng là một sự kiện.
4. Video phiên bảo vệ có băm, lưu riêng, khoá riêng.
5. Phán quyết bất biến; sửa bằng kháng nghị, không bằng chỉnh sửa.

---

## An toàn nội dung

| Rủi ro | Chặn |
|---|---|
| Ngưỡng cửa hướng dẫn việc bất hợp pháp/nguy hiểm | `redteam` + hội đồng an toàn + chặn theo từ khoá ở tầng đề xuất |
| Ngưỡng yêu cầu gặp mặt ngoài đời | `minor_safe = false` bắt buộc (INV-09); người lớn cần quy trình riêng |
| Lore xúc phạm văn hoá/tôn giáo | Hội đồng văn hoá có quyền phủ quyết |
| Nội dung bịa được trình bày như sử liệu | INV-07 + nhãn bắt buộc |
| Ngưỡng bảo mật dạy tấn công thật | Chỉ trong sandbox cách ly; không bao giờ nhắm vào hệ thống thật của bên thứ ba |

**Ngưỡng bảo mật là vùng nhạy cảm nhất.** Quy tắc: mọi mục tiêu tấn công phải là hệ thống do dự án dựng riêng cho ngưỡng đó; không bao giờ là hệ thống thật của ai.

---

## Riêng tư

- Thu thập tối thiểu. Không vị trí, danh bạ, lịch sử duyệt web, sinh trắc.
- **Không dịch vụ phân tích của bên thứ ba** trong ứng dụng người học (Hiến pháp phụ thuộc).
- Số liệu tự thu, tổng hợp, không theo dõi cá nhân qua các trang.
- Video bảo vệ: chỉ gác cổng chấm + hội đồng kháng nghị; hạn lưu ngắn.
- Vết chứa mã của người học — đối xử như tài sản riêng tư của họ.
- Ghi danh công khai là **lựa chọn**, mặc định dùng bút danh.

---

## An toàn tiền

| Kiểm soát | |
|---|---|
| Ký quỹ | Tiền nhà tài trợ bị giữ tới khi có phán quyết |
| Cửa sổ 72 giờ | Trước khi giải ngân, để kháng nghị |
| Chấm chéo bắt buộc | Trên ngưỡng vượt hạn mức tiền |
| Đối chiếu hàng ngày | Sổ cái nội bộ với cổng thanh toán; lệch ⇒ báo động ngay |
| Xác minh danh tính | Chỉ khi tiền tích luỹ vượt hạn mức pháp lý |
| Tách quyền | Người duyệt phán quyết ≠ người duyệt chi tiền |
| Vị thành niên | Tiền vào quỹ giám hộ, không vào tay trực tiếp |

---

## Sự cố

Bốn mức:

| Mức | Ví dụ | Phản ứng |
|---|---|---|
| **P0** | Trẻ vị thành niên gặp nguy hiểm · rò rỉ dữ liệu · mất tiền | Ngay lập tức, gọi người, có thể dừng hệ thống |
| **P1** | Agent sinh nội dung độc lọt lưới · gác cổng thông đồng | Trong ngày, ngắt phần liên quan |
| **P2** | Sandbox thoát · lỗi chấm hệ thống | Trong 72 giờ |
| **P3** | Nội dung sai · lỗi hiển thị | Theo quy trình thường |

Runbook chi tiết: [`52-runbooks.md`](52-runbooks.md).

**Nguyên tắc sau sự cố:** phân tích không đổ lỗi, công khai với người học khi có ảnh hưởng tới họ, và mỗi sự cố phải sinh ra một cơ chế chặn — không chỉ một lời hứa cẩn thận hơn.

---

## Danh mục kiểm tra trước khi mở cho người ngoài (G1)

- [ ] Sandbox đã thử thoát bởi người ngoài đội
- [ ] Chế độ vị thành niên hoạt động đầy đủ
- [ ] Vết ký tại nguồn, xác minh được
- [ ] Chuỗi băm sự kiện kiểm được
- [ ] Sao lưu đã thử khôi phục thật một lần
- [ ] Quy trình xuất/xoá dữ liệu chạy được
- [ ] Công tắc ngắt thử thật, < 60s
- [ ] Đường kháng nghị hoạt động
- [ ] Không dịch vụ phân tích bên thứ ba trong ứng dụng
- [ ] Không bí mật nào trong kho mã
