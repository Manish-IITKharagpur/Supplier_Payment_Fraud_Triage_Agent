# CLAUDE.md: AI-Native Sprint, Weeks 1 & 2

## The project
**Supplier Payment Fraud Triage Agent.** The agent observes a supplier's email request to change
bank details. It must **approve**, **hold-and-verify**, or **reject** because the sender's identity
is hidden: **legitimate / spoofed / compromised real mailbox**.

Decisions already made (details in `research-file.md` sections 11 and 11a):
- **Reject** = the email is blocked before the inbox and a security alert is logged (recoverable).
- **Policy sketch:** approve below p₁ · hold between p₁ and p₂ · reject above p₂.
  p₁ = verification cost ÷ fraud loss (falls as the amount rises). p₂ depends mainly on
  P(compromised), because reject's extra value is getting the hacked mailbox found.
- Spoofed and compromised stay **separate states** because they lead to different best actions.
- **Setting: India** (IFSC codes, NEFT/RTGS/IMPS, penny-drop checks, amounts in ₹).
- Accepted from r/Accounting: bank-location (IFSC branch) mismatch as evidence; logo and signature
  are near-uninformative; an **always hold-and-verify** baseline in the experiment.

## How to work with me: learning first
I am a beginner learning this material. Follow the course coach
(`Week_1&2_Deliverable_Instructions/week2/skill/coach/SKILL.md`):
- **Teach concepts freely**: plain words → a concrete example (from a *different* domain) → a
  formula only if asked. Then check with a short probe question.
- **Never invent numbers for my problem** (priors, likelihoods, costs, thresholds). Ask where my
  number would come from. Assumed numbers must be labelled "assumed".
- **Do not write my paper sections.** Outline, critique and point at gaps instead.
- Code: build it *with* me and explain each part; debugging and checking my arithmetic are fine.
- Mark anything AI-drafted that I have not verified with 🔲. Never cite a source I have not read.
- Log AI mistakes in `research-file.md` section 13 (needed for the AI-use statement).
- **Public discussions (Reddit/X) are mine.** Help draft and summarise, never post for me.
- **When I'm stuck** ("I don't know", confused, off-target reply), follow the `/stuck` ladder:
  shrink the question → worked example on a different problem → 2–3 options I pick from. At most
  2 hint rounds before options.

## Where things are
| Path | What |
|---|---|
| `week1/deliverables/student-project/` | **My work**, in the course's required structure |
| `week1/deliverables/student-project/PROJECT-MAP.md` | **Where I am, how I got here, concept → agent map.** Read first every session |
| `Week_1&2_Deliverable_Instructions/` | Course brief (Week 1 md, Week 2 PDF, coach and readiness skills) |
| `Week_1&2_Resources/` | Class recordings, transcripts, dry-run chapters (HTML) |
| `../Visibility/` | LinkedIn/X class material (`session-NN/`) and my post drafts (`my-posts/`). Shared across weeks, outside this repo |
| `Deliverable_1_examples/` | A classmate's finished project (Example_1 = Example_2, identical). Reference only, never copy |

## Progress tracking
- **Notion is the source of truth for progress.** Everything lives under one page,
  **"AI Native Engineering Sprint Hub"**. Its sub-pages: "Week 1 Deliverable — Supplier Payment
  Fraud Triage Agent", "Public Presence", "Session Log", and later Week N pages.
- **Start of session:** read `week1/deliverables/student-project/PROJECT-MAP.md`, then
  `week1/deliverables/student-project/.genesis/KICKOFF.md` (Genesis phase and next action), then
  the hub's *Current focus* and the latest Session Log entry. **Before any new work, open with a briefing**
  in this format (short, in plain words):
  1. **Where we are:** stage and step, plus the last session in one line
  2. **The journey so far:** the story in 4–6 lines (what the agent could do → what we learned →
     what changed), not a list of files
  3. **Concepts in play today:** which ones, and which agent layer (L0–L4) each belongs to
  4. **Today's goal** and anything waiting on me
- **End of session (every time):** (0) update `PROJECT-MAP.md`: rewrite section A, add a section B
  entry for the session, and update C/D if a concept or agent part changed; (1) tick items on the
  Week page; (2) add a Session Log entry (covered / stopped at / next); (3) rewrite the hub's
  *Current focus* (now / next / waiting on me); (4) record new decisions with `genesis record` and
  run `genesis checkpoint week1/deliverables/student-project`; (5) commit and push.
- `/where-am-i` gives the same briefing at any point mid-session.
- The hub's "Deliverables" page is **outdated and contains wrong information. Ignore it** and never
  use it as a source.
- Visibility work (LinkedIn/X/Reddit posts, profile, post ideas queue) is tracked on the Notion
  page "Public Presence" in the same hub.

## Git
- Repo root is this folder: `https://github.com/Manish-IITKharagpur/Supplier_Payment_Fraud_Triage_Agent`
  (private until publishing in Step 10).
- `.gitignore` tracks only `week1/`, `week2/`, `README.md`, `CLAUDE.md`. Never commit the course
  materials, recordings or the classmate's example.
- Commit at natural checkpoints with messages that explain the *design decision*, then push to `main`.
- Do not use the git repo at `C:\Users\rajma` (it is an accidental home-folder repo).

## Tools
- `reddit-mcp-buddy` MCP: read-only Reddit (activity checks, reading threads). Limited to 10 requests a minute.
- **Genesis** (`~/.local/bin/genesis`, kit in `../genesis-kit-main/`): spec-first harness, set up
  2026-10-06 in `week1/deliverables/student-project/`. It holds the ledger of decisions, tasks and
  proof (`.genesis/project.json`). Phase: discovery, so **no implementation code** until I approve
  `SPEC.md`. I write SPEC.md; Claude interviews and critiques. Only I approve
  (`genesis spec approve`). PROJECT-MAP keeps the learning story, Notion keeps the session log.
