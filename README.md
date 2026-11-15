# Horizon single-file launcher

`launch.svg` and `horizon-launch-sw.js` serve the single-file build of Horizon
from Google Drive through a service worker, adding the COOP/COEP headers the
game's Web Workers need.

Open the launcher **from jsDelivr**:

<https://cdn.jsdelivr.net/gh/retrograde-sdk/horizon-single@main/launch.svg>

The game is not in this repository. It is shared from Google Drive as
anyone-with-link, and the API key in `launch.svg` is restricted to the
`cdn.jsdelivr.net` referrer: opened from another host the key is refused, which
is why the jsDelivr URL above is the way in.

- file id: `1j18Zu67rB9h-VXdWtGym7yBFswT_tV7S`
- build: `8c6cef116639` (first 12 hex digits of the file's SHA-256)
- size: 355,343,333 bytes

The launcher downloads the file once, keeps it in that origin's IndexedDB, and
lets the worker serve it as `__horizon.html` with:

    Cross-Origin-Opener-Policy: same-origin
    Cross-Origin-Embedder-Policy: require-corp

Later visits go straight to LAUNCH; the bytes never leave the browser again.

`launch.svg?src=<url>` points at another copy of the single file for testing;
that copy must send CORS headers and allow range requests. Even then the page
has to be opened from a host the key allows, or from jsDelivr with `?src=` naming
a CORS-enabled copy.

Rebuilding: `python3 web/package.py single` in the Horizon tree, upload the
result to Drive as anyone-with-link, then update `DRIVE_ID` in `launch.svg` and
`BUILD` in `horizon-launch-sw.js` (both carry the build id, so a cached worker is
replaced rather than reused).

## Credits

The bundled **Visual Vibrance** shader pack is by Josh Britain (jbritain),
© 2025, and is redistributed unmodified for personal, non-commercial use under
its own source-available license (not OSI open source). See the `LICENSE` inside
`shaderpacks/Visual-Vibrance-v0.3.5a.zip` and `THIRD_PARTY_NOTICES.md` in the
source repository. <https://jbritain.net>
