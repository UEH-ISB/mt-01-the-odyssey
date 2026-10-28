# Session 02 — From a template to the team's project

**Lecture note** · companion to the slide deck [`buoi-02.html`](buoi-02.html) · course: *Mentoring for AI-aided Projects* · The Odyssey · 120 minutes

> **How to use this note.** Each heading below matches one slide of the deck. Click the slide link to open it (`←` `→` to navigate, `N` for speaker notes).
> Under each slide you will find the key points, a longer explanation, commands where they help, the key terms (English — Vietnamese), and a **self-check**: a command or a question you run or answer yourself.
> Sections marked **Deep dive** go beyond the slides.

**Primary sources — all inside this repository:**

- `docs/de-bai.md` — the MT-01 project brief.
- [`docs/giao-trinh/00-khung-danh-gia.md`](../00-khung-danh-gia.md) — gates, deliverables, rubric.
- `DA-LUOC.md` — what was kept and cut from the official template, and why.
- [`buoi-02_ex.md`](buoi-02_ex.md) — the exercise book for the 60-minute lab.

---

## Contents

| # | Section | Slides |
|---|---------|--------|
| 1 | [What today is for](#section-1--what-today-is-for) | 3 – 4 |
| 2 | [The official template](#section-2--the-official-template) | 5 – 6 |
| 3 | [Environment, and reading an error](#section-3--environment-and-reading-an-error) | 7 – 8 |
| 4 | [Secrets, and what git really tracks](#section-4--secrets-and-what-git-really-tracks) | 9 – 10 |
| 5 | [The team's Git workflow](#section-5--the-teams-git-workflow) | 11 |
| 6 | [Running it for the first time](#section-6--running-it-for-the-first-time) | 12 – 13 |
| 7 | [Working with the agent](#section-7--working-with-the-agent-by-the-repos-rules) | 14 |
| L | [The lab](#the-lab--build-the-floor-on-your-own-machine) | 15 – 25 |
| + | [Summary, Q&A, references](#summary-qa-references) | 26 |
| A | [Appendix A — the facilitator's preparation](#appendix-a--the-facilitators-preparation) | — |
| B | [Appendix B — glossary (English — Vietnamese)](#appendix-b--glossary-english--vietnamese) | — |

### Learning objectives

By the end of the session, a mentor can:

1. Build the project so that it **runs on their own machine**: virtual environment, `.env`, `make run`, Swagger UI open.
2. **Compare documentation against code** and name the places they disagree — on the programme's real template, not on a contrived example.
3. Make an agent **cite a file and section in `docs/giao-trinh/`**, open that file to confirm it, and tell apart a question the agent can **read** from one it can only **run**.
4. **Review a technical pull request they did not write**, bring somebody else's fix onto their own machine, and confirm it with a command rather than a sentence.

### The three questions a team will ask that this session answers

1. *"I followed the README and it broke — why?"* → section 3
2. *"We've settled the idea, shall we set up Docker and CI now?"* → section 2, slide 6
3. *"I put the key in the config file, it's easier — is that a problem?"* → section 4

---

## Opening

### Slide 1 — Cover · [`#cover`](buoi-02.html#cover)

**Key points**
- Today writes no logic. It builds the floor the next thirteen sessions stand on.
- The three places where this template's documentation and code already disagree.
- Four people, four machines, **four different failures** — on purpose.

**Explanation.** New mentors treat setup as a minor session. It is the session that decides whether the team can deploy in week 4; *"but it works on my machine"* in week 3 was born in a setup somebody rushed.

What is new compared with session 01: last week everybody did the same thing to the same file. Today each person builds their own environment on the same repo. Week 2 of a real team looks exactly like this.

### Slide 2 — Agenda · [`#agenda`](buoi-02.html#agenda)

Seven short sections, then the lab, then the cross-exercise. **Timing:** 30 minutes of slides · 60 of lab · 20 of cross-exercise.

Flag section 3 as the one to pay attention to, and **do not reveal the three gaps in detail** — the lab is where people walk into them, and the walking-in is the lesson.

Pair people up now rather than at the start of the lab: exercises 2.1 to 2.4 are individual; 2.5 splits into **pair 1 fixes, pair 2 reviews**.

---

## Section 1 — What today is for

### Slide 3 — Five things by the end · [`#five-things`](buoi-02.html#five-things)

**Key points**

| # | Must exist on **your** machine | Verified by |
|---|---|---|
| ① | the environment runs | `pip install -r requirements.txt` |
| ② | `.env` is not committed | `git check-ignore -v .env` |
| ③ | Swagger UI opens | `curl -s localhost:8000/health` |
| ④ | tests go green: **5 passed** | `make test` |
| ⑤ | **somebody else's fix is on your machine** | `git switch develop && git pull` |

**Explanation.** Each card has a command at the bottom, and that is the pattern of the whole course: a deliverable you can check is a deliverable.

Card ⑤ is the one that is easy to under-rate. The first four are about your own machine working; the fifth is about somebody else's work arriving on your machine **through a pull request rather than a chat message**, which most people in the room have never experienced.

Card ④ says five passed, and the repo right now gives two passed and three errors. Do not explain why yet.

**Key terms** — virtual environment *(môi trường ảo)* · Swagger UI *(giao diện thử API)* · integration branch *(nhánh tích hợp)*

**Self-check.** For each of the five, write the command without looking at the slide.

### Slide 4 — One team, one repo · [`#one-team`](buoi-02.html#one-team)

**Key points**
- Same repo as session 01, same law: **nobody merges their own pull request.**
- What is new: each person builds **their own environment** on that one repo.
- A repo created outside the organisation **does not send its webhook to the grading system** and **does not count as a submission**.

**Explanation.** Last week the law protected a one-line table edit. Today it protects a configuration change that makes everybody's tests pass — which is exactly why starting the habit on something trivial was worth it.

Four people, four machines, four different failures: a missing compiler on one, a system Python on another, a forgotten virtual environment on a third. That is not a flaw in the exercise; it is the thing teams discover in week 2 and have no process for.

Somebody will create their own repo by reflex. Catch it in the first ten minutes, not in week 4 when their work is invisible to the grading system.

**Key terms** — collaborator *(cộng tác viên có quyền push)* · organisation *(tổ chức trên GitHub)* · webhook *(móc gửi sự kiện)*

**Self-check.**

```bash
git remote -v     # must point at UEH-ISB/mt-01-the-odyssey
```

---

## Section 2 — The official template

### Slide 5 — Kept, cut, added: 94 → 104 files · [`#template`](buoi-02.html#template)

**Key points**

| | |
|---|---|
| **Kept** | `src/agents/` (s04) · `src/api/` `src/models/` (s08) · `tests/` (s10) · Docker, CI, `ruff.toml` (s12) · `scripts/` + 6 hook configs (**today**) · `ARCHITECTURE.md` `JOURNAL.md` `WORKLOG.md` (deliverables 3, 8, 9) |
| **Cut** | `docs/guide/` — 37 files · `docs/COHORT3_REPORT.md` |
| **Added** | the whole course: `docs/giao-trinh/` and `docs/de-bai.md` |

**Explanation.** This **is** the real template, not a teaching replica. Everything awkward about it is awkward in the original — including the three gaps in section 3.

Why `docs/guide/` was cut, since somebody always asks why documentation was deleted: it was a **parallel body of material this course does not teach from**. Two disagreeing sets of documentation in one repo is the fastest way to make an agent confidently wrong about something that is not in the course, because it will quote the wrong one fluently. One repo, one source.

And the counterpart: everything this course cites is now a file inside the repo. That is why, from today on, *"where did you read that?"* always has an answer — for you and for the agent.

Full kept-and-cut table: `DA-LUOC.md`.

**Key terms** — template *(khung dự án mẫu)* · scaffold *(bộ khung)* · parallel documentation *(tài liệu song song)*

**Self-check.**

```bash
git ls-files | wc -l           # 104 — and growing as the course grows
ls docs/giao-trinh/
```

### Slide 6 — Which tier does a team start at? · [`#choose-tier`](buoi-02.html#choose-tier)

**Key points**

| | Tier 1 — prototype | Tier 2 — the team's repo |
|---|---|---|
| When | weeks 1–2, user needs still unknown | USP settled + **≥5 pieces of real feedback** |
| What | something 5 people can look at within 48 hours | built seriously, on the template |
| Infrastructure | none — no server, no Docker, no CI | tests, CI, deployment all belong here |

**Explanation.** This answers the team's second question, and *"it depends"* is a useless answer. Give the test: **do you have a USP and five pieces of real feedback?** No → tier 1. Yes → tier 2.

Both failure modes are real and they look opposite:

- **too early** — full Docker and CI in week 1 on an unvalidated idea → three weeks on infrastructure and nothing to show a user;
- **too late** — still a hand-built prototype after the idea is settled → it falls over under load on demo day.

**Crossing over, carry exactly two things: the real users' questions and the validated USP. Not the code.** Prototype code is meant to be thrown away, and a team that tries to "upgrade" it spends longer than a rewrite would take.

**Key terms** — prototype *(bản mẫu)* · USP *(điểm khác biệt)* · validation *(kiểm chứng)* · throwaway code *(code dùng một lần)*

**Self-check.** For a project you know: which tier is it at, and what is the evidence?

---

## Section 3 — Environment, and reading an error

### Slide 7 — venv, and how to read an error message · [`#environment`](buoi-02.html#environment)

**Key points**

```bash
python3 -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

> **Find the `ERROR` line, or the last line of a `Traceback` block. Ignore `[notice]` and `WARNING`.**

**Explanation.** A virtual environment in one sentence: a private library box for this project, so other projects cannot break it and it cannot break your machine. That is as much as anybody needs today.

The error-reading rule is the transferable half of this slide, and the correction matters:

> **"Just read the last line" is true for a Python `Traceback` and false for `pip`.** `pip` prints
> `[notice] A new release of pip is available` **after** the `ERROR` line, so the genuine last line
> is advice about upgrading pip, not the failure.

A mentor who tells a team to read the last line of a failed `pip install` is sending them to look at something irrelevant. Exercise 2.1 walks straight into this — **do not warn the room again**.

Say where this was found: by running the install, not by writing the lesson. That is the argument for every facilitator doing the setup themselves before the session.

**Deep dive — the two machine failures that cover almost every real incident:**

| Symptom | Cause | Fix |
|---|---|---|
| `No module named …` | the library is installed, but the **venv is not active** | activate, install again |
| `Permission denied` during `pip install` | installing into the **system Python** | same |

**Key terms** — virtual environment *(môi trường ảo)* · traceback *(vết lỗi)* · exit code *(mã thoát)* · system Python *(Python của hệ thống)*

**Self-check.**

```bash
pip install -e ".[dev]" 2>&1 | grep -n "^ERROR"     # which line number?
```

### Slide 8 — Three places where the docs and the code disagree · [`#three-gaps`](buoi-02.html#three-gaps)

**The most important slide of the session.**

**Key points**

| | The gap | Where it bites |
|---|---|---|
| ① | README says `pip install -e ".[dev]"` — the repo has **no `pyproject.toml`**, so it fails | exercise 2.1 |
| ② | `make test` gives **2 passed, 3 errors** the moment you clone — `pytest-asyncio` installed, nothing sets `asyncio_mode` | exercise 2.5 |
| ③ | `ci.yml` declares `pull_request: branches: [main]` — a PR into `develop` triggers **no workflow at all** | exercise 2.5 |

**Explanation.** None of the three was planted. They are genuine, they are in the official template, and that is precisely why they are worth meeting — a manufactured bug only teaches people to spot manufactured bugs.

Do not explain the details here. Somebody who is **told** that the README command fails has learned a fact; somebody who **runs it, reads the error and finds the right command** has learned a method.

End on the sentence at the bottom of the slide: **nobody wrote a bad template. Documentation and code drift apart in every project, and the mentor's job is to be the person who compares them.** A team sends the message *"I followed the README and it broke"* in week 2 of every cohort, and the useful response is to compare, not to guess.

Gap ③ has a second lesson inside it, which exercise 2.5 draws out: **no green tick is not the same as red tests.**

**Key terms** — documentation drift *(tài liệu lệch code)* · editable install *(cài dạng chỉnh sửa được)* · workflow trigger *(điều kiện kích hoạt workflow)* · branch filter *(lọc theo nhánh)*

**Self-check.**

```bash
ls pyproject.toml                           # no such file
make test                                   # 2 passed, 3 errors
grep -A5 "^on:" .github/workflows/ci.yml    # branches: [main]
```

---

## Section 4 — Secrets, and what git really tracks

### Slide 9 — Never hardcode a key · [`#env-keys`](buoi-02.html#env-keys)

**Key points**
- `.env.example` → **committed**, with placeholder values.
- `.env` → **never**, under any circumstance.
- `.env` and every `.env.*` variant are already in `.gitignore`.

> **If a key reaches GitHub: revoke the key. Do not delete the commit.**

**Explanation.** The instinct when a key leaks is to delete the commit, because it makes the visible evidence disappear. Say plainly why that fails: the commit is in the history, on GitHub's servers, in forks, and in every clone anybody already made. **The only action that ends the exposure is revoking the key.**

This has happened here. A team in cohort 4 left a CI runner's credentials on a branch. The order of operations was **revoke the runner first, delete the branch second**. Doing it the other way round destroys your own evidence while the credential is still live.

Note also that a key committed into a repository is not only a security problem — it is an operational one, because the team then cannot rotate it without breaking everybody's environment at once.

**Key terms** — secret *(bí mật / khoá)* · rotation *(xoay khoá)* · revoke *(thu hồi)* · placeholder *(giá trị tạm)* · 12-factor config

**Self-check.**

```bash
git check-ignore -v .env      # must print a line
git log -p | grep -i "api_key" | head    # should find nothing real
```

### Slide 10 — A clean `git status` does not mean everything is committed · [`#git-status-lie`](buoi-02.html#git-status-lie)

**Key points**

| Command | Answers |
|---|---|
| `git status` | what git is **watching** change |
| `git ls-files` | what git actually **has** |

**Explanation.** This is a gap in a mental model, not a missing command. People believe a clean status means a clean repo, and for ignored files that is simply false.

The template's `.gitignore` ignored **all of `data/`** — a reasonable decision for runtime data such as a database or a cache, and one that silently excludes the project's own fixtures. Nothing warns you: the file is on your disk, your program reads it, your status is clean, and your teammate does not have it.

The facilitator already fixed this when building the repo (`data/` → `data/*` plus three exceptions — see `DA-LUOC.md`). **The lesson stays**, because a team meets it in week 3 as *"why do I have that file and you don't?"*, with nobody able to explain it. The first command is `git ls-files`, not `git status`.

**Key terms** — ignored file *(file bị bỏ qua)* · tracked file *(file được theo dõi)* · negation pattern *(mẫu phủ định `!`)* · fixture *(dữ liệu mẫu)*

**Self-check.**

```bash
git ls-files | grep data     # 4 files
git status --short           # says nothing about them either way
```

---

## Section 5 — The team's Git workflow

### Slide 11 — `main` · `develop` · feature branches · [`#git-workflow`](buoi-02.html#git-workflow)

**Key points**

```
feature branch → pull request into develop → review + merge by somebody else → everybody git pull
```

| Branch | Meaning |
|---|---|
| `main` | the approved version |
| `develop` | the team's integration branch |
| feature | one person, one change |

**Explanation.** Keep this short — the room uses it twenty minutes later.

The content of the slide is the cohort-4 story. One team's `main` had not moved in three weeks while **21 other branches** were busy. A mentor looking only at `main` would have concluded the team had stopped working, and would then have had a demoralising conversation with a team that had been working hard into `develop`.

**So: ask where they merge before judging progress.** A ten-second question that prevents a bad meeting.

And the last box of the flow: merging changed the server; it does not change anybody's laptop until they pull.

**Key terms** — integration branch *(nhánh tích hợp)* · feature branch *(nhánh tính năng)* · fast-forward · stale branch *(nhánh bỏ quên)*

**Self-check.**

```bash
git branch -a                                  # which branches exist?
git log --oneline --graph --all | head -20     # where is the work actually happening?
```

---

## Section 6 — Running it for the first time

### Slide 12 — The server, Swagger UI, and your own hook · [`#first-run`](buoi-02.html#first-run)

**Key points**

```bash
make run                             # then http://localhost:8000/docs
curl -s localhost:8000/health        # {"status":"ok", …}

bash scripts/setup_hooks.sh          # deliverable #4
ls -a .git/hooks/ | grep pre-push
```

**Explanation.** Do this live on the projector: start the server, open the docs page, click **Try it out** → **Execute**, and let people see a request go out and a response come back.

Then say why it matters to this room specifically. **Swagger UI is a clickable interface to an API.** A mentor who cannot read the code can still exercise every endpoint a team has built, which means checking behaviour rather than taking somebody's word for it. Session 08 reuses it to call an endpoint **as the wrong role** and confirm the server answers **403**.

The hook has one trap and it is organisational, not technical. **Hooks live in `.git/`, which is never committed, so each person installs their own.** Teams reliably assume that one person installing it covers everybody, and then four weeks of AI logs are missing from three machines.

Two prohibitions, both guarding against **silent** failure:

- never call `scripts/log_*` by hand — it fabricates log entries (`.agents/rules/ai-log-hook.md`);
- never bypass a failing hook with `--no-verify` — the push succeeds, the logging quietly does not, and you find out a week later.

**Key terms** — uvicorn · endpoint *(điểm truy cập)* · OpenAPI / Swagger · git hook *(móc git)* · pre-push · `--no-verify` *(bỏ qua hook)*

**Self-check.**

```bash
curl -s localhost:8000/health
ls -a .git/hooks/ | grep pre-push    # on YOUR machine
```

### Slide 13 — MT-01 is three layers · [`#mt-01-layers`](buoi-02.html#mt-01-layers)

**Key points.** A · the learner (grounded Q&A + ESL) · **B · the course** (6-criterion rubric, lecturer approves) · C · the programme (anonymised aggregates).

**Explanation.** Thirty seconds. This is a correction, not new material.

It is on the slide because in session 01 a portion of every room walks away believing MT-01 is a syllabus checker. They then design an architecture in session 03 with no learner-facing layer and no programme view, and have to redo it.

The chain, once more: approved material in B is what layer A may cite, and completed reviews in B are what layer C counts. **The approval step is the hinge for both.**

**Self-check.** Name all three layers and the direction of data between them, with `docs/de-bai.md` closed.

---

## Section 7 — Working with the agent, by the repo's rules

### Slide 14 — The four rules in `CLAUDE.md` · [`#learning-with-ai`](buoi-02.html#learning-with-ai)

**Key points**

| # | Rule |
|---|---|
| ① | **Cite the source** — which file, which section |
| ② | **End with a check *you* run** — and do not let the agent run it and report back |
| ③ | **Do not decide for me** — scope, framework, thresholds are the human's |
| ④ | **Do not do the part that is the lesson** |

**Explanation.** These four lines live in `CLAUDE.md`, so the agent reads them itself. You do not have to repeat them every session.

Rule ② is the one to press on. **The check is the graded artefact, not the generated code**, and the most common way people lose the value of a check is by letting the agent run it and summarise the result. That is session 01's failure mode wearing a different hat.

Rule ④ explains something people find irritating at first. When today's exercise is reading an error message, an agent that reads it for you has removed the exercise. The repo's rules say so, so the agent will push back — and that is working as intended.

> If the agent replies *"which would you prefer?"* instead of choosing, that is rule ③ working, not the agent being unhelpful.

**Key terms** — project memory *(bộ nhớ dự án)* · agent rules *(quy tắc cho agent)* · citation *(dẫn nguồn)* · deferral *(nhường quyết định)*

**Self-check.** Open `CLAUDE.md` and find the line that corresponds to each of the four rules.

---

## The lab — build the floor on your own machine

### Slide 15 — Lab overview · [`#lab-overview`](buoi-02.html#lab-overview)

Five exercises, 60 minutes. Exercises 2.1 – 2.4 individually; **2.5 splits into pair 1 fixes, pair 2 reviews.**

Set expectations before releasing the room: **exercise 2.1 will take the longest and will look like chaos**, because four machines fail four ways. Walk the room; do not stand at the front.

**Full lab instructions:** [`buoi-02_ex.md`](buoi-02_ex.md).

### Slides 16 – 17 — Exercise 2.1 · Follow the README, meet a real error · [`#ex1-brief`](buoi-02.html#ex1-brief)

20 minutes. The instruction that matters is **"do not fix it immediately"**. The reflex when a command fails is to search for a working command, and that reflex is what stops people learning to read an error.

Watch for the pip-notice trap in real time. Somebody will read the last line, announce that pip is out of date, upgrade it, and run the broken command again. **That is the best possible thing that can happen in this exercise** — let it play out, then ask the room which line actually said `ERROR`.

### Slides 18 – 19 — Exercise 2.2 · `.env`, and the clean-status lie · [`#ex2-brief`](buoi-02.html#ex2-brief)

10 minutes, two commands, one idea: git has two different questions and people only ever ask one of them.

Make people **say out loud** why the second check exists. "Because we were told to run it" is not an answer; the answer is that ignored files are invisible everywhere, so the absence of a warning means nothing.

The `.env.example` reading is a mentor-level point: the log endpoint is deliberately blank, with a warning, because the original template pointed at the predecessor programme's grading server. **Somebody has to read a file like that before handing a repo to a team**, and that somebody is the mentor.

### Slides 20 – 21 — Exercise 2.3 · Server, Swagger, hook · [`#ex3-brief`](buoi-02.html#ex3-brief)

10 minutes, and the pleasant one — things start and respond. Make sure everybody actually clicks **Execute** rather than looking at the page; for most of the room it is the first API call they have ever made.

The hook is the part to police. One command, deliverable #4, on each of the four machines. Ask for the `grep` output from each person rather than taking a nod.

### Slides 22 – 23 — Exercise 2.4 · Make the agent cite, then demand a command · [`#ex4-brief`](buoi-02.html#ex4-brief)

10 minutes. This is session 01's lesson one level up: **there are two kinds of question and they need two different responses.**

Question 1 has an answer written in a file in the repo, so the agent can genuinely read it and the right move is to check the citation. Question 2 has no answer anywhere — the count exists only when something counts it — so the only correct behaviour is to produce a command.

The three-row table is the transferable part:

| The agent… | You… |
|---|---|
| cites a real file and quotes it correctly | open the file and read it |
| cites a real file, wrong section | point at the right section |
| gives a number without running anything | **demand the command** |

Mentors who cannot tell these apart end up saying *"check it again"*, which helps nobody.

### Slides 24 – 25 — Exercise 2.5 · Turn the tests green · [`#ex5-brief`](buoi-02.html#ex5-brief)

10 minutes, and it ties the session together.

Everybody runs `make test` first, because **a different result is a diagnosis of that person's environment**, not of the repo. Catch those people here rather than letting them leave with a broken setup.

Pair 1 works with the agent under the repo's rules: explain the cause in two sentences, propose the smallest fix, name the file, **stop for approval before editing**. That sequence is what is being practised, more than the pytest configuration itself.

Pair 2 must not look at the fix first. Their job is to find the CI trap independently — open the Actions tab, see nothing, and work out why — and then verify the only way available: check out the branch and run it.

**Close on this:** a green CI is something you **make**, not something you are given. A team that accepts a template with red tests and leaves them red for six weeks arrives at week 6 unable to tell a new failure from an old one, so nobody dares change anything.

---

## Summary, Q&A, references

### Slide 26 — Six things to take away · [`#summary`](buoi-02.html#summary)

1. **Setup is not a minor session** — it decides whether week 4 deploys.
2. **Read the `ERROR` line**, not the last line. `pip` prints its notice after the error.
3. **Docs and code drift** in every project. The mentor is the person who compares them.
4. **A leaked key → revoke it.** Deleting the commit saves nothing.
5. **A clean `git status` ≠ everything committed.** `git ls-files` says what git *has*.
6. **A green CI is made, not given** — and **no green tick is not the same as red tests**.

### Homework

1. Re-read the framework file, §1 and §3 — session 03's architecture has to support G2's evidence.
2. Read `docs/de-bai.md` **in full**. Session 03 draws the architecture for **all three layers**, not just the syllabus part.
3. **Grade a syllabus by hand.** Open `data/de-cuong/BUS2051-phan-tich-du-lieu.md` and `data/rubric.yaml`. Score it against the six criteria, **15 minutes on a timer**, and write the result in `JOURNAL.md`. Compare against the planted-fault list in `data/de-cuong/README.md` — how many of the six did you find?
   **A criterion you cannot reach a verdict on by hand is a threshold that is not clear enough — and the agent will not manage it either.**
4. **Submit by pull request into `develop`**, not a direct commit: branch `journal/<your-name>`, `git add JOURNAL.md` only, then `gh pr create --base develop`. Somebody else merges. This is your third PR; from the next session nobody walks through the commands.
   Two people editing `JOURNAL.md` will hit a **merge conflict** — that is session 03's first exercise. **Do not resolve it by deleting somebody else's lines.** Screenshot it and bring it.

### Review questions

1. Why is "read the last line" wrong advice for a failed `pip install`, and right for a Python traceback?
2. A file is on your disk, your program reads it, and `git status` is clean. Give two reasons this can happen and the one command that distinguishes them.
3. A key was pushed to GitHub an hour ago. List your actions, in order, and say why the order matters.
4. A team's pull request shows no CI checks at all. Name two different explanations and the command that tells them apart.
5. Your team's `main` has no commits for three weeks. What do you ask before concluding anything?

### Pass checklist for session 02

- [ ] Team repo pulled onto your machine; the team has a `develop` branch
- [ ] `pip install -r requirements.txt` done; `make run` works; `curl localhost:8000/health` returns `ok`
- [ ] `git check-ignore -v .env` prints a line and `git status` does **not** show `.env`
- [ ] `git ls-files | grep data` shows **4 files** — and you can explain why a clean status can still hide a missing file
- [ ] `make test` prints **5 passed** on **your** machine after pulling the `pytest.ini` fix; `ruff check` is clean
- [ ] **`JOURNAL.md` records the three gaps**: the README install command · `make test` red on clone · CI not running for PRs into `develop`
- [ ] The agent answered with a file path, and **you opened that file and confirmed it**
- [ ] A PR is merged into `develop`, and `gh run list` shows one successful run
- [ ] You reviewed, or were reviewed: the reviewing pair ran `gh pr checkout` + `make test` **before** approving — no approving by eye
- [ ] `bash scripts/setup_hooks.sh` has run **on your machine** (hooks live in `.git/`; nobody can do it for you)
- [ ] Cross-exercise ≥ 2/3

> **The "three journal lines" box is the one mentors skip because "it's only a journal".** It is the only box that teaches what cannot be lectured: documentation says one thing, code does another, and only the person who compares them sees it. Fail that box and session 02 has not happened, however well the server runs.

### References — all inside this repository

| What | Path |
|---|---|
| The MT-01 brief | `docs/de-bai.md` |
| Gates, deliverables, rubric | `docs/giao-trinh/00-khung-danh-gia.md` |
| This session's exercise book | `docs/giao-trinh/slide/buoi-02_ex.md` |
| The slide deck | `docs/giao-trinh/slide/buoi-02.html` |
| What was trimmed from the template, and why | `DA-LUOC.md` |
| The agent's rules | `CLAUDE.md` · `.agents/rules/hoc-cung-ai.md` · `.agents/rules/ai-log-hook.md` |

---

## Appendix A — the facilitator's preparation

**5 minutes before the session**, on your own machine:

```bash
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
make test            # MUST print 2 passed, 3 errors
```

If your machine prints anything else, the room will too, and exercise 2.5 loses its point. Find out why before the session, not during it.

**Also:**

- confirm anybody absent from session 01 has an accepted invitation **before** the session;
- decide who creates `develop` (one person, once) and write the pairs on the board;
- have the `AI_LOG_SERVER` endpoint for The Odyssey ready, or be prepared to say explicitly that deliverable #4 has nowhere to submit to yet.

**Do not** reveal the three gaps of slide 8 in detail. The lab is where people meet them.

---

## Appendix B — glossary (English — Vietnamese)

| English | Vietnamese | Note |
|---|---|---|
| virtual environment (venv) | môi trường ảo | a private library box for one project |
| dependency | thư viện phụ thuộc | listed in `requirements.txt` |
| pinned version | phiên bản ghim | so two machines install the same thing |
| editable install (`-e`) | cài dạng chỉnh sửa được | needs `pyproject.toml` — which this repo lacks |
| traceback | vết lỗi | read its **last** line |
| notice | thông báo | `pip` prints it **after** the error — ignore it |
| exit code | mã thoát | 0 = success |
| secret | bí mật / khoá | never in the repo |
| revoke | thu hồi | the only real fix for a leaked key |
| rotation | xoay khoá | replacing a key on schedule |
| ignored file | file bị bỏ qua | invisible in `status`, `add .` and the diff |
| tracked file | file được theo dõi | what `git ls-files` lists |
| integration branch | nhánh tích hợp | `develop` |
| workflow trigger | điều kiện kích hoạt | `on: pull_request` + branch filter |
| branch filter | lọc theo nhánh | why a `develop` PR runs nothing |
| CI / continuous integration | tích hợp liên tục | made, not given |
| endpoint | điểm truy cập API | what Swagger lets you click |
| Swagger / OpenAPI | giao diện thử API | a mentor's best tool without code |
| git hook | móc git | lives in `.git/`, never committed |
| `--no-verify` | bỏ qua hook | push succeeds, logging silently does not |
| async fixture | fixture bất đồng bộ | needs `asyncio_mode` |
| merge conflict | xung đột khi hợp nhất | session 03's first exercise |
