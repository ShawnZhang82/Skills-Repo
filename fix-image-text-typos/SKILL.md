---
name: fix-image-text-typos
description: Use when a user asks to correct Chinese typos, OCR mistakes, wrong characters, or awkward Chinese text embedded in a raster image such as PNG, JPG, screenshot, infographic, poster, slide export, or diagram.
---

# Fix Image Text Typos

## Overview

Correct text inside raster images without rebuilding the whole design. Preserve the original composition, icons, colors, and visual hierarchy; change only the text areas needed for typo correction.

## Workflow

1. Treat the provided image as the edit target. If it is a local path, inspect it first with an image viewing tool so the target is visible.
2. Transcribe all visible Chinese text that may need correction. Use OCR if available, but always do a manual pass on zoomed crops because OCR often misses homophones and malformed glyphs.
3. Build a correction list before editing:
   - original text or suspected wrong character
   - corrected text
   - location in the image
   - confidence, if uncertain
4. Correct only high-confidence typos directly. If a change is a style rewrite, terminology choice, or could alter the user's intended meaning, ask before applying it.
5. Edit non-destructively. Save a sibling output such as `原文件名-错字修正版.png`; do not overwrite the original unless explicitly requested.
6. Re-check the final image at full-image and zoomed views. Confirm:
   - no new text overlaps
   - line breaks fit the original layout
   - corrected characters are legible
   - unchanged graphics remain intact
7. In the final response, provide the corrected image path, show the image if possible, and summarize the main corrections.

## Editing Strategy

Prefer deterministic local edits for business diagrams, screenshots, slides, and infographics:

- Use local image libraries such as PIL/Pillow when exact text replacement is feasible.
- Cover old text with a background sampled or blended from nearby pixels.
- Re-render replacement text with an installed Chinese font, matching weight, color, size, and line spacing as closely as practical.
- Use tighter font size and line breaks rather than allowing collisions.
- Keep temporary crops and helper scripts out of the final deliverables unless the user asks for them.

Use AI image editing only when local deterministic editing cannot preserve the background or typography. When using generative editing, explicitly instruct it to preserve every non-text visual element and change only the specified text.

## Chinese Correction Heuristics

Look for these common failure modes:

- shape-similar characters: `睛/晴/精`, `锁/销`, `伪/侪`, `密/蜜`
- homophones or near-homophones: `周度/调度`, `计发/计费`, `相应/响应`
- OCR-like substitutions: `绿破/爆破`, `潇除/消除`, `加譃/加密`
- missing or extra strokes in technical terms: `封锁`, `证书`, `隧道`, `路由`, `网关`, `接口`
- unnatural Chinese caused by literal generation: validate against the surrounding technical context before changing.

When the image contains technical content, preserve standard product and protocol names exactly unless clearly wrong: `OpenAI`, `Claude`, `API`, `HTTPS`, `TLS`, `SSH`, `VPN`, `SNI`, `Nginx`, `Xray`, `VLESS-Reality`, `Sub2API`, `Marzban`.

## Quality Bar

Do not claim completion from the correction list alone. Completion requires a rendered output image and visual verification. If automated text rendering makes the design worse, iterate on font size, line breaks, or crop boundaries before delivering.
