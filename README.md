# proofpad.co

The Proofpad website: a landing page and the privacy policy. Static HTML on GitHub Pages;
the app's design tokens, no build step. Push to `main` and it is live in ~30 seconds.

## Two page shapes, one stylesheet

`style.css` serves both, and the difference matters:

- **`index.html` carries `class="home"`** — ONE fixed viewport, centered, and it must
  **never scroll**. That is deliberate, not a bug: `body.home` is `height:100svh` with
  `overflow:hidden`, and every size in that block is a `clamp()` so the column compresses
  on a short screen instead of overflowing. Nothing on the page has a fixed aspect ratio,
  which is what makes "never scrolls" structural rather than tuned. Don't delete the
  `overflow:hidden`, and don't move any `.home` rule onto bare `body` — the privacy policy
  is long and depends on scrolling.
- **`privacy.html`** is a normal document page and scrolls. `/privacy` and
  `/privacy#delete` must both stay reachable: Meta requires those URLs for the app to
  remain Live.

`svh`, not `vh`: `100vh` resolves against the toolbar-retracted viewport, so on a phone the
page loads taller than the screen and clips. `100svh` is the smallest (toolbar-showing)
viewport — the only height that can be guaranteed. `100vh` is declared first as a fallback.

## Rules the landing page follows

- **No Birch.** The page's one action is the App Store, and Apple requires their own badge
  artwork for it, so that action wears Apple's color. The app's accent never appears here.
- **No Apple logo, no download badge** until the app is actually live. "for iPhone" and
  "Coming soon to the App Store" are Apple's sanctioned pre-release wording. On launch day
  Apple's official **white** App Store badge SVG replaces the `.slot` line and nothing else
  on the page moves.
- **No shadows, no gradients, no glow** — the app's Hard Rule 4. Depth is the Steel→Plate
  surface step and Seam hairlines.
- The drifting board behind the copy is the app's own sign-in gate, on the web. The rows are
  staged theater, the same rows the gate uses — decoration, `aria-hidden`, and it holds
  still under `prefers-reduced-motion`.

## Verifying a change to the landing page

No-scroll is the acceptance test. Serve locally and measure, don't eyeball:

```sh
python3 -m http.server 8899          # file:// breaks the absolute /style.css path
```

Then check `main.scrollHeight` against the viewport at 375×667, 390×844, 430×932, 844×390,
667×375, 768×1024, 1280×800 and 1920×1080. Any overflow is a failure — `overflow:hidden`
means overflowing content is *clipped and unreachable*, not scrolled to.

Headless Chrome screenshots of this page crop oddly; trust DOM measurements, and do the
visual check in real Safari (the iOS Simulator works: `xcrun simctl openurl <id> http://localhost:8899/`).

## The invite page and the app link (2026-08-26, Bouts)

- **`join.html`** serves `https://proofpad.co/join?b=<token>` — a Bout invite as anyone holding
  the link sees it. The site's first JavaScript: it calls the app's one anonymous RPC
  (`bout_invite_preview`) with the publishable key and renders the terms and the members'
  display names. [Open in Proofpad] is the `proofpad://join?b=` scheme link; [Get Proofpad]
  copies the invite URL to the clipboard (the app reads it once on a fresh install) and shows
  the coming-soon line — on launch day that button opens the App Store after copying.
- **`.well-known/apple-app-site-association`** makes the same URL a universal link (the app
  opens straight from Messages). Team-scoped: re-issue at the org switch. GitHub Pages serves
  it as `application/octet-stream`; verify after a push with
  `curl -sI https://app-site-association.cdn-apple.com/a/v1/proofpad.co` — a 200 means Apple's
  CDN accepted it. If it ever refuses the content-type, a header-setting host in front is the fix.
