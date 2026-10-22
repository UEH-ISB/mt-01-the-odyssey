# MT-01 — Repo khởi đầu · The Odyssey

Repo khởi đầu cho khoá **Đào tạo mentor mới — The Odyssey**. Bạn build repo này suốt 15 buổi,
từ con số không đến deploy thật, bằng cách **làm việc cùng AI agent** ngay trong repo.

Bạn **không cần biết code**. Bạn cần chạy được vài lệnh và **đọc được kết quả**.

| | |
|---|---|
| **Giáo trình 15 buổi** | [`docs/giao-trinh/`](docs/giao-trinh/) — bài học, khung đánh giá (Gates, deliverables, rubric). Nguồn duy nhất, nằm ngay trong repo |
| **Đề bài** | [`docs/de-bai.md`](docs/de-bai.md) — MT-01, hệ thống AI agent chuẩn hóa chất lượng đào tạo |
| **Quy tắc cho agent** | [`CLAUDE.md`](CLAUDE.md) · [`.agents/rules/hoc-cung-ai.md`](.agents/rules/hoc-cung-ai.md) |
| **Repo này đã lược gì** | [`DA-LUOC.md`](DA-LUOC.md) — template 94 file → repo này 68 file, kèm lý do từng mục |

---

## Đội ngũ

Bảng này là **bài tập đầu tiên của buổi 01**: mỗi người tự thêm tên mình vào **đúng dòng của
mình**, bằng một Pull Request — không ai sửa thẳng vào `main`.

| # | Tên | GitHub | Vai mình nhận trong đội | Câu mình chưa rõ về MT-01 |
|---|---|---|---|---|
| 1 | _(chưa điền)_ | _(chưa điền)_ | _(chưa điền)_ | _(chưa điền)_ |
| 2 | _(chưa điền)_ | _(chưa điền)_ | _(chưa điền)_ | _(chưa điền)_ |
| 3 | _(chưa điền)_ | _(chưa điền)_ | _(chưa điền)_ | _(chưa điền)_ |
| 4 | _(chưa điền)_ | _(chưa điền)_ | _(chưa điền)_ | _(chưa điền)_ |
| 5 | _(chưa điền)_ | _(chưa điền)_ | _(chưa điền)_ | _(chưa điền)_ |
| 6 | _(chưa điền)_ | _(chưa điền)_ | _(chưa điền)_ | _(chưa điền)_ |

**Ba luật của bảng này, đọc trước khi sửa:**

1. **Chỉ sửa dòng số của bạn.** Người dạy giao số ở đầu buổi. Hai người sửa cùng một dòng thì git
   báo **xung đột** — đó là bài của buổi sau, hôm nay tránh.
2. **Không ai merge PR của chính mình.** PR của bạn do người bên cạnh đọc và merge.
3. **Merge xong phải `git pull`.** Không pull thì máy bạn vẫn là bản cũ, và tuần sau bạn sẽ hỏi
   "sao máy em không có file đó".

Các bước cụ thể: [`docs/giao-trinh/buoi-01.md`](docs/giao-trinh/buoi-01.md) — Thực hành, bước 3.

---

## Chạy lần đầu

Cần sẵn: Python 3.12 (hoặc 3.11), Git. Kiểm: `python3 --version`.

```bash
python3 -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt
make run
```

Mở `http://localhost:8000/docs` — Swagger UI. Mở tab terminal thứ hai, giữ server chạy:

```bash
curl -s localhost:8000/health       # phải ra {"status":"ok", ...}
```

**Swagger UI là giao diện bấm được để thử API.** Chọn một endpoint → **Try it out** → **Execute**.
Đây là công cụ bạn dùng lại suốt 6 tuần để duyệt API của đội **mà không cần viết code**.

> ⚠️ **Đừng làm theo lệnh cài trong README của template gốc.** Nó bảo
> `pip install -e ".[dev]"`, nhưng repo **không có `pyproject.toml`** nên lệnh đó lỗi. Dùng
> `pip install -r requirements.txt`. Đây là chỗ tài liệu lệch code đầu tiên bạn gặp — và là bài
> học của buổi 02, không phải lỗi của bạn.

---

## `make test` sẽ ra đỏ. Đó là đúng.

```bash
make test        # 2 passed, 3 errors
```

Template có `pytest-asyncio` trong `requirements.txt` nhưng **không file nào đặt `asyncio_mode`**,
nên fixture async không được xử lý. **Bạn sẽ tự sửa ở buổi 02** — một file `pytest.ini` ba dòng,
làm cùng agent. Sau đó: `5 passed`.

Nhận một repo đã xanh sẵn thì mất bài học quan trọng nhất của buổi 02:
**CI xanh là thứ bạn làm ra, không phải thứ bạn được phát.**

---

## Cách đọc một thông báo lỗi — kỹ năng dùng lại suốt khoá

Tìm **dòng `ERROR`**, hoặc **dòng cuối của khối `Traceback`**. **Bỏ qua `[notice]` và `WARNING`.**

```
Obtaining file:///...
ERROR: ... neither 'setup.py' nor 'pyproject.toml' found.   ← dòng này
[notice] A new release of pip is available: 24.0 -> 26.2.2  ← không phải dòng này
```

Câu "đọc dòng cuối" chỉ đúng với Traceback của Python. `pip` in `[notice]` **sau** dòng lỗi — ai
đọc đúng dòng cuối cùng sẽ gặp lời khuyên nâng cấp pip, không gặp lỗi.

Đội của bạn sẽ gửi ảnh chụp một màn hình đầy chữ đỏ. Câu hỏi đúng là **"dòng ERROR viết gì?"**,
không phải "em thử lại chưa". Và yêu cầu họ **dán chữ, đừng gửi ảnh** — chữ thì dán được cho agent.

---

## Cấu trúc

```
docs/de-bai.md           đề bài xuyên suốt MT-01
data/
  rubric.yaml            rubric 6 tiêu chí — dữ liệu riêng của đề. Cả 6 đang da_cai: false
  plo.md                 chuẩn đầu ra chương trình (mẫu)
  de-cuong/              đề cương tập, CÓ LỖI CỐ Ý ở cả 6 tiêu chí
src/
  agents/                graph · state · nodes/ · tools/   → buổi 04
  api/ · models/         routes, Pydantic schema            → buổi 08
  services/llm.py        gọi model                          → buổi 04, 06
  config.py · main.py    cấu hình, entry point              → buổi 02
tests/                   test_agents/ · test_api/           → buổi 10
Dockerfile · docker-compose.yml · .github/workflows/        → buổi 12
scripts/                 AI logging hooks, deliverable #4   → buổi 02
ARCHITECTURE.md          deliverable #3                     → buổi 03
JOURNAL.md               deliverable #8 — ghi từ ngày đầu
WORKLOG.md               deliverable #9
eval/                    deliverable #10                    → buổi 10
presentation/            deliverable #7                     → buổi 13
```

`eval/` và `presentation/` đang rỗng. **Đừng xoá** — đó là hai deliverable mà **82% và 64%** đội
cohort trước thiếu, và là hai chỗ dễ ghi điểm nhất tính theo công sức.

---

## AI logging — deliverable #4, một lệnh

```bash
bash scripts/setup_hooks.sh
```

Hook tự log prompt vào `.ai-log/session.jsonl` và submit khi `git push`. Có sẵn cho Claude Code,
Cursor, Codex, Gemini CLI, Copilot, Antigravity.

**Đừng gọi tay** `scripts/log_*` — hook đã tự chạy, gọi tay tạo log giả. Hook báo lỗi thì **đừng**
bypass bằng `--no-verify`: push được nhưng log không được submit.

> 🔴 **Người dạy phải xử lý trước khi phát cho học viên.** `.env.example` đặt sẵn
> `AI_LOG_SERVER=https://ai-logs.note.transformerlabs.ai/api/ingest` — đó là **máy chủ chấm điểm
> của AI20K**, chương trình mà template này sinh ra để dùng. The Odyssey cần **endpoint riêng**,
> hoặc để trống biến đó nếu chưa có hệ thống thu log. Để nguyên nghĩa là gửi prompt của học viên
> sang hệ thống của một chương trình khác.

**Nói rõ với đội:** dùng AI **không** bị trừ điểm. Không log mới bị.

---

## Bốn quy tắc không được phá

1. **Không commit `.env`.** Kiểm: `git check-ignore -v .env` phải có kết quả. Key lỡ lên GitHub thì **thu hồi key**, không phải xoá commit.
2. **`git status` sạch không có nghĩa là đã commit hết.** Thứ bị `.gitignore` chặn không xuất hiện ở đâu cả. Phép kiểm đúng: `git ls-files | grep data` — phải thấy 4 file.
3. **Không dùng dữ liệu sinh viên thật.** Đề cương công khai hoặc tự soạn giả lập.
4. **Agent không tự công bố.** Báo cáo ở trạng thái `PENDING` tới khi có người duyệt. Buổi 07 dựng cơ chế chặn bằng code.

---

## Nguồn

| Thành phần | Nguồn |
|---|---|
| Khung dự án, `src/`, `tests/`, Docker, CI, AI logging hooks | [`hailoc12/ai20k-agent-template`](https://github.com/hailoc12/ai20k-agent-template) — template của AI20K, kế thừa cho The Odyssey |
| Giáo trình 15 buổi ở [`docs/giao-trinh/`](docs/giao-trinh/) | Soạn cho The Odyssey. **Đây là tài liệu học duy nhất của khoá** — không cần mở tài liệu nào bên ngoài |
| Gates G1–G6, 10 deliverables, rubric 6 tiêu chí × 5 mức | Kế thừa từ AI20K. **Đầu mỗi cohort The Odyssey phải đối chiếu lại** — trọng số từng tiêu chí có thể khác |
| Đề bài MT-01, rubric 6 tiêu chí, dữ liệu tập | Soạn cho khoá The Odyssey |

Phần `docs/guide/` của template **đã bị xoá** khỏi repo này: đó là một bộ tài liệu song song mà
khoá không dạy theo, và giữ hai bộ lệch nhau trong một repo là cách nhanh nhất để agent trả lời
sai rất tự tin. Lý do và bảng lược–giữ đầy đủ: [`DA-LUOC.md`](DA-LUOC.md).
