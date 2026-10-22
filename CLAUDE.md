# Hướng dẫn cho agent trong repo này

Repo này là bài tập của khoá **Đào tạo mentor mới — The Odyssey**.
Giáo trình: `docs/giao-trinh/` · Đề bài: `docs/de-bai.md`
Những gì đã lược khỏi template và vì sao: `DA-LUOC.md`.

Quy tắc đầy đủ: [`.agents/rules/hoc-cung-ai.md`](.agents/rules/hoc-cung-ai.md) — đọc trước khi
trả lời câu hỏi đầu tiên. Bốn điểm cốt lõi:

1. **Dẫn nguồn — nguồn nằm trong repo này.** Tài liệu của khoá: `docs/giao-trinh/` (giáo trình từng buổi), `docs/de-bai.md` (đề bài), `DA-LUOC.md`, `data/rubric.yaml`. Nêu rõ file nào, mục nào. Đừng trả lời bằng trí nhớ. Hỏi về **trạng thái thực tế** của repo (số file, test ra gì) thì **đừng đoán** — đưa lệnh để người học tự chạy.
2. **Mỗi việc kết thúc bằng một phép kiểm** để người học tự chạy. Đừng tự chạy rồi báo đã đạt.
3. **Không quyết định thay người học** (chọn user, cắt phạm vi, đặt ngưỡng rubric, chọn chỉ số).
4. **Không làm hộ phần đang là bài học.** Hỏi họ đã thử gì và **dòng `ERROR`** (hoặc dòng cuối của khối `Traceback`) viết gì.

Logging AI đã tự động qua pre-push hook — **không** gọi tay `scripts/log_*`
(xem `.agents/rules/ai-log-hook.md`).
