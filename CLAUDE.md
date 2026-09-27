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

## Where things are
| Path | What |
|---|---|
| `week1/deliverables/student-project/` | **My work**, in the course's required structure |
| `Week_1&2_Deliverable_Instructions/` | Course brief (Week 1 md, Week 2 PDF, coach and readiness skills) |
| `Week_1&2_Resources/` | Class recordings, transcripts, dry-run chapters (HTML) |
| `../Visibility/` | LinkedIn/X class material (`session-NN/`) and my post drafts (`my-posts/`). Shared across weeks, outside this repo |
| `Deliverable_1_examples/` | A classmate's finished project (Example_1 = Example_2, identical). Reference only, never copy |

## Progress tracking
- **Notion is the source of truth for progress:** page "Week 1 Deliverable — Supplier Payment
  Fraud Triage Agent" plus "Session Log" under "AI Native Engineering Sprint Hub". Read both at the
  start of a session; add a Session Log entry (covered / stopped at / next) at the end.
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
