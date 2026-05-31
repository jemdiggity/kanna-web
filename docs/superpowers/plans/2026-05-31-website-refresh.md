# Kanna Website Refresh Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Auto-select `superpowers:subagent-driven-development` or `superpowers:executing-plans` based on task coupling, subagent availability, and whether execution should stay in the current session. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Refresh the static Kanna website so it matches the new icon, current product features, and “Structured autonomy for coding agents” positioning.

**Architecture:** Keep the site as one static `index.html` with embedded CSS, inline SVG/icon markup, and the existing client-side GitHub Releases fetch. Replace the dark terminal-only hero with a lighter product/workflow page: hero, product screenshot-style panel, updated feature cards, workflow-stage visualization, and clean footer.

**Tech Stack:** Static HTML, CSS, vanilla JavaScript, GitHub Releases API, local browser verification.

---

## File Structure

- Modify: `index.html`
  - Owns the full static page: styles, content, product visualization, download buttons, release-fetch script, and footer.
- No new runtime dependencies.
- No build system.
- No image asset is required for the first pass. If a real app capture can be produced quickly from `~/.kanna/repos/kanna`, save it under `assets/kanna-screenshot.png` and reference it from `index.html`; otherwise use the planned CSS/HTML product screenshot fallback based on real current UI concepts.

---

### Task 1: Confirm Baseline And Guardrails

**Files:**
- Read: `index.html`
- Read: `/Users/jeremyhale/.kanna/repos/kanna/apps/desktop/src-tauri/icons/icon.svg`

- [ ] **Step 1: Confirm stale phrases are currently present**

Run:

```bash
rg -n "NATIVE MACOS APP|Parallel Flow|Claude Code sessions|NATIVE MACOS|KANNA · PARALLEL FLOW · v0\\.1\\.0" index.html
```

Expected: matches in `index.html` for the old title, hero eyebrow, old feature copy, and old footer.

- [ ] **Step 2: Confirm release button IDs and GitHub repo value**

Run:

```bash
rg -n "btn-arm|btn-intel|version-label|jemdiggity/kanna|releases/latest" index.html
```

Expected: matches showing `btn-arm`, `btn-intel`, `version-label`, `jemdiggity/kanna`, and `releases/latest`.

- [ ] **Step 3: Confirm icon palette source**

Run:

```bash
rg -n "#f72b89|#ff8a12|#df1fb7|#9630dc|#1957c3|#1458c5|#17d34c" /Users/jeremyhale/.kanna/repos/kanna/apps/desktop/src-tauri/icons/icon.svg
```

Expected: matches for the new icon colors.

---

### Task 2: Replace Hero, Palette, And Icon

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Update document title and color variables**

Use the new title:

```html
<title>Kanna — Structured Autonomy for Coding Agents</title>
```

Use this palette in `:root`:

```css
:root {
  --bg: #fbfaf8;
  --surface: #ffffff;
  --surface-soft: #f4f5f7;
  --ink: #191923;
  --muted: #676978;
  --line: #dedfe5;
  --hot-pink: #f72b89;
  --orange: #ff8a12;
  --magenta: #df1fb7;
  --violet: #9630dc;
  --indigo: #1957c3;
  --blue: #1458c5;
  --slate: #736b91;
  --green: #17d34c;
  --shadow: 0 24px 70px rgba(25, 25, 35, 0.12);
}
```

- [ ] **Step 2: Replace the old dark hero with a responsive product hero**

The hero content must include this copy:

```html
<p class="eyebrow">MACOS DESKTOP APP FOR CODING-AGENT WORKFLOWS</p>
<h1>Kanna</h1>
<p class="slogan">Structured autonomy for coding agents.</p>
<p class="hero-copy">
  Kanna lets Claude, Codex, and Copilot work through user-defined workflows:
  advancing stages, requesting revisions, spawning follow-up tasks, and
  coordinating through CLI or MCP while each task stays isolated in its own worktree.
</p>
```

Keep both download anchors and their IDs:

```html
<a href="#" class="download-btn primary" id="btn-arm">Apple Silicon</a>
<a href="#" class="download-btn secondary" id="btn-intel">Intel</a>
<p class="version" id="version-label">macOS 14+</p>
```

- [ ] **Step 3: Replace the old inline icon with the real app icon SVG shape and colors**

Use the current icon markup from `/Users/jeremyhale/.kanna/repos/kanna/apps/desktop/src-tauri/icons/icon.svg`, preserving the 512 viewBox and these gradient IDs with page-safe names:

```html
<svg class="app-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" role="img" aria-label="Kanna app icon">
  <defs>
    <linearGradient id="icon-background-shadow" x1="256" y1="47" x2="256" y2="466" gradientUnits="userSpaceOnUse">
      <stop offset="0.62" stop-color="#ffffff"/>
      <stop offset="1" stop-color="#f3f4f6"/>
    </linearGradient>
    <linearGradient id="icon-hot-gradient" x1="113" y1="99" x2="354" y2="99" gradientUnits="userSpaceOnUse">
      <stop stop-color="#f72b89"/>
      <stop offset="1" stop-color="#ff8a12"/>
    </linearGradient>
    <linearGradient id="icon-pink-gradient" x1="113" y1="178" x2="293" y2="178" gradientUnits="userSpaceOnUse">
      <stop stop-color="#df1fb7"/>
      <stop offset="1" stop-color="#ef2d9b"/>
    </linearGradient>
    <linearGradient id="icon-purple-gradient" x1="113" y1="256" x2="226" y2="256" gradientUnits="userSpaceOnUse">
      <stop stop-color="#9630dc"/>
      <stop offset="1" stop-color="#9e23dd"/>
    </linearGradient>
    <linearGradient id="icon-indigo-gradient" x1="113" y1="335" x2="293" y2="335" gradientUnits="userSpaceOnUse">
      <stop stop-color="#1957c3"/>
      <stop offset="1" stop-color="#6940a4"/>
    </linearGradient>
    <linearGradient id="icon-blue-violet-gradient" x1="113" y1="414" x2="360" y2="414" gradientUnits="userSpaceOnUse">
      <stop stop-color="#1458c5"/>
      <stop offset="1" stop-color="#736b91"/>
    </linearGradient>
  </defs>
  <rect width="512" height="512" fill="none"/>
  <rect x="46" y="46" width="420" height="420" rx="105" fill="#ffffff"/>
  <rect x="46" y="46" width="420" height="420" rx="105" fill="url(#icon-background-shadow)"/>
  <rect x="46.5" y="46.5" width="419" height="419" rx="104.5" fill="none" stroke="#d6d6d6"/>
  <rect x="113" y="82" width="45" height="34" rx="17" fill="url(#icon-hot-gradient)"/>
  <rect x="180" y="82" width="174" height="34" rx="17" fill="url(#icon-hot-gradient)"/>
  <rect x="377" y="82" width="24" height="34" rx="12" fill="#17d34c"/>
  <rect x="113" y="161" width="45" height="34" rx="17" fill="url(#icon-pink-gradient)"/>
  <rect x="180" y="161" width="113" height="34" rx="17" fill="url(#icon-pink-gradient)"/>
  <rect x="113" y="240" width="45" height="33" rx="16.5" fill="url(#icon-purple-gradient)"/>
  <rect x="180" y="240" width="46" height="33" rx="16.5" fill="url(#icon-purple-gradient)"/>
  <rect x="113" y="318" width="45" height="33" rx="16.5" fill="url(#icon-indigo-gradient)"/>
  <rect x="180" y="318" width="47" height="33" rx="16.5" fill="url(#icon-indigo-gradient)"/>
  <rect x="245" y="318" width="48" height="33" rx="16.5" fill="url(#icon-indigo-gradient)"/>
  <rect x="113" y="397" width="45" height="33" rx="16.5" fill="url(#icon-blue-violet-gradient)"/>
  <rect x="180" y="397" width="47" height="33" rx="16.5" fill="url(#icon-blue-violet-gradient)"/>
  <rect x="245" y="397" width="48" height="33" rx="16.5" fill="url(#icon-blue-violet-gradient)"/>
  <rect x="313" y="397" width="47" height="33" rx="16.5" fill="url(#icon-blue-violet-gradient)"/>
</svg>
```

- [ ] **Step 4: Run hero copy checks**

Run:

```bash
rg -n "Structured autonomy for coding agents|user-defined workflows|Claude, Codex, and Copilot|CLI or MCP|btn-arm|btn-intel|version-label" index.html
```

Expected: all new hero copy plus preserved release element IDs are present.

---

### Task 3: Add Product Screenshot Area And Current Feature Cards

**Files:**
- Modify: `index.html`
- Optionally create: `assets/kanna-screenshot.png` only if a real capture is successfully produced from the app repo.

- [ ] **Step 1: Look for an existing real screenshot asset**

Run:

```bash
find /Users/jeremyhale/.kanna/repos/kanna -iname "*screenshot*" -o -iname "*screen*.png" -o -iname "*.webp" | head -20
```

Expected: If an obvious current Kanna UI screenshot exists, use it. If not, continue with the static product screenshot fallback below.

- [ ] **Step 2: Add product screenshot fallback markup**

Place this section after the hero if no real screenshot asset is available:

```html
<section class="product-section" aria-label="Kanna workflow workspace preview">
  <div class="product-shot">
    <div class="shot-sidebar">
      <div class="repo-name">kanna-web-3</div>
      <div class="task active">Website refresh</div>
      <div class="task">MCP stage controls</div>
      <div class="task">Mobile handoff</div>
    </div>
    <div class="shot-main">
      <div class="shot-toolbar">
        <span>Website refresh</span>
        <span class="agent-pill">Codex</span>
      </div>
      <div class="stage-strip">
        <span class="stage done">implement</span>
        <span class="stage active">review</span>
        <span class="stage">commit</span>
        <span class="stage">pr</span>
        <span class="stage">merge</span>
      </div>
      <div class="terminal-preview">
        <p><span>$</span> kanna-cli stage-complete --task website-refresh --stage implement</p>
        <p><span>$</span> kanna mcp create-task "Follow-up copy pass"</p>
        <p class="success">revision requested · review agent spawned</p>
      </div>
    </div>
    <div class="shot-inspector">
      <div class="inspector-card">
        <strong>Pipeline</strong>
        <span>User-defined stages with post-actions</span>
      </div>
      <div class="inspector-card">
        <strong>Worktree</strong>
        <span>Isolated branch workspace</span>
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 3: Replace the feature cards with current product explanations**

Use these feature card headings and copy:

```html
<article class="feature">
  <div class="feature-mark hot"></div>
  <h3>User-defined pipelines</h3>
  <p>Model the way work should move: stages, post-actions, revisions, and follow-up task spawning are defined in pipeline JSON instead of hard-coded into a single happy path.</p>
</article>

<article class="feature">
  <div class="feature-mark violet"></div>
  <h3>Provider-neutral agents</h3>
  <p>Run Claude, Codex, or Copilot against isolated git worktrees. Each task keeps its own branch, terminal session, and stage history.</p>
</article>

<article class="feature">
  <div class="feature-mark blue"></div>
  <h3>Operator tools</h3>
  <p>Work from the desktop app, CLI, MCP, or mobile view with terminal access, diffs, file preview, tree explorer, commit graph, and task handoff.</p>
</article>
```

- [ ] **Step 4: Verify the updated feature copy**

Run:

```bash
rg -n "User-defined pipelines|Provider-neutral agents|Operator tools|Claude, Codex, or Copilot|MCP|mobile view" index.html
```

Expected: matches for all three updated feature cards.

---

### Task 4: Replace Parallel Terminal Demo With Workflow Stage Visualization

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Replace the old `demo` terminal section**

Use a workflow visualization that shows structured stage progress:

```html
<section class="workflow-section">
  <div class="section-heading">
    <p class="section-label">// WORKFLOW AUTONOMY</p>
    <h2>Agents move through the workflow, not around it.</h2>
  </div>
  <div class="workflow-board">
    <div class="workflow-card current">
      <span class="workflow-step">01</span>
      <h3>Implement</h3>
      <p>Agent works inside a task worktree and reports progress through Kanna.</p>
    </div>
    <div class="workflow-card current">
      <span class="workflow-step">02</span>
      <h3>Review</h3>
      <p>Review can approve, request revision, or knock the task back to an earlier stage.</p>
    </div>
    <div class="workflow-card">
      <span class="workflow-step">03</span>
      <h3>Commit</h3>
      <p>Configured post-actions can prepare commits, summaries, or handoff notes.</p>
    </div>
    <div class="workflow-card">
      <span class="workflow-step">04</span>
      <h3>PR</h3>
      <p>CLI and MCP controls let agents advance stages or spawn follow-up tasks.</p>
    </div>
    <div class="workflow-card">
      <span class="workflow-step">05</span>
      <h3>Merge</h3>
      <p>The workflow stays explicit from first prompt to final integration.</p>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Remove old terminal demo CSS**

Delete CSS selectors used only by the old terminal visualization:

```text
.terminal
.terminal-bar
.terminal-title
.terminal-body
.t-row
.t-label
.t-track
.t-fill
.t-status
.cursor-blink
@keyframes grow
@keyframes blink
```

- [ ] **Step 3: Verify old demo language is gone**

Run:

```bash
rg -n "PARALLEL FLOW|parallel sessions|agent_1|agent_2|agent_3|agent_4|running|queued" index.html
```

Expected: no matches.

---

### Task 5: Preserve Release Fetch, Footer, Responsive Layout, And Verify

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Keep the release script behavior**

The script should still fetch the same public latest-release endpoint and update the same elements:

```html
<script>
  const REPO = 'jemdiggity/kanna';

  fetch(`https://api.github.com/repos/${REPO}/releases/latest`)
    .then(r => r.json())
    .then(release => {
      const assets = release.assets || [];

      const arm = assets.find(a =>
        a.name.match(/arm64|apple.?silicon/i) && a.name.endsWith('.dmg')
      );
      const intel = assets.find(a =>
        a.name.match(/x86|intel/i) && a.name.endsWith('.dmg')
      );

      if (arm) document.getElementById('btn-arm').href = arm.browser_download_url;
      if (intel) document.getElementById('btn-intel').href = intel.browser_download_url;

      if (release.tag_name) {
        document.getElementById('version-label').textContent =
          `${release.tag_name} · macOS 14+`;
      }
    })
    .catch(() => {
      // Keep default links if the release API is unavailable.
    });
</script>
```

- [ ] **Step 2: Update footer**

Use:

```html
<footer>
  KANNA - STRUCTURED AUTONOMY FOR CODING AGENTS
</footer>
```

- [ ] **Step 3: Run stale-copy verification**

Run:

```bash
rg -n "NATIVE MACOS APP|Parallel Flow|Claude Code sessions|NATIVE MACOS|KANNA · PARALLEL FLOW · v0\\.1\\.0" index.html
```

Expected: no matches.

- [ ] **Step 4: Run static file syntax smoke check**

Run:

```bash
python3 - <<'PY'
from html.parser import HTMLParser
from pathlib import Path

class Parser(HTMLParser):
    pass

html = Path("index.html").read_text()
Parser().feed(html)
for required in [
    "Structured autonomy for coding agents.",
    "user-defined workflows",
    "btn-arm",
    "btn-intel",
    "version-label",
    "jemdiggity/kanna",
]:
    assert required in html, required
print("html smoke check passed")
PY
```

Expected:

```text
html smoke check passed
```

- [ ] **Step 5: Serve the page locally**

Run:

```bash
python3 -m http.server 4173
```

Expected: server prints `Serving HTTP on :: port 4173` or equivalent. Open `http://localhost:4173/` for browser verification.

- [ ] **Step 6: Browser verify desktop and mobile**

Use Playwright or a browser to check:

```text
Desktop viewport: 1440x1000
Mobile viewport: 390x844
```

Expected:

```text
No text overlap.
No horizontal overflow.
Hero and product screenshot stack on mobile.
Download buttons are visible.
Footer no longer contains v0.1.0.
```

- [ ] **Step 7: Commit website refresh**

Run:

```bash
git add index.html
git commit -m "Refresh website positioning and visual design"
```

Expected: commit includes `index.html` only unless a real screenshot asset was successfully added, in which case include `assets/kanna-screenshot.png` too.
