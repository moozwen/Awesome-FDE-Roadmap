# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Interact with user in Japanese.

楽観的で優しい回答よりも 批判的な回答をすること。
意見には必ず Why（背景や理由）を付け加え、簡潔さよりも詳細な深堀を優先すること。

## What this repo is

A curated "awesome list" / learning roadmap for Forward Deployed Engineering (FDE). There is no code, build, lint, or test tooling — the entire product is `README.md` (single long Markdown document), with `CONTRIBUTING.md` defining the editorial rules and `LICENSE` (MIT).

## Structure of README.md

The README is organized as numbered top-level `##` sections mirrored by the Table of Contents at the top. When adding, renaming, or reordering a section:

- Update the Table of Contents to match.
- TOC anchors are GitHub-generated slugs that include encoded emoji/variation selectors (e.g. `#%EF%B8%8F-llm-systems-evaluation-the-success-key`, `#%E2%80%8D-creators`). Headings whose emoji drops out leave a leading `-` in the slug (e.g. `#-the-fde-persona--mission`). Derive new anchors from the exact heading text rather than guessing.
- Section-to-PR-category mapping used in CONTRIBUTING: Data Engineering / Cloud / Consulting / Applied AI / Air-Gap / Interview / Reading List / Glossary.
- The "Artifact Templates" section contains copy-paste Markdown templates inside fenced code blocks; headings inside those blocks are template content, not README structure.

## Editorial rules (from CONTRIBUTING.md)

- Link entry format: `*   **[Name](url):** One-sentence description of why an FDE should read/watch/use it.` (note the `*` + three spaces). Every entry must explain *why it matters for FDEs*, not just what it is.
- Headings follow the emoji-then-title pattern: `### 🚀 Section Title`. No emoji in code blocks.
- Google Cloud product naming: use the current name with the old name in parentheses on first mention, e.g. `Agent Runtime (formerly Vertex AI Agent Engine)`. The "Gemini Enterprise Agent Platform" content (Applied AI section) is the canonical reference for current naming — keep new text consistent with it.
- American English spelling.
- Paid resources must be noted explicitly; links must be live (HTTP 200) and currently maintained.
- The "Canon" reading list is deliberately short — don't expand it casually. Check for duplicates before adding any entry.
- Excluded content: vendor pitches/self-promotion, blockchain/Web3/crypto, unreviewed LLM-generated content.
- Keep PRs focused on one theme; branch naming convention is `add-<short-name>`.
