# Session 01 — Six weeks, six gates, six criteria

**Lecture note** · companion to the slide deck [`buoi-01.html`](buoi-01.html) · course: *Mentoring for AI-aided Projects* · The Odyssey · 120 minutes

> **How to use this note.** Each heading below matches one slide of the deck. Click the slide link to open it (`←` `→` to navigate, `N` for speaker notes).
> Under each slide you will find the key points, a longer explanation, the key terms (English — Vietnamese), and a **self-check**: a command or a question you run or answer yourself.
> Sections marked **Deep dive** go beyond the slides.

**Primary sources — all inside this repository:**

- [`docs/giao-trinh/00-khung-danh-gia.md`](../00-khung-danh-gia.md) — gates, deliverables, rubric. Where this note says "the framework", it means that file.
- `docs/de-bai.md` — the MT-01 project brief.
- [`buoi-01_ex.md`](buoi-01_ex.md) — the exercise book for the 60-minute lab.

Nothing in this course cites a source outside the repository. That is deliberate: it means the agent working in this repo can read every source, and *"where did you read that?"* always has an answer.

---

## Contents

| # | Section | Slides |
|---|---------|--------|
| 1 | [What a mentor is for](#section-1--what-a-mentor-is-for) | 3 – 4 |
| 2 | [Six weeks, six gates](#section-2--six-weeks-six-gates) | 5 – 7 |
| 3 | ["Running" is not "done"](#section-3--running-is-not-done) | 8 – 11 |
| 4 | [MT-01, the project behind every session](#section-4--mt-01-the-project-behind-every-session) | 12 – 16 |
| 5 | [Why the agent sounds certain and is wrong](#section-5--why-the-agent-sounds-certain-and-is-wrong) | 17 |
| L | [The lab](#the-lab--four-people-one-team-one-repo) | 18 – 26 |
| + | [Summary, Q&A, references](#summary-qa-references) | 27 |
| A | [Appendix A — the facilitator's preparation](#appendix-a--the-facilitators-preparation) | — |
| B | [Appendix B — glossary (English — Vietnamese)](#appendix-b--glossary-english--vietnamese) | — |

### Learning objectives

By the end of the session, a mentor can:

1. State **without looking it up** which week is which gate, and what the minimum evidence for that gate is.
2. Name the **three deliverables the previous cohort most often failed to produce**, and explain why those are the easiest marks on the table.
3. **Catch an agent saying something false about their own repo**, and prove it with a command that produces a number.
4. **Open a pull request and review somebody else's** — branch, commit, push, PR, review, merge, pull. These six moves are reused in all fifteen sessions.

### The three questions a team will ask that this session answers

1. *"What exactly do I have to hand in this week?"* → section 2
2. *"It runs — does that mean I'm done?"* → section 3
3. *"I let the AI write most of the code. Do I lose marks?"* → section 1, slide 4

---

## Opening

### Slide 1 — Cover · [`#cover`](buoi-01.html#cover)

**Key points**
- Fifteen sessions that do not teach you to code; they teach you to **check**.
- You leave today with three things: a pull request of your own **merged by somebody else**, a written record of one place the agent was confidently wrong, and the six Git moves.
- The whole cohort is **one team on one repo**.

**Explanation.** Open with the worry that is actually in the room — *"I can't code, how am I supposed to mentor?"* — and answer it in the first five minutes rather than letting it sit. The comparison that lands is the building inspector: they do not mix mortar, but they know where to knock on the wall, which paperwork must exist, and what a crack near a window means.

### Slide 2 — Agenda · [`#agenda`](buoi-01.html#agenda)

Five sections, then the lab, then the cross-exercise. Sections 1 to 3 answer the three questions a team actually asks. Section 4 is the project those answers apply to. Section 5 is the single most important slide of the day, and the lab turns it into a habit.

**Timing:** 30 minutes of slides · 60 minutes of lab · 20 minutes of cross-exercise.

---

## Section 1 — What a mentor is for

### Slide 3 — Two jobs, not three · [`#mentor-role`](buoi-01.html#mentor-role)

**Key points**
- ① **Set the requirement** — say what "done" means *before* the work starts, in a form that can be checked.
- ② **Accept the work** — run the check yourself and read the result.
- ✗ **Write the code for them** — the third job that does not exist.

**Explanation.** A requirement nobody can check is a wish. "Make the agent answer well" is a wish; "the agent answers all five core questions with a citation, and refuses anything outside the material" is a requirement, because somebody can sit down and verify it.

The second job is where mentors drift. "The team said it works" and "the agent said it works" are both reports, not checks. A check is something you ran.

The third card is the failure mode, not a joke. Writing code for a team in week 2 produces a week 6 where the team cannot answer the judges' questions about their own system — because it is not theirs. Say this out loud, because the people most likely to do it are the ones most eager to help.

**Key terms** — requirement *(yêu cầu)* · acceptance *(nghiệm thu)* · checkable *(kiểm được)*

**Self-check.** Take any sentence you would say to a team this week and ask: *what command or observation would tell me whether this is done?* If there is no answer, it is a wish.

### Slide 4 — The four-step loop · [`#accept-loop`](buoi-01.html#accept-loop)

**Key points**

```
① you: a clear requirement, with its check
      ↓
② agent: does the work — fast, confident, sometimes wrong
      ↓
③ YOU: run the check                     ← the step people skip
      ↓
④ you: revise the requirement            ← if ③ failed, ① was unclear
```

**Explanation.** This is the answer to the team's third question. **Using AI to write code costs no marks.** Not being able to check what the AI wrote does.

Steps one, two and four can all be done with help. Step three cannot be delegated — it is you, a command, and the output on screen. Drop it and the other three steps become theatre: a requirement nobody verified, work nobody inspected, and a revision based on an impression.

Step four carries an idea worth stating: when a check fails, the first suspect is the requirement, not the worker. "Make it better" produced something not-better because it did not say what better means.

Every lab in all fifteen sessions ends with a check. **That check is the part that is graded** — not the code the agent produced.

**Key terms** — loop *(vòng lặp)* · check / verification *(phép kiểm)* · delegate *(giao việc)*

**Self-check.** Ask your agent to do something small in a scratch folder, then write down the one command you would run to decide whether it did it. If you cannot name the command, you have not specified the task.

---

## Section 2 — Six weeks, six gates

### Slide 5 — G1 to G6 · [`#six-weeks`](buoi-01.html#six-weeks)

**Key points**

```
W1 G1 idea + USP → W2 G2 architecture + repo → W3 G3 agent really runs
  → W4 G4 deploy + eval → W5 G5 before/after → W6 G6 Demo Day
```

**Explanation.** A gate is a **gate**, not a reporting milestone. The difference is consequential: a reporting milestone that slips means a late report. A gate that slips means the next week cannot start correctly, because its work depends on the previous gate's output.

A team three days late on G2 cannot be on time for G3: G3 is "the agent really runs", and an agent needs the architecture and the repo that G2 produces. Lateness accumulates and does not heal by itself. That is the sentence a mentor repeats in week 2, and it is more useful than any amount of encouragement.

**Key terms** — gate *(cổng)* · milestone *(mốc)* · USP — unique selling point *(điểm khác biệt)* · cumulative delay *(trễ cộng dồn)*

**Self-check.**

```bash
grep -n "^| G" docs/giao-trinh/00-khung-danh-gia.md
```

Can you say what each gate is without reading the output?

### Slide 6 — Minimum evidence, gate by gate · [`#gate-evidence`](buoi-01.html#gate-evidence)

**Key points**

| Gate | Week | Minimum evidence |
|---|---|---|
| G1 | 1 | USP statement + 1-page proposal + layer-1 prototype shown to **5 real users** |
| G2 | 2 | architecture diagram + running API skeleton + repo in the template's structure |
| G3 | 3 | the agent answers **5 core questions** + real tools are called + 1 guardrail layer |
| G4 | 4 | deployed to cloud + CI green + golden dataset **≥45 cases** + eval round 1 with numbers |
| G5 | 5 | before/after eval + feedback from **≥5 real users** + slop-check CLEAN + panel review |
| G6 | 6 | **10/10 deliverables** + demo video + AIDA pitch |

**Explanation.** This column is the answer to *"what do I have to hand in this week?"*, and the reason to memorise it is practical rather than academic. A team asks on Monday; if the mentor has to look it up and reply on Tuesday, the team has lost a day out of six.

Notice which parts are **hard numbers**: five users at G1, five questions at G3, forty-five cases and a green CI at G4, five users again at G5, ten out of ten at G6. Hard numbers are what make a gate checkable rather than arguable. "Enough user feedback" is a conversation; "five users, and here is what each said" is a check.

**Key terms** — minimum evidence *(bằng chứng tối thiểu)* · golden dataset *(bộ dữ liệu chuẩn)* · guardrail *(lớp chặn)* · AIDA — attention, interest, desire, action

**Self-check.** Cover the table and write the six evidence lines from memory. Then `diff` yours against the file. The lines you got wrong are the ones your team will ask about next week.

### Slide 7 — At-risk triggers · [`#at-risk`](buoi-01.html#at-risk)

**Key points**
- **2** standups missed in a row
- **3** days with no commit
- **>3** days late on a gate

**Explanation.** These three belong to the mentor, not to the organising committee. Nobody will send you a report; you watch for them.

Why mechanical numbers rather than judgement: a mentor who waits to *feel* that a team is struggling always notices in week 4, when there is no time to recover. Two missed standups is observable on day two.

The one people argue about is "three days with no commit" — *"they were thinking"*. Fine: then there is a design document to show. **No artefact for three days is the signal**, whatever the reason behind it.

When a trigger fires, open the L1 / L2 / L3 playbook in the operations course material. Do not wait for the next scheduled meeting; the point of a trigger is that it fires before anybody has formed an opinion.

**Key terms** — at-risk *(nguy cơ)* · trigger *(dấu hiệu kích hoạt)* · standup *(họp nhanh hằng ngày)* · artefact *(hiện vật / sản phẩm trung gian)*

**Self-check.**

```bash
# for any repo you can see:
git log --since="3 days ago" --oneline | wc -l    # 0 → trigger
```

---

## Section 3 — "Running" is not "done"

### Slide 8 — Ten deliverables · [`#done-vs-running`](buoi-01.html#done-vs-running)

**Key points.** Ten deliverables, each with a place in the repo. Most teams lose marks not for weak code but for **missing paperwork**.

| # | Deliverable | Previous cohort | Where it lives |
|---|---|---|---|
| 1 | Source code | most had it | `src/` |
| 2 | README.md | most had it | `README.md` |
| 3 | Architecture diagram | often missing | `ARCHITECTURE.md` |
| 4 | AI logs | most had it | hook + `.ai-log/` |
| 5 | Live URL | usually there | README + deploy |
| 6 | **Video demo** | **missing in 11/11** | `docs/video-demo.md` |
| 7 | **Pitch deck** | **~64% missing** | `docs/pitch-deck.pdf` |
| 8 | Development journal | most had it | `JOURNAL.md` |
| 9 | Worklog | most had it | `WORKLOG.md` |
| 10 | **Evaluation evidence** | **~82% missing** | `eval/` + `docs/evaluation.md` |

**Explanation.** Say the uncomfortable part plainly: **working code is the necessary condition, the record is the sufficient one.** A team with good code and no evaluation evidence scores below a team with ordinary code and a complete record. That is not unfair — it is what the rubric measures, and a product nobody can evaluate is a product nobody can trust.

Read the *why it goes missing* column in the framework file. None of the reasons is "too difficult". They are scheduling and ownership failures — which is to say, **mentoring failures**.

When you quote these numbers to a team, say where they come from: *"the previous cohort of the predecessor programme"*. Do not inflate it, and do not present it as this cohort's data.

**Key terms** — deliverable *(hiện vật phải nộp)* · evaluation evidence *(bằng chứng đánh giá)* · worklog *(nhật ký công việc)*

**Self-check.** Open any repo you have and tick the ten rows. How many could you produce today?

### Slide 9 — Three numbers to know by heart · [`#three-numbers`](buoi-01.html#three-numbers)

**Key points**

| Number | Deliverable | What it actually takes |
|---|---|---|
| **100%** | video demo | a screen recording with a voice-over — one afternoon |
| **82%** | evaluation evidence | a golden dataset and a number, not an opinion |
| **64%** | pitch deck | six weeks of work, judged in eight minutes of talking |

**Explanation.** These are the strongest coaching tool a mentor has, and all three cost **no extra coding skill**.

Here is how to use them. When a team says *"we'll skip the eval and add two more features"*, the useful answer is not general advice about quality. It is: **eighty-two percent of the previous cohort skipped it, and that is where the marks were lost.** A number ends the argument; advice invites negotiation.

A team that delivers all three is almost certainly ahead of most of the field — without writing one line of better code.

**Key terms** — demo video *(video trình diễn)* · pitch *(bài thuyết trình gọi vốn / giới thiệu)* · golden dataset *(bộ dữ liệu chuẩn)*

**Self-check.** Say the three numbers out loud, with the deliverable each belongs to, without looking. They are on the session's pass checklist.

### Slide 10 — Six criteria × five levels · [`#six-criteria`](buoi-01.html#six-criteria)

**Key points**

| # | Criterion | What the panel is really asking |
|---|---|---|
| 1 | Product completeness | Does the demo run without breaking? |
| 2 | AI engineering | Real AI work, or just an API call? |
| 3 | Evaluation | Is there data to prove it? |
| 4 | UI and experience | Usable, responsive, presentable? |
| 5 | Idea and real application | A real problem for real people? |
| 6 | Pitch | Clear story, convincing, good Q&A? |

Levels: 9–10 excellent · 7–8 good · 5–6 average · 3–4 weak · 1–2 poor.

**Explanation.** Teach the **middle column**, not the left one. "Product completeness" means nothing to a team; "does the demo run without breaking" means something they can act on this week.

Criterion 2 is where teams misjudge themselves most. Calling an LLM API and getting good answers is a five or six, not a nine. A nine needs retrieval over the team's own material, more than one guardrail layer, and a **mechanism** — not a prompt — that stops the model inventing things.

**Then say the limit honestly.** Criterion names and level descriptions are stable; **weights can change per cohort**. At the start of each cohort, compare against the released rubric. Sounding certain about something uncertain is the fastest way for a mentor to lose the room, and the room is the only authority a mentor has.

**Key terms** — rubric *(bảng tiêu chí)* · weight *(trọng số)* · edge case *(trường hợp biên)* · RAG — retrieval-augmented generation · hallucination *(bịa)*

**Self-check.** For each of the six criteria, name one thing you could open in a repo to form an opinion. If you cannot for a criterion, that is the criterion you will be vague about in week 5.

### Slide 11 — Six engineering mistakes you can catch by opening the repo · [`#six-mistakes`](buoi-01.html#six-mistakes)

**Key points**

| Mistake | The check |
|---|---|
| No CI/CD | is there `.github/workflows/`, and is the **latest** run green? |
| Evaluation skipped | is `eval/` empty? |
| Thin README | could a stranger install and run it from the README alone? |
| No clear environment setup | is there an `.env.example` and a pinned dependency list? |
| Everything in one file | one 2000-line file, or folders that match the diagram? |
| No tests | does `tests/` exist, and what does `make test` actually print? |

**Explanation.** None of these is an algorithm bug, which is exactly why a mentor who does not code can still find all six. You do not need to understand a team's retrieval logic to see that their eval folder is empty.

The check line on each card is the deliverable of this slide. Six cards, six commands or six clicks — that is a repo review, and it takes about ten minutes.

The last one carries a trap we use in today's lab: **do not ask whether tests exist, run them and read the output.** "We have tests" and "the tests pass" are different statements, and teams routinely confuse them.

**Key terms** — CI/CD — continuous integration / delivery *(tích hợp / phát hành liên tục)* · monolith *(gộp một khối)* · pinned dependency *(phiên bản ghim)*

**Self-check.**

```bash
ls .github/workflows/ ; ls eval/ ; ls tests/ ; make test
```

---

## Section 4 — MT-01, the project behind every session

### Slide 12 — Three layers · [`#mt-01`](buoi-01.html#mt-01)

**Key points**

| Layer | What it does |
|---|---|
| **A — the learner** | grounded Q&A over **approved** material, plus support for students studying in a second language |
| **B — the course** | syllabus review against a **6-criterion rubric**, with a lecturer who approves or rejects |
| **C — the programme** | anonymised, aggregated numbers across courses |

**Explanation.** Correct the misreading immediately, because it happens every time: MT-01 is **not** "a syllabus checker". That is layer B, the middle one.

The three layers are chained, and the chain is the design. Only material approved in B becomes a source for A; only reviews completed in B produce numbers for C. A team that proposes to build A first, over unapproved documents, has broken the chain — and the product no longer means anything, because its answers would be grounded in material nobody vouched for.

We build it for real, deploy it for real, evaluate it for real. By the end of the fifteen sessions each mentor owns a repo that has been through G1 to G6 — which is the only honest way to mentor a team through those gates.

**Key terms** — grounded *(có dẫn nguồn)* · ESL — English as a second language · approved *(đã duyệt)* · anonymised *(ẩn danh)* · aggregate *(tổng hợp)*

**Self-check.** Say the three layers and the direction of the arrows between them, with `docs/de-bai.md` closed.

### Slide 13 — The five-step flow of layer B · [`#mt-01-flow`](buoi-01.html#mt-01-flow)

**Key points**

1. A lecturer submits a syllabus (PDF or DOCX, with course code and term).
2. The agent reviews it against the rubric — **every finding cites the passage it came from**.
3. A draft report is produced, status `PENDING`: visible to the lecturer, invisible to students.
4. **The lecturer approves or rejects — the human in the loop.**
5. Approved content flows outward: to A as a citable source, to C as an anonymised number.

**Explanation.** Step 4 is the reason this project is worth building. An agent that reviewed syllabi and published the results itself would be unusable in a real university — and would also be much easier software. The approval gate is what makes it real, and what makes it technically interesting: a pending state, a permission check, and a flow that waits for a person.

Keep step 2's sentence for session 07: **a finding without a citation is a hallucination with good manners.** That is the G3 guardrail requirement, expressed in a form a team can test.

**Key terms** — human in the loop *(người trong vòng lặp)* · pending *(chờ duyệt)* · citation *(trích dẫn nguồn)*

**Self-check.** Which step would break if the agent were allowed to publish its own reports? Name every consequence for layers A and C.

### Slide 14 — Three roles, one boundary · [`#mt-01-roles`](buoi-01.html#mt-01-roles)

**Key points**

| Role | Can | Must not |
|---|---|---|
| Student | ask, get cited answers from approved material, ESL help | ever see a `PENDING` report |
| **Lecturer** | submit, read the draft, **approve or reject — their own courses** | approve somebody else's course → **403** |
| Programme manager | anonymised aggregates, compare across courses and terms | read one named lecturer's report |

**Explanation.** One sentence matters here: **permissions live on the server.**

Teams implement this in the front end first, because it is easier and it looks right in a demo. Then a judge opens the network tab, calls the endpoint directly, and the permission model evaporates. Session 03 tests exactly this, and it is worth saying today so nobody is surprised.

The manager's restriction is the subtle one. Aggregated and anonymised means the manager cannot drill down to a named lecturer **even though the data exists**. That is a design decision from the brief, not an oversight, and a team will try to "improve" it in week 3.

**Key terms** — role-based access control / RBAC *(phân quyền theo vai)* · 403 Forbidden · endpoint *(điểm truy cập API)* · least privilege *(quyền tối thiểu)*

**Self-check.** From session 03 onward: call the approve endpoint as the wrong role and confirm the server answers **403**. Hiding the button in the UI is not a permission system.

### Slide 15 — MT-01 across the six gates · [`#mt-01-gates`](buoi-01.html#mt-01-gates)

**Key points.** The same project seen from each gate, and the check a mentor runs at each one.

| Gate | The check a mentor runs |
|---|---|
| G1 | Who were the **5 users**? Name them, and say what each one said. |
| G2 | Clone the repo on a clean machine and follow the README. Does it run? |
| G3 | Ask it something outside the material. Does it **refuse**, or invent? |
| G4 | Open the live URL. Then open the CI run and read the **latest** result. |
| G5 | Which number moved, by how much, measured on which dataset? |
| G6 | Walk the ten deliverable rows and tick each one against the repo. |

**Explanation.** This table is the bridge between the framework and the project, and the right-hand column is your weekly job.

Note the word **latest** in G4. A green badge in a README can be months old; open the actual run. Small habit, catches a surprising number of overstatements.

G3's check is the one mentors enjoy once they try it: ask the system something its material cannot answer and watch. A good system refuses. A system that invents a confident answer has no guardrail, whatever the architecture diagram claims.

**Key terms** — clean machine *(máy sạch)* · refusal *(từ chối trả lời)* · baseline *(mốc so sánh)*

**Self-check.** Pick any public AI demo and run G3's check on it. Most fail.

### Slide 16 — Scope guard · [`#scope-guard`](buoi-01.html#scope-guard)

**Key points.** MT-01 deliberately does **not** do:

- real LMS integration
- grading of students
- multi-agent setups
- fine-tuning
- Next.js in the basic version

**Explanation.** These sound like limitations; they are the project's spine.

Briefly, why each: a real LMS integration is somebody else's credentials and somebody else's release cycle; grading students is an ethics review, not a sprint; multi-agent and fine-tuning are both excellent ways to spend six weeks and have nothing to demo; Next.js is a front-end decision the brief defers so the team can ship something visible in week 1.

Then the real point. **Scope creep in week 3 is the single most common way a team fails Demo Day**, and it never arrives as "let us abandon the plan". It arrives as one more good idea. A mentor who agreed the scope in writing in week 1 can say no in one sentence in week 3.

**Key terms** — scope *(phạm vi)* · scope creep *(phình phạm vi)* · LMS — learning management system · fine-tuning *(tinh chỉnh mô hình)*

**Self-check.** Write the five exclusions on a card. You will read them aloud to a team in week 3.

---

## Section 5 — Why the agent sounds certain and is wrong

### Slide 17 — The agent is most confident about what it never checked · [`#agent-guesses`](buoi-01.html#agent-guesses)

**The most important slide of the session.**

**Key points**

| You ask | It says | The truth |
|---|---|---|
| "How many files does this repo track?" | a confident number | `git ls-files \| wc -l` → **104** |
| "What happens if I run `make test`?" | "the tests pass" | `make test` → **2 passed, 3 errors** |

> **The rule of this whole course: a question about the state of a repo is answered by a command — never by a sentence.**

**Explanation.** Run both commands live if you can. The file count moves as the course material grows, so check it before the session and use today's number. The test result is the interesting one: **two passed, three errors, left red on purpose** — the template ships with a real defect so that session 02 has something real to fix.

Say the diagnosis carefully, because it changes how people treat the tool for the rest of the course. **The agent is not failing at arithmetic.** It answered from the most probable world rather than from this repo, and in the most probable world a repo's tests are green. That failure mode never announces itself; it arrives as a fluent, plausible sentence.

Everybody in the room will hear the human version of this from their own team within three weeks, phrased as *"it works on my machine"*.

**Deep dive — three kinds of question.** It is worth separating them, because they need different responses:

| Kind | Example | Correct behaviour |
|---|---|---|
| **Written down** in the repo | "what is the minimum evidence for G4?" | cite file + section; **you open it and check** |
| **Computable** from the repo | "how many files are tracked?" | produce a **command**; you run it |
| **A judgement** | "is this architecture good?" | options and trade-offs; **you decide** |

Confusing the second for the first is today's lesson. Session 02's exercise 2.4 practises telling them apart deliberately.

**Key terms** — hallucination *(bịa)* · grounding *(dẫn nguồn)* · tool use *(gọi công cụ)* · plausible *(nghe hợp lý)* · repo state *(trạng thái repo)*

**Self-check.**

```bash
git ls-files | wc -l
make test
```

Write both outputs down **before** you ask the agent anything.

---

## The lab — four people, one team, one repo

### Slide 18 — Lab overview · [`#lab-overview`](buoi-01.html#lab-overview)

**Key points.** Four exercises in 60 minutes, and the six Git moves that are all the GitHub you need for fifteen sessions.

| Move | What it actually means |
|---|---|
| `git switch -c <branch>` | open your own draft |
| `git add <file>` | choose exactly what you are submitting |
| `git commit -m "…"` | package it, with the reason |
| `git push -u origin <branch>` | send the draft to GitHub — still **not** in main |
| `gh pr create` | ask: "this is what I want to add" |
| `gh pr merge` | agree and take it in — **the reviewer's job** |

> **The team's law: nobody merges their own pull request.**

**Explanation.** The framing sentence comes first: this cohort is one team on one repo. Not one repo each. The difficulty of GitHub was never the commands — it is four people changing one repo without overwriting each other.

Read the right-hand column aloud before anybody types. The move people get wrong is the last one: authors push, reviewers merge.

State the law as a law, not a procedure. The reason to say it today, over a one-line README change, is that **a habit formed on a trivial change survives into week 4 when the change is frightening.**

Before releasing the room: anyone whose collaborator invitation is still in their GitHub inbox fixes it now. Otherwise they discover it mid-exercise with a push that fails.

**Full lab instructions:** [`buoi-01_ex.md`](buoi-01_ex.md).

### Slides 19 – 20 — Exercise 1.1 · Join the team repo · [`#ex1-brief`](buoi-01.html#ex1-brief)

18 minutes. Clone, **prove the push right**, read the brief, answer three questions with the files closed.

The only step that reliably goes wrong is the permission check, and it fails **silently**: cloning works for anybody who can read a public repo, so somebody with no push right looks fine until they try to push.

```bash
gh api repos/UEH-ISB/mt-01-the-odyssey --jq '.permissions.push'   # must be true
```

### Slides 21 – 22 — Exercise 1.2 · Your first pull request · [`#ex2-brief`](buoi-01.html#ex2-brief)

22 minutes, and the core exercise of the session. Each person fills in **their own assigned row** of the README team table, so four pull requests merge cleanly and nobody meets a conflict on their first day.

The transferable skill is the output check: **one `+` line and one `−` line**. A diff bigger than the change somebody described is the single most useful warning sign a reviewer has.

### Slides 23 – 24 — Exercise 1.3 · Review, then merge · [`#ex3-brief`](buoi-01.html#ex3-brief)

12 minutes, and the law is the content. The three checks — scope, real username, real question — are a code review shrunk to fit a one-line change. They are the same three questions you ask in week 4 about a guardrail change you cannot read.

Two things the room gets wrong, both worth naming:

- **merged but never pulled** — merging changed GitHub; your laptop finds out when you ask it to;
- **approved without opening `Files changed`** — signing a blank page, and contagious, because a team copies what a mentor does rather than what a mentor says.

### Slides 25 – 26 — Exercise 1.4 · Demand the command · [`#ex4-brief`](buoi-01.html#ex4-brief)

8 minutes, and it carries the session's one transferable idea. Two questions, one per turn, then run both commands and write a four-part journal line: **claim · truth · cause · command**.

The part people drop is the cause, and it is the part that makes the note reusable. "The agent was wrong" is an anecdote. "It guessed from the familiar case instead of running anything" is something you can recognise again in your own team's standup.

If the agent is careful and runs the command first — newer ones often do — that is a **better** outcome. Record which command it ran, run it yourself, compare. A mentor's question is *"what did you run?"*, not *"are you sure?"*.

---

## Summary, Q&A, references

### Slide 27 — Six things to take away · [`#summary`](buoi-01.html#summary)

1. **Two jobs** — set the requirement, accept the work. Never write it for them.
2. **A gate is a gate** — three days late at G2 means G3 cannot be on time.
3. **Running ≠ done** — ten deliverables in ten known places.
4. **100 / 82 / 64** — video, evaluation, pitch. None needs better code.
5. **MT-01 has three layers** — learner, course, programme. Syllabus review is the middle one.
6. **The rule of the course** — a question about repo state is answered by **a command**, never a sentence.

If only one of the six sticks, make it number six. Everything else is a table somebody can look up; the habit of answering a state question with a command is what makes a non-coding mentor useful rather than decorative.

### Homework

1. Read `docs/de-bai.md` and the framework file **in full** — not skim. From session 02 onward every check points back to these two files.
2. **The six-gate map — by hand first, then the agent.** Fill in a six-row table `Week | Gate | Minimum evidence | Session` from memory, **without opening the framework file**. Only then send:

   ```
   Read docs/giao-trinh/00-khung-danh-gia.md sections 1 and 2. Write docs/00-ban-do-khoa.md
   with a 6-row table: Week | Gate | Minimum evidence | Session of the course. Quote the
   evidence column verbatim, do not paraphrase. Add an "At-risk triggers" section, also verbatim.
   ```

   Then compare:

   ```bash
   diff <(sed -n '/^|/p' nhap.md) <(sed -n '/^|/p' docs/00-ban-do-khoa.md)
   ```

   **Done when** you can read every difference and say who was right. Where your memory was wrong is where your team will ask next week. Submit as a second pull request — this time nobody walks you through the commands.
3. **Install Python 3.12 (or 3.11)** and check with `python3 --version`. Session 02 builds the environment; anyone who cannot install loses half the session.
4. Draft **five use cases** of MT-01: five concrete groups of people and the concrete job each needs done. No need to choose yet — session 05 reuses the list.

### Review questions

1. Why is a gate not a milestone? Give the consequence in terms of G2 and G3.
2. A team says "it runs, we're done". Which three deliverables do you ask about first, and why those three?
3. What is the difference between a question the agent can read the answer to, and one it can only compute? Give an example of each from this repo.
4. Why must nobody merge their own pull request, given that it would be faster?
5. Your team's `main` has no commits for three weeks. What do you ask before concluding anything? *(Session 02 answers this one.)*

### Pass checklist for session 01

- [ ] Cloned `UEH-ISB/mt-01-the-odyssey`; `.permissions.push` prints `true`
- [ ] **Your PR is merged, and somebody else merged it** — `gh pr list --state merged`
- [ ] You reviewed somebody else's PR with a **content comment** before approving
- [ ] After `git pull`, the team table on your machine shows somebody else's row
- [ ] You can state MT-01's **three layers** and the three constraints that stop it being a chatbot
- [ ] **`JOURNAL.md` records ≥1 place the agent was wrong about repo state, with the cause and the command**
- [ ] You can say the six gates and the three numbers (100% / 82% / 64%) without opening anything
- [ ] Cross-exercise ≥ 2/3

> **Fail the sixth box and you do not move on to session 02.** The others are procedure and can be fixed later. The sixth is the trade.

### References — all inside this repository

| What | Path |
|---|---|
| Gates, deliverables, rubric | `docs/giao-trinh/00-khung-danh-gia.md` |
| The MT-01 brief | `docs/de-bai.md` |
| This session's exercise book | `docs/giao-trinh/slide/buoi-01_ex.md` |
| The slide deck | `docs/giao-trinh/slide/buoi-01.html` |
| What was trimmed from the template, and why | `DA-LUOC.md` |
| The agent's rules in this repo | `CLAUDE.md` · `.agents/rules/` |

---

## Appendix A — the facilitator's preparation

**One day before** (organisation invitations must be accepted, which takes a human):

```bash
# invite each mentor to the org as a member, then grant push on the repo
gh api -X PUT "repos/UEH-ISB/mt-01-the-odyssey/collaborators/<github-username>" \
  -f permission=push
```

**Also assign row numbers** in the README team table — mentor A row 1, B row 2, C row 3, D row 4 — and write them on the board. This is what keeps four pull requests conflict-free.

**On the day, before the session:**

```bash
git ls-files | wc -l     # today's number for slide 17 — it grows as the course grows
make test                # must be 2 passed, 3 errors
```

**Optional — enforce the law instead of relying on it.** A ruleset on `main`: *Require a pull request before merging* + *Require 1 approval*. The repo is public, so rulesets are free. Announce it first, or somebody will think their machine is broken when a direct push is rejected.

---

## Appendix B — glossary (English — Vietnamese)

| English | Vietnamese | Note |
|---|---|---|
| gate | cổng | not a milestone — the next week depends on it |
| minimum evidence | bằng chứng tối thiểu | the checkable part of a gate |
| deliverable | hiện vật phải nộp | one of the ten |
| rubric | bảng tiêu chí | 6 criteria × 5 levels |
| at-risk trigger | dấu hiệu nguy cơ | 2 standups · 3 days · >3 days |
| requirement | yêu cầu | must be checkable |
| acceptance / to accept work | nghiệm thu | the mentor's second job |
| check / verification | phép kiểm | the graded part |
| grounded | có dẫn nguồn | an answer tied to a source |
| hallucination | bịa | said confidently, never checked |
| guardrail | lớp chặn | the mechanism, not the prompt |
| human in the loop | người trong vòng lặp | step 4 of layer B |
| pending | chờ duyệt | invisible to students |
| anonymised | ẩn danh | layer C |
| scope guard | ranh giới phạm vi | what the project will not do |
| scope creep | phình phạm vi | arrives as one more good idea |
| branch | nhánh | your own draft |
| commit | commit / lần ghi | package + reason |
| pull request (PR) | yêu cầu hợp nhất | a place for somebody to read your work |
| review | duyệt / rà soát | read the diff, then approve |
| merge | hợp nhất | the reviewer's action |
| diff | khác biệt | what changed, and only that |
| merge conflict | xung đột khi hợp nhất | two people, one line |
| squash merge | gộp thành một commit | keeps history readable |
| repo state | trạng thái repo | answered by a command |
