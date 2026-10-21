# StoryForge Ghostty Surface

This repository is the private application surface used by StoryForge Studio.
It is a downstream fork of Ghostty, pinned from upstream tag `v1.3.1` at
commit `332b2aefc6e72d363aa93ab6ecfc86eeeeb5ed28`.

## Downstream contract

- The macOS bundle ID is `com.ryanpederson.storyforge.studio`, separate from
  the user's normal Ghostty installation, preferences, caches, and state.
- The app loads only `Contents/Resources/storyforge/config.ghostty` unless a
  development build explicitly supplies `GHOSTTY_CONFIG_PATH`.
- Every normal window and tab launches the bundled `story-tui studio` command.
- `story-daemon` and StoryForge's logging/data locations are injected into the
  child environment by the app.
- Ghostty's automatic and manual update paths are disabled. Engine upgrades are
  deliberate source updates in this fork.
- Terminal shell integration and AppleScript control are disabled in the
  shipped StoryForge profile so the user's Ghostty hooks cannot leak into the
  application.
- The local-release entitlement set removes Ghostty's broad terminal passthrough
  permissions for automation, microphone, camera, contacts, calendars,
  location, and photos.

The parent StoryForge repository owns packaging. Its
`scripts/bundle-studio-macos.sh` builds the Rust binaries, builds this fork,
copies the binaries and profile into the app bundle, and signs the result.

## Upgrading Ghostty

1. Add or fetch `https://github.com/ghostty-org/ghostty.git` as `upstream`.
2. Create a new downstream branch from a reviewed stable Ghostty tag.
3. Reapply and review the small StoryForge-specific diff described above.
4. Update the StoryForge submodule pointer only after the macOS bundle builds
   and the dedicated config/command launch is verified.

Ghostty remains MIT licensed. Keep upstream copyright and license files in all
source and binary distributions, and describe StoryForge as powered by Ghostty
rather than as an official Ghostty release.
