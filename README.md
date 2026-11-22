# SPLASH — Juice, in motion.

A scroll-scrubbed juice bar site. Three flavours — Mango, Anaar,
Chocolate — each a full-viewport scene: scroll down and the video plays
forward, scroll up and it plays in reverse. Videos are muted (audio
stripped at encode time). Text is minimal on purpose, so the video
stays clear.

## Stack

- Plain HTML/CSS/JS, no build step. GSAP 3.12.5 + ScrollTrigger +
  Lenis 1.1.14, self-hosted in `vendor/` (no CDN dependency).
- Every device scrubs pre-rendered WebP frame sequences on a `<canvas>`
  (no video seeking, so no stalls). Desktop: 1280x720 at the full 24 fps
  (`frames-hd/<clip>/0001.webp ...`). Touch: 854x480 at 12 fps with
  frame cross-blending (`frames-sm/<clip>/...`). Frames load coarse-to-fine.
- Frame counts live in `FRAME_COUNTS` in `script.js`.

## Files

- `index.html` — 3 scenes + minimal footer
- `styles.css` — dark base, one accent per flavour
- `script.js` — scrub engine (video + canvas paths), Lenis, counter
- `assets/logo-mark.png` — SPLASH droplet logo
- `videos/` — 3 web-ready clips (mango, anaar, chocolate)
- `posters/` — middle-frame posters for each clip
- `frames-hd/`, `frames-sm/` — per-clip WebP frame sequences (desktop / touch)
- `vendor/` — self-hosted GSAP, ScrollTrigger and Lenis

## Source videos

Rehan's own Gemini-generated clips (10s, 720p, 24fps), from his Drive
folder. Order on the page: Mango → Anaar → Chocolate.

## Re-generating frames (ffmpeg)

    ffmpeg -t 7 -i mango_juice.mp4 -vf "fps=24,scale=1280:720:flags=lanczos" -c:v libwebp -quality 80 frames-hd/mango/%04d.webp
    ffmpeg -t 7 -i mango_juice.mp4 -vf "fps=12,scale=854:480:flags=lanczos"  -c:v libwebp -quality 78 frames-sm/mango/%04d.webp

(-t = seconds used: mango 7, anaar 6, chocolate 5. Update FRAME_COUNTS to match.)
