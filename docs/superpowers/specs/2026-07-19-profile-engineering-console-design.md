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

- A GitHub-supported `<picture>` element selects a `960 x 360` horizontal SVG
  for desktop and a `720 x 1020` vertically stacked SVG at `max-width: 600px`.
- Both assets use a graphite background, deep-blue structural panels, teal
  route highlights, and a single amber action accent.
- Use clear sans-serif text for headings and a monospace face only inside the
  console panes.
- The outer frame, dividers, grid, and subtle glow give the three panes one
  coherent device-like surface.
- The mobile variant keeps the exact same console concept while stacking the
  panes and enlarging essential labels for readability at 320px CSS display
  width without horizontal scrolling.

## Motion And Accessibility

- Motion is decorative and restrained: a slow route packet, a cursor blink,
  and a low-frequency terminal indicator pulse.
- The SVG includes a `prefers-reduced-motion: reduce` media rule that disables
  all animation and leaves every state visible.
- Static teal arrowheads make the interface-to-API-to-deploy direction clear
  even when motion is reduced.
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

- Add `assets/engineering-console.svg` as the `960 x 360` desktop component
  and `assets/engineering-console-mobile.svg` as the `720 x 1020` mobile
  component.
- Update `README.md` to use a GitHub-supported `<picture>` element that selects
  the mobile asset at `max-width: 600px` and otherwise uses the desktop asset.
- Remove `assets/system-trace.svg` after the README reference changes.

## Verification

1. Both SVGs validate as XML, have the expected `960 x 360` desktop and
   `720 x 1020` mobile viewBoxes, and include `title`, `desc`, `role="img"`,
   and `aria-labelledby`.
2. Required pane labels, static route arrowheads, and route markers are
   present; prohibited telemetry and portfolio terms are absent.
3. Both sources include reduced-motion handling and no script, external URL,
   or data-fetching dependency.
4. README references the responsive console `<picture>` with both assets and
   keeps the technical identity structure without Arcade, Snake, project
   cards, or private links.
5. The change is merged through a focused PR and the temporary branch is
   removed after verification.
