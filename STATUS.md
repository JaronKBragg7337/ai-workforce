# STATUS — AI Workforce (Living Build Layer)

*System: AI Workforce — Hiring Compatible AIs to Work for You. Currently hireable: Grok · ChatGPT · Perplexity · Claude, plus pending future AIs as they prove compatible.*

This is the working file: what is being built right now, what is open, and what must be verified. The stable architecture is in SPEC.md. Maintenance convention: at the close of any session where something is built, tested, decided, or seriously considered, add a dated line to the Change Log at the bottom — including possibilities, which are logged rather than cut.

---

## Current Build Target

**Proving ground: Small tier — Grok + ChatGPT, on existing accounts.** This pair already shares the working mailbox, so its loop runs unattended from the start, and it tests everything genuinely hard about the architecture. The system is proven here before anything is added.

**Next: Medium tier — add Perplexity.** Reached only after Small is proven. Requires a shared mailbox all workers reach (Perplexity emails its own account address), which is the point at which the dedicated single-identity account is set up.

**Later: Max tier — add Claude** as the build/assist layer, gated on the Claude-scheduling verification below.

**Single next action:** re-run the scheduled-commit test ("heartbeat test") with the email-confirmation step built in from the start, because the first run did not include it and the body content did not land even though success was reported.

---

## Open Threads (live checklist)

- [ ] **Heartbeat re-test, with email confirmation built in.** Prove a scheduled task commits the *correct content* unattended — not just that a commit fires (that is confirmed), but that the intended content lands. The first run proved the commit and the email both fire on schedule; it did not prove the body content landed.
- [ ] **Cross-model verification loop.** A second task (ChatGPT, or Perplexity later) reads a verification email carrying the *literal original instruction*, reads the *live artifact*, and reports pass/fail. The checker must be a different model than the actor.
- [ ] **Verification keys.** The verifier must match on the email's literal content plus the specific task name plus the day's date/time, and must read the live artifact for truth — never the email summary, since the emailed version and the click-through version can differ.
- [ ] **Cold-start vs running-system behavior.** Define how the first task of the day behaves versus a system already running, and what the confirmation email must say in each case.
- [ ] **Assign Grok slots 2–9** (roles: outward / inward / lens / structural) after first runs show what the chain needs.
- [ ] **Build the ChatGPT task list (C1–C10)** and **Perplexity task list (P1+)** as roles firm up.
- [ ] **Design the live surface** — its structure, the write lanes, the protected set, the layout-restyle behavior, and the self-check state. (This is *what the surface is*.)
- [ ] **Set up the hosting/update task** — deploy the surface on Cloudflare Pages and have a worker update it. (This is *the plumbing* — distinct from designing the surface.)
- [ ] **Build the Continuity State Document** — format, contents, location, who reads and writes it.
- [ ] **Define the hand-off mechanism** — how each task's output becomes the next task's live trigger via the day's subject.
- [ ] **Define the daily restart** — how day N+1 begins fresh.
- [ ] **Confirm the stop/kill condition** — deleting the task is the stop; confirm it is sufficient for the observation-only build.
- [x] **Decide how the repo gets updated** — RESOLVED 2026-05-31: via Grok in a live chat, content pasted as text with exact filenames stated (see Verified Findings). This is the standard method for repo updates going forward.

## Conditions to Verify (test at the source; do not trust self-reports)

- [ ] **Content fidelity on unattended writes.** Scheduled-task-to-connector firing is confirmed (both email and GitHub write fired from a scheduled task). What is unconfirmed is whether the *full, correct content* lands — the title landed, the body did not. This is the heartbeat re-test plus verification.
- [ ] **Grok task ceiling** — observed at 10; whether a higher tier raises it is unconfirmed. Re-test before finalizing the ten-slot layout.
- [ ] **Perplexity scheduled-task limits** — undisclosed (max number, minimum frequency).
- [ ] **Send-to-shared-mailbox behavior.** Task-completion emails go to the worker's own account address; getting Perplexity's output into the mailbox Grok reads is the prerequisite for the Medium tier (see Testing-Phase Bridge below and the dedicated-account step). Note (2026-05-31): Perplexity's GitHub connector is now confirmed human-gated (see Verified Findings), which reinforces that Perplexity's loop contribution is research/produce-and-hand-off, not direct unattended repo-writing.
- [ ] **Scheduled-task media generation.** Whether a scheduled task (not a live chat turn) can generate an image or video and deliver it by email — needed for the production roles and the second value path.
- [ ] **Claude unattended participation (Max only).** Whether Claude can take part in the loop unattended without a computer is open; research suggests a cloud-scheduled path may exist, but this conflicts with the working assumption that Claude is live-turn-only. Resolve before building the Max tier. Does not affect Small or Medium.

## Verified Findings (proven by direct test — treat as settled until a product change overturns them)

- **Grok commits to the repo through its connector in a live chat, unattended-capable.** Confirmed 2026-05-31: Grok created the `ai-workforce` repo and committed all five files in one operation after a single approval. Long files landed whole, not truncated (SPEC.md 25,084 bytes, STATUS.md 11,911 bytes verified intact). The live-chat commit path through Grok is reliable.
- **Perplexity's GitHub connector is human-gated and cannot act unattended.** Confirmed 2026-05-31: to use the connector, Perplexity required pressing an in-app Approve button AND typing a prompt to make the connector available — every time. Its first attempt errored and produced a repo containing only a title. Conclusion: Perplexity cannot write to the repo unattended; its role is research-and-produce (output handed off), not autonomous repo-writing. (This was deleted and redone via Grok.)
- **Reliable way to get file content into Grok: paste as text and state the exact filename.** Confirmed 2026-05-31: raw file attachments were not accepted; content had to be carried through the Notes app and pasted in. Large pastes auto-formatted into files but were named as text rather than `.md`, so the exact filename had to be stated explicitly. Small content (the MIT license) would not convert to a file at all and had to be sent as prompt text. The robust method is therefore: paste content as text, name the file exactly, and tell Grok to save it under that name — not rely on attachment.
- **Adding verification instructions to the prompt works.** Confirmed 2026-05-31: instructing Grok to check filenames, confirm placement, and double-check its work produced a reply with per-file byte counts and a structure confirmation, which is how the whole-file landing was verified.

## To-Do List

- [ ] Run the heartbeat re-test with email confirmation built in.
- [ ] Build the cross-model verification loop (literal-instruction email + live-artifact read).
- [ ] Design the live surface (structure, write lanes, protected set, restyle behavior, self-check state).
- [ ] Set up the Cloudflare Pages hosting/update task.
- [ ] Build the Continuity State Document.
- [ ] Assign Grok slots 2–9; build out the ChatGPT and Perplexity task lists.
- [ ] Define the Archivist function (across-day pattern analysis) and where its output lives.
- [ ] Create the Known Unknowns Registry and the task that investigates it.
- [ ] Identify the authored reference repositories that serve as lens corpora.
- [ ] Define the hand-off mechanism and the daily restart.
- [ ] Build the engagement-to-monetization path, including the human-fed deeper-window analytics.
- [ ] Confirm the spark fallback chain (X tag → repo state → worker memory → appropriate generated media).
- [ ] Define the ledger (see Open Questions) and add the Surprise metric.
- [ ] Set up the dedicated single-identity account and shared mailbox at the Medium stage.

## Open Questions and Likely Additions (kept with full reasoning; tensed as open, not decided)

- **The ledger's unit of measure.** The cost side is effectively fixed: for subscription apps it is the sum of the monthly subscription prices, divided across usage — not the per-call variability of an API. So cost is settled. The open part is the *value* side: how to measure what the system produces in the same units, so it can be told whether each worker earns its keep. (This is one ledger — cost tracking and value tracking are the same accounting, recorded once.)
- **The Surprise / Discovery metric.** A daily reading (0 = fully predictable, 10 = discovered something unexpected) that indicates whether the system is genuinely learning or merely repeating patterns. Part of measuring the value side of the ledger.
- **Worker production roles.** The workers are meant to produce, not only verify (SPEC.md §6). Open possibilities, to confirm against capability: ChatGPT generating images; Perplexity producing and publishing video; either drafting post copy or producing the daily surface restyle as a creative output. Confirm scheduled-task media generation before committing any of these.
- **Second value path — produced media to publish.** Where a worker can generate and publish media (e.g., video to YouTube), a lower-manual revenue path opens. The choice of where the human trigger lives (at tagging/setup vs at each publish) is the operator's, made deliberately. Verify the unattended publishing behavior first.
- **Spark fallback chain.** When no fresh tagged post exists: repository state, each worker's own memory/context, and appropriate system-generated media, in order. Keeps the loop from starving on a quiet day.
- **Idempotency and deduplication.** The system must avoid re-processing the same item across cycles, or it will echo.
- **Observational connector candidates** (none committed). Candidates that increase observation: GitHub (repo/commit monitoring, issues, docs), Drive/Dropbox (corpus and archive storage), Cloudflare (deployment visibility), Figma (system maps), Gmail + Calendar (maintenance and subscription reminders). Held pending evaluation. (Note on the broader connector landscape: many long-tail connectors route through third-party proxies such as Pipedream or Composio, which hold the auth token and proxy the call — worth evaluating before connecting anything sensitive.)
- **Capability discovery across models.** A low-stakes early task to establish what each available app can actually do today, since the apps change and self-reports are unreliable.

## Testing-Phase Bridge (temporary — not part of the running system)

During proving runs, before the shared mailbox exists, Perplexity's task output can be moved to the mailbox Grok reads by hand: the operator brings Perplexity's emailed result into a chat with Claude, and Claude drafts it into the shared Gmail (faithful relay — passed through as produced, so the test shows whether Grok can handle Perplexity's real output). This is a capability test, not a workflow: it confirms Perplexity slots into the loop correctly without requiring the dedicated accounts yet. It is replaced entirely by the shared mailbox the moment the Medium tier goes real, when Grok is set to look for Perplexity's task by name, date, and time. (Expected to be resolved shortly; recorded so the method is not lost, and explicitly excluded from the permanent architecture.)

## Decisions Locked

- Host: **Cloudflare Pages** (the repository is the workers' shared workspace; the host is the public window that mirrors it).
- The **collector / daily close stays** — without it the system is disconnected tasks and emails; the close makes each day a complete, reviewable unit and keeps the operator never operating blind.
- Build sequence: **Small (Grok + ChatGPT) first on existing accounts → shared mailbox + Perplexity (Medium) as the graduation step → Claude (Max) later.**
- Cost is stated transparently: standard paid subscriptions, not free/mini tiers; no API, server, or code; ongoing goal to lower cost while holding intelligence, memory, and safety, at a moderate speed.

## Change Log

- **2026-05-31 (repo live)** — The `ai-workforce` repository was created and populated successfully via Grok (live chat, content pasted as text). All five files present and verified on `main`: README.md, SPEC.md (25,084 bytes), STATUS.md (11,911 bytes), LICENSE (exact MIT), .gitignore (exact content) — no templates, no modifications, long files whole. Added a Verified Findings section recording what the build proved: Grok commits unattended-capably through its connector; Perplexity's GitHub connector is human-gated (in-app approve + prompt each time) and cannot write to the repo unattended, so its role is research/produce-and-hand-off; the reliable way to load content into Grok is paste-as-text with the exact filename stated, not file attachment; and adding verification instructions to the prompt yields byte-count confirmation. Resolved the "how the repo gets updated" thread accordingly. Next action: heartbeat re-test with email confirmation built in.
- **2026-05-31** — Repo files rebuilt from the reconciled master after a full operator review. Applied: removed the "no money movement" framing; corrected the cost claim to "standard paid subscriptions, not free" (transparency); downgraded the scheduled-task-connector item from "riskiest assumption" to "content fidelity to verify" (scheduled firing is confirmed); recorded that Perplexity's own-address email makes a shared mailbox a Medium-tier prerequisite, with verification keyed to literal content + task name + date/time and truth read from the live artifact; widened worker roles to produce-and-verify with the image→video pipeline noted; re-expanded the outcome into near-term + long-term direction (re-tensed, not a roadmap); separated "design the surface" from "host the surface"; confirmed Cloudflare and framed the repo as the shared workspace; answered the spark fallback chain; merged cost/usage into one ledger with a fixed cost side and open value side; re-tensed self-modification and the deferred items so none reads as a present constraint; removed the prescriptive payment/treasury guard framing; locked the collector ending and the Small-first build sequence; added the temporary testing-phase email bridge as explicitly non-permanent. Next action: heartbeat re-test with email confirmation built in.