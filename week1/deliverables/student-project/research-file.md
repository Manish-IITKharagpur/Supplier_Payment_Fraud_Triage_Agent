# Research File — Supplier Payment Fraud Triage Agent

> Status legend: ✅ verified by me · 🔲 not yet verified · ❓ open question
> Anything marked 🔲 is an AI suggestion and must not be used in the paper until I check it.

---

## 1. Problem statement

The agent observes a supplier's email request to change their bank account details. It must select
**approve**, **hold-and-verify**, or **reject** because the sender's true identity is not known — it
may be the legitimate supplier, a spoofed impostor, or a criminal inside the supplier's compromised
real mailbox.

## 2. Project objective

To show how an agent acts under uncertainty by using probability to form a belief over hidden
states, then choosing one of three actions (approve, hold-and-verify, reject). The action threshold
is set by a human in Week 1 (to be derived from error costs in Week 2).

## 3. My experience level and what I do not know

- Experience: beginner in probabilistic agents; comfortable with Python.
- What the agent does not have: the sender's true identity, whether the supplier's mailbox is
  compromised, the supplier's real bank details (unless verified out-of-band), the real base rate
  of fraudulent change requests in my setting.

---

## 4. Technical terms

Sort each term into the last column myself: **U** = I can explain it in my own words, **N** = not yet.

### Fraud / email domain

| Term | Plain meaning | U / N |
|---|---|---|
| Business Email Compromise (BEC) | Fraud where an attacker uses email to trick a business into sending money | U |
| Vendor Email Compromise (VEC) | BEC where the attacker impersonates or controls a *supplier* rather than an executive | U |
| Account takeover (ATO) | Attacker has the real login for a real mailbox | U |
| Lateral phishing | Phishing sent *from* a genuinely compromised internal/partner account |U|
| Spoofing | Forging the "From" so the email appears to come from someone else |U|
| Display-name spoofing | Real-looking name, but a different email address behind it |U|
| Lookalike / cousin domain | `acme-supp1y.com` instead of `acme-supply.com` |U|
| SPF (Sender Policy Framework) / DKIM (DomainKey Identified Mail) / DMARC(Domain Based Message Authentication Reporting and Conformance ) | Email authentication checks that show whether a domain authorised the sender |U|
| Reply-To mismatch | Replies go to a different address than the From address |U|
| Out-of-band verification | Confirming via a *different channel* (e.g. phone number already on file, not one in the email) |U|
| Callback verification | The specific out-of-band check: call the supplier on a known number |U|
| Vendor master file | The company's record of each supplier's bank details |U|
| Segregation of duties | No single person can both change bank details and release payment |U|
| Penny-drop / beneficiary name check | Sending ₹1 to a new account to see whose name the bank returns |U|
| Three-way match | Matching purchase order, goods receipt and invoice before paying |U|

### Decision / probability

| Term | Plain meaning | U / N |
|---|---|---|
| Hidden state | The true situation the agent cannot see directly |U|
| Prior | Belief in each hidden state *before* looking at this email |U|
| Base rate | How common each state is in general | U |
| Likelihood P(evidence \| state) | If the state were X, how often would I see this evidence? | U |
| Posterior P(state \| evidence) | Updated belief after seeing the evidence | U |
| Bayes' rule | prior × likelihood, then normalise |U|
| False positive / false negative | Holding a legitimate request / approving a fraudulent one |U|
| Cost matrix | Cost of every (action, true state) combination |U|
| Expected cost | Probability-weighted average cost of an action | U |
| Decision threshold | Posterior probability at which the action switches | U |
| Calibration | When the agent says 70%, is it right 70% of the time? | U |
| Human-in-the-loop / escalation | Sending the decision to a person | U |
| Alert fatigue | Humans ignore holds because too many are false alarms | U |
| Adversarial adaptation | Attackers change behaviour once they learn the rules | U |

---

## 5. Search queries

**For practitioner knowledge**
- `vendor bank account change request verification process accounts payable`
- `"vendor impersonation" bank details change fraud callback`
- `supplier bank change fraud "compromised email" real supplier`
- `penny drop verification vendor onboarding India`

**For research**
- `"business email compromise" detection USENIX OR arXiv`
- `"lateral phishing" compromised account detection`
- `cost-sensitive classification decision threshold misclassification cost`
- `"vendor email compromise" dataset`

**For Reddit** (use `site:reddit.com`)
- `site:reddit.com accounts payable vendor changed bank details scam`
- `site:reddit.com "bank details" supplier email hacked paid wrong account`

---

## 6. Reddit communities — candidates to verify

Activity checked on 2026-09-25 with the `reddit-mcp-buddy` MCP server (newest post date).
The tool cannot read community rules, so **I must still read the rules by hand before posting.**

| Community | Why relevant | Active (post < 7 days) | Rules allow my question | Keep? |
|---|---|---|---|---|
| r/Accounting | AP staff who actually process bank-change requests | ✅ newest 2026-09-25 | ✅ my question was posted and got answers | ✅ **Kept.** Discussion done, see discussion-record.md |
| r/AccountsPayable | Most direct audience | ❌ does not exist or is not accessible | — | ❌ **Removed** |
| r/Bookkeeping | Small-business side; fewer controls, different costs | ✅ newest 2026-09-24 | 🔲 | |
| r/sysadmin | Admins who investigate compromised mailboxes | ✅ newest 2026-09-25 | 🔲 | |
| r/msp | Managed service providers clean up BEC incidents for clients | ✅ newest 2026-09-24 | 🔲 | |
| r/cybersecurity | Broad security practitioners; good for the spoofed vs compromised split | ✅ newest 2026-09-25 | 🔲 | |
| r/AskNetsec | Q&A format suits "which signal would you trust?" | ✅ newest 2026-09-25 | 🔲 | |
| r/smallbusiness | Victims' perspective: who bears the cost | ✅ newest 2026-09-25 | 🔲 | |
| r/procurement | Vendor onboarding and vendor master owners | 🔲 | 🔲 | |
| r/Scams | Real incident stories (historical comparable cases) | 🔲 | 🔲 | |
| r/CAIndia (or similar) | Indian practice, e.g. penny-drop, GST vendor checks | 🔲 | 🔲 | |

Target: keep 5–10 after verification. Remove any that are inactive or forbid this kind of question.

---

## 7. X accounts — to find and verify

Rule from the brief: researchers, engineers, users **and critics**, not just popular AI accounts.
Target 15–25.

| Category | How to find | Accounts (fill after checking recent posts are relevant) |
|---|---|---|
| Authors of the BEC / lateral phishing papers below | Search author names from Section 8 | |
| Email security researchers / threat intel | Search `"vendor email compromise"`, `"BEC"` on X, last 30 days | |
| Security journalists covering BEC cases | e.g. Brian Krebs (KrebsOnSecurity) 🔲 | |
| AP / treasury / finance-ops practitioners | Search `"accounts payable" fraud`, `"bank details change"` | |
| Decision theory / uncertainty / agent researchers | Search `"expected cost" agent`, `"calibration" LLM` | |
| Critics of AI fraud detection | Search `"false positives" fraud AI`, `"alert fatigue"` | |

---

## 8. Useful papers, reports and datasets

Existence confirmed by web search on 2026-09-25. **Mark ✅ only after I have opened and read the
relevant part myself.** Do not cite before that.

| # | Source | Why it matters to my agent | Read? |
|---|---|---|---|
| 1 | Cidon et al., *High Precision Detection of Business Email Compromise*, USENIX Security 2019. <https://www.usenix.org/conference/usenixsecurity19/presentation/cidon> | Real BEC detector (BEC-Guard); shows BEC has no malicious payload, so content and sender signals matter | 🔲 |
| 2 | Ho et al., *Detecting and Characterizing Lateral Phishing at Scale*, USENIX Security 2019. <https://www.usenix.org/conference/usenixsecurity19/presentation/ho> | Evidence for my **compromised real mailbox** state: attacks sent from genuine accounts | 🔲 |
| 3 | Liu et al., *A Large-Scale Analysis of Attacker Activity in Compromised Enterprise Accounts* (arXiv 2007.14030). <https://arxiv.org/abs/2007.14030> | What attackers do once inside a real mailbox; timing and behaviour signals | 🔲 |
| 4 | Elkan, *The Foundations of Cost-Sensitive Learning*, IJCAI 2001. <https://dl.acm.org/doi/10.5555/1642194.1642224> | Theory for deriving a decision threshold from error costs (needed for Week 2) | 🔲 |
| 5 | FBI IC3, *2024 Internet Crime Report*. <https://www.ic3.gov/AnnualReport/Reports/2024_IC3Report.pdf> | Scale and cost of BEC (reported ~$2.77B, 21,442 complaints in 2024), for motivation | 🔲 |
| 6 | AFP, *2025 Payments Fraud and Control Survey Report*. <https://www.financialprofessionals.org/topics/payment-topics/payments-fraud> | Practitioner survey; reports vendor/third-party impersonation rising | 🔲 |

**Datasets.** ❓ I have not found a public, labelled dataset of supplier bank-change requests.
The Enron email corpus is real email but has no fraud labels for this task. Current plan: a
**simulated** case set with every assumed number labelled as assumed. That is a limitation for the paper.

---

## 9. Questions I want to answer

### Hidden states
1. Are three states enough? Is there a **"something else"** state, e.g. a legitimate supplier
   whose change is a mistake, or an insider inside *my* company?
2. How do practitioners tell a spoofed sender from a compromised real mailbox?
3. What is a realistic base rate of fraudulent bank-change requests among all bank-change requests?

### Evidence
4. Which signals do AP teams actually trust: domain match, SPF/DKIM/DMARC, reply-to, urgency
   language, timing, change history?
5. Which signals are **useless for the compromised-mailbox case**, because everything genuinely
   comes from the real account?
6. How reliable is callback verification, and what does it cost (time, staff attention, supplier
   annoyance)?
7. Are some signals really measuring the same thing (e.g. lookalike domain and DMARC fail)?

### Actions
8. What does "hold-and-verify" involve in practice, and how long does it take?
9. When does a human have to decide regardless of the agent's confidence (e.g. above an amount)?
10. Is "reject" ever right, or is it always hold and verify?

### Errors and costs
11. Which costs more: a wrong approve (money sent to a criminal) or a wrong hold (late payment,
    damaged relationship, late-payment penalties)? By how much?
12. Can a wrong approve be corrected (bank recall)? How often, and within what time window?
13. Who bears each cost: AP team, finance, the supplier, or the bank?

---

## 10. Claims that need a source or a test

| Claim | Source or test needed |
|---|---|
| Compromised-mailbox fraud passes email authentication checks | Ho et al. 2019; practitioner confirmation |
| Callback verification catches most fraudulent changes | Practitioner answers; AFP report |
| A wrong approve costs much more than a wrong hold | Ask AP practitioners; cannot assume |
| My priors (base rates) | Must be labelled **assumed** unless I find data |
| My likelihood tables | Must be labelled **assumed/elicited**; state who or what they came from |
| The agent beats an "always hold" baseline on decision cost | My experiment (Step 6) |

---

## 11. Parts of my problem that are not clear yet

- Is the input only the email, or also vendor history (past changes, payment amounts)?
- ✅ **Decided (2026-09-29): the setting is India.** Payments go by NEFT/RTGS/IMPS; bank accounts are
  identified by IFSC code, so a bank-location check compares the IFSC branch with the supplier's
  known location, and a penny-drop name check is available.
  - Accepted from r/Accounting: bank-location mismatch as evidence · logo/signature near-uninformative ·
    always-hold-and-verify baseline.
- ✅ **Decided (2026-09-27): "reject" means the email is blocked and never reaches the receiver's
  inbox.** So the agent works as an email gateway in front of the AP inbox.
  - Cost of a wrong reject: a legitimate bank change is lost *silently*, and the supplier is paid
    to the old account or the payment fails.
  - ✅ **Decided (2026-09-27): the email is blocked and a security alert is logged.** A wrong
    reject is therefore *recoverable*: security can review the alert and release the email. The
    cost of a wrong reject depends on how quickly security reviews alerts (and alert fatigue
    applies to the security queue too).
- ✅ **Policy sketch (2026-09-27): reject only when confident it is fraud; hold when unsure.**
  This gives three zones: approve below p₁ · hold-and-verify between p₁ and p₂ · reject above p₂.
  p₁ = verification cost ÷ fraud loss (derived). ❓ p₂ still to derive from costs: where do hold
  and reject have equal expected cost?
  - ✅ **What reject gains over hold (2026-09-27): reject alerts security, so a hacked mailbox gets
    found.** That gain exists only in the *compromised* state; for *spoofed*, hold and reject both
    keep the money safe. So p₂ should depend mainly on **P(compromised)**, not on P(fraud) as a
    whole. **This is why spoofed and compromised must stay separate hidden states: they lead to
    different best actions.**
  - ✅ **Decided (2026-09-30): a failed callback also raises a security alert** (option B). A call
    alone shows *fraud*, not *which kind*: the supplier may assume spoofing and never check their
    own mailbox, so the alert is what gets a compromised mailbox found. Consequence: reject's
    advantage over hold shrinks. What is left (my answer, 2026-09-30): **timing**. Reject alerts
    immediately; hold alerts only after the callback fails. ❓ How long is that gap in practice,
    and what can the attacker do during it? ❓ Reliability: if the call is fooled, hold never alerts.
  - Cost of holding on a compromised mailbox with no alert (my answer, 2026-09-30): the attacker
    keeps access and can attack again.
- Is the decision per request or per payment? A request could be approved while the payment is held.
- What happens *after* the action (feedback): does the agent ever learn the true state?

---

## 11a. Insights from learning sessions (use in the paper)

- **SPF/DKIM cannot separate legitimate from compromised.** P(passes | legitimate) and
  P(passes | compromised) are both very high, so the signal only helps against *spoofed*.
  This matches TheElRojo's r/Accounting reply ("there would've been no clue").
- **Alert fatigue degrades the best evidence.** At high volume, "always call" means rushed calls,
  so P(callback catches it | fraud) drops. Testable claim: always-verify wins at low weekly
  volume; a selective agent can be cheaper *and* safer at high volume.
- **Adversarial adaptation makes stored likelihoods stale** (distribution shift). Attackers drop
  signals the agent is known to use, so the posterior for fraud comes out too low and false
  negatives rise. Prefer evidence the attacker cannot control: callback to a number on file,
  penny-drop name check.
- **Threshold is derived from costs** (umbrella problem): hold if P(fraud) > cost of one
  verification ÷ loss if fraud approved. This comes out below 1%, which matches the CPAs' "always
  call". The loss scales with the payment amount, so **the threshold must fall as the amount
  rises**.
- **Calibration is not enough on its own.** An agent that says "2% fraud" to everything can look
  calibrated and catch nothing. It also needs discrimination.
- **Two escalation rules:** (1) a stakes rule: above an amount cap, always go to a human;
  (2) an uncertainty rule: if beliefs are split across states, escalate.

## 12. AI prompts used

**Research prompt (from the course brief), run with Claude, 2026-09-25:**

```text
I am a beginner. I want to design an AI agent for this problem: a supplier emails a request to
change their bank account details; the agent must approve, hold-and-verify, or reject because the
sender's true identity (legitimate / spoofed / compromised real mailbox) is not known.
The agent must make decisions when information is not complete.

Help me prepare my research.
1. Give me the technical terms for this problem.
2. Give me useful search queries.
3. Find 5 to 10 relevant Reddit communities.
4. Tell me why each community is relevant.
5. Find relevant researchers and engineers on X.
6. Give me questions about hidden states, evidence, actions, and errors.
7. Identify each claim that needs a source or a test.
8. Tell me which parts of my problem are not clear.

Do not present uncertain information as fact.
```

## 13. Important AI errors and limits observed

| Date | Tool | What happened | How I caught it / what I did |
|---|---|---|---|
| 2026-09-25 | Claude | Suggested "Support agent" as my problem based on an old memory of a different project, when I had already chosen supplier payment fraud | I corrected it by pointing it to my Notion notes |
| 2026-09-25 | Claude | Could not verify Reddit communities (Reddit blocked automated access), so the list is unverified | Left as 🔲; I will verify by hand |
| 2026-09-25 | Claude | Could not confirm specific X handles, so it gave search methods, not a list of accounts | Will build the list myself |
| 2026-09-25 | Claude | Suggested r/AccountsPayable as "the most direct audience"; it does not exist or is not accessible | Activity check with reddit-mcp-buddy returned "not found"; removed |
| 2026-09-30 | Claude | Overstated that after a hold-and-verify call "the hacked mailbox is still hacked" and nobody knows. That ignored what the company does after a failed callback, a design choice that had not been made yet | I challenged it ("doesn't the phone call also make everyone aware?"). This led to deciding that a failed callback also raises a security alert (§11) |
| 2026-09-30 | Claude | Asked a check question implying the callback delay affects p₂ "but not p₁". Too strong: a longer callback also raises the cost of holding a legitimate supplier, which is part of p₁ (a small effect) | Claude corrected itself when reviewing my answer; logged |
| — | — | *(Add more here: invented citations, wrong numbers, overconfident claims)* | |
