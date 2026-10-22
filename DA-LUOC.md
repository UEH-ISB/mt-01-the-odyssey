# Đã lược những gì khỏi template, và vì sao

Repo này là **template chính thức của chương trình**
([`hailoc12/ai20k-agent-template`](https://github.com/hailoc12/ai20k-agent-template)),
giữ nguyên file của nó, chỉ **xoá phần cả 15 buổi không dùng tới**.

| | Template gốc | Repo này |
|---|---|---|
| Số file git theo dõi | 94 | **68** |
| Trong đó `docs/guide/` | 37 file · 436 KB · ~3/4 dung lượng | đã xoá |

## Đã xoá

| Đã xoá | Số file | Vì sao |
|---|---|---|
| `docs/guide/` | 37 | Sổ tay kỹ thuật bundled. **Khoá The Odyssey dùng giáo trình riêng** ở `docs/giao-trinh/`, không dùng sổ tay làm tài liệu học. Giữ lại một bộ tài liệu song song mà không ai dạy theo là nguồn gây sai nghiêm trọng nhất: agent đọc nó rồi trả lời rất tự tin về thứ không có trong khoá. |
| `docs/COHORT3_REPORT.md` | 1 | Báo cáo tổng hợp của BTC về Cohort 3 — tài liệu tham khảo, không dùng trong buổi nào. Bản online: [docs/COHORT3_REPORT.md](https://github.com/hailoc12/ai20k-agent-template/blob/main/docs/COHORT3_REPORT.md) |

Hai mục trên chiếm **38/94 file** và khoảng **3/4 dung lượng** của template. Phần còn lại
(94 − 38 = **56 file**) cộng **12 file thêm vào** = **68 file**:

| Thêm vào | Số file |
|---|---|
| Quy tắc cho agent: `CLAUDE.md`, `.agents/rules/hoc-cung-ai.md` | 2 |
| Bảng này: `DA-LUOC.md` | 1 |
| Dữ liệu đề bài: `data/rubric.yaml`, `data/plo.md`, `data/de-cuong/` | 4 |
| Đề bài: `docs/de-bai.md` | 1 |
| Giáo trình: `docs/giao-trinh/` | 4 |

`README.md` bị **ghi đè** (bản của khoá thay bản template) nên không cộng thêm file. Số 68 sẽ
tăng khi giáo trình soạn xong buổi 03–15.

## Giữ lại — và buổi nào dùng

Mọi thứ còn lại đều có buổi dùng tới. Đây là lý do danh sách xoá ngắn.

| Giữ | Dùng ở buổi |
|---|---|
| `src/agents/` — `graph.py`, `state.py`, `nodes/`, `tools/` | **04** — Agent Core: state, node, edge, tools |
| `src/api/routes.py`, `src/models/schemas.py` | **08** — FastAPI, Pydantic, kiểm quyền trong handler |
| `src/services/llm.py`, `src/config.py`, `src/main.py` | **02** chạy server · **04**, **06** gọi model |
| `tests/` — `conftest.py`, `test_agents/`, `test_api/` | **10** — Evaluation |
| `Dockerfile`, `docker-compose.yml`, `.dockerignore`, `.github/workflows/` | **12** — DevOps |
| `ruff.toml` | **12** — CI chạy `ruff check` |
| `scripts/` (9 file) + `.claude/` `.codex/` `.cursor/` `.gemini/` `.github/hooks/` `.agents/` | **02** — AI logging, deliverable #4 |
| `ARCHITECTURE.md` | **03** — kiến trúc, deliverable #3 |
| `JOURNAL.md`, `WORKLOG.md` | deliverable #8, #9 — ghi từ ngày đầu |
| `README_boilerplate.md` | **02**, **13** — deliverable #2 |
| `eval/`, `presentation/` | **10**, **13** — deliverable #10, #7 |

### Vì sao không xoá bớt `scripts/` dù khoá không dùng hết 6 công cụ AI

Đã kiểm, và kết quả khác với dự đoán ban đầu: `scripts/setup_hooks.sh` **hardcode**
`scripts/log_antigravity.py --auto` vào pre-push hook, còn cả 5 file cấu hình công cụ
(`.claude`, `.codex`, `.cursor`, `.gemini`, `.github/hooks`) đều trỏ về `scripts/log_hook.py` và
`scripts/_pyrun.sh`. Xoá bất kỳ file nào trong `scripts/` là làm pre-push hook báo lỗi mỗi lần
push. Chín file, 56 KB — giữ nguyên rẻ hơn sửa chuỗi phụ thuộc.

Hai file `setup_hooks.ps1` và `_pyrun.cmd` là bản Windows. Mentor dùng Windows cần chúng.

## Overlay thêm vào

| Thêm | Vì sao |
|---|---|
| `docs/giao-trinh/` | **Giáo trình từng buổi** — tài liệu học chính của khoá |
| `docs/de-bai.md` | Đề bài xuyên suốt MT-01. Nguồn duy nhất nằm ở thư mục giáo trình, script copy vào |
| `data/rubric.yaml` | Rubric 6 tiêu chí — **dữ liệu riêng** của đề, thứ khiến hệ thống không phải AI wrapper |
| `data/plo.md`, `data/de-cuong/` | PLO mẫu và đề cương tập **có lỗi cố ý ở cả 6 tiêu chí** |
| `CLAUDE.md`, `.agents/rules/hoc-cung-ai.md` | Quy tắc để agent làm việc **cùng** học viên, không làm hộ |

**Overlay không thêm dòng code nào.** Rubric chỉ là dữ liệu; code đối chiếu là bài của buổi 04,
viết trên `src/agents/` của template. Vì vậy buổi 01–03 cả 6 tiêu chí đều chưa được cài — đúng
sự thật, và đó là điểm đáng dạy: sản phẩm AI yếu là sản phẩm **báo ĐẠT cho thứ nó chưa hề kiểm**.

## Trạng thái cần biết trước

Ba chỗ dưới đây có thật trong template, **cố tình không sửa** — chúng là bài tập đối chiếu tài
liệu với code ở buổi 02.

| Chỗ | Kiểm bằng | Xử lý |
|---|---|---|
| `README.md` bảo `pip install -e ".[dev]"` nhưng repo **không có `pyproject.toml`** → lệnh lỗi. Đúng là `pip install -r requirements.txt` | `ls pyproject.toml` | Học viên tự gặp ở buổi 02 |
| `make test` ra **2 passed, 3 errors**: có `pytest-asyncio` trong `requirements.txt` nhưng không file nào đặt `asyncio_mode` | `make test` | Học viên **tự sửa** ở buổi 02 — thêm `pytest.ini` 3 dòng |
| `.github/workflows/ci.yml` khai `pull_request: branches: [main]` → **PR nhắm vào `develop` không kích hoạt CI** | `grep -A5 "^on:" .github/workflows/ci.yml` | Biết để không kết luận "CI hỏng" |

`README.md` gốc của template mô tả `docs/guide/` và "Technical Guidebook 10 chương" — hai thứ repo
này đã xoá, nên README đó đã được viết lại. Giáo trình của The Odyssey ở `docs/giao-trinh/` là tài
liệu học duy nhất; không cần đọc sổ tay kỹ thuật của chương trình tiền nhiệm.

## Một chỗ **đã sửa**, không để làm bài tập

`.gitignore` của template có dòng `data/` — bỏ qua cả thư mục `data/`, hợp lý cho dữ liệu runtime
và file DB. Nhưng dữ liệu **đề bài** cũng nằm trong `data/`, nên `rubric.yaml`, `plo.md` và đề
cương mẫu sẽ **không bao giờ được commit** — mà `git status` vẫn báo sạch, nên không ai phát hiện
cho tới khi người khác clone về và thấy thiếu dữ liệu.

`dung-repo.sh` sửa bằng cách đổi `data/` thành `data/*` rồi mở lại đúng ba mục của đề bài:

```gitignore
data/*
!data/rubric.yaml
!data/plo.md
!data/de-cuong/
```

Vì sao phải đổi `data/` thành `data/*`: git **không đi vào** một thư mục đã bị loại, nên viết
`!data/rubric.yaml` mà vẫn để `data/` thì dòng mở lại đó không có tác dụng.

Chỗ này **sửa luôn** chứ không để làm bài tập, vì để nguyên thì mất dữ liệu đề bài một cách im
lặng — đó là tai nạn, không phải bài học. Nhưng nó đáng kể lại ở buổi 02 như một câu:
**`git status` sạch không có nghĩa là mọi thứ đã được commit.**
