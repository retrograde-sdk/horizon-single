# Horizon single-file launcher

`launch.svg` serves the single-file build of Horizon from this site, with the
COOP/COEP headers the game's Web Workers need (via `horizon-launch-sw.js`).

Open the launcher:

<https://retrograde-sdk.github.io/horizon-single/launch.svg>

It downloads the game once, keeps it in this origin's IndexedDB, and serves it
from a service worker as `__horizon.html`. Later visits go straight to LAUNCH.

## The game's parts

GitHub Pages caps a file at 100 MB, and the single file is ~339 MB, so it is
published in four parts under `pkg/` (`horizon-part00.bin` … `horizon-part03.bin`)
with sizes and SHA-256 digests in `pkg/parts.json`. The launcher reads the
manifest, downloads each part with the retries and Range resume it used to
apply to one file, and stores the bytes concatenated: the game it serves is
byte-identical to the build these parts were cut from.

- build: `8c6cef116639` (first 12 hex digits of the file's SHA-256)
- size: 355,343,333 bytes
- parts: 4 × 90,000,000 + 85,343,333

Rebuilding: `python3 web/package.py single` in the Horizon tree, then re-cut the
parts above and rewrite `pkg/parts.json` (the sha256 of each part and the build
id of the whole file). `horizon-launch-sw.js`'s `BUILD` and `launch.svg`'s
`SW_BUILD` must both carry the new build id, or browsers keep serving the
cached worker.

## Other copies

`launch.svg?src=<url>` points at a single-file copy somewhere else; that copy
must send CORS headers and allow range requests. The earlier Google Drive copy
is still readable through the jsDelivr route, whose API key is restricted to
that referrer.

## Credits

The bundled **Visual Vibrance** shader pack is by Josh Britain (jbritain),
© 2025, and is redistributed unmodified for personal, non-commercial use under
its own source-available license (not OSI open source). See the `LICENSE` inside
`shaderpacks/Visual-Vibrance-v0.3.5a.zip` and `THIRD_PARTY_NOTICES.md` in the
source repository. <https://jbritain.net>
