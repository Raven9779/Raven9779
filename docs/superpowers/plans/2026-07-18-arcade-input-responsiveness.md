# Arcade Input and Responsiveness Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Make the public Arcade shooter fully playable on narrow touch screens and desktop input without changing its one-file static deployment model.

**Architecture:** Keep the 600 by 500 internal canvas coordinate system so existing game physics remain stable. Scale the canvas and HUD through CSS, then unify keyboard, pointer, and mobile-button handling around the existing `keys` map and `shoot()` function. Add a Node built-in source-contract test that prevents regressions in responsive CSS, pointer listeners, and restart cancellation.

**Tech Stack:** Static HTML, Canvas 2D, browser Pointer Events, Node.js built-in test runner, GitHub Pages.

## Global Constraints

- Modify only `index.html` and one source-contract test in `tests/`.
- Keep the game dependency-free and deployable from the `gh-pages` branch.
- Preserve the current 600 by 500 canvas drawing coordinates and visual palette.
- Support keyboard arrows, A/D, Space, pointer/tap shooting, and touch controls.
- Do not globally prevent touch scrolling; scope input prevention to the game canvas and controls.
- Publish through a PR against the `gh-pages` branch and verify the Pages URL after build completion.

---

### Task 1: Add an executable source-contract test

**Files:**
- Create: `tests/arcade-source.test.mjs`
- Test: `tests/arcade-source.test.mjs`

**Interfaces:**
- Consumes: inline script in `index.html`.
- Produces: a Node test that proves input listeners are wired and a repeated start cancels the old animation frame.

- [ ] **Step 1: Create the implementation branch from the Pages source**

```bash
git switch gh-pages
git pull --ff-only origin gh-pages
git switch -c agent/arcade-responsive-input
```

- [ ] **Step 2: Create the failing test file**

```js
import assert from 'node:assert/strict';
import test from 'node:test';
import { readFile } from 'node:fs/promises';

const html = await readFile(new URL('../index.html', import.meta.url), 'utf8');

test('the game uses responsive and pointer-safe input contracts', () => {
  assert.match(html, /canvas\s*\{[^}]*width:\s*100%/s);
  assert.match(html, /canvas\.addEventListener\('pointerdown'/);
  assert.match(html, /pointercancel/);
  assert.doesNotMatch(html, /document\.addEventListener\('touchmove'/);
});

test('restarting cancels the previous animation frame', () => {
  assert.match(html, /function startGame\(\)\s*\{\s*cancelAnimationFrame\(animationId\);/s);
});
```

- [ ] **Step 3: Run the test to verify it fails**

Run:

```bash
node --test tests/arcade-source.test.mjs
```

Expected: FAIL because responsive canvas CSS, pointer listeners, and restart cancellation are absent.

- [ ] **Step 4: Commit the failing test**

```bash
git add tests/arcade-source.test.mjs
git commit -m "test: define arcade input contracts"
```

### Task 2: Make the game canvas responsive without changing physics

**Files:**
- Modify: `index.html`

**Interfaces:**
- Consumes: existing `canvas` intrinsic width `600` and height `500`.
- Produces: CSS scaling for the game container, canvas, HUD, and mobile layout.

- [ ] **Step 1: Add the responsive CSS rules**

In `index.html`, replace the fixed layout declarations with these constraints:

```css
body {
    min-height: 100dvh;
    overflow-x: hidden;
    overflow-y: auto;
    padding: 24px 16px;
}

#game-container,
#hud {
    width: min(600px, calc(100vw - 32px));
}

#game-container { aspect-ratio: 6 / 5; }

canvas {
    width: 100%;
    height: auto;
    touch-action: none;
}

#mobile-controls { touch-action: none; }

@media (max-width: 650px) {
    h1 { font-size: 1.8rem; text-align: center; }
    .subtitle { font-size: 0.7rem; letter-spacing: 2px; text-align: center; }
    .hud-item { font-size: 0.8rem; }
    #mobile-controls { display: flex; }
}
```

Keep the canvas attributes `width="600" height="500"`; they are the game-coordinate contract.

- [ ] **Step 2: Verify the responsive CSS contract**

Run:

```bash
node --test tests/arcade-source.test.mjs
rg -n 'min\(600px, calc\(100vw - 32px\)\)|aspect-ratio: 6 / 5|touch-action: none' index.html
```

Expected: the test remains failing only for pointer/restart contracts; the CSS markers are present.

- [ ] **Step 3: Commit the responsive layout change**

```bash
git add index.html
git commit -m "fix: scale arcade canvas for narrow screens"
```

### Task 3: Unify keyboard, pointer, and mobile controls

**Files:**
- Modify: `index.html`

**Interfaces:**
- Consumes: existing `keys` object, `shoot()`, `gameRunning`, and `animationId` variables.
- Produces: `bindHoldControl(element, key)`, pointer/tap shooting, and safe start/game-over transitions.

- [ ] **Step 1: Replace the existing event-listener block with the unified handlers**

Replace all listeners beginning at `document.addEventListener('keydown'` through the global `touchmove` listener with:

```js
function preventGameScroll(event) {
    if (['ArrowLeft', 'ArrowRight', 'Space'].includes(event.code)) event.preventDefault();
}

function bindHoldControl(element, key) {
    const release = event => {
        event.preventDefault();
        keys[key] = false;
    };

    element.addEventListener('pointerdown', event => {
        event.preventDefault();
        element.setPointerCapture?.(event.pointerId);
        keys[key] = true;
    });
    element.addEventListener('pointerup', release);
    element.addEventListener('pointercancel', release);
    element.addEventListener('lostpointercapture', release);
}

document.addEventListener('keydown', event => {
    preventGameScroll(event);
    keys[event.code] = true;
    if (event.code === 'Space' && gameRunning) shoot();
});
document.addEventListener('keyup', event => {
    preventGameScroll(event);
    keys[event.code] = false;
});

canvas.addEventListener('pointerdown', event => {
    event.preventDefault();
    if (gameRunning) shoot();
});

bindHoldControl(document.getElementById('left-btn'), 'ArrowLeft');
bindHoldControl(document.getElementById('right-btn'), 'ArrowRight');
document.getElementById('fire-btn').addEventListener('pointerdown', event => {
    event.preventDefault();
    if (gameRunning) shoot();
});
```

At the first line of `startGame()`, add `cancelAnimationFrame(animationId);`. At the first line of `gameOver()`, add `if (!gameRunning) return;`. In `initGame()`, add `keys = {};` immediately after `player = createPlayer();`.

- [ ] **Step 2: Run the source-contract test**

Run:

```bash
node --test tests/arcade-source.test.mjs
```

Expected: PASS with two passing tests.

- [ ] **Step 3: Parse the inline game script**

Run:

```bash
node --input-type=module -e "import { readFileSync } from 'node:fs'; const html = readFileSync('index.html', 'utf8'); const script = html.match(/<script>([\s\S]*?)<\\/script>/)[1]; new Function(script); console.log('inline game script parses');"
```

Expected: `inline game script parses`.

- [ ] **Step 4: Commit the input and lifecycle fix**

```bash
git add index.html tests/arcade-source.test.mjs
git commit -m "fix: unify arcade touch and keyboard controls"
```

### Task 4: Publish and verify the Arcade PR

**Files:**
- Modify: repository metadata only through a pull request.

**Interfaces:**
- Consumes: commits from Tasks 1 through 3.
- Produces: a merged `gh-pages` change with mobile-safe game controls.

- [ ] **Step 1: Publish the existing branch**

```bash
git push -u origin agent/arcade-responsive-input
gh pr create --base gh-pages --head agent/arcade-responsive-input --title "fix: make arcade playable on touch screens"
```

- [ ] **Step 2: Verify PR scope**

```bash
gh pr view --json files,mergeStateStatus,mergeable,url
```

Expected: only `index.html` and `tests/arcade-source.test.mjs` are present and the PR is mergeable.

- [ ] **Step 3: Merge and verify Pages output**

```bash
gh pr merge --merge --delete-branch
curl -I https://raven9779.github.io/arcade/
```

Expected: the Pages URL returns HTTP 200.

- [ ] **Step 4: Manually validate real player paths**

At 390px viewport width: Start Game, hold left/right, tap the canvas to fire, tap Fire, lose all lives, and use Play Again. On desktop: repeat with ArrowLeft/ArrowRight, A/D, and Space. Verify no horizontal page overflow, no stuck movement after pointer cancellation, and no duplicate animation loop after Play Again.
