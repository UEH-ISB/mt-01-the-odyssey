# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

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

Logging AI đã tự động qua pre-push hook — **không** gọi tay `scripts/log_*`, **không** sửa
`.ai-log/`, **không** bypass `--no-verify` (xem `.agents/rules/ai-log-hook.md`).

## Lỗi cố ý — đừng tự sửa

Bảng "Trạng thái cần biết trước" trong `DA-LUOC.md` liệt kê chỗ tài liệu lệch code **có chủ đích**
(bài tập buổi 02). Gặp thì chỉ ra hai chỗ lệch, hỏi người học muốn sửa bên nào:

- Không có `pyproject.toml` → `pip install -e ".[dev]"` lỗi; cài bằng `requirements.txt`.
- `make test` ra **2 passed, 3 errors** vì thiếu `asyncio_mode` (người học tự thêm `pytest.ini`).
- CI (`.github/workflows/ci.yml`) chỉ chạy PR nhắm vào `main`, không chạy PR vào `develop`.

`data/` bị `.gitignore` chặn trừ `rubric.yaml`, `plo.md`, `de-cuong/`. `data/de-cuong/` **có lỗi
cố ý ở cả 6 tiêu chí** — là dữ liệu tập, đừng "sửa".

## Lệnh

```bash
python3 -m venv .venv && source .venv/bin/activate && pip install -r requirements.txt
make run          # uvicorn src.main:app --reload, port 8000; Swagger ở /docs, health ở /health
make test         # pytest tests/ -v
pytest tests/test_api/test_routes.py::<tên_test> -v   # một test
make lint         # ruff check src/ tests/   (CI cũng chạy lệnh này)
make format · make typecheck (mypy src/) · make check (lint + format + test)
```

Cấu hình đọc từ `.env` qua `src/config.py` (pydantic-settings, `get_settings()` có `lru_cache`).

## Kiến trúc

Khung FastAPI + LangGraph kế thừa từ `hailoc12/ai20k-agent-template`; phần lớn `src/` còn là code
mẫu, được thay dần theo buổi (bảng "Giữ lại — và buổi nào dùng" trong `DA-LUOC.md`).

- `src/main.py` — app FastAPI, CORS, mount `src/api/routes.py` dưới `/api/v1`, endpoint `/health`.
- `src/agents/graph.py` — `StateGraph(AgentState)`: `analyze` → (có `error` thì `END`) → `respond` → `END`; graph compile thành biến module `agent`. Node ở `nodes/`, tool ở `tools/`, state ở `state.py`.
- `src/services/llm.py` — `get_llm()` trả `ChatOpenAI` theo `model_name`/`openai_api_key` trong config.
- Rubric chỉ là dữ liệu (`data/rubric.yaml`, cả 6 tiêu chí `da_cai: false`); code đối chiếu rubric chưa có — là bài buổi 04. Báo cáo của agent phải ở trạng thái `PENDING` tới khi có người duyệt.
- `ARCHITECTURE.md`, `JOURNAL.md`, `WORKLOG.md`, `eval/`, `presentation/` là deliverable của học viên — đừng xoá dù đang rỗng/khung mẫu.
