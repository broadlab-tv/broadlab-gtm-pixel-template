# Broadlab Pixel for Google Tag Manager

Sends Broadlab conversion events to FreeWheel. Use it with the pixel spec your Broadlab contact sends you.

This template covers FreeWheel only. If your spec also lists Amazon events, set those up with Amazon's own GTM template.

## Set-up

1. In GTM, go to Tags, New, Tag Configuration, and search the Community Template Gallery for "Broadlab Pixel".
2. Create one tag per event in your Broadlab spec.
3. Fill in **Tag ID**, **Segment Key** and **Event type** exactly as the spec shows.
4. Under **Value to send**, add one row per value part in the spec, in the same order. Keep a row even if it has nothing to send. If the spec says "Value to send: Nothing", add no rows.
5. For Purchase, set **Order ID** to the variable holding your order or transaction ID. Without it, a refresh of the confirmation page counts as a new purchase.
6. Attach the trigger from the spec.
7. **Consent:** in the tag's Advanced Settings, Consent Settings, require the consent your spec names (usually `ad_storage`). A tag blocked by consent does not fire later on its own, so if your consent banner pushes an event when a visitor accepts (for example `OneTrustGroupsUpdated`), also add that event as a trigger. The template makes sure a page view is still only sent once per page.
8. Publish and tell your Broadlab contact. We will check every event and confirm in writing.

## What the template sends

Each event sends one request to FreeWheel (`cnv.event.prod.bidr.io`) containing:

| Field | Contains |
|---|---|
| `tag_id`, `segment_key` | The IDs from your Broadlab spec |
| `order` | The full page address (including any query string), the GTM event that triggered the tag, and the referring page address, separated by `|` |
| `value` | The value parts you set up, separated by `|` |

**Do not put personal data in value parts** (names, email addresses, phone numbers, customer IDs). Because the full page address is sent, also check that your site does not put personal data in its URLs, for example an email address in a query string.

The Order ID and the cookie contents below are never sent.

## What the template does by itself

- Builds `order` as page address | trigger event | referrer, in full.
- Keeps every value part in position, turns missing or `undefined` values into empty parts, and keeps `0`.
- With **Send as Number**, reads UK and European prices: `£1,079.00`, `1.079,00 €` and `1 079,00` all become `1079.00`. A single dot is always a decimal point, so `1.079` stays `1.079`. Anything that is not a number is sent empty, with a warning in GTM Preview.
- Applies the repeat firing for each event type. PageView sends once per page address. A visit ends after 30 minutes without any Broadlab tag running.

## Cookies

The template sets three first-party cookies on your site, only when the tag runs. They contain no personal details and are only read by this template on your site; none are sent to Broadlab. List them in your cookie policy under the same category as the Broadlab pixel.

| Cookie | Holds | Lasts |
|---|---|---|
| `_bl_v` | A random visit number and the time of the last Broadlab activity | 30 minutes, renewed with activity |
| `_bl_f` | Which Broadlab tags (and values, as a short code) already sent this visit | 30 minutes, renewed with activity |
| `_bl_o` | Recent order IDs, as a short code | 30 days |

## Support

Contact your Broadlab account team, or open an issue in this repository.
