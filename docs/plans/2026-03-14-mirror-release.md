# Mirror Release Script Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Create a shell script that copies releases (tag + DMG assets) from the private `jemdiggity/kanna-private` repo to the public `jemdiggity/kanna` repo.

**Architecture:** Single bash script using `gh` CLI. Downloads assets to a temp dir, creates a matching release on the public repo, uploads assets, cleans up.

**Tech Stack:** Bash, `gh` CLI

---

### Task 1: Push pending public repo changes

The public repo worktree has an unpushed commit (design doc). Push it so the public repo is up to date.

**Files:**
- None (git operation only)

**Step 1: Push to origin**

```bash
cd /Users/jeremyhale/Documents/work/jemdiggity/kanna-public/.kanna-worktrees/setup-gh-repo-for-website-and-release-hosting-inde
git push
```

Expected: `main -> main` with both commits pushed.

---

### Task 2: Create mirror-release.sh in the private repo

**Files:**
- Create: `mirror-release.sh` (in the `kanna` / `kanna-private` local repo root)

**Step 1: Write the script**

Create `mirror-release.sh` at `/Users/jeremyhale/Documents/work/jemdiggity/kanna/mirror-release.sh`:

```bash
#!/bin/bash
set -e

PRIVATE_REPO="jemdiggity/kanna-private"
PUBLIC_REPO="jemdiggity/kanna"

TAG="${1:?Usage: ./mirror-release.sh <tag>}"

# Check the release exists on the private repo
echo "Fetching release ${TAG} from kanna-private..."
gh release view "$TAG" --repo "$PRIVATE_REPO" > /dev/null

# Check the release doesn't already exist on the public repo
if gh release view "$TAG" --repo "$PUBLIC_REPO" > /dev/null 2>&1; then
  echo "Error: release ${TAG} already exists on ${PUBLIC_REPO}" >&2
  exit 1
fi

# Download assets to a temp directory
TMPDIR="$(mktemp -d)"
trap 'rm -rf "$TMPDIR"' EXIT

echo "Downloading assets..."
gh release download "$TAG" --repo "$PRIVATE_REPO" --pattern "*.dmg" --dir "$TMPDIR"

# Create release on public repo and upload assets
echo "Creating release ${TAG} on kanna..."
gh release create "$TAG" "$TMPDIR"/*.dmg --repo "$PUBLIC_REPO" --title "$TAG"

echo "Done! https://github.com/${PUBLIC_REPO}/releases/tag/${TAG}"
```

**Step 2: Make it executable**

```bash
chmod +x mirror-release.sh
```

**Step 3: Commit**

```bash
git add mirror-release.sh
git commit -m "feat: add mirror-release.sh to copy releases to public repo"
```

---

### Task 3: Test the script with v0.1.0

**Step 1: Run the script**

```bash
cd /Users/jeremyhale/Documents/work/jemdiggity/kanna
./mirror-release.sh v0.1.0
```

Expected output:
```
Fetching release v0.1.0 from kanna-private...
Downloading assets...
Creating release v0.1.0 on kanna...
Done! https://github.com/jemdiggity/kanna/releases/tag/v0.1.0
```

**Step 2: Verify the public release**

```bash
gh release view v0.1.0 --repo jemdiggity/kanna
```

Expected: Release exists with `Kanna-0.1.0-arm64.dmg` and `Kanna-0.1.0-x86_64.dmg` assets.

**Step 3: Verify the website download buttons**

Visit `https://jemdiggity.github.io/kanna/` and confirm the download buttons link to the correct DMG files. The version label should show `v0.1.0 · macOS 14+`.

---

### Task 4: Verify error handling

**Step 1: Test duplicate release error**

```bash
./mirror-release.sh v0.1.0
```

Expected: `Error: release v0.1.0 already exists on jemdiggity/kanna` and exit code 1.

**Step 2: Test missing tag error**

```bash
./mirror-release.sh v99.99.99
```

Expected: `gh release view` fails and script exits due to `set -e`.

**Step 3: Test no argument error**

```bash
./mirror-release.sh
```

Expected: `Usage: ./mirror-release.sh <tag>` error message.
