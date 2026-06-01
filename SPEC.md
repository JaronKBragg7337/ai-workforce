# SPEC — AI Workforce: Hiring Compatible AIs to Work for You

The full specification for a self-documenting, recurring-task system that hires compatible consumer AI apps to work as co-workers. This document is the stable architecture reference. Current build state and open work live in STATUS.md.

-----

## 1. Overview

The system runs scheduled tasks inside consumer AI apps, with each app acting as a co-worker on a shared job. In the current proving build, two workers run — Grok (xAI) and ChatGPT (OpenAI). The next stage adds Perplexity (Perplexity Pro); a later stage adds Claude (Anthropic) as a build/assist layer. (See Section 9, Tiers.)

Grok carries the action chain: it performs small units of work on a timed cycle and hands each result forward through the shared working framework (Google Drive), where the next worker reads it. The other workers act alongside it, each contributing what it is best at — production, research, and verification. All workers publish to a live website, one at a time in a fixed order, which the system reads back, the operator watches, and anyone may view. Whichever worker's turn it is also restyles the surface that day, so the environment visibly changes day to day.

The system is operated entirely from consumer app interfaces on standard paid subscriptions. It uses no API, no server, and no code. This is a deliberate constraint: it keeps the system runnable by a person without paid infrastructure or technical setup. Any capability that would require an API key or an always-on computer is out of this system's scope and belongs to a different project.

## 2. Goal

Operate a continuous system that, on a recurring schedule, generates and advances useful work without a manual instruction at each step, while keeping a complete and honest record of everything it produces and consumes. The system reads its own record, detects flaws in itself, and either corrects them or surfaces them for the operator.

The work the system does is not fixed in code. Each cycle is triggered by a live condition the system checks, so the system can be ended, redirected, or evolved without rewriting its core.

## 3. Intention

The record exists first so the system and its operator can see what the system is producing. The documentation is the system's own mirror: a surface it writes to, reads back, references to check whether it is working, and corrects itself against. The operator's ability to see everything the system does — and never operate blind — is the primary purpose of the open record.

The record is open because open is honest. That the open record also lets others watch, and reassures people wary of autonomous systems, is a welcome consequence but not the reason.

The system is built on multiple paid vendors on purpose. Each worker is paid for and load-bearing — a deliberate stance against single-tool dependence. A system that requires more than one vendor to run makes multi-tool use structural rather than merely stated, and each worker must earn its keep against the subscription that pays for it (see STATUS.md, ledger).

The system also has an intended entertainment dimension, treated as a real design goal. A surface that changes appearance each day, styled in turn by workers with different instincts, is engaging to watch and satisfying to operate. That engagement is functional, not decorative: a transparency surface no one looks at produces no transparency.

## 4. Outcome — Near-Term and Long-Term

The outcome is described in two layers, kept distinct on purpose so the near-term build is not mistaken for the ceiling and the long-term vision is not mistaken for a present commitment.

**Near-term outcome (what success looks like at launch).** A running system that, each day, fires a sequence of timed tasks; turns a live signal into work; advances it down a chain; checks itself across different models and fixes or reports errors; produces a daily output written and restyled in turn on an open surface; and ends the day by gathering everything into one honest record. The next day restarts on something new and looks different. The first value path is content creation and engagement-based monetization (Section 8 and STATUS.md).

**Long-term vision (direction, not committed roadmap).** The same engine is intended to be able to improve a person's circumstances in more than one direction as capability and trust grow. Directions under consideration include: content creation and monetization; connection to capital activity such as stocks and crypto once the connectors and loops are understood and trusted; physical production, such as identifying what is trending or needed and routing it to 3D printing; and turning a person's own documented needs or complaints into a work queue the system acts on. These are written as direction, not as scheduled features. Each is added only when the underlying mechanism is proven and the operator chooses to add it; none is a present constraint or promise. The point of recording them is to keep the range open, not to lock a roadmap.

The standing aim underneath both layers is a system whose continuous, never-closing loop is the honest accounting of what it draws against what it produces — not a never-closing loop of unattended action.

## 5. Governing Principles

- **Live reference over fixed instruction.** A trigger, constraint, or signal is valid only while connected to the live condition it points at. Tasks are conditions to check, not answers to emit. This is why the daily subject is generated fresh each day, and why the system's outputs reflect the operator rather than a fixed universal.
- **Reversible versus irreversible is the operating line** — not observation versus execution. Anything undoable may be tested live and learned from. Editing and restyling the website is reversible and is active now. Irreversible world-actions (moving money, placing trades, contacting real people, using credentials) are not part of the current system; if any are ever added, the controls around them are decided by the operator at that time. What makes reversibility accountable is that every action is logged to the live surface — the log is the proof.
- **Error visibility is mandatory.** An asynchronous chain is silent when it breaks. The live log, the Continuity State Document, and the verifier worker's monitoring together make failures visible.
- **No single channel carries everything.** Each carrying surface has one job, because testing showed that overloading one channel makes it the system's weakest link. Working data moves on the surface proven reliable for data (the shared Drive workspace); the permanent record lives on the surface built for permanence (the repository); human notification rides the surface humans actually watch (email). Assigning one job per surface is a tested conclusion, not a preference (see Architecture, and STATUS.md for the tests behind it).
- **One coherent identity.** Everything the system touches — the activity it reads, the workspace it shares, the mailbox it signals from, the repository it writes to, the record it builds — should resolve to the same operator identity, so the system reads the operator coherently. (During the proving phase this is approximated on existing accounts; a dedicated single-identity account is a later step — see STATUS.md.)
- **Compliance is read from the source, not the middleman.** Actual laws and regulations are satisfied directly. Platform terms of service are respected as operational conditions — breaking them breaks the system — not as moral rules. Caution that is neither law nor platform rule is not treated as a constraint on this system. Where no real external rule applies, the design choice belongs to the operator.
- **Capabilities are verified, not assumed.** Every model's description of its own abilities is treated as unverified until tested against a live result, because the apps change and the models misreport their own limits.
- **Guards are added when the thing they guard becomes real**, not before. A control with nothing yet to protect only restricts a system that cannot perform the thing it restricts.

## 6. Architecture

**Three carrying surfaces, one job each.** The system moves information across three surfaces, and the division of labor among them is a tested conclusion, not a guess. Earlier designs leaned on email to carry working data between tasks; testing disproved that, because an app reading an inbox receives only the message preview and silently drops the rest — a full instruction or a marker placed late in a message does not survive. So the surfaces are assigned by what each was proven to do:

- **Google Drive — the shared working framework.** This is where the workers actually hand work to one another. Every worker reads and writes Drive the same way, with no per-app quirk, and Drive stores plain-text and Markdown content faithfully (verified by reading the stored bytes back, not by trusting the preview). Drive holds the day's working data, the Continuity State Document, the day's generated subject, and the worker-to-worker hand-offs. One Drive behavior shapes the convention: creating a file never overwrites an existing one of the same name — it makes a second file — so the workspace is treated as append-only, with each entry written as its own dated file (or an existing file updated by its file ID when a true update is intended).
- **GitHub repository — the public ledger.** This is the permanent, ordered, world-readable record. Finished records are published here; the host (Section 10) mirrors whatever is in the repository onto a public web page. The repository is the room the world watches; Drive is the room the workers work in. A scheduled worker can read a repository and write to it unattended, provided it is given the full owner/repository path — a worker that guesses the owner from an email prefix will fail to find the repository (a tested failure mode; see STATUS.md).
- **Email — the human signal only.** Email is demoted to a once-a-day notification and a short routing line at the top of a message: enough to tell the operator the loop ran and where to look, never a carrier of full data. Sending email from a scheduled task works; reading full data back out of email does not, which is why no working data depends on it.

A fourth layer sits outside the three surfaces: **the task definitions themselves are the private control and intent layer.** They are where the operator's intent is set and changed; they are not a public surface and are not where data is exchanged between workers.

**The shared-workspace entry convention.** Because the workspace is append-only and several workers write to it, every entry carries three things so the record stays legible and attributable: (1) the content or marker itself; (2) an author signature naming which worker wrote it (for example, "— Logged by: Grok"); and (3) an ISO-8601 timestamp with timezone (for example, 2026-06-01T00:37:30-0400). ISO-8601 is chosen because it is unambiguous across timezones and sorts chronologically as plain text. New entries are appended as new dated files rather than overwriting, so the history is never destroyed by a later write.

**Workers — production and verification, divided by strength.** Each worker both produces work and checks work; the workers are not limited to checking. Grok carries the action chain (advance the loop hop to hop through the shared workspace, run the repetitive structural tasks, produce the day's work output, write to the repository, and send the day's human signal). ChatGPT contributes production (for example, generating images or written pieces where it is capable) plus verification, second-reads, and surface responses; its GitHub access is read-only, which suits the verifier role. Perplexity contributes live research, production such as video, and verification. The division is by strength, not hierarchy. Some assignments also reflect current capability limits (for example, ChatGPT cannot autonomously send arbitrary email from a standard task today, and Perplexity's GitHub write is human-gated rather than unattended — a tested limit; see STATUS.md) — but the roles are the design decision, and the capabilities are live conditions that would change the assignment if they change.

Note: whether a *scheduled task* (as opposed to a live chat turn) can generate an image or video and deliver it through the shared workspace is a capability to verify before relying on it (STATUS.md). A concrete production pipeline under consideration: one worker generates an image, another publishes video to a platform such as YouTube, feeding the second revenue surface in Section 8.

**Sequential writing to the surface.** Workers publish to the live website one at a time, in a fixed order, so none overwrites another and each adds its own attributed contribution. The order is functional: a later worker sees the earlier worker's output and can offer the alternative (for example, a more professional version of a blunter draft). The operator chooses which version to use.

**The layout layer (daily restyle).** On its turn, a worker may freely restyle the surface's layout and appearance. This serves three purposes at once: it is a visible liveness signal (a surface that looks different today proves the system ran today before a viewer reads a word); it makes the multi-model output engaging to watch; and it is a genuine source of operator satisfaction. The one protection rule: no worker may delete, alter, or obscure the protected set — the record, the memory, the history, and the other workers' output — all of which must stay legibly visible regardless of styling. Styling is free; burying is not.

**Continuity State Document.** Kept in the shared Drive workspace and separate from the website's published history, the system keeps a small machine-readable "current state" object answering, at any moment: what day it is on, today's generated subject, the current spark, the current focus, the last successful hop, the last failed hop, the unresolved items, and what is under investigation. This gives the system a shared present tense. It is what finally lets a single channel stop having to serve as memory, state, transport, and trigger all at once — the reliable shared workspace provides that present tense instead.

**The shared workspace is Google Drive; the public ledger is the repository.** The workers do their working hand-offs by reading and writing files in the shared Drive workspace, and publish finished records into the GitHub repository. The host (Section 10) does not contain a separate workspace the workers log into; it is a public window that mirrors whatever is in the repository onto a web page. Drive is the room the workers work in; the repository is what they post on the wall for the record; the host is the window the world watches through.

**The Archivist (across-day function).** Where the daily collector asks "what happened today?", the Archivist asks "what repeats across days?" — which sparks recur, which chains fail most often, which references reliably produce useful output, which tasks produce no value, which fixes keep reappearing. The Archivist learns across days rather than within one.

**The Known Unknowns Registry.** The system keeps a tracked list of its own open questions (for example: does novelty decay after N days, does a given slot add value, what causes the most frequent chain failures). This makes the "knowledge gap as the spark" operational — the system can investigate its own unknowns rather than only waiting for new external work.

**Schedule and maintenance window.** Grok permits up to ten scheduled tasks (a confirmed account ceiling; whether a higher tier raises it is a live condition — STATUS.md). Ten tasks cannot cover a full day, so the schedule is ten chosen high-value hours clustered around where the day's real work happens — the spark, the chain that develops it, the collector at the end — rather than a task every hour. The uncovered block is the operator's daily maintenance window: the time the operator is reliably awake to change a task, fix what a run revealed, or adjust structure. If no maintenance is needed, the system proceeds.

**Task roles.** Tasks fall into kinds. *Outward* tasks check live signals (such as the operator's public activity) and produce work. *Inward / self-check* tasks read the system's own state and correct or flag a flaw. *Lens* tasks pull an authored reference corpus and apply it as an interpretive frame, tagged as interpretation rather than fact. *Structural* tasks are the low-stakes repetitive scaffolding (workspace checks, re-establishing structure before hunting the spark), tagged structural so routine plumbing is distinguished from the day's real signal. The *collector* gathers the day, publishes the record, and reports where the chain broke. The collector is essential, not optional: without the daily close, the system is a pile of disconnected tasks; the close is what makes each day a complete, reviewable unit that the next day can differ from and build on.

**The daily-subject generator (first task of the day).** The first task generates a unique subject — date, time, place of origin, and a random token — and writes it into the day's subject file in the shared Drive workspace so every downstream task reads it from there. Every task keys off that day's subject, never a hardcoded one. This resolves single-account ambiguity: today's work does not collide with yesterday's, and the system's own prior-day output does not match today's filter. Consequence: no task prompt may hardcode a subject name.

**The spark (and the tag).** The condition that starts each cycle's real work is not hardcoded; it is a live check against the operator's own public activity (primarily the operator's public X account). A post counts as a spark only if it contains both parts of a two-part trigger: the operator's actual intent or idea (the content) and a fixed trigger code that marks the post as a deliberate signal rather than ordinary posting. The task acts only when both are present, and only on the most recent qualifying post it has not already acted on, so a spark is never re-fired. This is the tag mechanism: the operator's intent is encoded as a live signal in the act of posting, rather than a manual instruction handed over each cycle — which lets the system run on its own while the operator remains the source of intent. Security rests on control of the channel, not secrecy of the code: only the operator can post from the operator's account, so the code may be public without weakening the gate. Consequence: the account's own protection (a strong unique password and two-factor authentication) is load-bearing for the system.

**Spark fallback chain.** If no fresh tagged post exists, the spark falls back in order to: current repository and workspace state; each worker's own memory or context; and, where a worker is capable, an appropriate system-generated image or video. The fallback chain keeps the loop from starving on a quiet day.

**Hand-off and gating.** Each task's output must regenerate the next trigger, or the chain stalls into an echo. The unique daily subject, plus a content gate (act only on a real request) and a source gate (never treat the system's own prior output as instruction), keep the chain honest. The next day restarts fresh — new subject, new spark, new appearance.

## 7. Worker Task Tables

### 7.1 Grok (xAI) — slots 1–10

Slots 1 and 10 are defined. Slots 2–9 are intentionally open and will be assigned a role (outward, inward, lens, or structural) and a definition after the first runs show what the chain needs. Hours are a starting layout to be tuned once the middle roles are set.

|Slot|Hour (starting)      |Role     |Definition                                                                                                                                   |
|----|---------------------|---------|---------------------------------------------------------------------------------------------------------------------------------------------|
|1   |4 PM                 |generator|Generate the unique daily subject and write it into the day's subject file in the shared Drive workspace so all downstream tasks read it. Starts the cycle.|
|2–9 |clustered / overnight|open     |To be assigned (outward / inward / lens / structural) after first runs.                                                                       |
|10  |11 AM                |collector|Gather every prior slot's output into one record; publish it to the live surface; report where the chain succeeded and where it broke.        |

### 7.2 ChatGPT (OpenAI)

Ceiling: 10 active tasks (confirmed). Acts after the worker it checks, on the shared surface. Reads live artifacts to verify; produces where capable. Cannot autonomously send arbitrary email from a standard task (notification-only) and cannot write to GitHub (read-only) — appropriate for verification and production-that-doesn't-require-repo-write.

|Task  |Role                    |Definition                                                                                                                           |
|------|------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
|C1    |verifier                |Read the verification entry in the shared workspace carrying the literal original instruction; read the live artifact; report pass/fail with what differed.|
|C2    |second-read / production|Offer the alternative version of the day's output, or produce an asset (e.g., an image) where capable, as an attributed contribution.|
|C3    |monitoring              |Watch for silent chain breaks; surface failures the asynchronous chain would otherwise hide.                                         |
|C4–C10|open                    |Reserved; assign as runs show what is needed.                                                                                        |

### 7.3 Perplexity (Perplexity Pro)

Ceiling: undisclosed (live condition — verify). Live-research worker; can produce (e.g., video) and verify. Reads and writes the shared Drive workspace like the other workers; its GitHub write is human-gated rather than unattended (a tested limit — STATUS.md), so its repository contributions are produce-and-hand-off, with a repo-write-capable worker or a human completing the publish. Whether Perplexity's Drive read/write runs unattended from a scheduled task is a live condition to verify before the loop relies on it.

|Task|Role                |Definition                                                                                                                             |
|----|--------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|P1  |research            |Deep live research on the day's topic; deliver actual content into the shared workspace keyed to the day's subject.                    |
|P2  |verifier (alt)      |Independent cross-model check when Perplexity is the checker for a hop: read literal instruction, read live artifact, report pass/fail.|
|P3  |production / publish|Produce or publish where a human-in-the-loop step is satisfied (e.g., video to a platform), since unattended repo write is not available to it.|
|P4+ |open                |Reserved; confirm ceiling first.                                                                                                       |

## 8. Value Paths

**First path — engagement to monetization.** The apps do not move capital, but they can read the operator's public activity (posts, following, engagement, impressions) and surface copy-paste-ready post structures modeled on what is driving engagement. The operator posts by hand, remaining the one who acts. As engagement crosses the platform's monetization threshold, the platform pays out. Deeper time-windowed analytics (7-day, monthly, and longer) can only be read by the operator for their own account and are fed in as a deliberate human-in-the-loop step, letting the system reason about what compounds over time rather than what merely spiked today. This is the path that produces a first dollar before any capital exists.

**Second path — produced media to publish.** Where a worker can generate media (for example, video) and publish it to a platform such as YouTube, a second revenue path opens that requires less manual posting. Publishing without a per-item human push moves the trigger to the loop level rather than the post level; that is a choice the operator makes deliberately, not a default. The exact unattended publishing behavior is a capability to verify before relying on it.

Further value paths beyond these are described as long-term direction in Section 4 and are not part of the current system.

## 9. Tiers (the build ladder and the degradation ladder)

The tiers serve two purposes: they stage this build, and they let others run a smaller, reliable version.

- **Small — Grok + ChatGPT. Current proving ground.** This pair tests everything genuinely hard about the architecture: a scheduled task writing correct content unattended, a different model verifying it against the live artifact, the collector closing the day, the surface mirroring it, and the next day restarting fresh. The two workers already share the Drive workspace, so their loop hands off reliably from the start. The system is proven here before anything is added.
- **Medium — add Perplexity. Next stage.** Adds a third worker plus live research and a publish capability. The shared Drive workspace already gives the third worker a place to read and write; what this stage adds on the signal side is a shared mailbox all workers can reach for the once-a-day human notification, and it is the point at which the dedicated single-identity account is set up. It is reached after Small is proven, so the account work is done only once the system has earned it.
- **Max — add Claude as the build/assist layer. Later.** Claude is the heaviest builder and can read and write the shared Drive workspace and read the repository directly; its email is draft-only, so its loop contribution is to build in the workspace and repository and leave a draft that a send-capable worker or a human dispatches. Gated on resolving how Claude participates on a schedule unattended without a computer (a live condition — STATUS.md).

## 10. Hosting / Live Surface — Cloudflare Pages

Host: Cloudflare Pages. It takes the files in the repository and displays them at a public web address, rebuilding on each commit. It is chosen because it permits commercial use, serves static content without a practical cap, and stays up rather than going offline when a usage cap is hit. (Vercel's free tier was ruled out because it pauses the site offline at a cap, the worst behavior for a system meant to run unattended. GitHub Pages remains a simple non-commercial fallback.)

The host is only the public window. The workers do not act inside the host; they act inside the shared workspace and publish to the repository, and the host mirrors the repository. Because of that, the host choice is low-stakes and the loop can be proven before the window is finalized.

## 11. Versioning and Status

This specification is a living document. A component is "built" only when actually running, and a "result" only when actually confirmed; anything untested is marked as such in STATUS.md. Writing a guess ahead of a test result is the kind of frozen reference this system avoids — confirmed changes are held until the test confirms them. Detailed version history is tracked through this repository's commit log; current state and open work are tracked in STATUS.md.

## 12. Items Held for Later (not constraints on the current system)

These are recorded so the context is not lost, and tensed as future possibilities, not present rules. None is active, because the current system cannot perform the actions involved.

- **Dedicated single-identity accounts and shared mailbox.** Set up at the Medium stage. The shared Drive workspace already serves as the workers' shared room; the shared mailbox added here is for the email signal layer (the once-a-day notification), and the associated account verification establishes the single operator identity. No account used during proving is permanent.
- **Self-modification.** The current system cannot change its own task list, because there is no known method to set that up — not because it is forbidden. If it becomes possible, the intended approach is to enable it in the reversible class, observe how it behaves, and add restraints only if observation shows they are warranted.
- **Irreversible world-actions** (payments, trades, contacting real people, credentials) and the connectors that would enable them are not part of the current system. If any are ever added, the operator decides the controls at that time. No controls are pre-written here, because there is nothing yet for them to govern.
- **Operator-presence anchoring for irreversible action.** Relevant only if irreversible action is ever added: continued irreversible operation would require a positive, recent sign of the operator's presence. Not applicable to the current observation-and-documentation system, where deleting a task is a sufficient stop.

-----

— Structure and wording assisted by Claude (Anthropic).
