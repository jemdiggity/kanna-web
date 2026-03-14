# Mirror Release Script Design

## Problem

Releases with DMG assets are published on the private `jemdiggity/kanna-private` repo. The public website at `jemdiggity.github.io/kanna` fetches download links from `jemdiggity/kanna` releases via the GitHub API. We need a way to copy releases from private to public.

## Solution

A shell script `mirror-release.sh` in the `kanna-private` repo root that uses `gh` CLI to copy a release's assets from `kanna-private` to `kanna`.

## Usage

```
./mirror-release.sh v0.1.0
```

## Flow

1. Accept a tag argument (required)
2. Verify the tag exists on `kanna-private` via `gh release view`
3. Create a temp directory, download all `.dmg` assets via `gh release download`
4. Create a release on `jemdiggity/kanna` with the same tag, no release notes, upload assets
5. Clean up temp directory
6. Print the public release URL

## Constraints

- Pure `gh` CLI, no other dependencies
- `set -e` for fail-fast behavior
- Print status messages at each step for feedback
- Error out if the public release already exists (don't overwrite)
- Minimal release on public side: tag + assets only, no release notes body

## Output Messages

- `Fetching release <tag> from kanna-private...`
- `Downloading assets...`
- `Creating release <tag> on kanna...`
- `Done! https://github.com/jemdiggity/kanna/releases/tag/<tag>`
