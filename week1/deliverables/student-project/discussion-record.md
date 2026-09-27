# Discussion Record

Public discussions on Reddit and X, and how each one affected the agent.
Raw thread text is saved in `discussions/` as evidence.

Result types (from the brief): new assumption · new failure condition · new test ·
change to the agent · change to the probability model · no change (with reason).

> Rows marked **(proposed)** are AI-suggested design changes that I have not yet accepted or rejected.

| Platform | Community or account | Link | My first contribution | Human answer | My next answer | Design change |
|---|---|---|---|---|---|---|
| Reddit | r/Accounting | 🔲 ** | Asked how AP staff decide between processing and verifying a vendor bank-change request, what triggers a callback to a known number, and whether a legit-looking request ever turned out to be fraud. | **Funny-Lavishness-413:** stops and calls the number on file if the address is one letter off or the tone is unusually urgent. One request had the exact logo and signature, but the routing number was for a different region; the callback showed the vendor never sent it. | Asked whether they check routing region on every request, or only when something else already feels off. *(No reply yet.)* | **New evidence source (proposed):** bank-location mismatch (routing region; in India, the IFSC branch vs the supplier's location). **Change to probability model (proposed):** logo and signature carry almost no information, because an impostor copies them, so P(looks right \| state) is about the same for every state. |
| Reddit | r/Accounting | (same thread) | (same) | **TheElRojo (CPA, US):** "Always verify." A vendor's IT was compromised and an email came from their real account (with a malicious link). If it had asked for an ACH change instead, "there would've been no clue." | Asked whether a callback to a known number is the only defence in the account-compromise case, or whether anything else ever tips them off. *(No reply yet.)* | **Confirms a hidden state:** compromised real mailbox is a real, observed case. **New failure condition:** in that state every email-based signal looks clean, so an agent using only email evidence will approve it. Only out-of-band verification tells it apart. |
| Reddit | r/Accounting | (same thread) | (same) | **alphabet_sam (CPA, US):** "I would always call." Bank-change requests are rare enough that you are not calling vendors all day, and fraud losses are large. | "That base-rate point is the part I was missing, thanks." | **New test (proposed):** add an **always hold-and-verify** baseline. If the cost of a callback is tiny next to the loss, this baseline may be very hard to beat. The agent must show *when* it is cheaper, e.g. at high request volume or when callbacks are slow or costly. **New assumption to test:** a callback costs little because requests are rare. |

## Thread summary: r/Accounting

**Summary.** Three practitioners replied. All three verify by calling a number already on file. They
disagree on *when*: one calls when a signal looks off (lookalike address, urgency, wrong bank
region); two CPAs call on **every** request.

**Point of disagreement.** Signal-triggered verification versus unconditional verification. These are
two policies the experiment can compare directly.

**New testable assumption.** Unconditional verification is cheap because bank-change requests are
rare. The experiment can find the request volume, or callback cost, at which a selective agent
becomes cheaper than always calling.

**Change for the agent.** Add bank-location mismatch as evidence. Treat logo and signature as close to
uninformative. Add an always-verify baseline.

**Information to verify.**
- Is a routing-region / IFSC-branch mismatch a reliable fraud signal, or do legitimate suppliers
  often bank outside their home region?
- How long does a callback take, and what does a delayed payment cost?
- Does my setting (India or US) change which checks are available?

**AI summary checked against the original thread:** 🔲 (I must re-read the thread and tick this)
