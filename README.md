# create-readme

A Claude Code skill for creating, reviewing, and improving project READMEs. Unlike a one-shot generator, it audits the existing page and verifies the install path before writing. The output follows the short, documentation-first style of [anthropics/claude-cookbooks](https://github.com/anthropics/claude-cookbooks) and [openai/evals](https://github.com/openai/evals).

> Status: v0.3.0 · MIT.

## Setup

Clone this repository into your Claude Code skills directory:

```bash
git clone https://github.com/huajielong/create-readme.git \
  ~/.claude/skills/create-readme
```

Windows PowerShell:

```powershell
git clone https://github.com/huajielong/create-readme.git `
  "$env:USERPROFILE\.claude\skills\create-readme"
```

This repository is the merged version: create, audit, `README.txt` support, and Chinese heading detection. The upstream `github/awesome-copilot@create-readme` only creates. **Do not overwrite this directory with `npx skills add`; it replaces the merged version with the upstream one.**

## Usage

In Claude Code, ask:

- “Write a README for this repo.”
- “Review my README.”
- “README 审计”
- “修改这个项目的 README.txt”
- “Make this README as concise as anthropics/claude-cookbooks.”

The skill picks create or review mode automatically. In review mode it produces a scorecard before rewriting.

## Modes

| Mode | Trigger | Output |
|---|---|---|
| Create | No README, or “write a README” | Skeleton, draft, link and command check |
| Review / improve | “review my README”, “README 审计” | Band A/B scorecard, agreed diagnosis, rewrite, cold-reader test |

The pass line is all five Band A checks plus Band B ≥14/16. See [`references/readme-audit.md`](references/readme-audit.md).

## Style

The target is the documentation-first README used by the two reference repositories:

- One-to-three-sentence intro, no pitch or tagline stack.
- Direct headings: Setup, Usage, FAQ, Contributing, Additional Resources.
- One-line, link-dense lists and exact commands.
- 400–900 words and 5–10 sections for most projects.
- No centered header, badge wall, or hand-written TOC by default.
- Status and licence in one line or footer.

Longer skeletons for libraries, CLIs, services, and frameworks are in [`references/readme-skeletons.md`](references/readme-skeletons.md); use them only when the project needs them.

## Audit script

`scripts/readme-audit.sh` prints mechanical facts; it does not judge:

```bash
bash scripts/readme-audit.sh /path/to/repo
```

Example output:

```text
FILE: /path/to/repo/README.md
SIZE: 11746 bytes / 193 lines / 793 words
SECTION COUNT (h2-h6): 12
FIRST CODE BLOCK: line 45, after 143 words of prose
BADGES IN FIRST 40 LINES: 3
BROKEN RELATIVE LINKS: (none)
```

It reports shadowed README copies, truncation risk, the heading tree, section count, words before the first copyable command, badge count, empty alt text, raw HTML, broken relative links, unfinished markers, and content categories. `README.txt` is detected; its heading, badge, and link checks are partial.

The script needs bash; on Windows use Git Bash or WSL.

## Repository layout

```text
create-readme/
├── SKILL.md
├── README.md
├── CONTRIBUTING.md
├── CHANGELOG.md
├── references/
│   ├── readme-audit.md
│   ├── readme-skeletons.md
│   ├── before-after-examples.md
│   ├── published-findings.md
│   └── tooling-and-measurement.md
├── scripts/readme-audit.sh
├── evals/
│   ├── evals.json
│   └── skill-evaluator-2026-11-06.md
├── NOTICE.md
└── LICENSE
```

## Evaluation

- 17 cases in [`evals/evals.json`](evals/evals.json): 15 audit scenarios, 1 create-from-scratch case, 1 concise-style rewrite case.
- Security gate passed 5/5; report in [`evals/skill-evaluator-2026-11-06.md`](evals/skill-evaluator-2026-11-06.md).
- `SKILL.md` is the entry point; detailed guidance lives in `references/`.

Evaluation is a local check, not an official certification.

---

MIT · [LICENSE](LICENSE) · [NOTICE](NOTICE.md)
