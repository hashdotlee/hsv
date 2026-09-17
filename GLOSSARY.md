# TỪ ĐIỂN THUẬT NGỮ

> Đọc file này trước mọi file khác. Một hệ thống do nhiều người và nhiều agent cùng xây sẽ trôi dạt về ngữ nghĩa trước khi trôi dạt về kỹ thuật. Từ điển này là neo.
>
> Quy ước: **thuật ngữ trong sản phẩm dùng tiếng Việt**; **định danh trong code dùng tiếng Anh**. Cột "code" là từ bắt buộc dùng trong mã nguồn, schema và API.

---

## Năng lực & nội dung

| Tiếng Việt | Code | Nghĩa chính xác |
|---|---|---|
| **Nguyên tử kỹ năng** | `SkillAtom` | Đơn vị năng lực nhỏ nhất **quan sát được bằng hành vi**. Nếu không mô tả được bằng một hành vi quan sát được thì không phải atom. |
| **Bộ gen kỹ năng** | `SkillGenome` | Toàn bộ đồ thị atom + cạnh tiên quyết của một lĩnh vực. Có hướng, không chu trình. |
| **Trục năng lực** | `Axis` | Một trong sáu: `NEN` `CHAN` `DUNG` `THAM` `NGUOI` `TRUYEN`. Xem [`docs/11-skill-genome.md`](docs/11-skill-genome.md). |
| **Ngưỡng cửa** | `Challenge` | Một thử thách thực tế có gác cổng, rubric, bằng chứng và phán quyết. **Không gọi là "bài tập"** — bài tập có đáp án, ngưỡng cửa thì không. |
| **Vết chuyên gia** | `ExpertTrace` | Bản ghi cách một người giỏi **quyết định**: điểm quyết định, phương án đã loại và lý do loại, dấu hiệu đã chú ý, cách gỡ khi sai. Không phải lời giải mẫu. |
| **Vết quá trình** | `ProcessTrace` | Bản ghi cách người học làm: lệnh đã chạy, test đã viết, giả thuyết đã bỏ, prompt đã gửi, phản ứng với câu trả lời của AI. |
| **Chữ ký lỗi** | `ErrorSignature` | Kiểu sai đặc trưng của một người. Hai người "giống nhau" khi họ **sai giống nhau**, không phải khi họ cùng tuổi hay cùng ngành. |
| **Suy giảm** | `decay` | Năng lực tụt theo thời gian thật nếu không dùng. Là sự thật về con người, không phải cơ chế ép quay lại. |
| **Mức AI thay thế** | `aiSubstitutability` | 0–1. Mức độ một mô hình mạnh làm thay được atom này. Đo lại mỗi quý. |

## Con người & vai trò

| Tiếng Việt | Code | Nghĩa |
|---|---|---|
| **Người học** | `Learner` | Bất kỳ ai đang đi trên bản đồ. Không có "học viên", "khách hàng", "user". |
| **Gác cổng** | `Gatekeeper` | Người đã qua ngưỡng đó và được chọn để dẫn người sau qua. **Chức vụ có nhiệm kỳ**, không phải địa vị. |
| **Phiên bảo vệ** | `DefenseSession` | 15–30 phút trực tiếp, gác cổng hỏi "vì sao". Neo chính của toàn bộ hệ thống đánh giá. |
| **Nhập môn** | `Initiation` | Nghi thức bắt đầu một ngưỡng: gặp gác cổng, nghe rõ rubric, cam kết. |
| **Phòng kín** | `Chamber` | Không gian riêng của những người đang cùng làm một ngưỡng. |
| **Ghi danh** | `Inscription` | Tên được khắc công khai vĩnh viễn khi qua ngưỡng. Lấy từ bia đề danh tiến sĩ. **Ngưỡng có thể chết, ghi danh không chết.** |
| **Hội đồng văn hoá** | `CulturalCouncil` | Nghệ nhân/nhà nghiên cứu có quyền phủ quyết nội dung văn hoá. |
| **Kháng nghị** | `Appeal` | Quyền phản đối phán quyết trước một gác cổng khác. |
| **Ngủ đông** | `Dormancy` | Trạng thái vắng mặt **có phẩm giá**. Không phạt, không khoá. |

## Thế giới & bản đồ

| Tiếng Việt | Code | Nghĩa |
|---|---|---|
| **Bản đồ năng lực** | `CapabilityMap` | Địa hình sinh **từ dữ liệu**, không vẽ tay. Cao độ = độ sâu; gồ ghề = phương sai; sương mù = thiếu tin cậy; lối mòn = vết người đi trước. |
| **Sương mù** | `fog` | Hiển thị **độ bất định**, không phải phần thưởng mở khoá. Sương dày = hệ thống thật sự chưa biết. |
| **Định vị** | `positioning` | Ước lượng vị trí người học trên từng trục, **luôn kèm bán kính bất định** `σ`. |
| **Vùng biên giới** | `frontier` | Vùng &lt;20 lượt đi qua. Agent gần như toàn quyền. |
| **Vùng có người ở** | `settled` | 20–200 lượt. Thay đổi cần người duyệt. |
| **Vùng chính điển** | `canon` | &gt;200 lượt, rubric hội tụ. Gần như đóng băng. Đây là phần di sản. |
| **Đỉnh tối cao** | `summit` | Đích chung của mọi con đường. Không đạt được nếu chưa đưa ai qua một ngưỡng. |

## Agent & thế giới sinh

| Tiếng Việt | Code | Nghĩa |
|---|---|---|
| **Lõi thế giới** | `WorldKernel` | Tiến trình **duy nhất** được ghi vào trạng thái thế giới. |
| **Đề xuất** | `Proposal` | Đơn vị công việc của agent. Agent chỉ đề xuất, không bao giờ ghi. |
| **Trọng tài** | `Arbiter` | Agent xử mâu thuẫn ngữ nghĩa. Chỉ phán xử, **không sinh nội dung**. |
| **Khảo hạch** | `RedTeam` | Agent được thưởng khi phá được sản phẩm của agent khác. |
| **Thái ấp** | `Claim` | Quyền đề xuất độc quyền tạm thời trên một phạm vi. Chặn xung đột ghi ở mức thiết kế. |
| **Bảng chung** | `Blackboard` | Không gian agent công bố **ý định** để bổ sung nhau thay vì trùng nhau. |
| **Đấu thầu** | `Tender` | Cơ chế việc tìm agent thay vì agent tìm việc. |
| **Trôi dạt** | `drift` | Thế giới loãng và tự mâu thuẫn dần. **Kẻ thù số một** của kiến trúc sinh tự động. |
| **AI đối kháng** | `adversary` | Chế độ AI cố tình sai có kiểm soát, để đo năng lực phát hiện máy nói sai. |
| **Tiền thẩm** | `preAssessment` | AI tóm tắt bằng chứng cho gác cổng. Chỉ đọc, **không điểm số**. |

## Tiền

| Tiếng Việt | Code | Nghĩa |
|---|---|---|
| **Trả ngưỡng** | `thresholdPayout` | Tiền trả khi qua một ngưỡng. |
| **Trợ cấp lộ trình** | `pathStipend` | Trả cho tiến bộ kiểm chứng được liên tục. |
| **Tô nhượng di sản** | `legacyRoyalty` | Trả cho người để lại vết chuyên gia, mỗi khi vết đó giúp ai đó qua ngưỡng. |
| **Trả nỗ lực** | `effortPayout` | Khoản nhỏ cho người **chưa đạt** nhưng đã nỗ lực thật. |
| **Ký quỹ** | `escrow` | Tiền nhà tài trợ bị giữ tới khi có phán quyết. |
| **Quỹ Bảo tồn** | `preservationFund` | Quỹ cho kỹ năng không có nhà tài trợ thương mại. Khoá bằng điều lệ. |
| **CPQ** | `costPerQualified` | Chi phí cho một người **đạt năng lực**. Đơn vị bán hàng của hệ thống — không bán lượt hiển thị. |

## Văn hoá & tài sản

| Tiếng Việt | Code | Nghĩa |
|---|---|---|
| **Gói văn hoá** | `CulturePack` | Đơn vị đóng gói toàn bộ lớp văn hoá. Lõi không được biết tên văn hoá nào. |
| **Xuất xứ** | `provenance` | Nguồn gốc + quyền sử dụng của một asset. Thiếu ⇒ cách ly vĩnh viễn. |
| **Cách ly** | `quarantine` | Asset xử lý xong nhưng không bao giờ vào bản build. |
| **Nhãn nội dung** | `contentLabel` | `[sử liệu]` / `[truyền thuyết]` / `[hư cấu]`. Bắt buộc, không được làm mờ. |

---

## Những từ **không** dùng

| Không dùng | Dùng thay | Vì sao |
|---|---|---|
| "khoá học", "bài học" | ngưỡng cửa, con đường | Đây không phải trường học |
| "điểm", "score" | mức thành thạo `mastery` | Điểm mời gọi so sánh và xếp hạng |
| "level", "cấp" | vị trí trên trục | Cấp là tuyến tính; năng lực thì không |
| "gamification" | nghi thức | Gamification là dán phần thưởng lên việc nhàm chán |
| "streak", "chuỗi ngày" | — | Vi phạm Điều lệ §1 |
| "AI chấm bài" | AI tiền thẩm | Vi phạm Điều lệ §12 |
| "người dùng" trong UI | người học, gác cổng | Vai trò cụ thể, không phải "user" |
