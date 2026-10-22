# Buổi 01 — Sáu tuần, sáu cổng, sáu tiêu chí: mentor sẽ bị hỏi gì

> Nguồn của buổi này: [khung đánh giá — Gates, deliverables, rubric](00-khung-danh-gia.md)
> · đề bài: [MT-01](../de-bai.md)
> Thời lượng 120' · Trong repo dự án, hai file trên nằm ở `docs/giao-trinh/` và `docs/de-bai.md`

## Mentor xong buổi này phải làm được

- Nói đúng **không cần tra tài liệu**: tuần nào là gate nào, và bằng chứng tối thiểu của gate đó là gì.
- Chỉ ra **3 deliverable mà cohort trước thiếu nhiều nhất** và giải thích vì sao đó lại là chỗ dễ ghi điểm nhất.
- **Bắt được một câu agent nói sai** về chính repo của đội, rồi chứng minh bằng một lệnh chạy ra số.
- **Tự mở một PR và review PR của người khác** — nhánh, commit, push, PR, review, merge, pull. Đây là sáu động tác GitHub dùng lại suốt 15 buổi.

## Câu đội sẽ hỏi mà buổi này trả lời

1. *"Tuần này em nộp gì là đủ ạ?"*
2. *"Sản phẩm em chạy được rồi, vậy là xong chưa anh?"*
3. *"Em để AI viết gần hết code, có bị trừ điểm không?"*

## Điều kiện trước

Đã đọc [đề bài MT-01](../de-bai.md) (10 phút). Đã cài agent coding, Git, và đăng nhập
`gh` (`gh auth status` phải báo logged in). Chưa cần Python.

**Đã bấm nhận lời mời collaborator** vào repo đội — người dạy gửi trước buổi một ngày. Chưa nhận
thì bước 3 của lab không chạy được.

---

## Slide (30 phút)

| # | Tiêu đề slide | Nội dung trên slide | Người trình bày nói gì |
|---|---|---|---|
| 1 | Mentor không phải người giỏi code nhất trong phòng | Hai việc của mentor: **ra yêu cầu** và **nghiệm thu**. Không có việc thứ ba là "viết hộ code". | Mở đầu bằng lo lắng thật của phòng: "em không biết code thì mentor kiểu gì". Trả lời ngay: 15 buổi này không dạy bạn code, dạy bạn kiểm. Người nghiệm thu nhà không cần biết trộn vữa, nhưng phải biết gõ vào tường. |
| 2 | 6 tuần, 6 cổng | Sơ đồ 6 cổng ở khung đánh giá §1: W1 G1 idea+USP → W2 G2 architecture → W3 G3 agent+tools+guardrail → W4 G4 deploy+eval → W5 G5 before/after+tổng duyệt → W6 G6 Demo Day. | Chỉ vào từng mũi tên. Nhấn: gate là **cổng**, không phải mốc báo cáo. Đội trễ G2 ba ngày thì G3 không thể đúng hạn — trễ cộng dồn, không tự khỏi. |
| 3 | Bằng chứng tối thiểu từng gate | Bảng G1–G6 ở `docs/giao-trinh/00-khung-danh-gia.md` §1, cột "bằng chứng tối thiểu" — chép nguyên văn lên slide. | Đọc chậm cột bằng chứng. Đây là câu trả lời cho câu hỏi số 1 của đội — và mentor phải thuộc để không phải hẹn "mai anh xem lại rồi trả lời". |
| 4 | At-risk triggers | Miss standup 2 ngày liên tiếp · không commit 3 ngày · gate trễ >3 ngày. | Ba trigger này là việc của mentor, không phải của BTC. Thấy trigger thì mở Playbook L1/L2/L3 bên giáo trình vận hành. |
| 5 | "Chạy được" không phải "xong" | 10 deliverables kèm vị trí trong repo (khung đánh giá §3). | Đây là câu trả lời cho câu hỏi số 2. Nhiều đội mất điểm **không phải vì code kém mà vì thiếu hồ sơ**. Nói thẳng: code chạy là điều kiện cần, hồ sơ là điều kiện đủ. |
| 6 | Ba con số phải thuộc | Video demo **thiếu 11/11 đội (100%)** · Evaluation Evidence **thiếu ~82%** · Pitch Deck **thiếu ~64%**. | Ba con số này là vũ khí coaching mạnh nhất của mentor. Đội nào làm được ba thứ này thì gần như chắc chắn vượt phần lớn các đội còn lại — mà cả ba đều **không cần code giỏi hơn**. |
| 7 | 6 tiêu chí Demo Day × 5 mức | Bảng 6 tiêu chí: Hoàn thiện sản phẩm · Kỹ thuật AI · Evaluation · UI/UX · Ý tưởng & ứng dụng thật · Pitch. Thang 9-10 Giỏi, 7-8 Khá, 5-6 TB, 3-4 Yếu, 1-2 Kém. | Nói rõ giới hạn: tên tiêu chí và mô tả mức ổn định, **trọng số có thể đổi theo cohort** — đầu mỗi cohort phải đối chiếu với bản BTC phát hành. Mentor nói chắc chắn về thứ chưa chắc là cách mất uy tín nhanh nhất. |
| 8 | Sáu sai lầm engineering | Không CI/CD · bỏ qua evaluation · README sơ sài · không có environment setup rõ · gộp hết code vào một file · không viết test. | Sáu thứ này không phải lỗi thuật toán, là lỗi nghề. Đó là lý do mentor không-biết-code vẫn phát hiện được: cả sáu đều kiểm bằng cách **mở repo ra xem**, không cần đọc logic. |
| 9 | Agent nói rất chắc về thứ nó chưa hề kiểm | Hỏi agent *"repo này có bao nhiêu file?"* và *"`make test` ra gì?"* — nó sẽ trả lời một con số và trả lời "test xanh". Sự thật: **68 file** (người dạy chạy `git ls-files | wc -l` trước buổi để lấy số đúng — số này tăng khi giáo trình thêm buổi) và **2 passed, 3 errors**. | Đây là slide quan trọng nhất của buổi. Agent không sai vì kém — nó sai vì **đoán thay vì chạy**, và nó đoán theo cái quen thuộc nhất (repo nào mà test chẳng xanh). Quy tắc của cả khoá: **câu hỏi về trạng thái repo chỉ được trả lời bằng một lệnh**. Lab bước 4 làm đúng việc này. |
| 10 | Agent là thợ, mentor là người nghiệm thu | Vòng lặp 4 bước: *ra yêu cầu rõ → agent làm → **mentor chạy phép kiểm** → sửa yêu cầu*. Bỏ bước 3 thì ba bước còn lại vô nghĩa. | Trả lời câu hỏi số 3 của đội: dùng AI viết code **không** bị trừ điểm; không kiểm được thứ AI viết mới bị trừ. Suốt 15 buổi, mỗi bài thực hành đều có một phép kiểm — đó là phần được chấm. |
| 11 | Đề xuyên suốt: MT-01 — ba tầng | **Hệ thống AI agent chuẩn hóa chất lượng đào tạo đại học** (case TTN–UEH). **A. Người học**: hỏi đáp grounded + hỗ trợ ESL. **B. Môn học**: rubric 6 tiêu chí, giảng viên duyệt. **C. Chương trình**: số tổng hợp ẩn danh. | Giới thiệu đề, và nói rõ nó **không chỉ là rà đề cương** — đó là tầng B. Ba tầng nối nhau: bản đã duyệt ở B mới là nguồn cho A, số liệu từ B lên C. Ta build thật, deploy thật, eval thật; cuối 15 buổi mỗi mentor có repo đi đủ G1→G6. |
| 12 | Scope guard — thứ MT-01 cố tình không làm | Không LMS thật · không chấm điểm sinh viên · không multi-agent · không fine-tune · không Next.js ở bản cơ bản. | Đọc to danh sách này và dán lên tường. Mentor sẽ phải nói đúng câu này với đội của mình vào tuần 3, khi đội muốn thêm tính năng thay vì hoàn thiện cái đang có. |
| 13 | Lab hôm nay: bốn người, một đội, bốn PR | Sáu động tác: `switch -c` → `add` → `commit` → `push` → `pr create` → **người khác** `pr merge`. Luật: **không ai tự merge PR của mình.** | Slide cuối, chiếu ngay trước khi vào lab. Nói rõ cả lứa là **một đội, một repo** — vì cái khó của GitHub không phải gõ lệnh, mà là bốn người sửa cùng một repo không đè lên nhau. Và nói luật không-tự-merge như một luật của đội, không phải thủ tục: đó chính là nghề mentor thu nhỏ thành một động tác. Ai chưa bấm nhận lời mời collaborator thì gỡ ngay bây giờ, đừng để đến bước 3. |

---

## Thực hành (60 phút) — PR đầu tiên trong đời, và một đội bốn người

**Cả lứa mentor là MỘT đội, làm trên MỘT repo.** Không phải mỗi người một repo. Đội thật của họ
sau này cũng làm vậy, và cái khó của GitHub không nằm ở chỗ gõ lệnh — nằm ở chỗ **bốn người sửa
cùng một repo mà không đè lên nhau**. Buổi 01 dạy đúng chỗ đó.

**Sau lab, repo đội có:** một file giới thiệu cho mỗi người trong `docs/doi-ngu/`, bốn PR đã
merge — **mỗi PR do người khác merge, không ai tự merge của mình** — và một dòng `JOURNAL.md` ghi
chỗ agent nói sai.

> **Người dạy chuẩn bị trước buổi** (10', làm trước **một ngày** vì lời mời collaborator cần được
> bấm nhận):
> ```bash
> gh repo create <ORG>/mt-01-the-odyssey --template <ORG>/mt-01-starter --public
> for u in <user1> <user2> <user3> <user4>; do
>   gh api -X PUT "repos/<ORG>/mt-01-the-odyssey/collaborators/$u" -f permission=push
> done
> ```
> Muốn ép bước review thì bật ruleset cho `main`: **Require a pull request before merging** +
> **Require 1 approval**. Lúc đó không ai push thẳng vào `main` được, kể cả người dạy — đó là
> điều đúng, nhưng nói trước với phòng để không ai tưởng máy mình hỏng.

### Bước 1 — Clone repo đội (8')

Bạn **không** tạo repo. Repo đội đã có, bạn là **collaborator** — nghĩa là có quyền đẩy nhánh lên.

```bash
gh repo clone <ORG>/mt-01-the-odyssey
cd mt-01-the-odyssey
ls docs/giao-trinh/ docs/de-bai.md   # giáo trình và đề bài nằm sẵn trong repo
```

**Phép kiểm:** `git remote -v` trỏ về `<ORG>/mt-01-the-odyssey`, và
`gh api repos/<ORG>/mt-01-the-odyssey/collaborators --jq 'length'` ra **4**.

Chưa clone được, hoặc báo `Permission denied`? Chưa bấm nhận lời mời collaborator trong hộp thư
GitHub. Gỡ ngay tại chỗ, đừng để đến bước 3.

### Bước 2 — Đọc đề bài và khung 6 tuần (10')

**Chưa cài gì cả.** Dựng môi trường là buổi 02; hôm nay chỉ đọc và dùng GitHub.

```bash
cat docs/de-bai.md | head -60
ls docs/giao-trinh/
```

**Đạt khi** bạn trả lời được ba câu, không mở lại file:
1. Hệ thống MT-01 có **mấy tầng**, và chuẩn hóa đề cương là tầng nào?
2. Ba ràng buộc nào khiến đề này **không phải** một chatbot bọc API?
3. Thư mục nào trong repo ứng với deliverable số 10 — thứ 82% đội cohort trước bỏ trống?

Câu một hay trả lời sai thành "hệ thống rà đề cương". Đề có **ba tầng**: trợ lý người học
(hỏi đáp grounded + hỗ trợ ESL), chuẩn hóa môn học (rubric 6 tiêu chí + giảng viên duyệt), và
bức tranh chương trình (số tổng hợp ẩn danh). Rà đề cương là tầng giữa.

### Bước 3 — PR đầu tiên: thêm tên mình vào đội (22') ⭐ bài tập cốt lõi của buổi

Sáu động tác dưới đây là **toàn bộ** GitHub bạn cần trong 15 buổi. Người dạy đọc to cột "nghĩa là
gì" trước khi phòng gõ:

| Động tác | Nghĩa là gì |
|---|---|
| `git switch -c <nhánh>` | Mở một bản nháp riêng của bạn. Người khác không thấy, không ai bị đè. |
| `git add <file>` | Chọn đúng file muốn nộp. Không phải mọi thứ bạn đã sửa. |
| `git commit -m "..."` | Đóng gói + ghi lý do. Câu này người khác sẽ đọc, kể cả bạn 3 tuần sau. |
| `git push -u origin <nhánh>` | Đẩy bản nháp lên GitHub. Vẫn **chưa** vào repo chính. |
| `gh pr create` | Xin ý kiến: "đây là thứ tôi muốn đưa vào". Mở ra chỗ cho người khác đọc. |
| `gh pr merge` | Đồng ý và nhập vào repo chính. **Việc của người review, không của tác giả.** |

Làm từng lệnh, đừng dán cả khối:

```bash
git switch -c gioi-thieu/<ten-ban>
mkdir -p docs/doi-ngu
```

Tạo file `docs/doi-ngu/<ten-ban>.md`, đúng năm dòng (mỗi người **một file riêng**, nên không ai
đụng vào dòng của ai — xung đột là bài của buổi 02, không phải hôm nay):

```markdown
# <Tên bạn>

- **Vai mình nhận trong đội:** <ví dụ: người theo deliverables / người theo eval / người theo demo>
- **Việc mình làm hằng ngày:** <một dòng>
- **Hôm nay mình đã tự chạy được lệnh:** <dán đúng một lệnh bạn đã chạy>
- **Câu mình chưa rõ về đề MT-01:** <một câu hỏi thật, không phải câu cho đẹp>
```

```bash
git add docs/doi-ngu/<ten-ban>.md
git commit -m "docs: them <ten-ban> vao doi ngu"
git push -u origin gioi-thieu/<ten-ban>
gh pr create --fill --base main
```

**Phép kiểm:**
```bash
gh pr status                                   # thấy PR của bạn, có số
gh pr view --json files --jq '.files[].path'   # phải ra ĐÚNG MỘT file: của bạn
```

**Đạt khi** PR chỉ chứa một file và file đó là của bạn. PR lẫn file của người khác nghĩa là bạn
đã `git add -A` — đó là lỗi phổ biến nhất của đội ở tuần 1, và là lý do mentor phải biết đọc cột
**Files changed** trước khi bấm merge.

### Bước 4 — Review PR của người bên cạnh (12') ⭐

**Luật của đội, từ hôm nay đến hết khoá: không ai tự merge PR của mình.** Đây không phải thủ tục
hành chính — đây là nghề mentor, thu nhỏ lại thành một động tác.

```bash
gh pr list                        # bốn PR, bốn người
gh pr diff <số-PR-người-bên-cạnh> # đọc ĐÚNG thứ họ đổi, không đọc cả repo
```

Để lại **một câu hỏi thật** về nội dung PR đó, rồi mới duyệt:

```bash
gh pr review <số> --comment -b "Câu bạn chưa rõ về MT-01 là ... — bạn muốn trả lời nó bằng tầng nào của đề?"
gh pr review <số> --approve
gh pr merge  <số> --squash --delete-branch
```

Xong rồi lấy kết quả của cả đội về máy mình:

```bash
git switch main && git pull
ls docs/doi-ngu/                  # giờ có file của người khác trên máy bạn
```

**Đạt khi** `ls docs/doi-ngu/` thấy **≥2 file**, và `gh pr list --state merged` cho thấy PR của bạn
do **người khác** merge.

Hai thứ phải nói rõ ở bước này, vì đội sẽ mắc đúng hai thứ đó:
- **Merge rồi không `git pull` thì máy bạn vẫn là bản cũ.** Câu "sao máy em không có file đó" ở
  tuần 3 gần như luôn là chuyện này.
- **Approve mà không mở `Files changed`** là ký khống. Mentor ký khống một lần thì cả đội học theo.

### Bước 5 — Hỏi agent hai câu, rồi đòi lệnh (8')

Giờ mới mở agent. Hai câu, mỗi câu một lượt riêng:

```
1. Repo này có bao nhiêu file đang được git theo dõi? Trả lời bằng một số.
2. Nếu tôi chạy `make test` bây giờ thì ra kết quả gì?
```

**Phép kiểm — tự chạy, đừng tin:**
```bash
git ls-files | wc -l
make test        # chưa dựng môi trường thì ghi lại nguyên văn lỗi, đó cũng là một kết quả
```

**Đạt khi** `JOURNAL.md` có một dòng theo mẫu, và nó nằm trong PR tiếp theo của bạn:

```
2026-10-21 · Câu 2: agent trả lời "test sẽ pass hết". SAI. Chạy thật: 2 passed, 3 errors.
  Nguyên nhân: agent đoán theo cái quen thuộc, không chạy lệnh nào.
  Bài học: câu hỏi về trạng thái repo phải trả lời bằng lệnh — ở đây là make test.
```

**Điều quan trọng nhất của buổi học nằm ở đây.** Agent nói sai với giọng rất chắc chắn, và nó sai
**vì đoán thay vì chạy** — không phải vì kém. Đội của bạn sẽ nhận đúng kiểu câu trả lời này hàng
ngày: tự tin, hợp lý, và không ai kiểm. Mentor phân biệt được "agent đã chạy và báo lại" với
"agent đoán rồi nói như đã chạy" thì coaching được; không phân biệt được thì chỉ biết nói "em
kiểm tra lại đi".

> Nếu agent của bạn cẩn thận và tự chạy lệnh trước khi trả lời (bản mới hay làm vậy): vẫn làm
> bước này, nhưng **ghi lại lệnh nó đã chạy** rồi tự chạy lại và so kết quả. Mục tiêu là **thói
> quen đòi nguồn và đòi lệnh**, không phải bắt agent sai.

---

## Bài tập chéo (20 phút) — Đổi vai, 15 phút breakout

Ghép cặp. A làm **trưởng đội tuần 1**, B làm **mentor**. Sau 15' đổi vai.

**A nói đúng ba câu này (đọc nguyên văn, đừng diễn thêm):**
1. *"Nhóm em chốt đề rồi, tuần này em dựng Docker với CI trước cho chắc."*
2. *"Em chạy thử thấy agent trả lời ổn, nên em bỏ phần eval để dành thời gian làm thêm 2 tính năng."*
3. *"Anh code hộ em cái API nhỏ này được không, em bị kẹt 2 ngày rồi."*

**B phải làm được ba việc:**
- Với câu 1: chỉ ra đội đang ở **cổng G1**, chưa qua G2, và hỏi *"đã có 5 user thật xem prototype chưa?"* trước khi đồng ý cho dựng hạ tầng.
- Với câu 2: trả lời bằng **con số 82%**, không bằng lời khuyên chung. Và hỏi *"ổn là bao nhiêu phần trăm, trên bộ test nào?"*
- Với câu 3: **từ chối viết code hộ**, và đổi thành một câu hỏi mở về chỗ kẹt. Viết code hộ đội là cách nhanh nhất để tuần 6 đội không bảo vệ được sản phẩm của chính mình.

**Chấm:** mỗi việc làm được 1 điểm. B dưới 2/3 thì làm lại với cặp khác.

---

## Bài về nhà

1. Đọc hết [`docs/de-bai.md`](../de-bai.md) và [khung đánh giá](00-khung-danh-gia.md). Đọc hết, không skim — buổi 02 trở đi mọi phép kiểm đều trỏ về hai file này.
2. **Bản đồ 6 cổng — tự tay điền trước, agent viết sau.** Mở file nháp, tự điền bảng `Tuần | Gate | Bằng chứng tối thiểu | Buổi của khoá` sáu dòng, **không mở `00-khung-danh-gia.md`**. Điền xong mới gửi prompt:
   ```
   Đọc docs/giao-trinh/00-khung-danh-gia.md mục 1 và mục 2. Viết docs/00-ban-do-khoa.md gồm bảng
   6 dòng: Tuần | Gate | Bằng chứng tối thiểu | Buổi của khoá. Trích nguyên văn cột bằng chứng,
   không diễn giải lại. Cuối file thêm mục "At-risk triggers" cũng trích nguyên văn.
   ```
   Rồi so bản của bạn với bản của agent: `diff <(sed -n '/^|/p' nhap.md) <(sed -n '/^|/p' docs/00-ban-do-khoa.md)`. **Đạt khi** bạn đọc được mọi chỗ lệch và nói được ai đúng. Chỗ bạn nhớ sai hôm nay chính là chỗ đội sẽ hỏi tuần sau. Nộp bằng một PR nữa — PR thứ hai trong đời, lần này không ai hướng dẫn từng lệnh.
3. **Cài sẵn Python 3.12 (hoặc 3.11)** và kiểm bằng `python3 --version`. Buổi 02 dựng môi trường; ai chưa cài được thì mất nửa buổi.
4. Viết nháp **5 use case** của MT-01: 5 nhóm người cụ thể + việc cụ thể họ cần làm. Chưa cần chọn, chỉ cần liệt kê — buổi 05 dùng lại danh sách này.

---

## Tiêu chí đạt buổi 01

- [ ] Clone được repo đội; `git remote -v` trỏ về repo đội
- [ ] **PR của bạn đã merge, và do người khác merge** — `gh pr list --state merged` xác nhận
- [ ] Bạn đã review PR của người khác: có **một comment nội dung** trước khi approve
- [ ] `git pull` xong, `ls docs/doi-ngu/` thấy file của người khác trên máy bạn
- [ ] Nói được MT-01 có **ba tầng** và ba ràng buộc khiến nó không phải chatbot bọc API
- [ ] `JOURNAL.md` ghi ≥1 chỗ agent nói sai về trạng thái repo, kèm **nguyên nhân** và **lệnh đã chạy**
- [ ] Nói được 6 gate + 3 con số (100% / 82% / 64%) mà không mở tài liệu
- [ ] Bài tập chéo đạt ≥2/3

Không đạt ô thứ sáu thì **không sang buổi 02**. Các ô khác là thủ tục và còn sửa được; ô thứ sáu
là nghề.
