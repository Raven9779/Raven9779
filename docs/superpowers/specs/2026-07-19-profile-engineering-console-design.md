# Profile Engineering Console Design

## Intent

Give the GitHub profile one memorable technical component without turning the
README into a portfolio, dashboard, or activity feed. The component should make
Raven9779 feel like a full-stack engineer who thinks in complete operating
paths: interface, API, and deployment.

## Audience Outcome

Visitors should see a compact engineering signature rather than a collection of
claims. The component must communicate three ideas at a glance:

1. Raven thinks in build and delivery paths.
2. Raven connects user-facing interfaces to API and deployment boundaries.
3. Raven has an opinionated, playful but professional engineering voice.

## Information Architecture

Replace the existing standalone `System Trace` artwork with one full-width
`Engineering Console` SVG. It sits below the identity block and above `FOCUS`.
The README remains otherwise concise: identity, console, focus/now, and stack.

The SVG contains three equal visual panes within one shared console frame:

1. **Build Terminal**
   - Deterministic, non-live command transcript.
   - Example sequence: `route interface -> api`, `validate contract`, and
     `handoff deploy`.
   - Completion marks describe the illustrated path, not a real deployment.

2. **Request Machine**
   - A small state diagram: `INTERFACE -> API -> DEPLOY`.
   - One highlighted packet follows the route to make the architecture readable.
   - It represents the engineering model only; it never displays latency,
     traffic, uptime, release numbers, or other operational telemetry.

3. **Command Palette**
   - A restrained developer easter egg: `> raven.ship()` and a blinking cursor.
   - Supporting labels are static identity cues such as `MODE: PRODUCT
     ENGINEERING` and `READY: INTENTIONAL`.
   - It has no clickable behavior, repository links, or simulated shell input.

## Visual Direction

- One `960 x 300` SVG with a graphite background, deep-blue structural panels,
  teal route highlights, and a single amber action accent.
- Use clear sans-serif text for headings and a monospace face only inside the
  console panes.
- The outer frame, dividers, grid, and subtle glow give the three panes one
  coherent device-like surface.
- On a narrow GitHub mobile viewport the image scales to its container without
  requiring horizontal scrolling. Its text sizes remain readable at 320px CSS
  display width.

## Motion And Accessibility

- Motion is decorative and restrained: a slow route packet, a cursor blink,
  and a low-frequency terminal indicator pulse.
- The SVG includes a `prefers-reduced-motion: reduce` media rule that disables
  all animation and leaves every state visible.
- Provide an SVG `title` and `desc`, plus meaningful README `alt` text that
  describes the three panes and their illustrated route.
- The artwork is self-contained: no JavaScript, external fonts, remote assets,
  or data loading.

## Explicit Exclusions

- No project cards, timelines, contribution graphs, GitHub stats, trophy
  widgets, or skill badges beyond the existing compact stack row.
- No Arcade artwork or links in the profile README.
- No fake terminal prompt, live traffic, deployment timestamp, metric, uptime,
  or completion claim.
- No full-screen visual takeover, dense code block, or user interaction that a
  GitHub README image cannot actually support.

## Asset Transition

- Add `assets/engineering-console.svg` as the new component.
- Update `README.md` to reference the new asset in place of
  `assets/system-trace.svg`.
- Remove `assets/system-trace.svg` after the README reference changes.

## Verification

1. The SVG validates as XML, has the expected `960 x 300` viewBox, and includes
   `title`, `desc`, `role="img"`, and `aria-labelledby`.
2. Required pane labels and route markers are present; prohibited telemetry and
   portfolio terms are absent.
3. The source includes reduced-motion handling and no script, external URL, or
   data-fetching dependency.
4. README references only the new console asset and keeps the technical identity
   structure without Arcade, Snake, project cards, or private links.
5. The change is merged through a focused PR and the temporary branch is
   removed after verification.
