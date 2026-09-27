# Cyclamen Studio — marketing site

Six static pages, no build step, no JavaScript. `style.css` is shared.

| Page | Purpose |
|------|---------|
| `index.html` | Studio landing page — the root of cyclamen-games.com |
| `flat-peaks.html` | The Flat Peaks game page |
| `privacy.html` | **Required by Google Play.** This is the URL that goes in Play Console → App content → Privacy policy |
| `terms.html` | Terms of use |
| `licenses.html` | Open-source attributions for the libraries Flat Peaks ships with (Phaser, Capacitor, Sentry SDK) |
| `support.html` | Support contact + FAQ. The email here must match the Play Console support email |

The game itself links to `privacy.html`, `terms.html`, and `licenses.html` from
its in-app Settings screen (see `SettingsModal` in the
[FP-Game](https://github.com/litvak/fp-game) repo) — keep these three page
paths stable, since that's a hardcoded URL on the other side.

## Deploying to Cloudflare Pages

Connect this repository in the Cloudflare dashboard (Workers & Pages → Create → Pages → Connect to Git), then:

| Setting | Value |
|---------|-------|
| Framework preset | None |
| Build command | *(leave empty)* |
| Build output directory | `/` |

Add the custom domain under the project's **Custom domains** tab. Cloudflare issues the TLS certificate automatically.

## The support mailbox must exist before launch

Every page links `support@cyclamen-games.com`, and Google Play requires a working
support address on the store listing. **This mailbox does not exist yet.**

Cloudflare Email Routing is free and the domain is already on Cloudflare for
Pages: dashboard → the domain → **Email** → **Email Routing** → create the custom
address `support@cyclamen-games.com` forwarding to a personal inbox, then confirm
the verification mail Cloudflare sends there.

Until that is done the address bounces, which would fail the Play listing review.

## Keeping the privacy policy honest

`privacy.html` now describes the optional, opt-out crash reporting the game
ships with (see its "Crash and error reporting" section) rather than asserting
that nothing is ever collected — the in-game Settings screen has a visible
"Crash reports" toggle regardless of whether a given build's `SENTRY_DSN` is
set, so the policy has to hold either way.

What still depends on the actual build is the **Play Data Safety declaration**:
if `SENTRY_DSN` is set as a repository secret in the game repo, the shipped
build reports crashes remotely, and the Data Safety form must declare crash
logs / diagnostics collection (see `docs/Telemetry.md` and
`docs/todo/PlayStoreDeploymentChecklist.md` §6 in the
[FP-Game](https://github.com/litvak/fp-game) repo) before that build goes out.
If it's ever unset again, update the declaration back.

---
*Cloudflare Pages deployment test — 2026-09-27*
