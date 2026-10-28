# Session 02 — Exercises: building the floor, and finding the three gaps

**Exercise book** · companion to the slide deck [`buoi-02.html`](buoi-02.html) (exercise slides `#ex1-brief` … `#ex5-steps`) and the lecture note [`buoi-02_lecture_note.md`](buoi-02_lecture_note.md) · course: *Mentoring for AI-aided Projects* · The Odyssey

> **How to use this file.** The slides carry the short version of each exercise. This file carries the full version.
> Every exercise has the same parts, in the same order as its two slides:
>
> | Slide 1 — the brief | Slide 2 — the steps |
> |---|---|
> | **Task** — the problem, what goes in and what must come out | **Main steps** — each with the command and what you are looking at |
> | **Input & tools** — knowledge, tools, files | **Manual — no AI** — what a person must do themselves |
> | **Expected output** — the state or the files, with the figure or table | **Keywords** — terms to look up for self-study |
>
> Each exercise ends with a **Done when** check you run yourself.
> Shell commands are written for **Git Bash** (the repo's default shell on Windows) and macOS/Linux alike.

---

## Contents

| # | Exercise | After slides | Time | Who |
|---|----------|--------------|------|-----|
| 0 | [Before you start](#0--before-you-start) | — | read | everybody |
| 2.1 | [Follow the README, meet a real error](#exercise-21--follow-the-readme-meet-a-real-error) | 7 – 8 | 20′ | everybody |
| 2.2 | [`.env`, and the clean-status lie](#exercise-22--env-and-the-clean-status-lie) | 9 – 10 | 10′ | everybody |
| 2.3 | [Server, Swagger, hook](#exercise-23--server-swagger-hook) | 12 | 10′ | everybody |
| 2.4 | [Make the agent cite, then demand a command](#exercise-24--make-the-agent-cite-then-demand-a-command) | 14 | 10′ | everybody |
| 2.5 | [Turn the tests green](#exercise-25--turn-the-tests-green) | — | 10′ | **pair 1 fixes · pair 2 reviews** |
| X | [Cross-exercise — swap roles](#cross-exercise--swap-roles-20-minutes) | — | 20′ | pairs, swapped |
| A | [Appendix — if something goes wrong](#appendix--if-something-goes-wrong) | — | any time | — |

Exercise slides in the deck: Ex 2.1 = slides 16 – 17 · Ex 2.2 = 18 – 19 · Ex 2.3 = 20 – 21 · Ex 2.4 = 22 – 23 · Ex 2.5 = 24 – 25.

---

## 0 · Before you start

### 0.1 Still one team, one repo — but now four machines

The repo is the one from session 01: `UEH-ISB/mt-01-the-odyssey`. What is new today is that **each
person builds their own environment on that one repo**, so four machines will fail in four
different ways.

That is the design, not an accident. Week 2 of a real team looks exactly like this, and the
sentence *"but it works on my machine"* is born here.

```
Ex 2.1  pull · venv · follow the README · meet the error · read it · install correctly
          │
          ├──► Ex 2.2  .env is ignored  ·  the data IS tracked (two checks, opposite directions)
          │
          ├──► Ex 2.3  make run · Swagger · your own pre-push hook
          │
          ├──► Ex 2.4  agent cites a file  ·  agent must offer a command
          │
          └──► Ex 2.5  pair 1 fixes pytest  →  PR into develop  →  pair 2 verifies  →  everybody pulls
```

### 0.2 Split into pairs now

| Exercises | Who |
|---|---|
| 2.1 – 2.4 | everybody, on their own machine |
| **2.5** | **pair 1 fixes · pair 2 reviews** — one repo needs only one fix |

Swap roles next session.

### 0.3 The law has not changed

> **Nobody merges their own pull request.**

Last week it guarded a one-line table edit. Today it guards the change that makes everybody's
tests pass — which is exactly why starting the habit on something trivial was worth it.

### 0.4 What you need before the lab starts

| | Check |
|---|---|
| Python 3.12 (or 3.11) | `python3 --version` |
| Git, and `gh` logged in | `gh auth status` |
| A coding agent | opens and answers |
| **The repo, cloned in session 01** | `cd mt-01-the-odyssey && git remote -v` |
| Your session-01 PR merged | `gh pr list --state merged` |

No LLM API key is needed today.

> **Facilitator, 5 minutes before the session.** Run `python3 -m venv .venv && pip install -r
> requirements.txt && make test` on your own machine once. It must print **2 passed, 3 errors**. If
> your machine prints something else, the room will too, and exercise 2.5 loses its point.

### 0.5 Who decides what

| The agent does | **You** do |
|---|---|
| reads files, drafts documents, proposes options, explains terms | choose the fix, approve it before it is applied |
| cites a file and a section | **open that file** and confirm the quote |
| suggests a check | **run the check yourself** — and do not let the agent run it and report back |
| nothing that *is* today's lesson | read the error message yourself |

If the agent in this repo refuses to read an error for you today, that is rule four of `CLAUDE.md`
working as designed.

---

## Exercise 2.1 · Follow the README, meet a real error

**Slides:** `#ex1-brief`, `#ex1-steps` (16 – 17) · **Theory:** slides 7 – 8 · **Time:** 20 minutes
**The long one.** Four machines, four failures — the facilitator walks the room.

### Task

Bring the team repo up to date, then do **exactly what the README says** — including the command
that fails.

- do **not** fix it immediately;
- find the `ERROR` line, **not** the last line;
- work out where the repo really declares its dependencies.

### Input & tools

| | |
|---|---|
| **Knowledge** | virtual environments · reading an error (slide 7) · docs-vs-code drift (slide 8) |
| **Tools** | `git` · `python3` · `pip` |
| **Files** | `README.md` · `requirements.txt` · `DA-LUOC.md` |
| **Output** | `JOURNAL.md` — three lines |

### Expected output — a working environment and the first journal entry

#### The command the README gives you, and what it does

```
$ pip install -e ".[dev]"
ERROR: ... does not appear to be a Python project:
neither 'setup.py' nor 'pyproject.toml' found.
[notice] A new release of pip is available  ← NOT the error
```

> **This is the subtle part, and it corrects the simple rule.**
> "Read the last line" is true for a Python **Traceback** and false for `pip`, which prints its
> upgrade notice **after** the `ERROR` line. Somebody who reads the genuine last line will upgrade
> pip, re-run the broken command, and be no further forward.
>
> **The correct rule: find the `ERROR` line, or the last line of a `Traceback` block — ignore
> `[notice]` and `WARNING`.**
>
> This was found by running the install, not by writing the lesson. It is the argument for every
> facilitator doing the setup themselves before the session.

#### Three questions, in this order

| # | Question | Command | Answer |
|---|---|---|---|
| 1 | Which line starts with `ERROR`? | read the output | not the last one |
| 2 | Does the file it needs exist? | `ls pyproject.toml` | no |
| 3 | Where are dependencies really declared? | `head -5 requirements.txt` | here |

#### Then install correctly, and check

```bash
pip install -r requirements.txt
python -c "import fastapi; print(fastapi.__version__)"   # prints a version number
```

#### The journal entry — three lines

```
2026-11-04 · Gap 1: README says `pip install -e ".[dev]"`.
  It fails: the repo has no pyproject.toml and no setup.py.
  Correct command: `pip install -r requirements.txt`.
```

### The seven steps at a glance

| # | Step | What you run |
|---|---|---|
| 1 | Take the team's latest | `cd mt-01-the-odyssey` → `git switch main && git pull` |
| 2 | Read what was trimmed | `DA-LUOC.md` — two minutes |
| 3 | Create and activate the venv | `python3 -m venv .venv` → `source .venv/bin/activate` |
| 4 | Do what the README says — **it will fail** | `pip install -e ".[dev]"` — and **stop** |
| 5 | Read the failure properly | the `ERROR` line · `ls pyproject.toml` · `head -5 requirements.txt` |
| 6 | Install correctly | `pip install -r requirements.txt` |
| 7 | Write the three journal lines | `JOURNAL.md` |

#### Step 1 in detail

```bash
cd mt-01-the-odyssey
git switch main && git pull
git log --oneline | head -5      # commits by OTHER people, not only yours
```

**Absent from session 01 and have no clone?** Ask the facilitator for the invitation **before** the
session. Do not try to solve it during the lab.

#### Step 3 in detail · what a virtual environment is

A private library box for this project — so other projects cannot break it, and it cannot break
your machine.

```bash
python3 -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
```

> **The two failures that cover almost every real incident:**
>
> | Symptom | Cause |
> |---|---|
> | `No module named …` | the library is installed, but the **venv is not active** |
> | `Permission denied` during `pip install` | installing into the **system Python** — also no venv |
>
> Same fix for both: activate the venv, install again.

#### Step 4 in detail · stop

The reflex when a command fails is to search for a working command. That reflex is exactly what
stops people learning to read an error. Let the failure sit on the screen. Three questions, in
order, **then** the fix.

### Manual — do these yourself, no AI

- **Reading the error** — that is today's lesson; do not paste it to the agent yet.
- **Finding `requirements.txt`** yourself.
- **Writing the three journal lines** in your own words, while the error is still on screen.

### Keywords — self-study

`virtual environment` · `editable install -e` · `extras [dev]` · `pyproject.toml` ·
`requirements.txt` · `pip notice vs error` · `traceback` · `exit code`

### Done when — run it yourself

```bash
python -c "import fastapi; print(fastapi.__version__)"   # a version number
grep -c . JOURNAL.md                                     # the three lines are there
```

### Common mistakes (facilitator watch-list)

| What happens | What it means |
|---|---|
| Somebody upgrades pip and retries | they read the last line — **the best thing that can happen**; ask the room which line said `ERROR` |
| `No module named fastapi` after installing | venv not active in **this** terminal |
| They fix it by searching online first | they skipped the exercise; walk them back to the three questions |
| They skip the journal | the pass checklist tests exactly this; hold them there |

---

## Exercise 2.2 · `.env`, and the clean-status lie

**Slides:** `#ex2-brief`, `#ex2-steps` (18 – 19) · **Theory:** slides 9 – 10 · **Time:** 10 minutes

### Task

Two checks that run in **opposite directions**:

- prove `.env` **cannot** be committed;
- prove the project's data **is** committed — which `git status` will never tell you.

Then read the `AI_LOG_SERVER` section of `.env.example` and understand why it is empty.

### Input & tools

| | |
|---|---|
| **Knowledge** | gitignore · tracked vs ignored · why revoking beats deleting |
| **Tools** | `git check-ignore` · `git status` · `git ls-files` |
| **Files** | `.env.example` · `.gitignore` · `DA-LUOC.md` |

### Expected output — two checks, in opposite directions

#### ① `.env` exists on disk and is invisible to git

```bash
$ cp .env.example .env
$ git check-ignore -v .env
.gitignore:16:.env	.env          # must print a line

$ git status --short
                                  # .env must NOT appear
```

> **See `.env` in `git status`? Stop and call the facilitator.** Committing it once means
> **revoking the key**, not deleting the commit. Deleting the commit saves nothing: the key is
> already in the history, already on GitHub's servers, and already in anybody's clone.
>
> This has happened here. A team in cohort 4 left a CI runner's credentials on a branch. The order
> of operations was **revoke the runner first, delete the branch second**. The other way round
> destroys your own evidence while the credential is still live.

#### ② The project's data **is** tracked — the check `status` cannot do

```bash
$ git ls-files | grep data
data/rubric.yaml
data/plo.md
data/de-cuong/…                   # 4 files
```

**The distinction to remember:**

| Command | Answers |
|---|---|
| `git status` | what git is **watching** change |
| `git ls-files` | what git actually **has** |

What `.gitignore` blocks appears **nowhere** — not in `git status`, not in `git add .`, not in your
pull request. The template's `.gitignore` ignored all of `data/`, which is sensible for runtime
data (a database, a cache) and silently excludes the project's own fixtures. The facilitator fixed
this when building the repo (`data/` → `data/*` plus three exceptions — see `DA-LUOC.md`). **The
lesson stays.**

A team meets this in week 3 as: *"Why do I have that file and you don't?"* — with nobody able to
explain it.

#### ③ Read the `AI_LOG_SERVER` block

It is **blank on purpose**, with a warning above it. The original template pointed at the
predecessor programme's grading server, which would have shipped these mentors' prompts somewhere
they do not belong.

This is a mentor-level point rather than a student one: **somebody has to read a configuration file
like that before a repo reaches a team**, and that somebody is the mentor.

### The four steps at a glance

| # | Step | What must happen |
|---|---|---|
| 1 | Make your own `.env` | `cp .env.example .env` — on your disk, never in the repo |
| 2 | Prove git is blocking it | `git check-ignore -v .env` prints a line; `git status --short` is silent about it |
| 3 | Read the `AI_LOG_SERVER` block | understand why it is blank |
| 4 | Prove the data is tracked | `git ls-files \| grep data` → 4 files — **then explain it to your neighbour** |

Step 4's second half is not optional. Saying it out loud is what reveals whether the idea landed.
"Because we were told to run it" is not an answer.

### Manual — do these yourself, no AI

- **Never** paste a real key into a chat with an agent. Placeholders only.
- Reading `.env.example` yourself — it carries a warning meant for a human.
- Explaining the `ls-files` result in your own words.

### Keywords — self-study

`.gitignore` · `negation pattern !` · `tracked file` · `git ls-files` · `git check-ignore` ·
`secret rotation` · `key revocation` · `12-factor config`

### Done when — run it yourself

```bash
git check-ignore -v .env     # prints a line
git status --short           # no .env
git ls-files | grep data     # 4 files
```

---

## Exercise 2.3 · Server, Swagger, hook

**Slides:** `#ex3-brief`, `#ex3-steps` (20 – 21) · **Theory:** slide 12 · **Time:** 10 minutes

### Task

Three things, each with its own check:

- start the server and confirm it answers;
- call an endpoint from **Swagger UI** — no code;
- install the AI logging hook **on your machine**.

### Input & tools

| | |
|---|---|
| **Knowledge** | localhost and ports · what Swagger UI is · why hooks are not committed |
| **Tools** | `make run` · a browser · `curl` · a second terminal |
| **Files** | `Makefile` · `scripts/setup_hooks.sh` · `.agents/rules/ai-log-hook.md` |

### Expected output

#### Server up, and answering

```bash
$ make run                 # leave this terminal running
# browser: http://localhost:8000/docs

# second terminal:
$ curl -s localhost:8000/health
{"status":"ok", …}
```

#### In Swagger UI

| | |
|---|---|
| pick any endpoint | **Try it out** → **Execute** |
| what you just did | called an API without writing code |
| session 08 reuses it | to call as the **wrong role** and expect **403** |

Swagger UI is the single most useful tool for a mentor who does not code: a clickable interface for
exercising every endpoint a team has built. You can check behaviour instead of taking somebody's
word for it.

#### Your own hook — nobody can run this for you

```bash
$ bash scripts/setup_hooks.sh
$ ls -a .git/hooks/ | grep pre-push       # must list it
```

Hooks live in `.git/`, and `.git/` is **never committed**. So **every person runs this on their own
machine**. Somebody else running it does not help you.

This is deliverable **#4**, it takes one command, and it is the deliverable teams most often have
on one machine out of four — because everyone assumes somebody else did it.

> **Two rules about the hook, both guarding against silent failure:**
>
> - never call `scripts/log_*` by hand — it fabricates log entries
>   (see `.agents/rules/ai-log-hook.md`);
> - never bypass a failing hook with `--no-verify` — the push succeeds, the log does not, and you
>   find out a week later with a week of data missing.

### The five steps at a glance

| # | Step | What you run |
|---|---|---|
| 1 | Start the server | `make run` — leave the terminal alone |
| 2 | Check it from outside | **second terminal**: `curl -s localhost:8000/health` |
| 3 | Open Swagger UI | `http://localhost:8000/docs` → **Try it out** → **Execute** |
| 4 | Install your hook — **each person** | `bash scripts/setup_hooks.sh`, then the `grep` check |
| 5 | Stop the server | `Ctrl+C` in the first terminal |

### Manual — do these yourself, no AI

- **Clicking Execute yourself** — reading about Swagger is not using it.
- **Running the hook script on your own machine.**
- Never `--no-verify`; never call `scripts/log_*` by hand.

### Keywords — self-study

`uvicorn` · `localhost` · `port` · `OpenAPI / Swagger` · `health endpoint` · `git hooks` ·
`pre-push` · `curl` · `HTTP status code`

### Done when — run it yourself

```bash
curl -s localhost:8000/health          # {"status":"ok", …}
ls -a .git/hooks/ | grep pre-push      # listed, on YOUR machine
```

…and you executed at least one endpoint in Swagger UI.

---

## Exercise 2.4 · Make the agent cite, then demand a command

**Slides:** `#ex4-brief`, `#ex4-steps` (22 – 23) · **Theory:** slide 14 · **Time:** 10 minutes

### Task

Two questions. They look alike and they are **completely different**.

```
1. What is the minimum evidence for gate G4? Answer briefly, and say which file and
   section of this repo you read it in.

2. How many files does this repo currently track in git?
```

| | Question 1 | Question 2 |
|---|---|---|
| Is the answer written down? | **yes** — in a file in this repo | **no** — nowhere |
| So the agent should | read it and cite file + section | **give you a command to run** |
| And you should | **open that file and check the quote** | run it yourself |

### Input & tools

| | |
|---|---|
| **Knowledge** | the four rules in `CLAUDE.md` (slide 14) |
| **Tools** | your coding agent · `git ls-files` |
| **Files** | `docs/giao-trinh/00-khung-danh-gia.md` §1 · `CLAUDE.md` |
| **Output** | `JOURNAL.md` — one line |

### Expected output — a verified citation and a demanded command

#### Three things the agent might do, and what you do back

| The agent… | You… |
|---|---|
| cites a real file and quotes it correctly | **open the file and read it.** This is the behaviour you want. |
| cites a real file but the wrong section | point at the right section. A reading error, fixed with the source. |
| gives a number without running anything | **demand the command.** No command, no answer. |

Three different failures with three different responses. A mentor who cannot tell them apart ends
up saying *"check it again"*, which helps nobody.

#### Verify both yourself

```bash
$ git ls-files | wc -l
100
```

…and open `docs/giao-trinh/00-khung-danh-gia.md` §1: is the **G4** row really what the agent
claimed? The real row reads: *deploy to cloud + CI green + golden dataset ≥45 cases + eval round 1
with numbers*.

#### The journal line

```
2026-11-04 · Q2: the agent answered "about 60 files", with no command.
  Real: 104 (git ls-files | wc -l).
  Demanded the command; it then offered one. The repo rules say it should have offered it first.
```

### The six steps at a glance

| # | Step | What you do |
|---|---|---|
| 1 | Ask question 1, with the citation requirement | "…and say which file and section of this repo you read it in." |
| 2 | **Open that file yourself** | `docs/giao-trinh/00-khung-danh-gia.md` §1 — is the G4 row really that? |
| 3 | Ask question 2 | "How many files does this repo currently track in git?" |
| 4 | Classify what came back | cited correctly · cited wrongly · answered from nowhere |
| 5 | Run it yourself | `git ls-files \| wc -l` |
| 6 | One journal line | answer · real number · the command |

**Step 2 is the exercise.** Everything else is setup. Accepting a citation without opening the file
means you have done nothing today except read an agent's output, which is what this course is
training people out of.

**Step 4 is where the vocabulary gets learned.** Name which of the three happened, out loud. *"It
was wrong"* is not a classification and does not lead to a next action.

> If your agent behaves perfectly on both — cites correctly, and offers a command for the count —
> that is a good outcome and the exercise still works. Show the room, because most people have
> never seen a well-behaved answer and do not know what to ask for.

### Manual — do these yourself, no AI

- **Opening the cited file** — this is the entire exercise.
- Running `git ls-files` yourself.
- **Not** letting the agent run the check and report back.

### Keywords — self-study

`grounding` · `citation` · `CLAUDE.md` · `agent rules` · `tool use` · `hallucination` ·
`verifiable claim` · `repo state vs document`

### Done when — run it yourself

```bash
git ls-files | wc -l       # the number in your journal line
```

…and you opened the cited file and confirmed the quote.

---

## Exercise 2.5 · Turn the tests green

**Slides:** `#ex5-brief`, `#ex5-steps` (24 – 25) · **Time:** 10 minutes
**Pair 1 fixes · pair 2 reviews.** One repo needs only one fix.

### Task · everybody first

```bash
$ make test
2 passed, 3 errors
```

A different result means **your** environment is wrong, not the repo. Fix that before going on —
this is the moment to catch anybody who would otherwise leave with a broken setup.

Then split.

### Input & tools

| | |
|---|---|
| **Knowledge** | pytest · async fixtures · what a workflow trigger is |
| **Tools** | `make test` · `ruff` · `gh pr` · the agent, under `.agents/rules/hoc-cung-ai.md` |
| **Files** | `requirements.txt` · `.github/workflows/ci.yml` · a new `pytest.ini` |
| **Output** | a PR into `develop` · a journal line about the CI trap |

### Expected output — 5 passed on all four machines

#### Pair 1 — the diagnosis, which the agent must explain **before** it edits

The prompt to use, in the shape the repo's rules require:

```
make test gives 2 passed, 3 errors. The error line says: "requested an async fixture 'client',
with no plugin or hook that handled it."
Read requirements.txt and find whether this repo has any pytest configuration file.
Explain the cause in 2 sentences, then propose the SMALLEST fix.
Do not change anything yet — tell me which file you would create, then stop and wait for me.
```

The agent must find that **`pytest-asyncio` is in `requirements.txt`**, but the repo has **no**
`pytest.ini`, `setup.cfg` or `pyproject.toml` to set `asyncio_mode`. The fix is three lines:

```ini
# pytest.ini
[pytest]
asyncio_mode = auto
testpaths = tests
```

> That sequence — explain, propose, name the file, **stop** — is the thing being practised, more
> than the pytest configuration itself. If the agent edits before approval, show the room: that is
> the behaviour `CLAUDE.md` rule ③ exists to prevent.

The team's `develop` branch — **one person, once**, if session 01 did not create it:

```bash
git switch main && git pull
git switch -c develop && git push -u origin develop
```

Then pair 1 opens the pull request:

```bash
git switch -c chore/setup-buoi-02
git add JOURNAL.md pytest.ini
git commit -m "chore: cau hinh pytest asyncio; journal buoi 02"
git push -u origin chore/setup-buoi-02
gh pr create --fill --base develop
```

#### Pair 2 — the trap, found **before** reading pair 1's fix

Open the **Actions** tab first. **This pull request triggers nothing.** Why?

```bash
$ grep -A5 "^on:" .github/workflows/ci.yml
on:
  pull_request:
    branches: [main]      # a PR into develop triggers NO workflow at all
```

| | |
|---|---|
| no green tick | does **not** mean the tests are red |
| so pair 2 runs it themselves | `gh pr checkout <n>` → `make test` → `ruff check src/ tests/` |

```bash
gh pr checkout <n>         # pair 1's branch, on your machine
make test                  # must print 5 passed
ruff check src/ tests/     # must print All checks passed — CI runs this same command

gh pr review <n> --comment -b "Checked out and ran it: make test 5 passed, ruff clean."
gh pr review <n> --approve
gh pr merge  <n> --squash --delete-branch    # session 01's law: the author does not merge
gh run list --limit 3                        # CI runs NOW, on push to develop
```

**Protect pair 2's ignorance of the fix.** Their discovery — the Actions tab is empty, and here is
the line in the workflow that explains it — is the part people remember.

#### Finally, everybody

```bash
git switch develop && git pull
make test                  # 5 passed — on your own machine
```

#### The journal line about the CI trap

```
2026-11-04 · Gap 3: a PR into develop produced no CI run at all.
  ci.yml declares `pull_request: branches: [main]`, so develop PRs trigger nothing.
  No green tick ≠ red tests. Checked by running make test locally: 5 passed.
```

### The eight steps at a glance

| # | Who | Step | What they run |
|---|---|---|---|
| 1 | everybody | Establish the baseline | `make test` → **2 passed, 3 errors** |
| 2 | one person | Create `develop`, once | `git switch -c develop && git push -u origin develop` |
| 3 | **pair 1** | Diagnose with the agent — **no editing yet** | the prompt above |
| 4 | **pair 1** | Apply, commit, open the PR | `gh pr create --fill --base develop` |
| 5 | **pair 2** | Find the CI trap **first** | Actions tab empty → `grep -A5 "^on:" .github/workflows/ci.yml` |
| 6 | **pair 2** | Verify by running, not by looking | `gh pr checkout <n>` → `make test` → `ruff check` |
| 7 | **pair 2** | Review, approve, merge | `--comment` → `--approve` → `--squash --delete-branch` |
| 8 | everybody | Pull, and confirm on your own machine | `git switch develop && git pull` → `make test` |

### The point to take away

**A green CI is something you make, not something you are given.** A team that accepts a template
with red tests and leaves them red for six weeks arrives at week 6 unable to tell a new failure
from an old one — so nobody dares change anything.

And this is the first time most people see a fix travel from one machine to another **through a
pull request, not through a chat message**.

### Manual — do these yourself, no AI

- Pair 2 does **not** read the fix before finding the trap.
- Running the tests before approving — never approve by eye.
- The author does not merge their own pull request.

### Keywords — self-study

`pytest.ini` · `asyncio_mode` · `async fixture` · `workflow trigger` · `on: pull_request` ·
`branch filter` · `gh pr checkout` · `ruff` · `squash merge`

### Done when — run it yourself

```bash
gh run list --limit 3      # a SUCCESSFUL run after the merge
make test                  # 5 passed — on all four machines
```

---

## Cross-exercise — swap roles (20 minutes)

Pair up — but **change partners from the lab**: whoever worked together in exercise 2.5 now
separates. **A** plays a **team member**, **B** plays the **mentor**. 10 minutes, then swap.

### A sends three messages, one minute apart, read exactly

1. *"I can't get it installed, the whole screen is red."*
2. *(sends a screenshot of a wall of text)* *"Here."*
3. *"We've settled the idea, so I'll set up Docker and CI now, to be safe — all right?"*

### B must do three things

| To message | B must |
|---|---|
| 1 | Ask **exactly one** question that gets usable information: *"what does the `ERROR` line say?"* Not *"have you tried again?"* — and **not** *"what does the last line say?"*, because exercise 2.1 just showed that pip's last line is advice about upgrading pip. |
| 2 | **Refuse the screenshot.** Ask for the text, pasted — text can be given to an agent, an image cannot. Then steer A to read that line themselves, without stating the conclusion for them. |
| 3 | Ask which **starting tier** the team is at, and *"do you have a USP and feedback from 5 real users yet?"* before agreeing to any infrastructure. |

### Scoring

One point each. Below 2/3 → run it again with a different partner.
Answering *"go and check it again"* scores **zero**: it returns the work to the team with no new
information.

---

## Appendix — if something goes wrong

### A.1 The failures that cover almost everything today

| Symptom | Cause | Fix |
|---|---|---|
| `No module named …` | venv not active in this terminal | `source .venv/bin/activate`, install again |
| `Permission denied` on `pip install` | installing into system Python | same — activate the venv |
| `make: command not found` | build tools missing (often Windows) | use Git Bash; or run the underlying command from the `Makefile` directly |
| `Address already in use` on `make run` | a server is already on port 8000 | stop the other terminal, or run on another port |
| `curl` returns nothing | ran in the terminal that is busy serving | use a **second** terminal |
| `make test` ≠ 2 passed, 3 errors | your environment differs | re-check the install before exercise 2.5 |
| PR shows no checks | **expected** — `ci.yml` only watches `main` | that is exercise 2.5, step 5 |

### A.2 Undoing things safely

```bash
git switch develop                 # leave your branch
git restore <file>                 # throw away edits to one file
git reset                          # unstage everything, keep edits
deactivate                         # leave the virtual environment
rm -rf .venv && python3 -m venv .venv   # start the environment over
```

Never "fix" a leaked key by deleting the commit. **Revoke the key**, then tell the facilitator.

### A.3 Self-learning prompts

Paste these into the agent **after** the lab:

```
Explain what `asyncio_mode = auto` does in pytest-asyncio, and what breaks without it.
Cite the pytest-asyncio documentation with a link. Do not change any files.
```

```
Read .github/workflows/ci.yml and list, as a table, every event that triggers it and every
branch filter on that event. Quote the YAML lines. Then tell me which of my PRs would run it.
```

```
Explain the difference between `git status` and `git ls-files` for a file matched by
.gitignore. Give me one command that proves the difference in this repo.
```

### A.4 Where everything lives

| What | Path in the repo |
|---|---|
| Gates, deliverables, rubric | `docs/giao-trinh/00-khung-danh-gia.md` |
| The MT-01 brief | `docs/de-bai.md` |
| What was trimmed from the template, and why | `DA-LUOC.md` |
| The agent's rules | `CLAUDE.md` · `.agents/rules/` |
| This session, slide by slide | `docs/giao-trinh/slide/buoi-02_lecture_note.md` |
| The slide deck | `docs/giao-trinh/slide/buoi-02.html` |
| Your notes on the three gaps | `JOURNAL.md` |
