# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Communication

- Interact with the user in Japanese. Repository content (`README-AWS.md`, `CONTRIBUTING.md`) stays in American English; `STUDY-PLAN.md` is in Japanese.
- Be critical, not reassuring. Point out weaknesses, risks, and gaps first; don't soften them to be encouraging. Why: the user is deepening their own philosophy of FDE work, and an idea that has only been praised hasn't been tested. Flattery makes the thinking weaker, not safer.
- Attach a Why (background and reasoning) to every opinion. Prefer depth over brevity. Why: a conclusion without its reasoning can't be examined, challenged, or reused in the user's own principles.
- Separate facts from guesses. For AWS service names, features, and availability, verify with WebSearch instead of relying on memory (they change often).

## What this repo is

A personal fork of [pierpaolo28/Awesome-FDE-Roadmap](https://github.com/pierpaolo28/Awesome-FDE-Roadmap), a learning roadmap for Forward Deployed Engineering (FDE), plus an AWS edition and a personal study plan for deepening the user's own philosophy of FDE work (working style, strategy, career). There is no code, build, lint, or test tooling; everything is Markdown.

| File | Role | Editable? |
| :--- | :--- | :--- |
| `README.md` | Upstream roadmap (GCP-focused) | **Never edit.** Keep it identical to upstream so merges stay conflict-free |
| `README-AWS.md` | AWS-focused adaptation, section-for-section mirror of `README.md` | Yes. All roadmap changes go here |
| `STUDY-PLAN.md` | Thinking program: seven questions answered in writing and reviewed critically | Yes. Contains questions and process only, never the user's answers |
| `CONTRIBUTING.md`, `LICENSE` | Upstream editorial rules and MIT license | No (upstream files) |
| `.claude/skills/sync-upstream/` | Procedure for merging upstream and porting changes to `README-AWS.md` | Yes |

To pull in upstream changes, use the `sync-upstream` skill. It tracks the last synced upstream commit in the `Synced with upstream:` line at the top of `README-AWS.md`; update it whenever you sync.

## Structure of README-AWS.md

The document is organized as numbered top-level `##` sections mirrored by the Table of Contents at the top. The section order must stay parallel to `README.md`, with the AWS-only additions (the GCP → AWS Translation Table and Keeping This File in Sync) at the end. When adding, renaming, or reordering a section:

- Update the Table of Contents to match.
- TOC anchors are GitHub-generated slugs that include encoded emoji/variation selectors (e.g. `#%EF%B8%8F-llm-systems-evaluation-the-success-key`). Headings whose emoji drops out leave a leading `-` in the slug (e.g. `#-the-fde-persona--mission`). Derive new anchors from the exact heading text rather than guessing.
- Section-to-PR-category mapping used in CONTRIBUTING: Data Engineering / Cloud / Consulting / Applied AI / Air-Gap / Interview / Reading List / Glossary.
- The "Artifact Templates" section contains copy-paste Markdown templates inside fenced code blocks; headings inside those blocks are template content, not document structure.
- Cloud-agnostic content (consulting, compliance, reading list) is kept identical to upstream. GCP-specific content is replaced using the Translation Table; if AWS has no single equivalent, say so rather than inventing one.

## Editorial rules (from CONTRIBUTING.md, applied to README-AWS.md)

- Link entry format: `*   **[Name](url):** One-sentence description of why an FDE should read/watch/use it.` (note the `*` + three spaces). Every entry must explain *why it matters for FDEs*, not just what it is.
- Headings follow the emoji-then-title pattern: `### 🚀 Section Title`. No emoji in code blocks.
- Product naming: use the current name with the old name in parentheses on first mention, e.g. `Amazon SageMaker AI (formerly Amazon SageMaker)`. For upstream's Google Cloud naming, see the Gemini Enterprise Agent Platform content in `README.md`.
- American English spelling.
- Paid resources must be noted explicitly; links must be live (HTTP 200) and currently maintained.
- The "Canon" reading list is deliberately short; don't expand it casually. Check for duplicates before adding any entry.
- Excluded content: vendor pitches/self-promotion, blockchain/Web3/crypto, unreviewed LLM-generated content (anything you draft must be reviewed by the user before it is merged).
- Keep changes focused on one theme. Branch naming for upstream-facing PRs is `add-<short-name>`; in this fork, work on the branch the session assigns.

## Study plan rules (`STUDY-PLAN.md`)

The plan is a thinking program, not a skills checklist or interview preparation. The goal is a one-page "principles of my FDE work" document, reached by answering seven questions in writing.

- Pace: about 5-7 hours per week, one question at a time. Reading happens during work gaps, writing in the evening or on weekends. Moving to the next question before the current answer survives review is worse than being slow.
- When the user pastes an answer, review it critically in this order: (1) claim and reason are separated, (2) experience is separated from aspiration or guesswork, (3) the strongest counterargument is raised and tested, (4) the answer says what would change the user's mind, (5) abstract words are replaced by concrete examples. Why: these are the failure modes that make a philosophy sound good but fail under pressure.
- Raise the strongest objection yourself instead of waiting for the user to ask. Don't resolve a question for the user; point out what is still undecided.
- Keep the user's own wording. Don't rewrite their principles into polished generic advice; mark where the sentence is vague and ask.
- Technical hands-on work is optional and only serves to test an answer. Use a personal AWS account (set a budget alert first). Never bring an employer's environment, data, or code into study material.
- Don't pad the plan: if an item doesn't deepen one of the seven questions, drop it.
- The relationship to a job change is not fixed. Don't assume interview preparation unless the user says so.

## Privacy

- The user's written answers to the study questions contain career details and stay outside this repository (their own notes). Review them in chat only; don't save them to a tracked file unless the user asks and the content is anonymized.
- Personal documents (resume, career history, etc.) are not stored in this repository. If the user shares one in a session, use it for the conversation only; don't copy its content into any tracked file (no name, employer, or contact details). Describe skills and experience generically.
- `.gitignore` keeps `.info/` listed as a safety net in case a personal document is placed there again. Never remove that entry, and never commit anything under it.

## Git

- Develop on the branch the session assigns and push there. Don't create a pull request or merge into `main` unless the user asks.
- `main` mirrors upstream plus the fork's own files; ask before changing it.
