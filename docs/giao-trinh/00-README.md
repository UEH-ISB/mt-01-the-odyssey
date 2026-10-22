# Đào tạo mentor mới — The Odyssey · 15 buổi

Giáo trình **15 buổi** cho mentor **chưa từng làm kỹ thuật**, bằng cách cho họ tự build một sản
phẩm thật — **làm việc cùng AI agent ngay trong repo của dự án**.

Giáo trình này **tự đứng một mình**: mọi thứ cần để học một buổi nằm trong file buổi đó và trong
repo dự án. Mentor không phải mở tài liệu nào bên ngoài.

| | |
|---|---|
| **Đề xuyên suốt** | [MT-01 — Hệ thống AI agent chuẩn hóa chất lượng đào tạo đại học](../de-bai.md) · case TTN–UEH |
| **Khung đánh giá** | [Gates G1–G6 · 10 deliverables · rubric 6×5](00-khung-danh-gia.md) — nguồn duy nhất, mọi buổi trỏ về đây |
| **Repo mẫu** | [`repo-mau/`](repo-mau/) — template chính thức, đã lược + giáo trình copy vào repo: **94 file → 68 file** |
| **Giáo trình vận hành** (song song) | [../../02_Quy_trinh_mentor/Giao_trinh_mentor_moi/](../../02_Quy_trinh_mentor/Giao_trinh_mentor_moi/index.html) — 12 buổi coaching, mentor duty, cách chấm Gate |

## Bốn nguyên tắc

**1. Mentor không viết code. Mentor phải kiểm được code.**
Mọi bài thực hành có hai nửa: *prompt gửi cho agent* và **phép kiểm mentor tự chạy**. Nửa thứ hai
là nửa được chấm. Agent viết sai mà mentor không phát hiện thì buổi đó thất bại, dù code chạy.

**2. Học cùng AI, và nguồn nằm trong chính repo.**
`dung-repo.sh` copy giáo trình vào `docs/giao-trinh/` và đề bài vào `docs/de-bai.md` ngay trong
repo dự án, nên agent trả lời được **từ nguồn** mà học viên mở lại được. File `CLAUDE.md` buộc
agent: dẫn nguồn rõ file nào mục nào · kết thúc mỗi việc bằng một phép kiểm để học viên tự chạy ·
không quyết định thay học viên · không làm hộ phần đang học. Riêng câu hỏi về **trạng thái thật của
repo** — bao nhiêu file, test ra gì — agent phải đưa lệnh cho học viên tự chạy, không được đoán.

**3. Repo mẫu là template chính thức, đã lược.**
Không viết lại khung. `repo-mau/dung-repo.sh` clone
[`hailoc12/ai20k-agent-template`](https://github.com/hailoc12/ai20k-agent-template) rồi **xoá đúng
hai mục** cả 15 buổi không dùng tới — `docs/guide/` (37 file tài liệu song song) và
`docs/COHORT3_REPORT.md`. Hai mục đó là 3/4 dung lượng template. Mọi file còn lại đều có buổi dùng
tới; bảng lược-giữ kèm lý do ở `repo-mau/overlay/DA-LUOC.md`. Script thêm vào đó quy tắc cho agent,
dữ liệu tập, đề bài và **giáo trình** (`docs/giao-trinh/`) — **không thêm dòng code nào**; code là
bài của học viên. **94 file → 68 file**, và số này tăng khi soạn xong buổi 03–15.

**4. Mỗi buổi để lại hiện vật.**
Không đảo thứ tự buổi. Mỗi buổi kết thúc bằng một thứ kiểm được trong repo, không kết thúc bằng
"đã hiểu".

## 15 buổi

Mỗi buổi **120 phút**: slide 30' · thực hành 60' · bài tập chéo 20' · chốt 10'.
Nhịp 2 buổi/tuần → **8 tuần**.

| Buổi | Nội dung | Hiện vật trong repo |
|---|---|---|
| [01](buoi-01.md) | Lộ trình 6 tuần, Gates G1–G6, 10 deliverables, 6 tiêu chí chấm · **PR đầu tiên** | `docs/doi-ngu/<tên>.md` qua PR đã merge · `JOURNAL.md` |
| [02](buoi-02.md) | Khởi tạo dự án: môi trường, `.env`, AI logging hooks, git workflow, PR đầu | `make run` chạy · `pytest.ini` · PR merged |
| 03 | Thiết kế kiến trúc 3 tầng, vẽ diagram, viết ADR | `ARCHITECTURE.md` · `docs/adr/0001` |
| 04 | Agent Core: state, nodes, edges, tools, ReAct, error handling | `src/agents/` chạy năng lực B |
| 05 | Từ đề bài đến USP: wrapper test, beachhead, positioning | `usp/` 5 file |
| 06 | RAG thực tế: 4 cấp, chunking, hybrid, citation | năng lực A hỏi đáp có trích nguồn |
| 07 | Workflow & reliability: edge case, failback, **HITL**, governance | `interrupt()` chặn công bố báo cáo |
| 08 | FastAPI: routes, Pydantic, error handling, kiểm quyền trong handler | endpoint thật + 403 cho vai sai |
| 09 | Giao diện: Streamlit 30 phút, nối UI–API, responsive | UI 3 vai, luồng duyệt chạy hết |
| 10 | Evaluation: benchmark 4-tuple, golden dataset ≥45 case, LLM-judge | `eval/` dataset + kết quả vòng 1 |
| 11 | Privacy & governance: mask PII, red-team, cost lock, audit log | `mask_pii` + audit log + case red-team |
| 12 | DevOps: Docker, CI/CD, deploy, đọc log khi hỏng | Live URL + CI xanh |
| 13 | Demo Day: 10 deliverables, rubric 6×5, nghệ thuật demo | 10/10 deliverables + pitch + video |
| 14 | Case studies: học từ 3 thế hệ đội | `docs/mentor-playbook-MT-01.md` |
| 15 | Tài nguyên + Gates G1–G6, nhận đội thật | Bài thi cuối: chấm 1 repo đội thật |

> **Buổi 04 có thể cần hai buổi.** Agent Core là buổi nặng nhất của khoá: state/node/edge, tools,
> ReAct, harness, MCP, loop, cascade, error handling, testing. Người dạy tự quyết tách hay không
> sau khi chạy thử buổi 03; đừng tách sẵn trên giấy.

## Cấu trúc mỗi file buổi

1. **Mentor xong buổi này phải làm được** — hành vi quan sát được, không phải "hiểu".
2. **Câu đội sẽ hỏi mà buổi này trả lời** — lý do buổi học tồn tại.
3. **Slide** — bảng *tiêu đề · nội dung trên slide · người trình bày nói gì*. Dựng deck từ bảng này.
4. **Thực hành** — từng bước: prompt cho agent + **phép kiểm** + điều kiện đạt.
5. **Bài tập chéo** — mentor chấm mentor. Hai định dạng luân phiên: *đổi vai* (một người làm đội,
   một người làm mentor, 15 phút như mentor duty thật) và *gài bẫy* (người dạy làm hỏng một thứ,
   người học phải tìm ra trong 10 phút).
6. **Bài về nhà** · **Tiêu chí đạt** — checklist, không đạt thì không sang buổi sau.

## Điều kiện trước khi vào buổi 01

| Việc | Vì sao |
|---|---|
| Terminal, Python 3.12 (hoặc 3.11), Git, VS Code | Buổi 02 trở đi mentor phải tự chạy lệnh |
| Tài khoản GitHub, `gh` đã đăng nhập, đã vào org của khoá | Buổi 02 tạo repo từ template |
| Agent coding đã cài (Claude Code / Cursor / Codex / Gemini CLI) | Agent là người viết code suốt 15 buổi |
| 1 API key LLM còn hạn mức | Từ buổi 04. Người dạy phát danh sách tài khoản miễn phí còn dùng được ở buổi 01 |
| Đã đọc [đề bài MT-01](../de-bai.md) (10 phút) | Để buổi 01 không mất thời gian giới thiệu đề |

Không cần biết Python. Không cần từng viết API. Không cần hiểu Docker.

## Đánh giá cả chương trình

| Thành phần | Tỷ lệ | Cách chấm |
|---|---|---|
| 15 checklist "Tiêu chí đạt" | 40% | Tự chấm, người dạy spot-check 3 buổi bất kỳ |
| Repo MT-01 cuối kỳ | 30% | Rubric 6 tiêu chí × 5 mức trong [khung đánh giá](00-khung-danh-gia.md) — mentor phải tự đạt ≥7 ở cả 6 |
| Bài thi cuối buổi 15 | 20% | Chấm 1 repo đội Cohort-4 thật + note 5 dòng; mỗi nhận định phải trỏ tới một hiện vật |
| Bài tập chéo | 10% | Số lần tìm ra bẫy / số lần được gài |

**Mốc loại:** mentor không tự chạy được `docker compose up` ở buổi 12 thì chưa nhận đội. Không
phải vì cần biết Docker, mà vì tuần 4 đội sẽ gửi "deploy hỏng rồi anh" và mentor phải mở được log.

## Nguồn và phần kế thừa — nói rõ với người học

Bộ giáo trình này **kế thừa khung của AI20K** và được dùng lại cho **The Odyssey**. Phần kế thừa
và phần soạn mới:

| Thành phần | Nguồn |
|---|---|
| Repo khởi đầu: `src/`, `tests/`, Docker, CI, AI logging hooks | [`hailoc12/ai20k-agent-template`](https://github.com/hailoc12/ai20k-agent-template) — template của AI20K |
| **Gates G1–G6** · **10 deliverables** · **rubric 6 tiêu chí × 5 mức** | Kế thừa từ AI20K |
| Ba con số **100% / 82% / 64%** (video demo · eval evidence · pitch deck thiếu) | Số liệu cohort AI20K trước |
| Giáo trình 15 buổi, đề MT-01, rubric 6 tiêu chí, dữ liệu tập | Soạn cho The Odyssey |

Nền kiến thức kỹ thuật tham khảo khi soạn là sổ tay kỹ thuật nội bộ AI20K Cohort 3. **Giáo trình
này thay thế nó**: mentor không cần và không nên học theo sổ tay đó song song — hai bộ tài liệu
lệch nhau là nguyên nhân sai nghiêm trọng nhất, và `dung-repo.sh` xoá bản bundled trong template
đúng vì lý do này.

**Hai việc The Odyssey phải tự quyết trước khi chạy cohort đầu:**

1. **Đối chiếu lại khung đánh giá.** Gates, số deliverables và trọng số từng tiêu chí là quy định
   của AI20K. Tên tiêu chí và mô tả mức thì ổn định, nhưng **trọng số có thể khác** — lấy bản The
   Odyssey phát hành làm chuẩn cuối cùng, đừng dạy lại con số cũ như thể nó chắc chắn.
2. **Endpoint thu log AI.** `.env.example` của template trỏ sẵn về máy chủ chấm điểm của AI20K.
   The Odyssey cần endpoint riêng, hoặc để trống. `dung-repo.sh` đã bỏ giá trị đó và để lại cảnh
   báo — nhưng **ai dựng repo phải điền endpoint của The Odyssey vào**, nếu không thì deliverable
   #4 không có chỗ nộp.

Ba con số 100/82/64 vẫn dùng được làm công cụ coaching, nhưng khi nói với đội thì nói đúng nguồn:
*"cohort trước của chương trình tiền nhiệm"*, không nói *"cohort trước của The Odyssey"* khi The
Odyssey chưa có cohort nào.

## Giới hạn cần nói thật với người học

- Template chính thức có **ba chỗ tài liệu nói khác code** (README cài bằng `pip install -e`
  nhưng repo không có `pyproject.toml` · `make test` ra 2 passed 3 errors · CI không chạy cho PR
  vào `develop`). Giáo trình **giữ nguyên ba chỗ đó làm bài học buổi 02**, không sửa sẵn.
- Danh mục version thư viện **có ngày hết hạn** — review lại đầu mỗi cohort trước khi dạy.
- MT-01 là **đề tập**, không phải sản phẩm triển khai cho Trường Tài năng. Không dùng dữ liệu sinh viên thật.
- Giáo trình hiện có **buổi 01 và 02**. Buổi 03–15 là bảng nội dung + hiện vật, chưa soạn chi tiết.
  Nói rõ điều này với người học thay vì để họ phát hiện ở buổi 03.

---

> **Ghi chú cho bản trong repo:** vài link trong file này trỏ ra ngoài repo (`repo-mau/`, giáo
> trình vận hành) — những thứ đó nằm ở bộ kit của người dạy. Mọi thứ cần để học một buổi thì nằm
> trong repo: giáo trình ở `docs/giao-trinh/`, đề bài ở `docs/de-bai.md`.
