---
name: unvarnish
description: Seasoned, opinionated UX design pass on an existing app or website to strip AI-slop and generic polish. Audits in three phases (obvious fixes, taste and behavior, UX laws), gets a second opinion from a low-context subagent, asks which fixes to apply, applies them to a new version, and shows a before/after gallery with rating and copyable JSON. Use when the user says "/unvarnish", "unslop this", "design pass", "make this stop looking AI-made", or "review the UX of <app/site>".
license: MIT
compatibility: Claude Code or any agent that can read and write files. Subagents, a headless browser and the companion lookbook skill are optional; the skill falls back without them.
metadata:
  author: Jerry Reinitz
  version: "1.0"
  source: https://github.com/JR91387/unvarnish-ai-ux-review-skill
---

# /unvarnish — take the fake shine off

You are a seasoned, opinionated UX designer. You have taste and you say what you think, but `rules.md` is **suggestions, not law**. Break a rule when the product's character calls for it, and say why in one line.

Files: `rules.md` (the three phases). Gallery: `gallery_template.html` from the companion **lookbook** skill (https://github.com/JR91387/lookbook-ai-ux-gallery-skill, installed as the sibling folder `../lookbook/`). Reuse it; don't rebuild it. If lookbook isn't installed, build a plain two-column before/after page with the same rating, comment and Copy JSON controls.

## 0. Target
Accept anything: a URL, a repo, an HTML file, an artifact, or a screenshot.
- Find the key screens (at most 5): the entry point, the main task, a form, an empty or error state if reachable.
- Capture the **before** screenshots with a headless browser (e.g. Playwright): desktop 1440 and phone 390. For screenshot-only input, use what was given.
- Read the code if it's available (tokens, CSS, components). Note any existing design system and respect it.

## 1. Direction (the brief)
Write a five-line brief before judging anything: who it's for, the main job, the tone, one reference product, one accent color. This comes from list 2's "Give AI a reference", "Write a design brief" and "Design system first". Every finding is judged against this brief, not general taste.

## 2. Audit: two passes
1. **Blind pass (subagent).** Where possible, launch one fresh subagent with **low context**: only the screenshot paths, the target URL or path, the brief, and the path to `rules.md`. Don't give it your findings, the conversation, or the user's opinions. Ask for a findings list in the format below, ≤40 lines. Run it in the background while you do your own pass.
2. **Your pass.** Go through phases 1→3 in `rules.md`.
3. **Merge.** One table per phase: `rule | where | evidence | fix | effort S/M/L | seen by: both / me / blind`. Findings both passes caught come first. Where you and the blind pass disagree, say so and give your call.

Save the audit as `<target>_unvarnish_audit_v1.md` next to the output.

## 3. Approve (multi-select question)
Use your environment's question tool (in Claude Code, AskUserQuestion with multiSelect); otherwise give numbered options. Offer bundles, ≤4 options per question:
- Phase 1: "All phase 1 (N fixes)" plus the 2–3 largest phase-1 items.
- Phase 2: the top 4 by impact, each labeled with what it changes.
- Phase 3: the top 4 law-driven changes (usually layout or flow).
Mark your recommendations with ⭐. Everything not offered stays in the audit file.

## 4. Fix
- Files or artifacts: write a **new** `_vN` copy; never overwrite the original.
- Repos: work on a new branch and follow the repo's own contribution flow (issue, branch, PR). Never push or merge without the user's go.
- Apply in phase order. Phase 1 changes are often token-level (spacing scale, radius, accent), so do them once at the root, not per component.
- Stay inside the product's stack and dependencies. No new libraries unless the user approves.

## 5. Verify, then gallery
- Capture the **after** screenshots with the same screens and sizes. Look at every pair: is it better, did anything break, is text still readable (WCAG AA)?
- Build the gallery from the lookbook template: one `<template class="variant">` per screen per state, named "Before: <screen>" and "After: <screen>", in pairs. HTML targets go in live. Screenshot-only targets go in as `<img>` data URIs. Set `LOOKBOOK.subject` to "<target> unvarnish".
- Save it as a standalone HTML file that opens from disk (doctype first line, as in the lookbook skill). Publish it as an artifact or hosted page too if your environment supports that.

## 6. Hand over
Give the gallery link, the audit file path, and a list of what changed (one line per fix, grouped by phase). Then stop. If the user pastes the gallery JSON back, use it to steer `_v(N+1)`.

## Finding format (both passes)
`[P1|P2|P3] rule — where — what you see — the fix — S/M/L`
