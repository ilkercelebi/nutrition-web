# nutriva-web

The public pages for the **Nutriva** iOS/Android app: the landing page a shared
weekly recap points at, plus the Privacy Policy, Terms of Service and Support
pages that App Store Guideline 3.1.2 and Google Play both require to resolve.

Live at **https://ilkercelebi.github.io/nutrition-web/**

| Path | File | Used by |
|---|---|---|
| `/` | `index.html` | The link in a shared weekly recap card (`AppConfig.shareUrl`) |
| `/privacy` | `privacy/index.html` | `AppConfig.privacyPolicyUrl`, both store listings |
| `/terms` | `terms/index.html` | `AppConfig.termsOfServiceUrl`, both store listings |
| `/support` | `support/index.html` | `AppConfig.supportUrl`, App Store support URL |

## Two things that are deliberate

**Every page is a directory with an `index.html`**, not `privacy.html` at the
root. That way `/privacy` resolves on any static host without relying on the
server guessing a `.html` extension. These URLs go into a store submission, and
a 404 there is a rejection.

**Every cross-page link is relative** (`privacy/`, `../terms/`). The pages used
to hardcode `https://nutrition-90058.web.app/...`, which broke the moment they
moved. Now they don't care which host serves them.

`.nojekyll` turns off Jekyll — nothing here needs it, and it would silently skip
any future file or folder starting with an underscore.

## Before the App Store listing exists

`index.html` holds `IOS_APP_ID = ''`. While it's empty the page shows an honest
"coming to the App Store shortly" instead of linking to an Apple 404. Fill in the
numeric ID once App Store Connect assigns one. The Android link is built from the
package name and needs no edit.

## Moving to a custom domain later

Add a `CNAME` file containing the bare domain, point DNS at GitHub, then rebuild
the app with the new URLs — all four are `String.fromEnvironment` overrides:

```
--dart-define=PRIVACY_POLICY_URL=https://example.com/privacy
--dart-define=TERMS_URL=https://example.com/terms
--dart-define=SUPPORT_URL=https://example.com/support
--dart-define=SHARE_URL=https://example.com/
```

Also update `og:url` in `index.html` — it has to be absolute, so it's the one
link on the site that knows its own address.
