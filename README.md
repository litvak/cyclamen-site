# Cyclamen Studio — marketing site

Five static pages, no build step, no JavaScript. `style.css` is shared.

| Page | Purpose |
|------|---------|
| `index.html` | Studio landing page — the root of cyclamen-games.com |
| `flat-peaks.html` | The Flat Peaks game page |
| `privacy.html` | **Required by Google Play.** This is the URL that goes in Play Console → App content → Privacy policy |
| `terms.html` | Terms of use |
| `support.html` | Support contact + FAQ. The email here must match the Play Console support email |

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

`privacy.html` states that the game collects nothing. That is true only while the
app is built **without** a Sentry DSN. If `SENTRY_DSN` is ever set as a repository
secret in the game repo, the shipped build reports crashes remotely — this page
and the Play Data Safety declaration must both be updated first. See
`docs/Telemetry.md` in the [FP-Game](https://github.com/litvak/fp-game) repo.
