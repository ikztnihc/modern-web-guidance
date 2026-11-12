---
name: modern-web-guidance
description: |
  Modern web platform guidance (CSS, layout, animation, performance, forms/autofill, client-side security, PWA install, accessibility) from the Chrome/Edge/Firefox/MDN teams, read from local guide files. Use before writing non-trivial CSS, layout, animation, view transitions, scroll effects, Core Web Vitals work (LCP, INP, CLS), form autofill/validation, HTML sanitization, Trusted Types, passkeys, or dates/timezones.
  For UI components that shadcn/ui provides (dialog, sheet, drawer, select, switch, tabs, menu, tooltip, popover, toast, accordion, checkbox, slider, date picker), the shadcn component ALWAYS wins: use this skill only for its accessibility and behaviour rules, never to replace the component.
  Do not use for backend, SQL, infra, CI, build tooling, or tests.
---

# Modern Web Guidance (house fork)

Fork of [GoogleChrome/modern-web-guidance](https://github.com/GoogleChrome/modern-web-guidance) v1.0.0. Changes from upstream are listed in `FORK.md` at the repo root.

## Rule 0: shadcn/ui always wins (overrides every guide)

This project builds UI with **shadcn/ui** (Base UI primitives) and Tailwind. The guides below are written in vanilla HTML/CSS/JS. When a guide conflicts with that:

1. **Never replace a shadcn component with a native element.** No `<dialog>`, `showModal()`, `popover` attribute, native `<select>`, `<input type="checkbox" switch>`, `<details>` accordion, `title=` tooltip or hand-built toast when `src/components/ui/` has (or `npx shadcn add` can add) the component. This holds even where a guide says MANDATORY, DON'T or "must".
2. **Apply the guide's rules inside the shadcn component** where they still fit: labels and accessible names, keyboard behaviour, focus return, `prefers-reduced-motion`, touch target size, error announcement, autofill attributes (`autocomplete=`), input types and `inputmode`.
3. **Ignore guide rules that the shadcn/Base UI component already handles** (focus trap, `role="dialog"`, `aria-modal`, Esc handling, scroll lock). Do not remove them from the component because a guide says the native element does it for free.
4. Guides marked **[shadcn]** in `INDEX.md` are about components shadcn owns. Read them only for points 2 and 3.
5. Project instructions (`CLAUDE.md`, `AGENTS.md`) override this skill. This skill overrides upstream guide text.

## Browser support

Our users are mostly on phones, and iPhone Safari is the weakest browser we support. Unless the project's `CLAUDE.md` says otherwise:

- Baseline **Widely available** features: use freely.
- Newly available or not-yet-Baseline features: only with feature detection (`@supports`, `'x' in window`) and graceful degradation that still works on the last two iOS Safari major versions. No polyfill over ~20 lines, no new dependency for a polyfill.
- Follow each guide's fallback section for anything not Widely available.

## How to find and read a guide (local files, no network)

All guides ship in this skill folder. Do **not** run `npx modern-web-guidance` (it downloads code from npm, sends telemetry, and is blocked in our cloud agent environment).

1. **Search the index** with 1-3 plain keywords for the platform capability, not the framework or app noun:
   `grep -i -E "view transition|transition" <skill-dir>/INDEX.md`
   (`<skill-dir>` = the folder holding this `SKILL.md`.) `INDEX.md` is large; grep it, don't read it whole.
2. **Read the matching guide(s)**: `<skill-dir>/guides/<category>/<id>.md`. Also read the category's overview guide when one exists and applies (for example `css/css.md`, `forms/forms.md`, `performance/performance.md`, `security/security.md`, `accessibility/accessibility.md`).
3. **No good match after one rephrase**: proceed without a guide. Don't force an unrelated one.
4. **Check your code against the guide** before finishing, through Rule 0.

## Translating vanilla guides to React + Tailwind

- New HTML attributes React doesn't type yet: spread them (`{...({ inert: '' } as any)}`) rather than dropping the feature.
- Per-instance IDs and `anchor-name`s: `useId()`.
- Modern CSS (container queries, `oklch`, `@starting-style`, `text-wrap`, `interpolate-size`, scroll-driven animations): Tailwind utilities or arbitrary values where they exist, otherwise a small rule in the app's CSS file inside `@layer`. Keep `@starting-style` and `:root` rules top level.
- Motion: transforms and opacity only, 150-300 ms, honour `prefers-reduced-motion`.

## Reporting

When a guide shaped your code, mention it in one line in your summary (guide id + the rule it applied). No separate section.
