# fire-releases

Sparkle appcast and update artifacts for the **Fire** maintained fork of [jordanbaird/Ice](https://github.com/jordanbaird/Ice).

This repository exists solely as a publication target — there is no source code here. The Fire app (built from [pdurlej/Ice](https://github.com/pdurlej/Ice)) reads `appcast.xml` from GitHub Pages at <https://pdurlej.github.io/fire-releases/appcast.xml> to discover new releases.

## How releases flow

1. A tag matching `v*-fire.*` is pushed to `pdurlej/Ice`.
2. The `build-dmg.yml` workflow on `pdurlej/Ice` builds an unsigned (ad-hoc-codesign) `Ice.app`, packages it into a DMG, publishes a GitHub Release on the source repo.
3. The same workflow signs the DMG with the Sparkle EdDSA private key (held as the `SPARKLE_PRIVATE_KEY` Actions secret), generates a new `<item>` block, and commits it to this repository.
4. Existing installs read the updated feed on their next check-for-updates cycle and offer the new build.

## Key custody

The Sparkle EdDSA public key is baked into every Fire `Ice.app` build via `Info.plist:SUPublicEDKey`. The matching private key never leaves the maintainer's Keychain and the GitHub Actions secret — it is not stored in either repository.

## Releases

See <https://github.com/pdurlej/Ice/releases> for download links and per-release notes. The appcast carries only the metadata Sparkle needs to advertise the update.
