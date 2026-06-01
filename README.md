# AI Workforce — Hiring Compatible AIs to Work for You

**Currently hireable:** Grok · ChatGPT · Perplexity · Claude — *plus pending future AIs as they prove compatible.*

A self-documenting recurring-task system that hires compatible consumer AI apps to work as co-workers. It runs entirely from the apps' phone and web interfaces on standard consumer subscriptions — no API integration, no server, no always-on computer, and no coding required. On a recurring schedule it reads live signals (the operator's own public activity), produces and advances work, surfaces ways it could generate value, checks its own output across different models, restyles its own public surface daily, and writes everything to an open record on a live website.

This README is the front door. The full specification is in **SPEC.md**. The current build state, open work, and verification checklist are in **STATUS.md**.

**Creator / origin:** Jaron Kyler Bragg — Fort Wayne, IN (place of origin; not an exact address). The legal name and place of origin are carried deliberately, for accountability and transparency.

-----

## What these outputs are, and are not

The outputs of this system are a mirror of what these specific AI models read the operator to intend. Working inside the operator's own accounts and reading the operator's own public signals, the workers produce suggestions, posts, sparks, and styling that reflect their reading of this particular person. They are not a universal output that anyone would reproduce.

If you build your own version, expect different results. A different person, with different public signals, read by a different AI — or even the same AI reading a different person — will produce different sparks, suggestions, tone, and choices. That is the nature of the system, not a flaw: it is live-referenced to its operator, which is exactly why its specific behavior cannot be copied, only its structure.

**What transfers is the architecture, not the behavior.** The principles, the layers, the protected set, and the staging are reusable. No specific output, suggestion, or post from this system should be taken as advice, instruction, or a model of what another person's AI should say.

## What it costs to run (stated plainly)

This system runs on **paid consumer subscriptions** to each AI app, at roughly each app's **standard tier** — not the free tiers or the reduced "mini" tiers, which reach usage limits too quickly to sustain a continuous loop. It requires no API budget, no server, and no developer tools. Each hired AI runs on its own subscription: Grok (SuperGrok), ChatGPT (standard paid tier), Perplexity (Pro), and — at the later Max tier — Claude (paid tier). A given operator only pays for the AIs they choose to hire at their chosen tier, so the cost scales with how many workers are on staff (see SPEC.md, Tiers).

A continuing design goal is to reduce that subscription cost over time while preserving intelligence, memory, and safety, accepting a moderate operating speed as the tradeoff. The system is built for people who do not yet have a business of their own, so it is designed around affordable, no-code consumer tools rather than paid infrastructure — but it is transparent that those tools still cost money. There is a real, stated cost floor; the work over time is to lower it without lowering capability.

## Where the system keeps its information

The system writes to three separate surfaces, and each one has a single job, because testing showed that asking one channel to carry everything turns that channel into the weakest link. **GitHub is the public ledger** — the permanent, ordered, world-readable record that feeds the live website and that anyone can audit. **Google Drive is the shared working framework** — the reliable workspace every hired AI reads and writes the same way, where the real working data and the hand-offs between workers live. **Email is a human signal only** — a once-a-day notification and a short routing line, never a carrier of full data, because testing proved that an app reading an inbox sees only the message preview and silently loses the rest. Keeping these three jobs on three surfaces is a direct result of what the system was tested to do, not a preference. The full detail is in SPEC.md.

## How this was built

This system was designed primarily in conversation with Claude (Anthropic), which served as the reasoning and architecture partner across its development, with smaller contributions from the in-system models. The distinction matters for anyone building their own version: the models that run inside the system are not the ones that designed it. Designing a system like this benefits from a tool that can hold the full, evolving context and reason across it; running it benefits from tools that can schedule tasks and send email on their own. Those turned out to be different strengths in different tools. If you assume the in-system models also did the design work, you may find they lack the context depth to carry it — which is itself useful to know before you start.

## Design basis

The system is built on one published design rule, the **Live-Reference Principle**: a trigger, constraint, or signal is valid only while it stays connected to the live condition it points at, and a rule frozen into a stored answer becomes a gesture rather than a working function. In practice this means tasks are defined as conditions to check, not fixed answers to emit, and every model's self-reported capability is treated as unverified until tested against a live result. (See the principle's own repository for the full argument; this project applies it, it does not restate it.)

## Files in this repository

- **README.md** — this front door.
- **SPEC.md** — the full system specification (architecture, principles, worker roles and task tables, constraints, tiers, hosting).
- **STATUS.md** — the living build layer: current target, open threads, conditions to verify, to-do list, and open questions.
- **LICENSE** — MIT.
- **.gitignore** — standard ignore list (plumbing).

-----

— Structure and wording assisted by Claude (Anthropic).
