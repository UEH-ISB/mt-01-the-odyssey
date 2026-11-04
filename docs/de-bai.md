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
| Hệ thống AI agent trong giáo dục đại học | Ba năng lực A · B · C, nối thành một vòng lặp học tập |
| Chuẩn hóa nâng cao chất lượng đào tạo | Learning outcome **đo theo từng PLO** từ bài trắc nghiệm, không dựa vào cảm nhận |
| Quốc tế hóa cho nước có tiếng Anh là ngôn ngữ thứ hai | **Chưa chốt** — xem câu 5 ở mục "Cần chốt" |
| Nghiên cứu trường hợp tại Trường Tài năng, UEH | Dữ liệu tập mô phỏng bối cảnh TTN–UEH; **không** dùng dữ liệu thật |

---

## Vấn đề

Đánh giá learning outcome qua bài trắc nghiệm hiện dừng ở một con số điểm:

| Khâu | Hôm nay | Hậu quả |
|---|---|---|
| Ra đề | Giảng viên soạn câu hỏi tay; việc câu nào đo PLO nào không được ghi lại nhất quán | Không biết bài kiểm tra có phủ đủ PLO hay không |
| Đánh giá | Sinh viên nhận tổng điểm | Một câu có thể đo nhiều PLO, nên tổng điểm không cho biết sinh viên thiếu PLO nào |
| Sau bài kiểm tra | Mọi sinh viên học tiếp cùng một nội dung | Lỗ hổng của từng người không được bù |

## Đầu vào

| Đầu vào | Dùng để |
|---|---|
| Syllabus | Lấy danh sách PLO của môn và cách chia session |
| Nội dung môn học | Nguồn để sinh câu hỏi và nguồn duy nhất để gợi ý phần cần đọc |
| Bài assignment sinh viên nộp | Tính mức đạt theo PLO của từng sinh viên |

## Ba năng lực

**A · Ngân hàng câu hỏi gắn PLO.** Từ syllabus và nội dung môn, AI tạo test bank trắc nghiệm,
mỗi câu gắn với một hoặc nhiều PLO. Vì một câu có thể đo nhiều PLO, AI **gợi ý trọng số** PLO cho
từng câu.
*Ràng buộc:* câu hỏi và trọng số ở trạng thái `PENDING` tới khi **giảng viên duyệt**. Câu chưa duyệt
không được đưa vào assignment.

**B · Đánh giá learning outcome theo PLO.** Mỗi session có một assignment **10 câu**. Sinh viên nộp
bài, hệ thống tính mức đạt của sinh viên đó trên từng PLO theo trọng số đã duyệt, rồi chỉ ra PLO còn
thiếu.
*Ràng buộc:* kết quả phải truy ngược được tới từng câu và trọng số đã tạo ra nó.

**C · Cá nhân hoá theo sinh viên.** Với PLO còn thiếu, AI gợi ý sinh viên đọc đúng phần tương ứng
trong nội dung môn. Đọc xong, sinh viên được làm **assignment mới tập trung vào các PLO đang thiếu**.
*Ràng buộc:* gợi ý đọc phải trích đúng chỗ trong nội dung môn; assignment mới chỉ lấy câu đã duyệt.

```
Syllabus + nội dung môn ──> [A] test bank + trọng số ──> GIẢNG VIÊN DUYỆT
                                                              │
                                                              ▼
                 ┌──────────────────────> assignment 10 câu mỗi session
                 │                                            │ sinh viên nộp
                 │                                            ▼
   [C] gợi ý đọc phần thiếu <── PLO còn thiếu <── [B] mức đạt theo từng PLO
       rồi assignment mới
```

**Vòng lặp theo từng sinh viên:** hai sinh viên cùng lớp có thể nhận assignment tiếp theo khác nhau.
Giảng viên chưa duyệt thì hệ thống không có câu nào để phát — đó là hành vi đúng, không phải thiếu
tính năng.

## Hai vai và ranh giới quyền

| Vai | Làm được | **Không** làm được |
|---|---|---|
| Sinh viên | Làm assignment, xem kết quả theo PLO của mình, xem gợi ý đọc của mình | Xem kết quả của sinh viên khác, xem câu `PENDING`, duyệt bất cứ thứ gì |
| Giảng viên | Nộp syllabus và nội dung, sửa và duyệt câu hỏi và trọng số của môn mình, xem kết quả của lớp mình | Xem hoặc sửa dữ liệu môn của giảng viên khác |

Ranh giới quyền phải nằm **trong handler ở server**. Ẩn nút trên giao diện không phải kiểm quyền.

## Yêu cầu đầu ra

**Cơ bản**
- Web app deploy công khai, dùng được trên màn hình 360 px; đăng nhập 2 vai
- (A) Từ syllabus và nội dung môn sinh ra test bank, mỗi câu có PLO và trọng số gợi ý
- (A) Câu hỏi `PENDING` tới khi giảng viên duyệt; **server từ chối** yêu cầu duyệt từ vai khác
- (B) Assignment 10 câu mỗi session; nộp bài ra mức đạt theo từng PLO
- (C) PLO thiếu → gợi ý phần cần đọc có trích nguồn → assignment mới tập trung vào PLO đó
- Golden dataset ≥ 45 case + bảng số liệu eval vòng 1

**Acceptance demo.** Vai giảng viên: nộp syllabus và nội dung, mở test bank, sửa một trọng số, duyệt.
Đổi sang vai sinh viên: làm assignment 10 câu, xem PLO còn thiếu, mở gợi ý đọc và cho thấy nó trỏ
đúng chỗ trong nội dung môn, nhận assignment mới tập trung vào PLO đó. Gọi API duyệt bằng vai sinh
viên **và cho thấy server trả 403**.

## Cần chốt

Bản ghi đề mới chưa nói rõ các điểm dưới đây. Đề này **không tự đặt** thay người phụ trách:

1. Mức đạt dưới bao nhiêu thì một PLO bị coi là "đang thiếu"?
2. Trọng số PLO trong một câu có buộc cộng lại bằng 100% không?
3. Assignment mới có đúng 10 câu không, và vòng lặp dừng khi nào?
4. Làm sao biết sinh viên "đã đọc xong": tự xác nhận hay cần kiểm tra?
5. Có giữ yêu cầu ESL (glossary song ngữ, ESL readiness) của đề cũ không?
6. Có giữ vai quản lý chương trình và dashboard tổng hợp của đề cũ không?

## Phạm vi cố tình loại khỏi đề

Nói rõ từ buổi 01, nhắc lại mỗi khi có người muốn thêm:

- Không tích hợp LMS thật (Canvas/Moodle). Nhập file là đủ.
- Chỉ xử lý bài trắc nghiệm. Không chấm bài tự luận.
- Không multi-agent, không fine-tune, không vector DB có phí.
- Không Next.js ở bản cơ bản. Streamlit trước, đẹp sau.

**Từ chối quan trọng ngang lựa chọn** — và đây cũng là câu mentor phải nói với đội ở tuần 3.

## Dữ liệu và giới hạn

Dữ liệu tập gồm syllabus và nội dung môn mẫu, PLO mẫu (`data/plo.md`) và bài nộp **mô phỏng**.
**Không dùng dữ liệu sinh viên thật. Không dùng tài liệu chưa được phép chia sẻ.**

MT-01 là **đề tập để mentor học kỹ thuật**, không phải thiết kế để triển khai ở Trường Tài năng.
Hướng nghiên cứu này muốn đi tiếp thành đề tài thật thì cần khảo sát người dùng thật, dữ liệu thật
và phê duyệt của trường — ba thứ khoá đào tạo không có.
