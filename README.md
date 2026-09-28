# Applied Kinesiology Sacramento — V2 preview

**Live preview:** https://carlcelinodspnza.github.io/applied-kinesiology-sacramento-preview/

A shareable build of the homepage V2 pass ("concept C") for team review. Every one of the
48 pages carries `noindex,nofollow`, so this preview cannot compete with the client's own
site in search.

---

## What changed in V2

All V2 work is on the **homepage**. The other 47 pages are byte-identical to V1.

### Hero — photo-led, with a typing service line

The V1 hero was a woven lattice of service words. V2 replaces it with a photograph of a
clinician and patient, and a single two-line slot that **types each service in turn**
behind the figures.

- The type passes *behind* the people. That occlusion is a real alpha mask, not a crop:
  `BG2.png` supplies an alpha channel that is applied with `mask-image`, while `background`
  supplies the colour.
- Mobile uses `MBG1.png`. It is the same photograph as the desktop plate — confirmed by
  normalised cross-correlation at **0.9975** — so the desktop cutout's alpha was warped onto
  it to produce `MBG1-cut.png` rather than hand-rolling a segmentation.
- The eyebrow and H1 sit inside the clear top band of the image, which also solved the
  old problem of the H1 falling below the fold on a phone.
- Background is flat white, matching the bottom edge of the photograph. The eyebrow's
  leading rule is removed at all widths.

### Section 03 — pathway figure

Original layout kept. New photograph (`clinic-band-exercise.jpg`), no caption, no frame,
image width matched to the text block, and the arc extension recoloured to the section's
own dark blue.

### Section 04 — service pills

Two-column image rail kept. The pills are now equal width via
`repeat(auto-fit, minmax(200px, 1fr))`, so they balance without breakpoint-by-breakpoint
tuning.

### Service cards — one-row carousel

Seven cards previously wrapped to two rows and pushed the section to 1551px against a
950px viewport. Now a single scroll-snap rail that fits one viewport.

### Section 08 — looping clip

Replaced with `physio-exercise.mp4`. The video column fills the full row: partial widening
was measured and *increases* the leftover gap (7/5 → 330px, 6/6 → 366px, 5/7 → 374px), so
it goes full-row. The arc moved to the foot of the section, headings hold one line, and the
prose sits after the video at video width.

### Section 10 — "What it costs to find out"

- `clinic-band-session.jpg` added on the right. The section shipped as a single
  `col--span-8`, so the row was split to span-7 (text) + span-5 (photo). The h2 measures
  519px of ink and still sets on one line in the narrower column.
- CTAs returned to left alignment.
- The accent rule above the CTAs was running the full 729px column while the prose it
  underlines is capped at 608px. Re-capped to the text measure.

---

## How it is built

`design-system/concept-c.css` is a **new stylesheet, linked last on `index.html` only**.

It is not appended to `structural.css` on purpose. In the working preview this block was an
inline `<style>` after the last `<link>`, so it resolved after `brand-card-bespoke.css`.
`structural.css` loads *before* bespoke, and the V1 log records that bespoke silently beats
equal-specificity rules there. Shipping as the last sheet keeps the exact cascade position
the design was approved in — and leaves `structural.css` untouched, so the other 47 pages
need no cache-hash bump and stay byte-identical to V1.

Verified after consolidation: the real site matches the approved preview on every layout
probe (hero plate, section 03 frame, section 08 video, section 10 columns, chip grid,
carousel overflow), with no console errors, no failed requests and no broken images.

## Known items

- `rel="canonical"` and `sitemap.xml` still point at the original
  `kayl-blip.github.io/client-previews/…` preview URL. Harmless while every page is
  `noindex`, but stale.
- `assets/physio-exercise.mp4` was re-encoded from the 12 Mbps source: **27.0 MB -> 2.6 MB**
  (H.264 1920x1080 CRF 28, audio stripped - the source carried no audio track, and the clip
  is muted on the page anyway). Measured against the original at **SSIM 0.985 / PSNR 43.5 dB**,
  and indistinguishable from it in a 100% frame comparison. Resolution was kept at 1080p
  deliberately: 720p halves the file again but is visibly softer on a retina display, and the
  clip renders 1112px wide on desktop.
- Under `prefers-reduced-motion`, the hero's rest state repeats the H1 wording. Open.
