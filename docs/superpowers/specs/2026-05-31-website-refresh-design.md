# Kanna Website Refresh Design

## Goal

Refresh the static Kanna website so it reflects the current product: a desktop workflow system that gives coding agents more autonomy inside user-defined pipelines. Keep the existing GitHub release download flow with Apple Silicon and Intel DMG buttons.

## Positioning

Primary slogan:

> Structured autonomy for coding agents.

Hero support:

> Kanna lets Claude, Codex, and Copilot work through user-defined workflows: advancing stages, requesting revisions, spawning follow-up tasks, and coordinating through CLI or MCP while each task stays isolated in its own worktree.

The page should stop describing Kanna as a native macOS app or a Claude-only launcher. It should describe Kanna as a workflow layer for coding agents, while still making clear that the current download is a macOS desktop app.

## Visual Direction

Use the new app icon from `~/.kanna/repos/kanna/apps/desktop/src-tauri/icons/icon.svg` as the visual anchor. Replace the current dark purple synthwave palette with a lighter neutral UI:

- White/off-white background surfaces from the icon.
- Hot pink to orange accents for primary action and high-priority workflow paths.
- Magenta, violet, blue, muted slate, and green accents for staged workflow status.
- Less decorative grid/scanline treatment; more product/workflow-focused layout.

The logo should use the real icon asset or inline SVG derived from it instead of the older dark icon.

## Page Structure

1. Hero
   - Brand name: Kanna.
   - Product line: macOS desktop app for coding-agent workflows.
   - Slogan and supporting copy from this spec.
   - Keep Apple Silicon and Intel download buttons wired to the latest GitHub release assets.
   - Keep the version label populated from the latest release API when available.

2. Product Screenshot
   - Add a real screenshot area beneath or beside the hero.
   - Preferred source: capture the current desktop app with seeded/e2e data from `~/.kanna/repos/kanna`.
   - If a capture cannot be produced during implementation, use a clearly product-like static fallback built from real UI concepts, but do not leave the fake terminal-only demo as the main product visual.

3. What It Does
   Replace the existing three feature cards with copy based on current code:
   - User-defined pipelines: stages, post-actions, revisions, and follow-up tasks.
   - Provider-neutral agents: Claude, Codex, and Copilot in isolated worktrees.
   - Operator tools: terminal, diff, file preview, tree explorer, commit graph, mobile access, CLI, and MCP.

4. Workflow Demo
   Replace the current generic terminal bars with a workflow-stage visualization showing tasks moving through implement, commit, review, PR, and merge stages. This should reinforce structured autonomy rather than raw parallelism.

5. Footer
   Remove stale `v0.1.0` copy. Use a neutral footer such as `KANNA - STRUCTURED AUTONOMY FOR CODING AGENTS`, with the release version already shown near downloads.

## Behavior

The website remains a single static `index.html` with no build system. The GitHub Releases fetch remains client-side and points at `jemdiggity/kanna`. Download buttons should remain usable if the API succeeds and harmless if it fails.

The page should remain responsive on mobile and desktop. Text must not overlap or overflow; hero and screenshot layout should stack on small screens.

## Testing

Verification should include:

- Opening the static page in a browser or local static server.
- Checking desktop and mobile viewport screenshots.
- Confirming the release-fetch script still updates the download links and version label when GitHub responds.
- Confirming no stale phrases remain: `NATIVE MACOS APP`, `Parallel Flow`, `Claude Code sessions`, or the old footer version.
