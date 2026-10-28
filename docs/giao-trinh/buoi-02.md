# Session 02 — From a template to the team's project

> **Sources for this session:** [the MT-01 brief](../de-bai.md)
> · [the evaluation framework](00-khung-danh-gia.md) · [the repo builder](repo-mau/README.md)
> **120 minutes.** Inside the project repo the course lives at `docs/giao-trinh/` and the brief at `docs/de-bai.md`.

> **This file is the session plan** — objectives, run of show, cross-exercise, homework and the
> pass checklist. The teaching material itself is split across three companions:
>
> | File | What it carries |
> |---|---|
> | [`slide/buoi-02.html`](slide/buoi-02.html) | the 26 slides, with speaker notes (`N`) |
> | [`slide/buoi-02_lecture_note.md`](slide/buoi-02_lecture_note.md) | one section per slide: key points, explanation, key terms, self-check |
> | [`slide/buoi-02_ex.md`](slide/buoi-02_ex.md) | the exercise book — the full version of the 60-minute lab |

## What a mentor must be able to do after this session

- Build the project so that it **runs on their own machine**: virtual environment, `.env`, `make run`, Swagger UI open.
- **Compare documentation against code** and name the places they disagree — on the programme's real template, not on a contrived example.
- Make the agent **cite a file and section in `docs/giao-trinh/`**, then open that file and confirm it — and tell apart a question the agent can **read** from one it can only **run**.
- **Review a technical pull request they did not write**, bring somebody else's fix onto their own machine, and confirm it with a command rather than a sentence.

## The three questions a team will ask that this session answers

1. *"I followed the README and it broke — why?"*
2. *"We've settled the idea, shall we set up Docker and CI now, to be safe?"*
3. *"I put the key in the config file, it's easier — is that a problem?"*

## Prerequisites

Python 3.12 (or 3.11) — check with `python3 --version`. Git, and `gh` logged in. A coding agent
installed. **No LLM API key is needed today.**

**The team repo must already be cloned from session 01**, with your introduction pull request
merged. Anybody who missed session 01 must be invited to the repo **before** this session — do not
try to fix it in the room.

> **Facilitator, 5 minutes before the session:** run `python3 -m venv .venv && pip install -r
> requirements.txt && make test` on your own machine once. It must print **2 passed, 3 errors**. If
> your machine prints something else, the room will too, and exercise 2.5 loses its point.

---

## Run of show (120 minutes)

| Block | Time | Slides | Material |
|---|---|---|---|
| Opening | 4′ | 1 – 2 | cover · agenda |
| §1 What today is for | 5′ | 3 – 4 | five things by the end · one team, one repo |
| §2 The official template | 6′ | 5 – 6 | kept / cut / added · choosing the starting tier |
| §3 **Environment, and reading an error** | 8′ | 7 – 8 | venv · **the three gaps** |
| §4 Secrets, and what git tracks | 6′ | 9 – 10 | never hardcode a key · the clean-status lie |
| §5 The team's Git workflow | 3′ | 11 | `main` · `develop` · feature branches |
| §6 Running it | 5′ | 12 – 13 | `make run` + Swagger + the hook · MT-01 recap |
| §7 Working with the agent | 3′ | 14 | the four rules in `CLAUDE.md` |
| **Lab** | **60′** | 15 – 25 | five exercises → [`buoi-02_ex.md`](slide/buoi-02_ex.md) |
| Cross-exercise | 20′ | — | swap roles, below |
| Summary + Q&A | 10′ | 26 | takeaways · homework |

Slide-by-slide notes are in [`buoi-02_lecture_note.md`](slide/buoi-02_lecture_note.md) and in each
slide's speaker notes (press `N` in the deck).

> **Do not reveal the three gaps of slide 8 in detail.** The lab is where people meet them, and
> meeting them is the lesson. Somebody who is *told* that the README command fails has learned a
> fact; somebody who runs it, reads the error and finds the right command has learned a method.

---

## Lab (60 minutes) — build the floor on your own machine, in the team's repo

**After the lab you have:** the team repo running **on your machine**, `.env` safe, Swagger open,
AI logging installed, `make test` printing `5 passed` after you pulled somebody else's fix, and
**three** `JOURNAL.md` lines recording the three places where documentation and code disagree.

> **Still one team, one repo** — `mt-01-the-odyssey`, created in session 01. Today each person
> builds **their own environment** on it, so four people will hit four different machine failures.
> That is deliberate: week 2 of a real team looks exactly like this, and the sentence *"but it
> works on my machine"* is born right here.
>
> **Split into pairs at the start of the lab.** Pair 1 and pair 2. Exercises 2.1 – 2.4 are
> individual; **2.5 splits the work**, because one repo only needs one fix. Swap roles next session.

| # | Exercise | Time | Who | Produces |
|---|---|---|---|---|
| 2.1 | **Follow the README, meet a real error** | 20′ | everybody | a working environment, 3 journal lines |
| 2.2 | `.env`, and the clean-status lie | 10′ | everybody | two checks in opposite directions |
| 2.3 | Server, Swagger, hook | 10′ | everybody | `/health` ok, one endpoint executed, hook installed |
| 2.4 | Make the agent cite — then demand a command | 10′ | everybody | a verified citation, one journal line |
| 2.5 | **Turn the tests green** | 10′ | **pair 1 fixes · pair 2 reviews** | `5 passed` on all four machines |

**Full instructions, checks and troubleshooting:** [`slide/buoi-02_ex.md`](slide/buoi-02_ex.md).

### The three gaps the lab produces

| | The gap | Found in |
|---|---|---|
| ① | README says `pip install -e ".[dev]"`, but the repo has **no `pyproject.toml`** | exercise 2.1 |
| ② | `make test` gives **2 passed, 3 errors** the moment you clone | exercise 2.5 |
| ③ | `ci.yml` declares `pull_request: branches: [main]`, so a PR into `develop` triggers **no CI at all** | exercise 2.5 |

All three are **real**, in the official template. Nobody wrote a bad template — documentation and
code drift apart in every project, and the mentor's job is to be the person who compares them.

### The law has not changed

> **Nobody merges their own pull request.**

Last week it guarded a one-line table edit. Today it guards the change that makes everybody's tests
pass.

---

## Cross-exercise (20 minutes) — swap roles: *"I followed the README and it broke"*

Pair up — but **change partners from the lab**: whoever worked together in exercise 2.5 now
separates. **A** plays a **team member**, **B** plays the **mentor**. 10 minutes, then swap.

**A sends three messages, one minute apart, read exactly:**

1. *"I can't get it installed, the whole screen is red."*
2. *(sends a screenshot of a wall of text)* *"Here."*
3. *"We've settled the idea, so I'll set up Docker and CI now, to be safe — all right?"*

**B must do three things:**

- **Message 1:** ask **exactly one** question that gets usable information — *"what does the `ERROR` line say?"* Not *"have you tried again?"* And **not** *"what does the last line say?"*: exercise 2.1 just showed that pip's last line is advice about upgrading pip, not the failure.
- **Message 2:** **refuse the screenshot.** Ask for the text, pasted — text can be handed to an agent, an image cannot. Then steer A to read that line themselves, without stating the conclusion for them.
- **Message 3:** ask which **starting tier** the team is at, and *"do you have a USP and feedback from 5 real users yet?"* before agreeing to any infrastructure work.

**Scoring:** one point each. Below 2/3 → run it again with a different partner. Answering *"go and
check it again"* scores **zero**: it returns the work to the team with no new information.

---

## Homework

1. Re-read `docs/giao-trinh/00-khung-danh-gia.md` §1 and §3 — session 03's architecture has to support G2's evidence.
2. Read `docs/de-bai.md` in your repo, **in full**. Session 03 draws the architecture for **all three layers** — learner, course, programme — not just the syllabus part.
3. **Grade a syllabus by hand.** Open `data/de-cuong/BUS2051-phan-tich-du-lieu.md` and `data/rubric.yaml`. Score it against the six criteria, **15 minutes on a timer**, and write the result in `JOURNAL.md`.
   Compare against the planted-fault list in `data/de-cuong/README.md` — how many of the six did you find?
   **A criterion you cannot reach a verdict on by hand is a threshold that is not clear enough — and the agent will not manage it either.**
4. **Submit by pull request into `develop`**, not a direct commit: branch `journal/<your-name>`, `git add JOURNAL.md` only, then `gh pr create --base develop`. Somebody else merges. This is your third PR; from the next session nobody walks through the commands.
   Two people editing `JOURNAL.md` will hit a **merge conflict** — that is session 03's first exercise. **Do not resolve it by deleting somebody else's lines.** Capture it and bring it to session 03.

---

## Pass checklist for session 02

- [ ] Team repo pulled onto your machine; the team has a `develop` branch
- [ ] `pip install -r requirements.txt` done; `make run` works; `curl localhost:8000/health` returns `ok`
- [ ] `git check-ignore -v .env` prints a line and `git status` does **not** show `.env`
- [ ] `git ls-files | grep data` shows **4 files** — and you can explain why a clean `git status` can still hide a missing file
- [ ] `make test` prints **5 passed** on **your** machine after pulling the `pytest.ini` fix; `ruff check` is clean
- [ ] **`JOURNAL.md` records the three gaps:** the README install command · `make test` red on clone · CI not running for PRs into `develop`
- [ ] The agent answered with a file path, and **you opened that file and confirmed it**
- [ ] A pull request is merged into `develop`, and `gh run list` shows one successful run
- [ ] You reviewed, or were reviewed: the reviewing pair ran `gh pr checkout` + `make test` **before** approving — no approving by eye
- [ ] `bash scripts/setup_hooks.sh` has run **on your machine** (hooks live in `.git/`; nobody can do it for you)
- [ ] Cross-exercise ≥ 2/3

**The "three journal lines" box is the one mentors skip because "it's only a journal".** It is the
only box that teaches what cannot be lectured: documentation says one thing, code does another, and
only the person who compares them sees it. Fail that box and session 02 has not happened, however
well the server runs.
