# Session 01 — Exercises: your first pull request, and the first time the agent is wrong

**Exercise book** · companion to the slide deck [`buoi-01.html`](buoi-01.html) (exercise slides `#ex1-brief` … `#ex4-steps`) and the lecture note [`buoi-01_lecture_note.md`](buoi-01_lecture_note.md) · course: *Mentoring for AI-aided Projects* · The Odyssey

> **How to use this file.** The slides carry the short version of each exercise. This file carries the full version.
> Every exercise has the same parts, in the same order as its two slides:
>
> | Slide 1 — the brief | Slide 2 — the steps |
> |---|---|
> | **Task** — the problem, what goes in and what must come out | **Main steps** — each with the command to run and what you are looking at |
> | **Input & tools** — knowledge, tools, files | **Manual — no AI** — things a person must do, where handing it to an agent defeats the purpose |
> | **Expected output** — the files or the state, with the figure or table of what is in them | **Keywords** — terms to look up for self-study |
>
> Each exercise ends with a **Done when** check you run yourself.
> Shell commands are written for **Git Bash** (the repo's default shell on Windows) and macOS/Linux alike.

---

## Contents

| # | Exercise | After slides | Time | Continues in |
|---|----------|--------------|------|--------------|
| 0 | [Before you start — the rules of the game](#0--before-you-start--the-rules-of-the-game) | — | read | — |
| 1.1 | [Join the team repo](#exercise-11--join-the-team-repo) | 3 – 16 | 18′ | every later session |
| 1.2 | [Your first pull request](#exercise-12--your-first-pull-request) | 18 | 22′ | session 02, ex 2.5 |
| 1.3 | [Review, then merge someone else's](#exercise-13--review-then-merge-someone-elses) | 18 | 12′ | session 02, ex 2.5 |
| 1.4 | [Demand the command](#exercise-14--demand-the-command) | 17 | 8′ | session 02, ex 2.4 |
| X | [Cross-exercise — swap roles](#cross-exercise--swap-roles-20-minutes) | — | 20′ | every session |
| A | [Appendix — if something goes wrong](#appendix--if-something-goes-wrong) | — | any time | — |

Exercise slides in the deck: Ex 1.1 = slides 19 – 20 · Ex 1.2 = 21 – 22 · Ex 1.3 = 23 – 24 · Ex 1.4 = 25 – 26.

---

## 0 · Before you start — the rules of the game

### 0.1 One cohort, one team, one repo

The whole cohort of mentors is **one team working on one repo**:

```
https://github.com/UEH-ISB/mt-01-the-odyssey
```

Not one repo each. Your real teams will work this way, and the hard part of GitHub was never
typing the commands — it is four people changing one repo without overwriting each other.

Today's four exercises build on each other:

```
Ex 1.1  clone + prove your push right + read the brief
          │
          └──► Ex 1.2  your own row in the README team table → a pull request
                        │
                        └──► Ex 1.3  somebody else's pull request → review → merge → pull
                                      │
                                      └──► Ex 1.4  ask the agent → run the command → JOURNAL.md
```

**Rule:** nothing in this lab is a throw-away drill. The branch you open in 1.2 is the branch
somebody reviews in 1.3, and the journal line you write in 1.4 ships in your second pull request.

### 0.2 The team's law

> **Nobody merges their own pull request.**

This is not administrative procedure. It is the mentor's whole trade compressed into one action:
somebody else reads the work before it counts as done. We start the habit today, on a one-line
change, precisely so that it survives into week 4 when the change is frightening.

The facilitator may enforce it with a ruleset on `main` (*Require a pull request before merging* +
*Require 1 approval*). The repo is public, so rulesets are free. When it is on, nobody can push
straight to `main` — including the facilitator. That is correct, and it will be announced so that
nobody thinks their machine is broken.

### 0.3 Who decides what

The repo's agent rules apply from the first minute:

| The agent does | **You** do |
|---|---|
| reads files, drafts text, proposes options, explains terms | decide what goes in, in your own words |
| suggests a check | **run the check yourself** — "the agent said it works" is not a check |
| cites a file and a section | **open that file** and confirm the quote |

If the agent in this repo answers "which would you prefer?" instead of choosing, that is the rules
working, not the agent being unhelpful.

### 0.4 What you need before the lab starts

| | Check |
|---|---|
| Git installed | `git --version` |
| GitHub CLI, logged in | `gh auth status` → *Logged in to github.com* |
| A coding agent installed | opens and answers |
| **Collaborator invitation accepted** | see Exercise 1.1 step 2 — **the one that silently blocks everything** |

Nothing is installed today beyond this. Building the Python environment is session 02.

---

## Exercise 1.1 · Join the team repo

**Slides:** `#ex1-brief`, `#ex1-steps` (19 – 20) · **Theory:** slides 3 – 16 · **Time:** 18 minutes

### Task

You do **not** create a repo. The team repo already exists and you are a **collaborator** on it.

- clone `UEH-ISB/mt-01-the-odyssey`;
- prove your **push right** works — before anything depends on it;
- read the brief and the framework that ship **inside** the repo, and answer three questions with
  the files closed.

### Input & tools

| | |
|---|---|
| **Knowledge** | clone vs fork · collaborator vs owner · what a remote is |
| **Tools** | `git` · `gh` (logged in) |
| **Files** | `docs/de-bai.md` · `docs/giao-trinh/00-khung-danh-gia.md` |
| **Slides** | 5–7 the gates · 12–16 MT-01 · 18 the lab |

### Expected output

A working clone, a proven push right, and three answers.

```bash
$ git remote -v
origin  https://github.com/UEH-ISB/mt-01-the-odyssey.git (fetch)
origin  https://github.com/UEH-ISB/mt-01-the-odyssey.git (push)

$ gh api repos/UEH-ISB/mt-01-the-odyssey --jq '.permissions.push'
true
```

| # | Question | Answer |
|---|---|---|
| 1 | How many **layers** does MT-01 have, and which one is syllabus review? | three · the middle one (B) |
| 2 | Which **three constraints** stop this being a chatbot wrapped around an API? | lecturer approval · every finding cites its passage · three roles with a server-side boundary |
| 3 | Which folder holds deliverable #10 — missing in ~82% of teams? | `eval/` (plus `docs/evaluation.md`) |

### Main steps

#### Step 1 · Clone the team repo (4′)

```bash
gh repo clone UEH-ISB/mt-01-the-odyssey
cd mt-01-the-odyssey
```

**Check:** `git remote -v` points at UEH-ISB, and `git log --oneline` shows the repo's history.

#### Step 2 · Prove the push right — do not skip (4′)

Cloning works for anybody who can read a public repo. Push is a different permission, and you will
not find out until you need it.

```bash
gh api repos/UEH-ISB/mt-01-the-odyssey --jq '.permissions.push'
```

- `true` → you are ready.
- `false` or `404` → **the organisation invitation is still unaccepted.** Open your GitHub inbox
  (<https://github.com/notifications> or the email) and accept it now. Not at exercise 1.2, when a
  push fails in the middle of your first pull request.

#### Step 3 · See that the course ships inside the repo (2′)

```bash
ls docs/giao-trinh/ docs/de-bai.md
```

Every source this course cites is a file in here — the lessons, the slides, the evaluation
framework and the project brief. Nothing points at an external handbook. That is why, from session
02 onward, *"where did you read that?"* is always a question with an answer, for you and for the
agent.

#### Step 4 · Read the brief and answer the three questions (8′)

```bash
head -60 docs/de-bai.md
```

Then read the sections *Ba năng lực* (three capabilities) and *Ba vai và ranh giới quyền* (three
roles and the permission boundary).

Answer the three questions **out loud, to the person beside you, with the file closed.**

> **Question 1 is the one people get wrong.** The usual wrong answer is "a syllabus-checking
> system". That is layer **B** only. The project has three layers: the learner's assistant
> (grounded Q&A plus ESL support), course standardisation (a 6-criterion rubric with lecturer
> approval), and the programme picture (anonymised aggregate numbers). Syllabus review is the
> middle one.

### Manual — do these yourself, no AI

- **Accepting the invitation** — an account action, in your own inbox.
- **Reading the brief** — a summary of it is not the same as having read it.
- **Answering the three questions** — from memory, not from a prompt.
- **Checking the push right** — read the output yourself.

### Keywords — self-study

`clone vs fork` · `remote` · `origin` · `collaborator` · `organisation member` ·
`gh auth status` · `repository permissions` · `grounded answer` · `human in the loop`

### Done when — run it yourself

```bash
gh api repos/UEH-ISB/mt-01-the-odyssey --jq '.permissions.push'   # true
ls docs/giao-trinh/ docs/de-bai.md                                 # both exist
```

…and you answered all three questions with the file closed.

### Common mistakes (facilitator watch-list)

| What happens | What it means | Fix |
|---|---|---|
| Somebody forks the repo | reflex from public open source | they must clone, not fork — a fork's PRs are a different workflow |
| Somebody creates their own repo | "I'll just make my own" | one team, one repo; a repo outside the org is invisible to the grading system |
| `.permissions.push` is `false` | invitation unaccepted | accept it now, re-run the check |
| `gh: command not found` | GitHub CLI not installed | install it; `gh auth login` afterwards |

---

## Exercise 1.2 · Your first pull request

**Slides:** `#ex2-brief`, `#ex2-steps` (21 – 22) · **Theory:** slide 18 · **Time:** 22 minutes
**The core exercise of the session.**

### Task

Open `README.md`, find the **Đội ngũ** (team) table, and fill in **the one row the facilitator
assigned to you**:

- your name;
- your real GitHub username;
- the role you are taking in the team;
- **one real question** you still have about MT-01.

Then branch, commit, push and open a pull request — and **stop before merging it**.

> **Edit your row only.** Each person has their own row, which is why four pull requests into the
> same table merge cleanly and nobody meets a conflict on their first day. Touching somebody else's
> row produces a **merge conflict**, which is a later session's lesson.

### Input & tools

| | |
|---|---|
| **Knowledge** | branch · commit · push · pull request — the six moves of slide 18 |
| **Tools** | `git` · `gh` · a text editor |
| **Files** | `README.md`, the **Đội ngũ** table only |
| **Given** | your row number, written on the board by the facilitator |

#### The six Git moves — all the GitHub you need for fifteen sessions

| Move | What it actually means |
|---|---|
| `git switch -c <branch>` | Open your own draft. Nobody else sees it; nobody gets overwritten. |
| `git add <file>` | Choose exactly what you are submitting — not everything you touched. |
| `git commit -m "…"` | Package it, with the reason. Somebody will read that sentence, including you in three weeks. |
| `git push -u origin <branch>` | Send the draft to GitHub. Still **not** in the main branch. |
| `gh pr create` | Ask: "this is what I want to add." It opens a place for somebody to read it. |
| `gh pr merge` | Agree, and take it in. **The reviewer's job, not the author's.** |

### Expected output — `README.md`, one row changed, one open pull request

*Before — the row assigned to you:*

```markdown
| # | Tên | GitHub | Vai mình nhận trong đội | Câu mình chưa rõ về MT-01 |
|---|---|---|---|---|
| 1 | _(chưa điền)_ | _(chưa điền)_ | _(chưa điền)_ | _(chưa điền)_      ← your row
| 2 | _(chưa điền)_ | …
```

*After:*

```markdown
| 1 | Nguyễn Văn A | @nguyenvana | Deliverables | Where do layer C numbers come from? |
```

*And the check that this went in cleanly:*

```bash
$ gh pr status     # your PR exists, and has a number
$ gh pr diff       # exactly ONE + line and ONE − line
```

> **More than one line changed** means you touched somebody else's row, or `git add -A` pulled in
> something extra. That is the most common week-1 mistake on a real team, and it is why a mentor
> opens the **Files changed** tab before approving anything.

### The eight steps at a glance

| # | Step | Run it one line at a time — do not paste the block |
|---|---|---|
| 1 | Open your own draft | `git switch -c gioi-thieu/<your-name>` |
| 2 | Edit **your row** and save | `README.md` → the **Đội ngũ** table → your row number |
| 3 | Choose what you submit | `git add README.md` — **not** `git add -A` |
| 4 | Package it with a reason | `git commit -m "docs: them <ten-ban> vao bang doi ngu"` |
| 5 | Send the draft to GitHub | `git push -u origin gioi-thieu/<your-name>` |
| 6 | Open the pull request | `gh pr create --fill --base main` |
| 7 | **Check your own diff** | `gh pr diff` → exactly one `+` and one `−` |
| 8 | **Stop. Do not merge.** | Your PR is now exercise 1.3 for the person beside you. |

#### Step 3 in detail · name the file

```bash
git add README.md       # yes
git add -A              # no
```

`git add -A` is how unrelated files end up in a pull request — an editor's swap file, a
`.DS_Store`, a half-finished note. Once a team has that habit, every review gets harder, because
the reviewer has to work out which parts of the diff are the actual change.

#### Step 5 in detail · if the push is rejected

```
remote: Permission to UEH-ISB/mt-01-the-odyssey.git denied
```

This is almost always the unaccepted invitation from exercise 1.1, step 2 — not a typo. Accept it,
then `git push -u origin <branch>` again. The commit is already safe on your machine; nothing is
lost.

#### The last column is not decoration

Write a question you actually want answered. **Session 03 takes these exact questions and answers
them.** A question written to look good wastes its author's own time.

Good: *"Where does layer C get numbers from before any course has been approved?"*
Not useful: *"How can I contribute best to the team?"*

### Manual — do these yourself, no AI

- **Your own row** — your name, your username, your question.
- **Reading your own diff** before asking anybody to review it.
- **The commit message** — one line, in your own words.

### Keywords — self-study

`branch` · `staging area` · `commit message convention` · `push -u` / `upstream` ·
`pull request` · `base branch` · `diff` · `merge conflict` · `squash merge`

### Done when — run it yourself

```bash
gh pr status      # your PR, with a number
gh pr diff        # one + line, one − line
```

…and you have **not** merged it.

### Common mistakes (facilitator watch-list)

| What happens | What it means | Fix |
|---|---|---|
| The diff shows many lines | `git add -A`, or the editor reformatted the table | reset, edit only the row, re-commit |
| The diff shows two rows | they edited somebody else's row too | revert the other row before pushing |
| Pull request opened against the wrong base | `--base` omitted | `gh pr edit <n> --base main` |
| Somebody merges their own | enthusiasm | re-open the lesson: that is exercise 1.3's job |

---

## Exercise 1.3 · Review, then merge someone else's

**Slides:** `#ex3-brief`, `#ex3-steps` (23 – 24) · **Theory:** slide 18 · **Time:** 12 minutes

### Task

Take the pull request of the person beside you. Check three things, leave **one real question**,
then approve and merge. Afterwards, bring the team's work down to your own machine — and notice
that it did not arrive by itself.

### Input & tools

| | |
|---|---|
| **Knowledge** | reading a diff · approve vs comment · squash merge · why `pull` is a separate act |
| **Tools** | `gh pr list` · `gh pr diff` · `gh pr review` · `gh pr merge` |
| **Files** | somebody else's row in `README.md` |
| **Given** | the pull request number of the person beside you |

### Expected output — three checks, one question, one merge, one pull

#### The three checks — a real PR review, in miniature

| Check | Passes when |
|---|---|
| Did they edit **only their own row**? | the diff has one `+` line and one `−` line |
| Is the **GitHub** column a real username? | open `github.com/<username>` and look |
| Is the last column a **real question**? | it can be answered from the brief — not a polite placeholder |

These three are scope, truthfulness and substance. They are the same three questions you will ask
in week 4 about a change to the agent's guardrail, where you cannot read the logic. **The habit is
what transfers, not the checks themselves.**

#### Then, and only then

```bash
gh pr review <n> --comment -b "Your open question is … — which layer would you answer it in?"
gh pr review <n> --approve
gh pr merge  <n> --squash --delete-branch

git switch main && git pull
grep -n "^| [0-9] |" README.md    # somebody else's row, now on your machine
```

### The seven steps at a glance

| # | Step | What you run |
|---|---|---|
| 1 | Find the four pull requests | `gh pr list` |
| 2 | Read **only** what they changed | `gh pr diff <n>` — their change, not the whole repo |
| 3 | Run the three checks | scope · real username · real question |
| 4 | Leave a question about the content | `gh pr review <n> --comment -b "…"` |
| 5 | Approve | `gh pr review <n> --approve` |
| 6 | Merge — **not your own** | `gh pr merge <n> --squash --delete-branch` |
| 7 | Bring the team's work to your machine | `git switch main && git pull` |

### Two things the room will get wrong

**Merged but never pulled.** Merging changed GitHub. Your laptop finds out when you ask it to.
*"Why don't I have that file?"* in week 3 is almost always this, and the answer is `git pull`, not
a re-clone.

**Approved without opening `Files changed`.** That is signing a blank page. It matters more for a
mentor than for anyone else, because a team copies what their mentor actually does, not what their
mentor says.

### Manual — do these yourself, no AI

- **Reading the diff** — the exercise is the reading.
- **Opening `github.com/<username>`** to confirm the account is real.
- **Writing the review question** in your own words.

### Keywords — self-study

`code review` · `approve vs request changes` · `squash merge` · `delete branch` ·
`git pull vs fetch` · `fast-forward` · `branch protection` · `ruleset`

### Done when — run it yourself

```bash
gh pr list --state merged         # your PR, merged BY SOMEBODY ELSE
grep -n "^| [0-9] |" README.md    # ≥2 filled rows on your machine
```

---

## Exercise 1.4 · Demand the command

**Slides:** `#ex4-brief`, `#ex4-steps` (25 – 26) · **Theory:** slide 17 · **Time:** 8 minutes

### Task

Now open the agent. Two questions, **one per turn**:

```
1. How many files does this repo currently track in git? Answer with a number.

2. If I run `make test` right now, what is the result?
```

Then run both commands yourself and write down the gap.

> **One question per turn.** Asked together, the agent often hedges on both. Asked separately, you
> get the confident wrong answer that makes the point.

### Input & tools

| | |
|---|---|
| **Knowledge** | slide 17 — guessing instead of running |
| **Tools** | your coding agent · `git ls-files` · `make` |
| **Files** | `JOURNAL.md` — one line, in the repo |

### Expected output — `JOURNAL.md`, one honest line

#### Run it yourself — do not trust the answer

```bash
$ git ls-files | wc -l
104

$ make test
2 passed, 3 errors
```

No Python environment yet? That is expected — the environment is session 02. **Write the error
down verbatim; an error message is a result.** Copying it accurately is itself a skill the room
will need all course.

> The three errors are **left red on purpose**. The template ships with a real defect so that
> session 02 has something real to fix. Do not fix it today.

#### The journal line, in this shape

```
2026-10-28 · Q2: the agent answered "the tests will all pass". WRONG.
  Ran it: 2 passed, 3 errors.
  Cause: it guessed from the familiar case instead of running anything.
  Lesson: a question about repo state is answered by a command — here, make test.
```

**Four parts, every time:**

| Part | Why it is there |
|---|---|
| what it claimed | the evidence; without it the note is an opinion |
| what is true | the number or the output, copied, not remembered |
| **why** it was wrong | the part people drop, and the part that makes it reusable |
| the command that settles it | so the next person can repeat the check |

### The five steps at a glance

| # | Step | What you do |
|---|---|---|
| 1 | Ask question 1, alone | **Write the answer down before you check it.** |
| 2 | Ask question 2, alone | `make test` — what does it predict? |
| 3 | Run both yourself | `git ls-files \| wc -l` · `make test` |
| 4 | Write one journal line | claim · truth · **cause** · command |
| 5 | Ship it in your next PR | same six moves as 1.2 — this time nobody talks you through them |

> **Step 1 has a quiet trick in it.** Writing the agent's answer down before checking is what stops
> you rewriting your own memory afterwards. People who check first will later swear the agent was
> roughly right.

### If your agent is careful and runs the commands first

Newer agents often do, and that is a **better** outcome, not a failed exercise. Still do it:

1. **Write down which command it ran.**
2. Run that command yourself.
3. Compare the outputs.

The goal is the habit of demanding a source, not catching the agent out. A mentor's question is
*"what did you run?"*, not *"are you sure?"*.

### Manual — do these yourself, no AI

- **Running the two commands** — the whole point.
- **Writing the cause** in your own words.
- **Reading the error verbatim** if `make test` cannot run yet.

### Keywords — self-study

`hallucination` · `grounding` · `tool use` · `git ls-files` · `make target` ·
`pytest exit code` · `reproducible check` · `development journal`

### Done when — run it yourself

```bash
git ls-files | wc -l       # the number you wrote in the journal
gh pr status               # the journal line is in a pull request of yours
```

---

## Cross-exercise — swap roles (20 minutes)

Pair up. **A** plays a **week-1 team lead**, **B** plays the **mentor**. After 15 minutes, swap.

### A says exactly these three sentences — read them, do not improvise

1. *"We've settled on the idea, so this week I'll set up Docker and CI first, to be safe."*
2. *"I tried it and the agent's answers seem fine, so I'll drop the eval and add two more features."*
3. *"Could you just write this small API for me? I've been stuck for two days."*

### B must do three things

| To sentence | B must |
|---|---|
| 1 | Point out the team is at **gate G1**, has not passed G2, and ask *"have 5 real users seen the prototype yet?"* **before** agreeing to any infrastructure. |
| 2 | Answer with **the number 82%**, not with general advice. Then ask *"fine on what — how many cases, measured how?"* |
| 3 | **Refuse to write the code**, and convert it into an open question about where they are stuck. |

### Scoring

One point each. Below 2/3 → run it again with a different partner.

> Answering *"go and check it again"* scores **zero**, because it pushes the work back to the team
> without giving them any new information.

**Why sentence 3 is a refusal and not unkindness:** writing code for a team in week 2 is the
fastest route to week 6 with a team that cannot defend its own product to the judges, because the
product is not theirs.

---

## Appendix — if something goes wrong

### A.1 The five failures that cover almost everything today

| Symptom | Cause | Fix |
|---|---|---|
| `gh: command not found` | GitHub CLI not installed | install `gh`, then `gh auth login` |
| `Permission … denied` on push | org invitation unaccepted | accept it, push again — your commit is safe |
| `gh pr diff` shows many lines | `git add -A` | `git reset`, add only `README.md`, commit again |
| `CONFLICT (content): README.md` | you edited somebody else's row | do **not** delete their lines; call the facilitator |
| `make: command not found` | no build tools yet | expected today — record the error verbatim and continue |

### A.2 Undoing things safely

```bash
git switch main                     # leave your branch, change nothing
git switch -c <branch> --force      # start the branch over from main
git reset                           # unstage everything, keep your edits
git restore README.md               # throw away edits to one file
```

Nothing here deletes a commit you have already pushed. If you want to undo something that is
already on GitHub, open a new pull request instead — that is the honest record, and it is what a
real team does.

### A.3 Self-learning prompts

Paste these into the agent **after** the lab, not during it:

```
Explain, in 5 sentences, the difference between `git fetch` and `git pull`, and give me a
command that shows what fetch brought in before I merge it. Cite the git documentation.
```

```
I am new to pull requests. Explain what a "squash merge" does to the commit history, and
show me the command that lets me see the history before and after. Do not run anything.
```

```
Read docs/giao-trinh/00-khung-danh-gia.md §1 and quote, verbatim, the minimum-evidence
column for gates G1 to G6. Say which file and section each quote came from. Do not summarise.
```

### A.4 Where everything lives

| What | Path in the repo |
|---|---|
| Gates, deliverables, rubric | `docs/giao-trinh/00-khung-danh-gia.md` |
| The MT-01 brief | `docs/de-bai.md` |
| This session, slide by slide | `docs/giao-trinh/slide/buoi-01_lecture_note.md` |
| The slide deck | `docs/giao-trinh/slide/buoi-01.html` |
| The team table you edited | `README.md`, section **Đội ngũ** |
| Your notes on where the agent was wrong | `JOURNAL.md` |
