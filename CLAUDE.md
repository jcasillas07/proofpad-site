# CLAUDE.md — proofpad.co

The Proofpad website: static HTML on GitHub Pages, no build step, wearing the app's design tokens. `README.md` is the technical source of truth for this repo — the two page shapes, the must-stay-live URLs (`/privacy`, `/privacy#delete`, `/support`, `/join`, `/.well-known/apple-app-site-association`), and the deploy (push to `main`, live in about thirty seconds). Read it before touching anything. The app repo is `~/Projects/proofpad`; its `docs/app-privacy.md` is the policy's rationale and the App Store privacy answer sheet, and its `CLAUDE.md` describes the site's place in the product.

## Built to sell — read first

Proofpad is built from day one to be **sold** (John Warrillow's principles; decided 2026-09-16). The binding rules are `~/Projects/proofpad/docs/built-to-sell.md` §3 and the app repo's CLAUDE.md rule 11; `~/Projects/proofpad/docs/operations.md` is the operator handbook. They bind this repo too. What touches the site: no founder identity on any page — the site speaks as Proofpad, the address is `support@proofpad.co`, never a person's; a process step (in the README or a page's comment) names **the operator**, never a person; every account behind the site (GitHub Pages, Namecheap DNS) is registered in `operations.md` §1 under the business, not a person; a terms-of-service page is on the pre-launch checklist (`built-to-sell.md` §6) and lands here beside `/privacy` when it is written. Keep the policy truthful to the data model — any change to what the server stores updates `/privacy` in the same session (the app repo's rule).

## Rules

1. **The human owns git.** Multiple chats share this working tree; `claude backup` commits and pushes it, and a push publishes the working tree to proofpad.co. Never stage, commit or push from a chat unless asked in that session; nothing destructive, ever.
2. **Explain before building.** State the plan in plain language and wait for approval for anything beyond a copy fix.
3. **Every user-visible string starts capitalized**, and the site's tokens come from the app's style doc (`~/Projects/proofpad/docs/proofpad-style.md`) — no invented colors or sizes.
4. **Verify after a push** that `/privacy#delete` and `/support` still load and that `curl -sI https://app-site-association.cdn-apple.com/a/v1/proofpad.co` still answers — Meta's Live mode, the App Store listing and the Bout invite link depend on them.
