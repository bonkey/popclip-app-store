# App Store — PopClip extension

Select any text on your Mac, then click **App Store** in the PopClip bar to look
it up on the App Store. The **market** (country storefront) and the **platform**
(iPhone / iPad / Mac) are both configurable.

Defaults reproduce the basic case:

```
https://apps.apple.com/us/iphone/search?term=<your selection>
```

Switch the market to, say, Poland and the platform to Mac, and the same
selection opens:

```
https://apps.apple.com/pl/mac/search?term=<your selection>
```

## Install

1. Double-click `AppStore.popclipext`. PopClip shows an install prompt.
2. Click **Install Extension**.

Because this is a pure *URL action* (no shell/JS/AppleScript code), PopClip
installs it without any "this extension contains code" security warning, and no
code signing is required.

> Alternatively, install it as a *snippet*: open
> `AppStore.popclipext/Config.yaml`, **select the whole text block**, and PopClip
> detects it and offers an **Install Extension** action in its bar. (You can also
> save the YAML as a `.popcliptxt` file and double-click it.)

## Use

1. Select text in any app.
2. In the PopClip bar, click the **App Store** button.
3. Your browser opens the App Store search results for that text.

## Configure

Open **PopClip → ⚙︎ → Extensions → App Store** to set:

- **Market** — the country storefront to search (United States, Poland,
  Germany, Japan, …). Defaults to United States.
- **Platform** — iPhone, iPad, or Mac. Defaults to iPhone.

## How it works

The whole extension is the `url` line in `Config.yaml`:

```yaml
url: https://apps.apple.com/{popclip option market}/{popclip option platform}/search?term=***
```

- `***` is replaced by the selected text, trimmed and URL-encoded by PopClip.
- `clean query: true` collapses newlines/tabs in the selection into single
  spaces, so multi-line selections still make a sensible query.
- `{popclip option market}` and `{popclip option platform}` are replaced by the
  values you pick in settings. They're plain alphanumeric codes (e.g. `us`,
  `iphone`), so there are no URL-encoding edge cases.

## Icon & copyright

The icon is the generic SF Symbol [`storefront.fill`](https://developer.apple.com/sf-symbols/),
not Apple's App Store logo. The App Store logo is an Apple trademark with strict
brand rules, so it's avoided here. SF Symbols are permitted for an in-UI button
glyph like this (Apple only forbids using them as app icons or logos), and a
plain storefront isn't anyone's trademark.

`storefront.fill` needs macOS 14+ (SF Symbols 5). On older macOS, change the
`icon:` line in `Config.yaml` to the always-available `symbol:bag.fill`.

## Adding more markets

The market dropdown ships with ~28 common storefronts. To add another, edit
`AppStore.popclipext/Config.yaml` and add a matching pair under the `market`
option — one entry in `values` (the 2-letter App Store country code) and one in
`value labels` (the display name), keeping them in the same order:

```yaml
    values:
      - us
      - pt        # <- added
    value labels:
      - United States
      - Portugal  # <- added
```

App Store country codes are ISO-3166 alpha-2 codes (`gb` for the UK, `cn` for
China mainland, etc.). Re-install the extension after editing.

> Prefer free-form entry over a dropdown? Replace the whole `market` option with
> a single string field:
>
> ```yaml
>   - identifier: market
>     type: string
>     label: Market
>     default value: us
> ```
>
> Then you can type any 2-letter code, at the cost of the convenient dropdown.

## Files

```
popclip-appstore/
├── AppStore.popclipext/
│   └── Config.yaml      # the extension definition
└── README.md
```
