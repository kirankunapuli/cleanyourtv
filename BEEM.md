# Beem launcher fork

The default branch, `beem-customizations`, holds the Portronics Beem 470 launcher changes on top
of upstream CleanYourTV. `main` follows upstream; the device-specific work stays on
this branch so it can be updated and backed up without sending a launcher restyle
to the upstream project.

The fork also includes the lossless image optimizations from Imgbot PR #1.
Custom launcher artwork uses WebP to keep the release APK below the existing
400 KiB limit. CI runs on every push to this branch.

## Included changes

- Beem firmware Settings component and app visibility defaults.
- Clock seconds, date below the clock, Wi-Fi signal fan, and compact VPN badge.
- Long-press Settings → Appearance for ambient glow colors and bundled wallpaper.
- Long-press an app tile to pin/unpin it or open App info.
- Custom Netflix and SmartTube artwork, with a bundled background.
- Upstream app library, reorder controls, optional stats, and theme support.

The launcher has no weather requests or hardcoded user location. Session transcripts,
device dumps, ADB keys, and local machine settings stay outside Git.

## Build and check

Use JDK 17, Android SDK platform 34, and build-tools 34.0.0:

```sh
python3 -m unittest discover -s tests -v
cd launcher
./gradlew --no-daemon testReleaseUnitTest lintRelease assembleRelease
```

The APK is written to `launcher/app/build/outputs/apk/release/app-release.apk`.
Building does not install it or change the projector. Runtime behavior after an
upstream integration still needs verification on the device before installation.

## Git remotes

- `origin`: `kirankunapuli/cleanyourtv`, the personal fork and backup destination.
- `upstream`: `Gathin23/cleanyourtv`, the shared project.

Keep upstream contributions on separate branches based on `upstream/main`.
Commit launcher changes on `beem-customizations` and push with:

```sh
git push origin beem-customizations
```

Merged one-off contribution branches can be removed after their commits are reachable
from upstream. Keep a branch while its pull request is open.

Private session notes, transcripts, and original artwork are archived locally under
ignored `backup/local-only/`. They are not part of the public source. Restored launcher
work is committed on this branch, so ongoing development does not depend on a stash.
