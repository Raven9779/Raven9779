# Profile Engineering Console Implementation Plan

> **Execution:** Use `superpowers:subagent-driven-development` after this plan
> is committed. Each task needs an implementer report, task-scoped review, and
> a ledger entry before the next task starts.

**Goal:** Replace the profile README's single static System Trace with one
compact, accessible Engineering Console SVG. It should signal product
engineering through deterministic, illustrative interfaces rather than a
portfolio, an activity dashboard, or fake operational data.

**Architecture:** Keep the profile README deliberately small. A single local
SVG owns the visual language, metadata, decorative grid, three equal console
panes, CSS-only motion, and reduced-motion behavior. `README.md` only embeds
the asset with meaningful alternate text.

**Tooling:** GitHub profile README Markdown, hand-authored SVG/CSS, `xmllint`,
ImageMagick `magick` when available, and GitHub CLI for the release PR.

## Global Constraints

- Work from a clean feature branch, never directly on `master`.
- Keep the existing centered identity, `FOCUS`, `NOW`, and `STACK` sections.
- Add exactly one full-width `Engineering Console` SVG between the identity
  block and `FOCUS`.
- The console has exactly three equal-pane concepts: Build Terminal, Request
  Machine, and Command Palette. It is one cohesive console, not three cards.
- Use a graphite/deep-blue panel base, teal route signal, and a restrained
  amber accent. Do not introduce remote images, JavaScript, web fonts, links,
  forms, or dependencies.
- The terminal transcript is illustrative and deterministic. It must not imply
  real build completion, live metrics, or an active command session.
- The request pane must communicate `INTERFACE -> API -> DEPLOY` but must not
  claim traffic, latency, uptime, release history, online state, or metrics.
- The command palette must include `> raven.ship()` and a blinking cursor, but
  it is not interactive.
- Implement only CSS/SVG animation: a moving route signal, blinking cursor,
  and terminal indicator pulse. `prefers-reduced-motion: reduce` must disable
  every one of those animations.
- The SVG must include `role="img"`, `aria-labelledby`, a `<title>`, and a
  `<desc>`. README alternate text must describe the visual rather than repeat
  the filename.
- Do not reintroduce Arcade/Snake, project cards, GitHub stats, trophies,
  activity graphs, dashboards, or skill badges beyond the existing `STACK`
  icon row.
- `assets/system-trace.svg` is obsolete after integration and must be removed.
- Preserve any untracked `.playwright-mcp/` content exactly as found.

## Task 1: Publish The Approved Design And Plan

**Files:**
- Verify: `docs/superpowers/specs/2026-07-19-profile-engineering-console-design.md`
- Create: `docs/superpowers/plans/2026-07-19-profile-engineering-console.md`

1. Confirm the spec contains the approved three-pane console, constraints,
   exclusions, accessibility requirements, and SVG asset path.
2. Run `git diff --check` and verify no tracked profile content has changed.
3. Commit the implementation plan with the already committed design spec.
4. Push `agent/profile-engineering-console-design`, create a documentation PR,
   merge it with the GitHub CLI, and delete the remote branch.
5. Confirm local `master` matches `origin/master` and delete the local
   documentation branch after checking it out of the branch.

**Verification:**

```sh
git diff --check
git status --short
gh pr view --json state,mergedAt,url
git branch --format='%(refname:short)'
```

## Task 2: Add The Engineering Console SVG

**Files:**
- Create: `assets/engineering-console.svg`

1. Begin from `master` on `agent/profile-engineering-console`.
2. Create a self-contained SVG with `viewBox="0 0 960 360"`, `role="img"`,
   `aria-labelledby="console-title console-description"`, a short title, and
   a complete descriptive sentence.
3. Use local `<style>` rules with a stable mono fallback stack such as
   `ui-monospace, SFMono-Regular, Menlo, Consolas, monospace`; no external
   font import is allowed.
4. Draw one exterior frame and three equal panels. Use concise, high-contrast
   labels sized for a 960px asset that remains visually legible when embedded
   at 320px wide; favor short labels and large type over dense copy.
5. Build the left pane as a deterministic terminal with the exact visible
   command concepts `route interface -> api`, `validate contract`, and
   `handoff deploy`, plus language that makes the display illustrative instead
   of a real log.
6. Build the center pane around labeled nodes `INTERFACE`, `API`, and `DEPLOY`
   joined by one teal route. Overlay a dashed teal signal path with CSS
   `stroke-dashoffset` animation rather than SMIL so reduced motion can disable
   it reliably.
7. Build the right pane as a non-interactive command palette containing the
   exact `> raven.ship()` command, a small blinking cursor, and the supporting
   labels `MODE: PRODUCT ENGINEERING` and `READY: INTENTIONAL`.
8. Use three shared `motion` class hooks: `route-flow`, `cursor`, and
   `indicator`. Give each a CSS animation. In `@media (prefers-reduced-motion:
   reduce)`, set their animation to `none`.
9. Keep static decoration restrained: one subtle grid, panel borders, and
   only teal/amber accents. Do not add cards, graphs, fake percentages, or
   operational telemetry.
10. Commit the self-contained asset.

**Suggested implementation structure:**

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 960 360" role="img"
  aria-labelledby="console-title console-description">
  <title id="console-title">Raven9779 Engineering Console</title>
  <desc id="console-description">An illustrated build terminal, request route
    from interface through API to deploy, and command palette for Raven9779.</desc>
  <style>
    .motion { transform-box: fill-box; transform-origin: center; }
    .route-flow { animation: flow 6s linear infinite; }
    .cursor { animation: blink 1s steps(1, end) infinite; }
    .indicator { animation: pulse 2.4s ease-in-out infinite; }
    @keyframes flow { to { stroke-dashoffset: -48; } }
    @keyframes blink { 50% { opacity: 0; } }
    @keyframes pulse { 50% { opacity: .35; } }
    @media (prefers-reduced-motion: reduce) {
      .route-flow, .cursor, .indicator { animation: none; }
    }
  </style>
  <!-- Full-width shell and three equally sized panes follow. -->
</svg>
```

**Verification:**

```sh
xmllint --noout assets/engineering-console.svg
rg -n 'ENGINEERING CONSOLE|BUILD TERMINAL|REQUEST MACHINE|COMMAND PALETTE|INTERFACE|API|DEPLOY|raven\.ship|prefers-reduced-motion|aria-labelledby' assets/engineering-console.svg
rg -n '<script|fetch\(|url\(' assets/engineering-console.svg
rg -n -i 'uptime|latency|traffic|metric|release history|online state' assets/engineering-console.svg
git diff --check
```

The first `rg` scan must find the required content. The final two `rg` scans
must return no matches for forbidden executable/external/telemetry patterns.

## Task 3: Integrate The Console And Retire System Trace

**Files:**
- Modify: `README.md`
- Delete: `assets/system-trace.svg`

1. Replace the `assets/system-trace.svg` image reference with
   `assets/engineering-console.svg` in the existing centered full-width image
   block; do not add another visual block.
2. Set the image alternate text to: `Engineering Console with a deterministic
   build terminal, a request route from interface through API to deploy, and a
   command palette reading raven.ship.`
3. Delete `assets/system-trace.svg`.
4. Confirm `README.md` retains its centered identity and the existing `FOCUS`,
   `NOW`, and `STACK` sections in their present order.
5. Confirm no `Arcade`, `Snake`, old System Trace asset reference, GitHub stats
   image, trophy, graph, or project-card markdown was added.
6. Commit integration and retirement together.

**Verification:**

```sh
test -f assets/engineering-console.svg
test ! -e assets/system-trace.svg
rg -n 'engineering-console\.svg|Engineering Console with a deterministic build terminal' README.md
rg -n -i 'arcade|snake|system-trace|trophy|activity|stats-card|project card' README.md
git diff --check
```

The first three checks must succeed and the final `rg` must return no matches.

## Task 4: Visual, Motion, And Accessibility QA

**Files:**
- Verify only: `README.md`, `assets/engineering-console.svg`

1. Render the SVG to a temporary PNG at native width and at 320px width, then
   inspect both images. Check that the three panes remain balanced, labels are
   not clipped, panel spacing is intentional, and the teal signal and amber
   accent remain restrained against the dark base.
2. Inspect the SVG source to confirm the `<title>`, `<desc>`, role, and
   `aria-labelledby` identifiers align exactly.
3. Inspect the CSS to confirm all three animations use the classes named in
   Task 2 and the reduced-motion media query disables each class.
4. Verify the file has no network dependency, no scripts, no links, and no
   fake operational claims.
5. Add no repository files during QA; use `/private/tmp` for temporary output.

**Verification:**

```sh
magick -background '#0b1220' assets/engineering-console.svg /private/tmp/engineering-console.png
magick -background '#0b1220' -resize 320 /private/tmp/engineering-console.png /private/tmp/engineering-console-320.png
identify /private/tmp/engineering-console.png /private/tmp/engineering-console-320.png
xmllint --noout assets/engineering-console.svg
git status --short
```

## Task 5: Release The Profile Update

**Files:**
- Verify only: `README.md`, `assets/engineering-console.svg`, repository history

1. Rebase or merge the current `origin/master` only if it changed after the
   feature branch started, resolve only conflicts inside this feature's files,
   and rerun Tasks 2-4 verification after resolution.
2. Push `agent/profile-engineering-console`, create one PR with a concise
   feature-focused title and body, and inspect the diff for scope only in
   `README.md`, `assets/engineering-console.svg`, and deletion of
   `assets/system-trace.svg`.
3. Merge after the PR's checks and GitHub mergeability are clear. Delete the
   remote branch and local branch only after confirming `master` contains the
   merged commit.
4. Finish on local `master` synchronized with `origin/master`. Do not remove
   `.playwright-mcp/`.

**Verification:**

```sh
gh pr view --json state,mergeable,statusCheckRollup,url
gh pr view --json state,mergedAt,mergeCommit,url
git fetch origin
git status --short --branch
git branch --format='%(refname:short)'
git ls-remote --heads origin 'agent/profile-engineering-console'
```

## Final Review

1. Generate a review package from the implementation branch merge base to
   `HEAD` and run a whole-branch review with a fresh reviewer.
2. Resolve every Critical or Important finding with one focused fix task and
   re-run the final review.
3. Record completed tasks and review outcomes in `.superpowers/sdd/progress.md`.
4. Report the live profile URL and, if GitHub's media render is visible, a
   direct rendered visual confirmation.
