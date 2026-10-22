# Khung đánh giá — Gates, deliverables, rubric

> Đây là **nguồn duy nhất** về khung đánh giá cho cả 15 buổi. Mọi slide, mọi bài thực hành, mọi
> prompt gửi agent đều trỏ về file này. Trong repo dự án, file này nằm ở
> `docs/giao-trinh/00-khung-danh-gia.md`.
>
> **Nguồn:** kế thừa từ AI20K (Sổ tay Mentor Duty + bảng tiêu chí BTC Demo Day), transcribe
> 26/09/2026. **Trọng số từng tiêu chí có thể khác ở The Odyssey** — xem
> [00-README.md](00-README.md) mục "Nguồn và phần kế thừa".

## 1. Gates G1–G6 — rubric tiến độ theo tuần

Mentor dùng 6 cổng này check đội mỗi tuần. Tự chấm trước buổi mentor duty để không bị bất ngờ.

| Gate | Tuần | Bằng chứng tối thiểu | Buổi của khoá |
|---|---|---|---|
| G1 — Idea & scope | 1 | USP statement + 1 trang proposal + prototype tầng 1 đã cho 5 user xem | 05 · 02 |
| G2 — Architecture | 2 | Kiến trúc diagram + API skeleton chạy + repo đúng cấu trúc template | 03 · 02 |
| G3 — Agent chạy | 3 | Agent trả lời được 5 câu hỏi cốt lõi + tools thật được gọi + guardrail 1 lớp | 04 · 07 |
| G4 — Production | 4 | Deploy lên cloud + CI xanh + golden dataset ≥45 case + eval vòng 1 có số | 12 · 10 |
| G5 — Production-ready | 5 | Eval before/after + feedback ≥5 user thật + slop-check CLEAN + tổng duyệt BGK | 10 · 11 |
| G6 — Demo Day | 6 | 10/10 deliverables + video demo + pitch theo AIDA | 13 |

```
G1 USP+proposal → G2 diagram+API skeleton → G3 agent+guardrail
  → G4 deploy+CI+eval vòng 1 → G5 before/after+slop CLEAN → G6 10/10 deliverables
```

**Gate là cổng, không phải mốc báo cáo.** Đội trễ G2 ba ngày thì G3 không thể đúng hạn — trễ cộng
dồn, không tự khỏi.

## 2. At-risk triggers — việc của mentor, không phải của BTC

- Miss standup **2 ngày** liên tiếp
- Không commit **3 ngày**
- Gate trễ **>3 ngày**

Thấy trigger thì mở Playbook L1/L2/L3 bên giáo trình vận hành
([../../02_Quy_trinh_mentor/Giao_trinh_mentor_moi/](../../02_Quy_trinh_mentor/Giao_trinh_mentor_moi/index.html)).

## 3. Mười deliverables — kèm tình trạng cohort trước

| # | Deliverable | Cohort trước | Vì sao hay thiếu | Vị trí trong repo |
|---|---|---|---|---|
| 1 | Source Code (GitHub repo) | Phần lớn có | Ngại push code dở | `src/` |
| 2 | README.md | Phần lớn có | Viết cuối cùng khi hết giờ | `README.md` |
| 3 | Architecture Diagram | Thường thiếu | Coi là "vẽ cho đẹp" | `ARCHITECTURE.md` |
| 4 | AI Logs | Phần lớn có | Không biết config | hook + `.ai-log/` |
| 5 | Live URL | Thường có | Deploy để ngày cuối | README + deploy |
| 6 | Video Demo | **Thiếu 11/11 đội (100%)** | Để cuối, sợ quay, không ai nhận việc | `docs/video-demo.md` |
| 7 | Pitch Deck | **Thiếu ~64% đội** | Viết slide khó hơn viết code | `docs/pitch-deck.pdf` |
| 8 | Development Journal | Phần lớn có | Ghi chép mỗi ngày thấy "thừa" | `JOURNAL.md` |
| 9 | Worklog (commit history) | Phần lớn có | Không biết nộp sao | `WORKLOG.md` |
| 10 | Evaluation Evidence | **Thiếu ~82% đội** | Không biết đo gì, đo thế nào | `eval/` + `docs/evaluation.md` |

**Ba con số phải thuộc: 100% / 82% / 64%.** Video demo, evaluation evidence, pitch deck — ba thứ
thiếu nhiều nhất, và cả ba **không cần code giỏi hơn**. Đội nào làm được cả ba thì gần như chắc
chắn vượt phần lớn các đội còn lại.

Khi nói với đội, nói đúng nguồn: *"cohort trước của chương trình tiền nhiệm"*.

## 4. Rubric Demo Day — 6 tiêu chí × 5 mức

| Mức điểm | Xếp loại |
|---|---|
| 9–10 | Giỏi |
| 7–8 | Khá |
| 5–6 | Trung bình |
| 3–4 | Yếu |
| 1–2 | Kém |

| # | Tiêu chí | Câu hỏi BGK ngầm đặt ra | Mức Giỏi (9–10) đòi gì |
|---|---|---|---|
| 1 | Hoàn thiện sản phẩm (Demo) | Demo có chạy ổn định không lỗi không? | Tính năng chính đủ, xử lý được edge case, có test cho edge case |
| 2 | Kỹ thuật AI / Ứng dụng AI | Kỹ thuật AI thật hay chỉ gọi API? | RAG / guardrail ≥2 lớp / conditional edge thật / chống hallucination bằng cơ chế |
| 3 | Đánh giá hiệu quả (Evaluation) | Có số liệu chứng minh không? | Golden dataset ≥45 case, số đo, benchmark baseline, feedback người dùng |
| 4 | Giao diện & Trải nghiệm | Đẹp, tiện dụng, responsive không? | Luồng chính đi hết không kẹt, không có placeholder treo |
| 5 | Ý tưởng & ứng dụng thật | Giải quyết vấn đề thật cho người thật không? | Người dùng thật đã dùng và phản hồi được dẫn lại |
| 6 | Thuyết trình (Pitch) | Kể chuyện rõ, thuyết phục, Q&A tốt không? | Pitch theo AIDA + video demo + trả lời được câu hỏi khó |

**Giới hạn phải nói thật:** tên tiêu chí và mô tả mức thì ổn định, **trọng số có thể đổi theo
cohort**. Đầu mỗi cohort phải đối chiếu với bản The Odyssey phát hành. Mentor nói chắc chắn về thứ
chưa chắc là cách mất uy tín nhanh nhất.

## 5. Sáu sai lầm engineering kiểm được bằng cách mở repo

Không CI/CD · bỏ qua evaluation · README sơ sài · không có environment setup rõ · gộp hết code vào
một file · không viết test.

Cả sáu đều **không phải lỗi thuật toán**, nên mentor không-biết-code vẫn phát hiện được: mở repo ra
xem là thấy.

## 6. Danh mục version — có NGÀY HẾT HẠN

> Verify **2026-09**. Đầu mỗi cohort phải review lại trước khi phát cho học viên.

| Thành phần | Phiên bản đề xuất (2026-09) | Ngày cần review |
|---|---|---|
| Python | 3.12.x | mỗi cohort |
| LangGraph / LangChain | 1.2.x / 1.4.x | mỗi cohort (breaking changes thường xuyên) |
| FastAPI / Pydantic | 0.141.x / 2.13.x | 6 tháng |
| Model OpenAI | gpt-4o / gpt-4o-mini / gpt-4.1 | mỗi cohort — kiểm model đã retire chưa |
| Model Anthropic | claude-sonnet-4-5 / claude-haiku-4-5 | mỗi cohort |
| Model Google | gemini-2.5-pro / gemini-2.5-flash | mỗi cohort |
| RAGAS | 0.4.x | mỗi cohort |
| ~~gpt-3.5-turbo~~ | ĐÃ RETIRE — không dùng | — |
