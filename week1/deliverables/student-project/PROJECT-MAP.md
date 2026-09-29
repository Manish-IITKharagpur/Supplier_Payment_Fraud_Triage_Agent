# Project Map: Supplier Payment Fraud Triage Agent

> **What this file is:** the story of how the agent is being built, and which concept powers which
> part of it. Read section A to know where you are. Read B to remember how you got here. Use C and D
> to see how everything connects. It is updated at the end of every session.
>
> Legend: ✅ done / understood · 🟡 in progress / introduced · ⬜ upcoming · 🔲 needs my check

---

## A. You are here *(updated 2026-09-29)*

- **Stage:** Week 1, **Step 3 (agent design on paper)**. The **policy** layer (L4) is being derived.
- **Last session:** accepted 3 design changes from r/Accounting; set the setting to **India**.
- **Right now:** deriving **p₂**, the threshold between *hold* and *reject*.
- **Next:** finish p₂ → evidence list + likelihood tables (L1/L3) → 2 more Reddit posts → X list →
  read Ho 2019 + Elkan 2001.
- **Waiting on me:** one line "reason for accepting" in `discussion-record.md` · Gaussian
  teach-back · confirm the 🔲 entries in section B.

---

## B. The journey so far

Each stage: **what the agent could do → what went wrong or what we noticed → the concept → the
design change.**

### Part 1: The course chapters, *The Belief Engine* (Week 1 Saturday)
*Stages 1–4 are reconstructed from the course dry runs and my Notion Learning Tracker. 🔲 Check that
they match how I remember them.*

**1. Ch0, The 92% Lie: an agent that guesses and forgets** 🔲
- **Agent:** V0 reads an email and gives **one guess, one action**. No memory, no reasons.
- **Noticed:** a weather man "right 92 nights out of 100" can still be useless. In a dry town,
  saying "no rain" every night scores high. The total hides *which* mistakes he made.
- **Concepts:** evaluation harness (human labels = the answer key), class imbalance, accuracy trap,
  confusion matrix, precision, recall, three-agent comparison, cost of mistakes (a false approve
  loses money irreversibly; a false hold only delays).
- **Design change:** never judge the agent by accuracy alone. Look at the four boxes and at what
  each mistake *costs*.

**2. Ch1, The Email Behind the Curtain: the truth is hidden** 🔲
- **Agent:** V1. Message 101: "we changed our bank account, pay ₹4,80,000 by noon".
- **Noticed:** like a friend who hasn't shown up, several *complete stories* could be true, and one
  already *is* true, just hidden. Waiting is also a decision with a cost.
- **Concepts:** sample space (possible worlds), probability distribution (shares of belief that sum
  to 1), events, random variables, Bernoulli, V1 architecture (hidden reality → visible clues →
  current belief).
- **Design change:** the agent holds a **belief over worlds** instead of jumping to one answer. The
  course used 4 worlds: legit / copied sender / hacked mailbox / **spam**.

**3. Ch2, The Base-Rate Trap: where the starting belief comes from** 🔲
- **Agent:** updates its belief when a supplier callback says "we did not change our account".
- **Noticed:** the same text ("stuck in traffic") means different things from Arjun vs Vikram. The
  *starting pile* matters, and chapter 0's "8 dangerous in 100" is the wrong pile for bank-change
  requests.
- **Concepts:** base-rate trap, reference class, the mirror swap P(A|B) ≠ P(B|A) (*a known slip point
  of mine*), prior / likelihood / posterior, Bayes' rule, contrast case.
- **Design change:** the prior must come from **comparable** cases (bank-change requests), and a
  strong clue is a *sighting*, not proof.

**4. Ch3, The Distribution Zoo: the layered agent** 🟡
- **Agent:** organised into layers: **L0** what a mistake costs → **L1** possible worlds → **L2**
  what is usual in *this* inbox → **L3** the shape of the answer → **L4** the decision.
- **Concepts:** Binomial ✅, Poisson ✅, Exponential ✅, **Gaussian 🟡 (teach-back pending)**, Uniform ⬜,
  expectation and variance ⬜.
- **Design change:** each layer can fail in its own way, so a failure can be traced to a layer.

### Part 2: My project sessions

**5. Choosing the problem (2026-09-24 → 25)** ✅
- **Decision:** supplier bank-change emails; **3 hidden states**: legitimate / spoofed / compromised
  real mailbox; actions approve / hold-and-verify / reject.
- **Noticed:** the **compromised** mailbox is the blind spot, because every email signal looks clean.
- **Evidence:** `research-file.md` §1–3.

**6. Asking real people: r/Accounting (≈ 1 month ago; logged 2026-09-25)** ✅
- **Heard:** "perfect logo and signature, still fraud"; "email came from their real account, there'd
  be no clue"; "I would always call".
- **Concepts:** an uninformative signal (same likelihood in every state); the compromised state is real.
- **Design changes (accepted 2026-09-29):** bank-location (IFSC) mismatch as evidence · logo and
  signature ≈ no information · an **always hold-and-verify baseline**.
- **Evidence:** `discussion-record.md`.

**7. Learning the decision vocabulary (2026-09-26)** ✅
- **Concepts:** base rate (again, applied), likelihood vs posterior (the forward/backward trick),
  SPF/DKIM can't separate legit from compromised, **alert fatigue** (always-call degrades at high
  volume), **adversarial adaptation** (attackers make likelihoods stale → distribution shift),
  **expected cost**, **derived threshold** (umbrella: 1/10 = 10%), **calibration vs discrimination**,
  **escalation** (stakes rule + uncertainty rule).
- **Design change:** **p₁ = verification cost ÷ fraud loss**. It comes out under 1%, which matches the
  CPAs, and falls as the amount rises.
- **Evidence:** `research-file.md` §11a.

**8. Defining the actions (2026-09-27)** ✅
- **Decisions:** **reject** = block the email + log a security alert (recoverable). Three zones:
  approve < p₁ < hold < p₂ < reject.
- **Noticed:** reject beats hold **only in the compromised state**, because it gets the hacked
  mailbox found.
- **Design change:** p₂ depends on **P(compromised)**, which is why spoofed and compromised must stay separate.

**9. Setting and first decisions accepted (2026-09-29)** ✅
- **Decision:** the setting is **India** (IFSC, NEFT/RTGS/IMPS, penny-drop, ₹).
- **Now:** deriving p₂ (hold vs reject). My cost answers so far: hold on legit → relationship cost;
  hold on compromised → money safe, but *"I don't know"* what else; reject on legit → costly;
  reject on compromised → money safe.

---

## C. Concept → agent map

| Concept | In plain words | Where it lives in the agent | Learned | Status |
|---|---|---|---|---|
| Accuracy trap / class imbalance | A high score can hide the mistakes that matter | Evaluation (Step 6) | Ch0 | ✅ |
| Confusion matrix, precision, recall | The four boxes; "of my alarms, how many real?" / "of real frauds, how many caught?" | Evaluation (Step 6) | Ch0 | ✅ |
| Cost of mistakes | Different wrong answers cost different amounts | **L0** cost matrix | Ch0 | ✅ |
| Sample space / hidden states | The complete list of possible worlds | **L1** worlds | Ch1 | ✅ |
| Probability distribution | Beliefs across worlds, summing to 1 | **L1–L2** belief | Ch1 | ✅ |
| Base rate / reference class | How common each world is among *comparable* cases | **L2** prior | Ch2 | ✅ |
| Likelihood P(evidence \| state) | If the world were X, how often would I see this? (forward) | **L3** evidence tables | Ch2 | ✅ |
| Posterior P(state \| evidence) | The updated belief after seeing evidence (backward) | **L3→L4** | Ch2 | ✅ |
| Bayes' rule | prior × likelihood, then normalise | **L3** update step | Ch2 | ✅ |
| Binomial / Poisson / Exponential | Shapes for yes-no counts, counts in a window, waiting times | **L3** shapes | Ch3 | ✅ |
| Gaussian | Values clustering around a usual value (e.g. invoice amount) | **L3** shapes | Ch3 | 🟡 |
| Uniform · expectation and variance | A fair pick · average and spread | L3 / auditing | Ch3 | ⬜ |
| Uninformative evidence | Same likelihood in every state carries no information | **L3** (logo, signature; SPF vs compromised) | Session 7 | ✅ |
| Alert fatigue | Too many alarms → careless checks | **L4** policy + cost of holds | Session 7 | ✅ |
| Adversarial adaptation | Attackers change → likelihoods go stale | Limitations; **L3** robustness | Session 7 | ✅ |
| Expected cost | Probability-weighted cost of each action | **L4** decision rule | Session 7 | ✅ |
| Derived threshold p₁ | Verification cost ÷ fraud loss | **L4** approve/hold line | Session 7 | ✅ |
| Calibration vs discrimination | "70% means 70%" and "different cases get different numbers" | Evaluation (Step 6, Week 2) | Session 7 | ✅ |
| Escalation | Send to a human for high stakes or split beliefs | **L4** policy | Session 7 | ✅ |
| Threshold p₂ | Where hold and reject cost the same | **L4** hold/reject line | Session 9 | 🟡 |
| Entropy | How spread out (uncertain) the belief is, in bits | L1–L3 diagnostics | Week 2 | ⬜ |
| Information gain (expected) | How much a check is *expected* to reduce uncertainty | Choosing which check to run | Week 2 | ⬜ |
| Value of information · stop rule | A check is worth it only if some result would change the action | **L4** information selection | Week 2 | ⬜ |
| Conditional entropy · mutual information | Average uncertainty left after a check · shared information | Information selection | Week 2 | ⬜ |
| Cross-entropy · KL · Jensen–Shannon | Distances between belief distributions | Calibration and shift checks | Week 2 | ⬜ |
| Distribution shift | The world changes after you estimated it | Limitations; robustness test | Week 2 | ⬜ |
| Exploration vs exploitation | Try something new vs use what works | Policy learning (feedback) | Week 2 | ⬜ |

---

## D. The agent as it stands

| Part (course brief) | Layer | Status | What is decided | Where | Paper section |
|---|---|---|---|---|---|
| **Input** | — | 🟡 | A supplier bank-change email (+ vendor history? open) | research-file §11 | Agent design |
| **Hidden states** | L1 | 🟡 | legitimate / spoofed / compromised. Open: a "something else" state (the course had *spam*) | CLAUDE.md, §1 | Probability model |
| **Belief** | L2 | ⬜ | prior over 3 states. Where do the numbers come from? (not yet) | — | Probability model |
| **Evidence** | L3 | 🟡 | candidates: lookalike domain, SPF/DKIM/DMARC, reply-to, urgency, **IFSC mismatch** (accepted), logo/signature (≈ uninformative), callback, penny-drop | discussion-record, §4 | Probability model |
| **Actions** | L4 | ✅ | approve · hold-and-verify (callback) · reject (block + security alert) | research-file §11 | Agent design |
| **Cost** | L0 | 🟡 | wrong approve = payment lost; wrong hold = delay + relationship; wrong reject = recoverable delay; hold on compromised = ? (being worked out) | §11, session 9 | Decision rule |
| **Policy** | L4 | 🟡 | three zones; **p₁ derived**; p₂ in progress; escalation: stakes + uncertainty | §11a | Decision rule |
| **Feedback** | — | ⬜ | what the agent learns after acting (callback result, security finding) | — | Agent design |
| **Human reasoning function** | — | ⬜ | candidate: change belief after new evidence (callback) | — | Agent design |
| **Experiment** | — | ⬜ | policies + baselines incl. **always hold-and-verify**; India setting; simulated cases | — | Test method / Results |
