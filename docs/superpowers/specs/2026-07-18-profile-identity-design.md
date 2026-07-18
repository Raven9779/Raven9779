# GitHub Profile: Technical Identity Design

## Intent

Present Raven9779 as a full-stack engineer who ships complete systems. The
profile should feel like a concise technical introduction, not a portfolio,
resume, project gallery, or animated dashboard.

## Audience Outcome

Within a few seconds, a visitor should understand three things:

1. Raven builds full-stack systems rather than isolated interfaces.
2. Raven works across interface, API, and deployment boundaries.
3. Raven is actively building and prefers shipping complete operating paths.

## Information Architecture

The README contains only four visible content blocks:

1. **Identity**
   - `Raven9779`
   - `Full-stack engineer`
   - `从界面到部署，把产品做成可运行的系统。`
2. **System Trace**
   - A single custom SVG with a restrained systems-monitor visual language.
   - It states the engineering path: `interface -> API -> deploy`.
   - It uses factual, durable language such as `STATUS: SHIPPING`.
3. **Focus**
   - A compact line naming full-stack systs, self-hosted products, and
     product engineering.
   - One short `NOW` note about the personal publishing system, without a project card, repository link, or release checklist.
4. **Stack**
   - One compact technology row: Next.js, TypeScript, Node.js, Docker, Nginx, and AWS.

## Visual Direction

- Use the existing deep blue, teal, and graphite palette from the current header and Build Signal asset.
- Prefer monospace labels, clear hierarchy, and generous whitespace.
- The System Trace is the only decorative technical detail. It is static, accessible through descriptive alt text, and does not simulate live data.
- Do not add game art, project thumbnails, contribution graphs, badges, or animated elements.

## Explicit Exclusions

- No `BUILD LOG` project cards.
- No links to private repositories.
- No `SIDE QUEST` or Arcade preview in the profile README.
- No contribution Snake image in the README.
- No unverified operational claims, release gates, or fake telemetry.

## Snake Removal

- Remove the README image block and generated SVG outputs.
- Keep the Snake workflow disabled so it cannot recreate generated files.
- Delete the workflow source when an authenticated GitHub credential with the `workflow` scope is available; GitHub currently rejects that file deletion for the active credential.

## Arcade Boundary

The arcade shooter remains an independent public experiment. It is not part of the profile information architecture. Its mobile layout and input defects are handled in the Arcade repository through a separate implementation and PR.

## Verification

1. Rendered README contains the four intended blocks and no project gallery.
2. All referenced profile assets return HTTP 200 and use meaningful alt text.
3. No README link points to a private repository.
4. Snake is absent from the README and its workflow is disabled.
5. Changes are merged through a PR, then only `master` remains remotely and locally.
