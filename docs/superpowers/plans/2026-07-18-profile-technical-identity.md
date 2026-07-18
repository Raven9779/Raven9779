# Profile Technical Identity Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the project-gallery README with a concise technical identity that communicates Raven's engineering scope without portfolio cards or decorative noise.

**Architecture:** Keep GitHub's native profile README as the only content surface. Replace the deployment-specific `build-signal.svg` with one static `system-trace.svg`, then reduce `README.md` to identity, System Trace, focus, and stack. Snake is removed from the rendered page and its workflow is disabled so it cannot regenerate output files.

**Tech Stack:** GitHub profile README Markdown, static SVG, GitHub Contents/Actions APIs, GitHub pull requests.

## Global Constraints

- Do not add project cards, project thumbnails, contribution graphs, badges, animations, or private-repository links.
- Keep the header palette deep blue, teal, graphite, and amber; use monospace labels only inside the SVG.
- Use factual copy only; do not claim unverified release states or simulate live telemetry.
- Keep `arcade` out of the profile README; the game is maintained in its own repository.
- Remove Snake from the README and generated output files; keep its workflow disabled until a credential with `workflow` scope can delete the source file.
- Create and merge a normal PR; delete its feature branch after merge.

---

### Task 1: Create the static System Trace asset

**Files:**
- Create: `assets/system-trace.svg`
- Delete: `assets/build-signal.svg`

**Interfaces:**
- Consumes: GitHub README's relative image resolution for `assets/system-trace.svg`.
- Produces: a 960 by 220 static SVG with accessible title and description for `README.md`.

- [ ] **Step 1: Create the implementation branch before changing files**

```bash
git switch master
git pull --ff-only origin master
git switch -c agent/profile-technical-identity
```

- [ ] **Step 2: Write the asset contract check**

Run:

```bash
test -f assets/system-trace.svg
```

Expected: FAIL because the asset does not exist.

- [ ] **Step 3: Create `assets/system-trace.svg`**

Use this complete document structure. Preserve the stated labels and accessible text; the artwork must remain static.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 960 220" role="img" aria-labelledby="title desc">
  <title id="title">Raven9779 system trace</title>
  <desc id="desc">A static engineering trace from interface through API to deployment, marked as shipping.</desc>
  <defs>
    <linearGradient id="bg" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0%" stop-color="#0f172a"/>
      <stop offset="58%" stop-color="#123447"/>
      <stop offset="100%" stop-color="#0f3d3e"/>
    </linearGradient>
    <pattern id="grid" width="24" height="24" patternUnits="userSpaceOnUse">
      <path d="M24 0H0V24" fill="none" stroke="#cbd5e1" stroke-opacity=".07"/>
    </pattern>
    <filter id="glow" x="-30%" y="-80%" width="160%" height="260%">
      <feGaussianBlur stdDeviation="4" result="blur"/>
      <feMerge><feMergeNode in="blur"/><feMergeNode in="SourceGraphic"/></feMerge>
    </filter>
  </defs>
  <rect width="960" height="220" rx="18" fill="url(#bg)"/>
  <rect width="960" height="220" rx="18" fill="url(#grid)"/>
  <rect x="1" y="1" width="958" height="218" rx="17" fill="none" stroke="#67e8f9" stroke-opacity=".22"/>
  <circle cx="44" cy="42" r="5" fill="#2dd4bf" filter="url(#glow)"/>
  <text x="60" y="47" font-family="Courier New, monospace" font-size="16" font-weight="700" fill="#f8fafc">RAVEN9779 // SYSTEM TRACE</text>
  <text x="916" y="47" text-anchor="end" font-family="Courier New, monospace" font-size="12" fill="#94a3b8">FULL-STACK ENGINEERING</text>
  <path d="M42 70H918" stroke="#cbd5e1" stroke-opacity=".16"/>
  <text x="42" y="102" font-family="Courier New, monospace" font-size="12" font-weight="700" fill="#94a3b8">WORKING PATH</text>
  <text x="42" y="136" font-family="Courier New, monospace" font-size="22" font-weight="700" fill="#f8fafc">INTERFACE</text>
  <path d="M188 128H356" stroke="#2dd4bf" stroke-width="3" stroke-linecap="round"/>
  <path d="M344 120L356 128L344 136" fill="none" stroke="#2dd4bf" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/>
  <text x="384" y="136" font-family="Courier New, monospace" font-size="22" font-weight="700" fill="#f8fafc">API</text>
  <path d="M442 128H610" stroke="#f59e0b" stroke-width="3" stroke-linecap="round"/>
  <path d="M598 120L610 128L598 136" fill="none" stroke="#f59e0b" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/>
  <text x="638" y="136" font-family="Courier New, monospace" font-size="22" font-weight="700" fill="#f8fafc">DEPLOY</text>
  <rect x="42" y="166" width="152" height="28" rx="14" fill="#115e59" stroke="#2dd4bf" stroke-opacity=".72"/>
  <text x="118" y="185" text-anchor="middle" font-family="Courier New, monospace" font-size="11" font-weight="700" fill="#99f6e4">STATUS: SHIPPING</text>
  <text x="220" y="185" font-family="Courier New, monospace" font-size="12" fill="#cbd5e1">Build the whole operating path, not just the screen.</text>
</svg>
```

- [ ] **Step 4: Remove the superseded deployment-specific asset**

Delete `assets/build-signal.svg` using an `apply_patch` delete-file patch. Do not modify unrelated SVG assets.

- [ ] **Step 5: Verify SVG syntax and semantic contract**

Run:

```bash
xmllint --noout assets/system-trace.svg
rg -n 'SYSTEM TRACE|INTERFACE|API|DEPLOY|STATUS: SHIPPING|aria-labelledby' assets/system-trace.svg
test ! -e assets/build-signal.svg
```

Expected: XML succeeds, all six System Trace markers are found, and the old asset is absent.

- [ ] **Step 6: Commit the isolated asset change**

```bash
git add assets/system-trace.svg assets/build-signal.svg
git commit -m "feat: add technical system trace"
```

### Task 2: Replace the profile README with the four-block identity

**Files:**
- Modify: `README.md`

**Interfaces:**
- Consumes: `assets/system-trace.svg` created in Task 1 and `skillicons.dev` for the compact stack row.
- Produces: a profile README with identity, System Trace, focus, and stack only.

- [ ] **Step 1: Write the content-contract check**

Run:

```bash
rg -n 'BUILD LOG|SIDE QUEST|arcade|snake|blog-frontend|blog-backend|collab-server-mvp' README.md
```

Expected: the command currently finds obsolete portfolio and Snake content.

- [ ] **Step 2: Replace `README.md` with the exact identity content**

```markdown
<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f172a,55:155e75,100:0f766e&height=172&section=header&text=Raven9779&fontSize=58&fontColor=f8fafc&fontAlignY=35&animation=fadeIn" width="100%" alt="Raven9779" />

  <h1>Raven9779</h1>
  <p><strong>Full-stack engineer</strong></p>
  <p><sub>从界面到部署，把产品做成可运行的系统。</sub></p>
</div>

---

<p align="center">
  <img src="assets/system-trace.svg" width="100%" alt="System trace from interface through API to deployment, with status shipping." />
</p>

---

## FOCUS

Full-stack systems · Self-hosted products · Product engineering

## NOW

正在构建个人发布系统，把阅读、内容管理与 API 部署放进同一条可维护的链路。

## STACK

<p align="center">
  <img src="https://skillicons.dev/icons?i=nextjs,ts,nodejs,docker,nginx,aws&theme=dark&perline=6" alt="Next.js, TypeScript, Node.js, Docker, Nginx, and AWS" />
</p>
```

- [ ] **Step 3: Verify the rendered-content contract**

Run:

```bash
git diff --check
rg -n 'Raven9779|Full-stack engineer|FOCUS|NOW|STACK|system-trace.svg' README.md
! rg -n -i 'BUILD LOG|SIDE QUEST|arcade|snake|blog-frontend|blog-backend|collab-server-mvp' README.md
```

Expected: whitespace check succeeds, all required identity markers are present, and the exclusion search has no matches.

- [ ] **Step 4: Commit the README rewrite**

```bash
git add README.md
git commit -m "feat: refocus profile on technical identity"
```

### Task 3: Remove visible Snake artifacts and verify workflow state

**Files:**
- Delete: `output/snake.svg`
- Delete: `output/snake-dark.svg`
- Keep disabled: `.github/workflows/snake.yml`

**Interfaces:**
- Consumes: GitHub Actions workflow identifier `.github/workflows/snake.yml`.
- Produces: no Snake image in the repository's profile surface and no scheduled regeneration.

- [ ] **Step 1: Verify the workflow is disabled before deleting outputs**

Run:

```bash
gh api repos/Raven9779/Raven9779/actions/workflows/snake.yml --jq '.state'
```

Expected: `disabled_manually`. If the state is not disabled, run:

```bash
gh api --method PUT repos/Raven9779/Raven9779/actions/workflows/snake.yml/disable
```

- [ ] **Step 2: Delete only the generated Snake assets**

Delete `output/snake.svg` and `output/snake-dark.svg` using `apply_patch` delete-file patches. Keep the disabled workflow file unchanged because the active credential lacks the required `workflow` scope to edit it.

- [ ] **Step 3: Verify Snake is absent from the public source surface**

Run:

```bash
! rg -n -i 'snake' README.md output
gh api repos/Raven9779/Raven9779/actions/workflows/snake.yml --jq '.state'
```

Expected: no README/output match and workflow state remains `disabled_manually`.

- [ ] **Step 4: Commit the Snake cleanup**

```bash
git add output/snake.svg output/snake-dark.svg
git commit -m "chore: remove profile snake artifacts"
```

### Task 4: Publish and verify the profile PR

**Files:**
- Modify: repository metadata only through pull request and GitHub Actions API state.

**Interfaces:**
- Consumes: commits from Tasks 1 through 3.
- Produces: one merged profile implementation PR and a clean branch list.

- [ ] **Step 1: Publish the existing implementation branch**

```bash
git push -u origin agent/profile-technical-identity
gh pr create --base master --head agent/profile-technical-identity --title "feat: refocus profile on technical identity"
```

- [ ] **Step 2: Verify PR scope and mergeability**

```bash
gh pr view --json files,mergeStateStatus,mergeable,url
```

Expected: only `README.md`, `assets/system-trace.svg`, `assets/build-signal.svg`, and Snake output files appear; merge state is `CLEAN`.

- [ ] **Step 3: Merge and delete the feature branch**

```bash
gh pr merge --merge --delete-branch
git fetch --prune origin
git switch master
git pull --ff-only origin master
```

- [ ] **Step 4: Verify public delivery**

```bash
curl -I https://github.com/Raven9779
curl -I https://raw.githubusercontent.com/Raven9779/Raven9779/master/assets/system-trace.svg
gh api 'repos/Raven9779/Raven9779/branches?per_page=100' --jq '.[].name'
```

Expected: both URLs return HTTP 200 and the only remote branch is `master`.
