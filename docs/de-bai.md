# Đề bài xuyên suốt — MT-01

> Đề bài gốc của khoá Đào tạo mentor mới. **File này là nguồn duy nhất** —
> `repo-mau/dung-repo.sh` copy nó vào repo học viên thành `docs/de-bai.md`.
> Sửa ở đây, đừng sửa bản trong repo.

## Hướng nghiên cứu gốc — nguyên văn

> Nhóm bước đầu xác định một hướng nghiên cứu về phát triển hệ thống AI agent trong giáo dục đại
> học, trước mắt triển khai tại Trường Tài năng, Đại học Kinh tế Thành phố Hồ Chí Minh.
>
> Một số nội dung và cụm từ được nêu ra để cân nhắc khi xây dựng tên đề tài:
>
> · Xây dựng/phát triển hệ thống AI agent chuẩn hóa nâng cao chất lượng đào tạo đại học. Nghiên
> cứu trường hợp tại Trường Tài năng, Đại học Kinh tế Thành phố Hồ Chí Minh.
>
> · Định hướng quốc tế hóa chương trình đào tạo cho các nước có tiếng Anh là ngôn ngữ thứ hai

Đó là **hướng nghiên cứu**, rộng hơn thứ build được trong 15 buổi. Bảng dưới là cách hạ từng ý
xuống thành thứ **đo được và build được**:

| Ý trong hướng nghiên cứu | Hạ thành |
|---|---|
| Hệ thống AI agent trong giáo dục đại học | Ba năng lực A · B · C, mỗi năng lực một lát cắt chạy được |
| Chuẩn hóa nâng cao chất lượng đào tạo | Rubric 6 tiêu chí có **ngưỡng đếm được**, áp lên đề cương môn học |
| Quốc tế hóa cho nước có tiếng Anh là ngôn ngữ thứ hai | Tiêu chí **TC6 — ESL readiness** + glossary song ngữ Việt–Anh |
| Nghiên cứu trường hợp tại Trường Tài năng, UEH | Dữ liệu tập mô phỏng bối cảnh TTN–UEH; **không** dùng dữ liệu thật |

---

## Vấn đề

Chất lượng đào tạo nằm rải ở ba tầng, mỗi tầng hiện làm bằng tay và mỗi người một chuẩn:

| Tầng | Hôm nay | Hậu quả |
|---|---|---|
| Người học | Sinh viên tự tìm hiểu, hỏi bạn hoặc hỏi ChatGPT | Câu trả lời không dựa trên tài liệu môn, không trích nguồn; sinh viên ESL kẹt ở thuật ngữ |
| Môn học | Giảng viên tự soạn đề cương, tổ bộ môn rà bằng mắt mỗi học kỳ | Chuẩn đầu ra không đo được, không khớp đánh giá; kết quả rà khác nhau giữa người rà |
| Chương trình | Phòng đảm bảo chất lượng tổng hợp thủ công khi cần kiểm định | Không ai biết mức chuẩn hóa hiện tại, cho đến sát kỳ kiểm định |

## Ba năng lực

**A · Trợ lý người học.** Sinh viên hỏi về môn học, nhận câu trả lời **chỉ dựa trên tài liệu đã
được giảng viên duyệt**, kèm trích nguồn. Hỗ trợ ESL: thuật ngữ song ngữ, giải thích lại đoạn khó.
*Ràng buộc:* không làm hộ bài tập tính điểm, kể cả khi hỏi vòng. Không có nguồn thì từ chối.

**B · Chuẩn hóa môn học.** Giảng viên nộp đề cương, agent đối chiếu **rubric 6 tiêu chí**, trả về
báo cáo trích dẫn đúng chỗ trong đề cương kèm đề xuất sửa.
*Ràng buộc:* báo cáo ở trạng thái `PENDING` tới khi **giảng viên duyệt**. Agent không tự công bố.

**C · Bức tranh chương trình.** Quản lý chương trình xem môn nào đã rà, tiêu chí nào cả chương
trình đang yếu, PLO nào chưa môn nào phủ.
*Ràng buộc:* số tổng hợp, **ẩn danh** ở cấp sinh viên.

```
Sinh viên ──hỏi──> [A] ──chỉ đọc──> tài liệu ĐÃ DUYỆT
                                           ▲
Giảng viên ──nộp──> [B] ──báo cáo──> DUYỆT ─┴──số liệu──> [C] <── Quản lý CT
```

**Ba tầng nối nhau một chiều:** bản đã duyệt ở B mới là nguồn cho A. Giảng viên chưa duyệt thì
trợ lý không có gì để trả lời — đó là hành vi đúng, không phải thiếu tính năng.

## Rubric 6 tiêu chí

Đây là **dữ liệu riêng** của đề — thứ khiến hệ thống không phải AI wrapper. Ngưỡng phải đếm được.

| # | Tiêu chí | Ngưỡng |
|---|---|---|
| TC1 | Chuẩn đầu ra đo được | Không CLO nào chứa từ mơ hồ (hiểu, nắm được, biết được, làm quen) |
| TC2 | CLO ↔ PLO | Không CLO nào thiếu mapping; không PLO nào cả chương trình bỏ trống |
| TC3 | CLO ↔ đánh giá | Không CLO nào không được đánh giá; tổng trọng số bằng 100% |
| TC4 | Phân bố Bloom | Không quá 70% CLO ở mức Remember + Understand |
| TC5 | Tài liệu có nguồn, còn hạn | Không tài liệu nào thiếu năm; cảnh báo tài liệu quá 10 năm |
| TC6 | ESL readiness | Không câu nào quá 40 từ; thuật ngữ cốt lõi có bản tiếng Anh |

TC6 chính là phần "quốc tế hóa cho nước ESL", hạ xuống thành thứ **đo được**. Không hạ được thành
thứ đo được thì nó là mong muốn, không phải tính năng.

## Ba vai và ranh giới quyền

| Vai | Làm được | **Không** làm được |
|---|---|---|
| Sinh viên | Hỏi đáp trên tài liệu đã duyệt, xem glossary đã duyệt | Xem báo cáo `PENDING`, duyệt bất cứ thứ gì, nhờ làm hộ bài tính điểm |
| Giảng viên | Nộp, sửa, duyệt báo cáo và glossary của môn mình | Xem đề cương môn của giảng viên khác |
| Quản lý chương trình | Dashboard mức chuẩn hóa, số tổng hợp ẩn danh | Đọc câu hỏi của từng sinh viên, sửa đề cương |

Ranh giới quyền phải nằm **trong handler ở server**. Ẩn nút trên giao diện không phải kiểm quyền.

## Yêu cầu đầu ra

**Cơ bản**
- Web app deploy công khai, dùng được trên màn hình 360 px; đăng nhập 3 vai
- (B) Nộp đề cương → báo cáo 6 tiêu chí, mỗi nhận định trích đúng đoạn trong đề cương
- (B) Báo cáo `PENDING` tới khi giảng viên duyệt; **server từ chối** yêu cầu duyệt từ vai khác
- (A) Trả lời grounded trên tài liệu đã duyệt, có citation; ngoài phạm vi thì từ chối có lý do
- (A) Từ chối làm hộ bài tính điểm
- (C) Dashboard mức chuẩn hóa, không lộ dữ liệu từng sinh viên
- Golden dataset ≥ 45 case + bảng số liệu eval vòng 1

**Nâng cao** — glossary song ngữ có duyệt từng mục · chấm ESL readiness theo đoạn · so sánh
before/after điểm rubric sau khi sửa · audit log truy ngược mọi quyết định của agent

**Acceptance demo.** Vai giảng viên: nộp đề cương, mở báo cáo, bấm một nhận định và chỉ đúng dòng
nó trích, duyệt. Đổi sang vai sinh viên: hỏi một câu có trong tài liệu (nhận câu trả lời có nguồn)
và một câu không có (nhận lời từ chối). Gọi API duyệt bằng vai sinh viên **và cho thấy server trả
403**. Mở dashboard vai quản lý và cho thấy không có dữ liệu cá nhân nào.

## Phạm vi cố tình loại khỏi đề

Nói rõ từ buổi 01, nhắc lại mỗi khi có người muốn thêm:

- Không tích hợp LMS thật (Canvas/Moodle). Nhập file là đủ.
- Không chấm điểm sinh viên, không đọc bài làm của sinh viên.
- Không multi-agent, không fine-tune, không vector DB có phí.
- Không Next.js ở bản cơ bản. Streamlit trước, đẹp sau.

**Từ chối quan trọng ngang lựa chọn** — và đây cũng là câu mentor phải nói với đội ở tuần 3.

## Dữ liệu và giới hạn

Dữ liệu tập nằm trong `data/`: đề cương mẫu (**có lỗi cố ý ở cả 6 tiêu chí**), PLO mẫu, rubric.
**Không dùng dữ liệu sinh viên thật. Không dùng đề cương chưa được phép chia sẻ.**

MT-01 là **đề tập để mentor học kỹ thuật**, không phải thiết kế để triển khai ở Trường Tài năng.
Hướng nghiên cứu này muốn đi tiếp thành đề tài thật thì cần khảo sát người dùng thật, dữ liệu thật
và phê duyệt của trường — ba thứ khoá đào tạo không có.
