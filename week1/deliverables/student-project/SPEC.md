# Product specification — Supplier Payment Fraud Triage Agent

> Status: draft. A coding agent must not implement product code until this specification is approved through Genesis.

## Problem

*My ideas, wording completed by Claude 🔲 (check before approval).*

- **The clerk's problem:** a compromised email is hard to detect, fraudsters create urgency, and
  because fraud is rare the clerk gets fatigued and checks less carefully.
- **Why it is hard:** the possible worlds look similar to the clerk, and fraud keeps changing.
- **What the agent does:** it does not know the hidden reality, so it holds a probability for each
  of the three possible senders (legitimate / spoofed / compromised mailbox) and chooses the action
  with the lowest expected cost.
- **When and why it beats "always call":** at high volume, calling on every request becomes rushed
  and careless; the agent saves careful callbacks for the suspicious cases, so they stay careful.

## Users

*Confirmed by me 2026-10-06; wording by Claude 🔲.*

- **Accounts-payable clerk:** receives the bank-change email and acts on the agent's decision (pay,
  call the supplier on a number on file, or see the email blocked).
- **Finance manager:** sets the costs the thresholds are built from (cost of a callback, cost of a
  delay, loss if fraud is approved); bears responsibility for losses.
- **Security / IT team:** receives alerts from rejects and failed callbacks; investigates the
  mailbox and can release a wrongly blocked email.
- **Supplier (affected, not a user):** a legitimate supplier is delayed by a hold or a wrong reject;
  a compromised supplier learns their mailbox is hacked through the alert.

## Functional requirements

*Confirmed by me 2026-10-06 (from my recorded decisions); wording by Claude 🔲.*

- FR-1: Takes one bank-change request as input: email clues (domain, SPF/DKIM, reply-to, thread,
  urgency), the new bank details (IFSC, account) and the supplier's change history.
- FR-2: Holds a probability for each of the 3 worlds (legitimate / spoofed / compromised),
  starting from a supplier-specific prior (L2).
- FR-3: Updates that probability with each clue using Bayes' rule and the evidence table (L3).
- FR-4: Computes the expected cost of approve / hold-and-verify / reject from the finance
  manager's costs and chooses the lowest (approve below p₁, hold between p₁ and p₂, reject above p₂).
- FR-5: On hold, calls the supplier on a number already on file (never one taken from the email)
  and runs a penny-drop name check; updates the belief with both results.
- FR-6: If the callback says the supplier never sent the request, raises a security alert.
- FR-7: On reject, blocks the email before the inbox and logs a security alert; security can
  release a wrongly blocked email.
- FR-8: Escalates to a human above an amount cap (stakes rule) or when the belief is split across
  worlds (uncertainty rule).
- FR-9: Shows the clerk why it decided: the belief over the 3 worlds, the clues that moved it most,
  and the expected cost of each action.

## Non-functional requirements

*Confirmed by me 2026-10-06; wording by Claude 🔲.*

- NFR-1: (honest numbers) every probability and cost either cites a source or is labelled
  "assumed". No unlabelled numbers.
- NFR-2: (reproducible) anyone can re-run the experiment from the README and get the same results
  (fixed random seed for the simulated cases).
- NFR-3: (privacy) no real emails, bank details or personal data; simulated cases only.
- NFR-4: (robust to unknowns) results report how decisions change when the uncertain inputs vary
  (callback delay, prior, likelihoods), i.e. a sensitivity check.
- NFR-5: (calibration) the results check whether the agent's probabilities match reality on the
  simulated cases (of cases given about 70% fraud, about 70% are fraud), alongside discrimination.

## Constraints

*Confirmed by me 2026-10-06; wording by Claude 🔲.*

- Setting is India: IFSC codes, NEFT/RTGS/IMPS, amounts in ₹, penny-drop name check.
- No public labelled dataset exists, so cases are simulated (30–50, per the course brief).
- Small and simple build: one Python file or spreadsheet (course Step 5).
- Clues are treated as independent within each world (a simplification; see Risks).
- Output is a 4-page IJCAI-style preprint (course Step 9).

## Non-goals

*Confirmed by me 2026-10-06; wording by Claude 🔲.*

- No connection to a real email inbox or bank system.
- Does not make or stop real payments; it only recommends an action.
- Does not detect other fraud types (fake invoices, phishing links, malware).
- No machine learning from data: probabilities come from sources or labelled assumptions.

## Acceptance criteria

*Confirmed by me 2026-10-06; wording by Claude 🔲.*

- AC-1: For every test case, the agent outputs one action plus a probability for each of the 3
  worlds, and the 3 probabilities sum to 1.
- AC-2: A sanity case with clean email clues and wrong money clues gives the compromised world the
  highest probability.
- AC-3: The experiment runs the agent, a second policy and the always hold-and-verify baseline on
  30–50 simulated cases and reports, for each, a confusion matrix and total expected cost (not
  accuracy alone).
- AC-4: 5 wrong decisions are examined and each failure is traced to its layer (L0–L4).
- AC-5: The sensitivity check (callback delay, prior, likelihoods) and the calibration check are run
  and reported.
- AC-6: One uncertain case is walked through every layer in the decision record (course Step 7).
- AC-7: Every number in the code or configuration is labelled with a source or "assumed".
- AC-8: The results state whether and when the agent beats always hold-and-verify. Winning is
  not required: an honest "always-call wins at low volume" passes.

## Risks

*Confirmed by me 2026-10-06; wording by Claude 🔲.*

- **Circular testing:** simulated cases built from my own assumptions may make the agent look good
  because it is tested on its own beliefs. Mitigation: generate cases with settings different from
  the agent's, and state this in the paper.
- **Unsourced numbers** (likelihoods, prior). Mitigation: label them "assumed"; sensitivity check (NFR-4).
- **Clue independence is false for some pairs** (lookalike domain + SPF/DKIM). Mitigation: known
  simplification, reported as a limitation.
- **Attackers adapt:** attacker-controlled clues (urgency, lookalike domain) fade. Mitigation:
  prefer clues the attacker cannot control (callback to a number on file, penny-drop).
- **False alarms:** sole-proprietorship accounts in the owner's personal name; legitimate suppliers
  banking in another city.
- **Callback fails** if the clerk is rushed or the number comes from the email. Mitigation: FR-5
  requires a number on file.
- **Practitioner input is mostly US-based and a small sample.**

## Open questions

*Confirmed by me 2026-10-06.*

- How long does a callback take in practice? (p₂ depends on it.)
- What is the range of the prior, and the split between spoofed and compromised?
- Do Indian companies alert security after a failed callback?
- Should there be a fourth "something else" hidden state?
- What else does the agent learn after acting (feedback)?
