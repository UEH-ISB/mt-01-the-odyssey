# Session 01 — Six weeks, six gates, six criteria: what a mentor will be asked

> **Sources for this session:** [the evaluation framework — gates, deliverables, rubric](00-khung-danh-gia.md)
> · [the MT-01 brief](../de-bai.md)
> **120 minutes.** Inside the project repo these two files live at `docs/giao-trinh/` and `docs/de-bai.md`.

> **This file is the session plan** — objectives, run of show, cross-exercise, homework and the
> pass checklist. The teaching material itself is split across three companions:
>
> | File | What it carries |
> |---|---|
> | [`slide/buoi-01.html`](slide/buoi-01.html) | the 27 slides, with speaker notes (`N`) |
> | [`slide/buoi-01_lecture_note.md`](slide/buoi-01_lecture_note.md) | one section per slide: key points, explanation, key terms, self-check |
> | [`slide/buoi-01_ex.md`](slide/buoi-01_ex.md) | the exercise book — the full version of the 60-minute lab |

## What a mentor must be able to do after this session

- State **without looking it up**: which week is which gate, and what the minimum evidence for that gate is.
- Name the **three deliverables the previous cohort most often failed to produce**, and explain why those are the easiest marks on the table.
- **Catch an agent saying something false about their own team's repo**, and prove it with a command that produces a number.
- **Open a pull request and review somebody else's** — branch, commit, push, PR, review, merge, pull. These six moves recur in all fifteen sessions.

## The three questions a team will ask that this session answers

1. *"What exactly do I have to hand in this week?"*
2. *"It runs — does that mean I'm done?"*
3. *"I let the AI write most of the code. Do I lose marks?"*

## Prerequisites

Read the [MT-01 brief](../de-bai.md) (10 minutes). A coding agent installed, Git
installed, and `gh` logged in (`gh auth status` must report *logged in*). No Python needed yet.

**The collaborator invitation must already be accepted** — the facilitator sends it a day before.
Without it, exercise 1.2 cannot run.

---

## Run of show (120 minutes)

| Block | Time | Slides | Material |
|---|---|---|---|
| Opening | 5′ | 1 – 2 | cover · agenda |
| §1 What a mentor is for | 5′ | 3 – 4 | two jobs · the four-step loop |
| §2 Six weeks, six gates | 8′ | 5 – 7 | G1–G6 · minimum evidence · at-risk triggers |
| §3 "Running" is not "done" | 8′ | 8 – 11 | ten deliverables · **100 / 82 / 64** · six criteria · six mistakes |
| §4 MT-01 | 8′ | 12 – 16 | three layers · the five-step flow · three roles · across the gates · scope guard |
| §5 **The agent guesses** | 6′ | 17 | the most important slide of the day |
| **Lab** | **60′** | 18 – 26 | four exercises → [`buoi-01_ex.md`](slide/buoi-01_ex.md) |
| Cross-exercise | 20′ | — | swap roles, below |
| Summary + Q&A | 10′ | 27 | takeaways · homework |

Slide-by-slide notes, including what the presenter says, are in
[`buoi-01_lecture_note.md`](slide/buoi-01_lecture_note.md) and in each slide's speaker notes
(press `N` in the deck).

---

## Lab (60 minutes) — the first pull request of your life, in a team of four

**The whole cohort of mentors is ONE team working on ONE repo.** Not one repo each. Their real
teams will work this way, and the hard part of GitHub was never typing the commands — it is **four
people changing one repo without overwriting each other**. Session 01 teaches exactly that spot.

**After the lab the team repo has:** the **Đội ngũ** table in `README.md` filled in by all four
people, four merged pull requests — **each merged by somebody else, nobody merging their own** —
and a line in `JOURNAL.md` recording where the agent was wrong.

> **Team repo:** <https://github.com/UEH-ISB/mt-01-the-odyssey>

| # | Exercise | Time | Produces |
|---|---|---|---|
| 1.1 | Join the team repo — clone, **prove the push right**, read the brief | 18′ | a working clone, three answers |
| 1.2 | **Your first pull request** — your own row in the team table | 22′ | one open PR, one `+` and one `−` line |
| 1.3 | **Review, then merge** somebody else's | 12′ | your PR merged **by another person** |
| 1.4 | **Demand the command** — ask the agent, then run it | 8′ | one four-part line in `JOURNAL.md` |

**Full instructions, checks and troubleshooting:** [`slide/buoi-01_ex.md`](slide/buoi-01_ex.md).

### The team's law

> **Nobody merges their own pull request.**

Not administrative procedure — the mentor's whole trade compressed into one action. We start the
habit on a one-line change precisely so that it survives into week 4, when the change is
frightening.

### Facilitator preparation

**One day before** (organisation invitations need a human to accept them):

```bash
gh api -X PUT "repos/UEH-ISB/mt-01-the-odyssey/collaborators/<github-username>" -f permission=push
```

**Assign row numbers** in the team table — mentor A row 1, B row 2, C row 3, D row 4 — and write
them on the board. That is what keeps four pull requests conflict-free.

**On the day, before the session:**

```bash
git ls-files | wc -l     # today's number for slide 17 — it grows as the course grows
make test                # must print 2 passed, 3 errors
```

**Optional — enforce the law instead of relying on it.** A ruleset on `main`: *Require a pull
request before merging* + *Require 1 approval*. The repo is public, so rulesets are free. Nobody
can then push straight to `main`, including the facilitator — that is correct, but announce it, or
somebody will think their machine is broken.

---

## Cross-exercise (20 minutes) — swap roles, 15-minute breakout

Pair up. **A** plays a **week-1 team lead**, **B** plays the **mentor**. After 15 minutes, swap.

**A says exactly these three sentences — read them, do not improvise:**

1. *"We've settled on the idea, so this week I'll set up Docker and CI first, to be safe."*
2. *"I tried it and the agent's answers seem fine, so I'll drop the eval and add two more features."*
3. *"Could you just write this small API for me? I've been stuck for two days."*

**B must do three things:**

- **Sentence 1:** point out the team is at **gate G1** and has not passed G2, and ask *"have 5 real users seen the prototype yet?"* **before** agreeing to any infrastructure work.
- **Sentence 2:** answer with **the number 82%**, not with general advice. Then ask *"fine on what — how many cases, measured how?"*
- **Sentence 3:** **refuse to write the code**, and convert it into an open question about where they are stuck. Writing code for a team is the fastest route to a week 6 in which the team cannot defend its own product.

**Scoring:** one point each. Below 2/3 → run it again with a different partner. Answering *"go and
check it again"* scores **zero**: it returns the work to the team with no new information.

---

## Homework

1. Read [`docs/de-bai.md`](../de-bai.md) and [the framework](00-khung-danh-gia.md) **in full** — not skim. From session 02 onward every check points back to these two files.
2. **The six-gate map — by hand first, agent second.** Fill in a six-row table `Week | Gate | Minimum evidence | Session` from memory, **without opening `00-khung-danh-gia.md`**. Only then send:

   ```
   Read docs/giao-trinh/00-khung-danh-gia.md sections 1 and 2. Write docs/00-ban-do-khoa.md
   with a 6-row table: Week | Gate | Minimum evidence | Session of the course. Quote the
   evidence column verbatim, do not paraphrase. Add an "At-risk triggers" section, also verbatim.
   ```

   Then compare the two:

   ```bash
   diff <(sed -n '/^|/p' nhap.md) <(sed -n '/^|/p' docs/00-ban-do-khoa.md)
   ```

   **Done when** you can read every difference and say who was right. Where your memory was wrong
   is where your team will ask next week. Submit as a second pull request — this time nobody walks
   you through the commands.
3. **Install Python 3.12 (or 3.11)** and check with `python3 --version`. Session 02 builds the environment; anyone who cannot install loses half of it.
4. Draft **five use cases** of MT-01: five concrete groups of people and the concrete job each one needs done. No need to choose yet — session 05 reuses the list.

---

## Pass checklist for session 01

- [ ] Cloned `UEH-ISB/mt-01-the-odyssey`; `gh api repos/UEH-ISB/mt-01-the-odyssey --jq '.permissions.push'` prints `true`
- [ ] **Your pull request is merged, and somebody else merged it** — confirmed by `gh pr list --state merged`
- [ ] You reviewed somebody else's PR, leaving **a comment about the content** before approving
- [ ] After `git pull`, the **Đội ngũ** table in `README.md` on your machine shows somebody else's row
- [ ] You can state that MT-01 has **three layers**, and the three constraints that stop it being a chatbot wrapped around an API
- [ ] **`JOURNAL.md` records ≥1 place the agent was wrong about repo state, with the cause and the command that settled it**
- [ ] You can say the six gates and the three numbers (**100% / 82% / 64%**) without opening anything
- [ ] Cross-exercise ≥ 2/3

**Fail the sixth box and you do not move on to session 02.** The other boxes are procedure and can
still be fixed. The sixth is the trade.
