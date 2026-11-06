---
name: create-readme
description: Create, review, or improve a project README (README.md or README.txt). Use when the user asks to write a README, audit or optimize an existing README, fix a bad first impression, reduce a badge wall, or prepare an open-source launch — including "review my README", "my README is bad", "README 审计", "README 优化", "修改 README", "项目介绍", "repo first impression", "too many badges", or "write a README for this repo". It detects project type and audience, audits the existing README against evidence, verifies claims and the install path, then rewrites it in a short, documentation-first style (like anthropics/claude-cookbooks and openai/evals) with direct Setup/Usage/FAQ headings. Do not use for API reference documentation, changelogs, profile READMEs, or prompt-only tasks.
compatibility: Claude Code on Windows, macOS, or Linux. The audit script requires bash (Git Bash or WSL on Windows).
version: 0.3.0
author: huajielong (merged from github/awesome-copilot create-readme and samber/developer-relations-skills readme-optimization)
license: MIT
---

# Create or Improve a README

A README is a landing page and a router, not a documentation dump. It answers three questions fast — what is this, why choose it over the alternative, and what is the first command — then sends the reader onward. Judge it on whether a stranger can evaluate the project, not on documentation completeness.

## Style target

Match the short, documentation-first style of [anthropics/claude-cookbooks](https://github.com/anthropics/claude-cookbooks) and [openai/evals](https://github.com/openai/evals):

- One-to-three-sentence intro: what it is and who it is for. No pitch, no tagline stack.
- Direct headings: Setup, Usage, Running, Writing, FAQ, Contributing, Additional Resources.
- One-line, link-dense lists; exact commands; code blocks only where the reader needs to copy something.
- 400–900 words and 5–10 sections for most projects. Four sections are complete for a small project.
- No centered HTML header, badge wall, or hand-written table of contents by default. Add a TOC only when the README exceeds ~10 sections.
- Status and licence in one short line or footer, not a section.
- Admonitions only when a warning genuinely prevents a mistake; a bold sentence is usually enough.

## Mode selection

| Mode | When | First action |
|---|---|---|
| **Create** | No README, or the user asks you to write one | Review the project, choose a skeleton, write |
| **Review / improve** | A README exists and the user asks to review, audit, optimize, or fix it | Audit first; do not rewrite before diagnosis |

For `README.txt`: preserve the existing filename and plain-text conventions. The audit script reports partial checks for `.txt` (headings, badges, and links are Markdown-specific). Recommend `README.md` only if the user wants GFM features; never force a rename.

## Non-negotiables

1. **Read the project.** Review manifests, source layout, license, recent commits, examples, and scripts. Never write from memory or from the README alone.
2. **Verify every claim against its source.** Check commands, flags, defaults, environment variables, file paths, exported symbols, version requirements, and behavioural assertions. Any performance number, compatibility matrix, or "production ready" claim needs a benchmark, CI matrix, changelog entry, or other repo evidence — or it comes out.
3. **Run the documented install and first usage example** from a clean state. If you cannot execute them, say so plainly and ask the user to run them; never mark this check passed on inspection.
4. **In review mode, produce a scorecard before any rewrite** and get the user's agreement on the diagnosis. A maintainer who rejects the diagnosis rejects the treatment.
5. **Do not invent** statistics, user counts, benchmarks, status, or compatibility. When you cannot verify something, downgrade the claim to what you can verify and say which part is unconfirmed.
6. **Keep it concise.** Match the style target: short intro, direct headings, no marketing padding, no emojis. Do not add `LICENSE`, `CONTRIBUTING`, or `CHANGELOG` sections; a one-line footer is enough.
7. **Use GFM** (headings, tables, fenced code, relative links). Do not add a centered header, badge row, or hand-written TOC by default. Use a logo only if the project already has one; never invent one.
8. **Write in the project's language.** For Chinese projects, keep Chinese headings and terminology; do not translate product names or commands.

## Workflow

### 1. Declare the audience and project type

Ask one question at a time; skip anything you can infer from the repository. Answers gate the rewrite:

- **Project type:** library/SDK, CLI tool, application/service, framework/platform, or template/learning resource?
- **Audience:** developers in one ecosystem, polyglot developers, platform/ops engineers, or non-developer evaluators such as security/procurement?
- **Adoption path:** an individual installs and decides alone, or a company approves licence, security, and support?
- **Goal:** more installs, contributors, stars, enterprise conversations, or fewer repetitive support questions?
- **Honest status:** experimental, actively maintained, feature-frozen, or seeking maintainers?
- **Alternatives:** what does the reader already know, and what do those do better?
- **Constraints:** legal/brand requirements, other render targets (registry page, docs site, terminal), deadline, effort ceiling.

Record the declared audience at the top of the scorecard so every later judgement is checkable.

### 2. Gather evidence

- Read the README in full and note where you personally got confused.
- Read the manifests, licence, recent commits, and open-issue volume; check that examples and referenced files exist on disk.
- Run the mechanical audit in review mode:

  ```bash
  bash scripts/readme-audit.sh <repo-root>
  ```

  It reports shadowed copies, truncation risk, the heading tree, section count, words before the first copyable command, badge count, empty alt text, raw HTML, broken relative links, placeholders, and content categories. It emits facts, not judgement.
- Verify commands, flags, defaults, env vars, paths, exported symbols, version requirements, and behavioural assertions against their source of truth (argument parser, definition site, route table, changelog).
- Run the documented install command and first usage example exactly as written; fold every gap back into the README as an explicit prerequisite or verification step.
- If the README is also published on a package registry, check that render too; relative links and raw HTML often break there.

### 3. Create mode: choose a skeleton and write

Pick from [`references/readme-skeletons.md`](references/readme-skeletons.md) by **what the reader is deciding**, not by what the code happens to be. Default to the documentation-first skeleton — title, one-to-three-sentence intro, Setup, Usage, FAQ, Contributing/Additional Resources, licence footer — and use the more elaborate library/CLI/application skeletons only when the project genuinely needs them.

Order sections so a reader can disqualify the project as early as possible: identity → differentiation → usage → install → detail → background.

For each section:

1. Write the shortest version that answers its question completely.
2. Replace every unfalsifiable claim ("simple", "blazing fast", "developer-friendly") with something checkable — a number, a dependency count, a supported-platform list, a named trade-off.
3. State what the project deliberately does not do, and when to use something else. This is the highest-trust sentence in most READMEs.
4. Link every term a reader outside the project's ecosystem may not know.
5. Move anything that grows past a screen into a linked document and leave a one-line pointer.

Before writing the opening, draft three candidate one-liners on different axes — what it does mechanically, what problem it removes, what it lets the reader stop using — present each trade-off, and let the user choose.

### 4. Review mode: score before rewriting

Use [`references/readme-audit.md`](references/readme-audit.md). Produce the scorecard first, with evidence for every check — a line number, a quoted sentence, or command output:

```text
README audit ; owner/repo   (audience: ...)
Band A: A1 pass · A2 FAIL (reason) · A3 FAIL · A4 pass · A5 pass
Claim check: N unverifiable ; ...
Band B: 13/16 (B1 2, B2 2, B3 1, ...)
Verdict: passes / does not pass ; ...
Fix in this order: ...
```

- **Band A (blocking):** A1 identity in the first screen; A2 the first command runs; A3 every claim verifiable; A4 status stated honestly; A5 licence reachable from the README.
- **Band B (0–2 each):** B1 differentiation; B2 time to a copyable command; B3 runnable usage example; B4 funnel ordering; B5 badge discipline; B6 nothing critical trapped in an image; B7 links resolve everywhere; B8 overflow lives elsewhere.
- **Pass:** all five Band A checks pass **and** Band B totals at least **14/16**.

The 14/16 threshold and the 60-second cold-reader test are working defaults, not published standards; let the user move them for a stated reason and record it. Get the user's agreement on the score before rewriting. Then rewrite section by section, using [`references/before-after-examples.md`](references/before-after-examples.md) for the opening, differentiation, status, badge row, install block, and usage snippet.

### Order the fixes

Order the fix list by what each fix returns per hour spent, not by how easy it is to write and not by how much damage it undoes. When the orderings disagree, state all three and lead with the third:

- effort, least first: `status line == badge row > claim check > install path > differentiation paragraph > funnel restructure`
- value, most first: `install path > differentiation paragraph > claim check > status line > funnel restructure > badge row`
- efficiency, do first: `status line > install path > claim check > differentiation paragraph > badge row > funnel restructure`

Default to the top three for a first pass. Promote the funnel restructure when the cold reader never scrolls far enough, or when a launch is the reason for the audit. Delete every fix the user's effort ceiling rules out instead of listing it last.

### Enterprise audiences

Run this only when the audience is a company or procurement evaluator. List which trust signals are present and which are missing, in this order:

`support expectation == release cadence > security policy > named adopters > supply-chain attestation > governance and maintainer depth > foundation affiliation`

That line doubles as the fix order. These are not scored; they are pass/absent facts a buyer will check anyway.

### 5. Verify

- Re-run the audit and re-score. Iterate until the threshold is met or the user explicitly accepts a lower score for a stated reason.
- Run the cold-reader test: five questions, 60 seconds, five correct answers — what is this; why over the alternative; does it fit my language/platform/licence; what is the first command; is it alive?
- Check every relative link, the install path, and (if applicable) the registry render.
- Report what changed, what remains unverified, and any remaining risk.

## Badge rules

Default to **zero badges**. Add at most two or three **assessment signals** only when they answer a reader's question: released version, licence, supported runtime/platform. Cut **conventional signals** such as "PRs welcome". A badge wall is a cost, not a feature; build-status badges mostly serve maintainers.

## Style references

Primary — short, documentation-first:

- https://github.com/anthropics/claude-cookbooks
- https://github.com/openai/evals

More examples:

- https://raw.githubusercontent.com/Azure-Samples/serverless-chat-langchainjs/refs/heads/main/README.md
- https://raw.githubusercontent.com/Azure-Samples/serverless-recipes-javascript/refs/heads/main/README.md
- https://raw.githubusercontent.com/sinedied/run-on-output/refs/heads/main/README.md
- https://raw.githubusercontent.com/sinedied/smoke/refs/heads/main/README.md

## Common failure modes

- Rewriting before diagnosing; fixing prose while preserving a broken install.
- Padding to look serious; four sections are complete for a single-purpose project.
- Marketing voice; superlatives without evidence lower trust.
- Centered header + badge wall + hand-written TOC on a small project; it reads as a template, not a page.
- A "Why choose us" section with no concrete alternative; the reference style states the value in the intro and moves on.
- Auditing only one render target; registry pages have different rules.
- Fabricating status or benchmarks; one glance at the commit history disproves them.
- Leading with the cheapest fix (badge row) instead of the highest-value fix (install path).
- Applying Markdown-only advice (badges, admonitions, GFM tables) to a plain-text `README.txt` without checking the render target.
- Quoting a single short-run tok/s peak as sustained performance.

## References

- [`references/readme-audit.md`](references/readme-audit.md) — Band A/B scorecard, report format, cold-reader test.
- [`references/readme-skeletons.md`](references/readme-skeletons.md) — section orderings by project type.
- [`references/before-after-examples.md`](references/before-after-examples.md) — worked positive and negative examples.
- [`references/published-findings.md`](references/published-findings.md) — sourced statistics and the folklore list.
- [`references/tooling-and-measurement.md`](references/tooling-and-measurement.md) — scoring, linting, and attribution tools.
- [`scripts/readme-audit.sh`](scripts/readme-audit.sh) — mechanical facts the audit depends on.

Attribution: see [`NOTICE.md`](NOTICE.md).
