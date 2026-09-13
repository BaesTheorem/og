# og

Image host for surfaces that block image attachments but still render link previews.

Facebook comment boxes are the motivating case. When photo upload is disabled, a
pasted URL still gets scraped by `facebookexternalhit`, and whatever the page
declares as `og:image` shows up as the comment's preview card. A bare `.jpg` URL
has no HTML and therefore no OG tags, so it has nothing to render. This repo
publishes one small page per image, on GitHub Pages, whose only job is to carry
those tags.

Works the same way for Discord, Zulip, iMessage, Slack, and anything else that
unfurls links.

## Use

```
bin/mist-og add ~/Desktop/chart.png --title "2026 usage" --desc "source: Evergy"
bin/mist-og list
bin/mist-og verify <slug>
bin/mist-og rm <slug>
```

`add` builds a 1200x630 card (the image contained, never cropped, over a blurred
copy of itself), writes the page, commits, pushes, waits for the Pages deploy,
then re-fetches the live URL as Facebook's crawler and confirms the `og:image`
returns an actual image under 8MB. It prints the URL to paste and a Sharing
Debugger link.

Flags: `--bg white|black` for a flat backdrop instead of the blur, `--slug` to
set the URL, `--replace` to overwrite one, `--alt` for alt text, `--no-push` to
build without publishing.

## Notes

- **Everything here is public.** It is a public repo serving a public site, so
  only put images in it that you are about to post publicly anyway.
- Card filenames carry a content hash. Re-adding a changed image under the same
  slug produces a new image URL, so Facebook cannot serve a stale cached picture.
- Warm the scrape before pasting somewhere that matters, otherwise the first
  viewer may see a bare card while Facebook fetches the image.
- Animated GIFs: the page shows the animation, the card uses frame one. Link
  previews are static everywhere.
- Some Facebook groups suppress link previews in comments entirely via link
  restrictions. Nothing on this end fixes that.
- Do not make the page serve different content to crawlers than to people.
  Facebook flags domains for that.
