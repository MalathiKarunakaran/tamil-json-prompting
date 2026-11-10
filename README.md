# JSON Prompting for Tamil Visuals (Week 2, Day 2)

**Scene, in one sentence:** A Mylapore woman in a Chettinad kandangi cotton sari draws a white rice-flour pulli kolam on her doorstep before dawn in Margazhi.

Why this scene: Margazhi (mid-Dec to mid-Jan) is the month of large dawn kolams in Chennai. Kandangi is the checked cotton sari woven in Chettinad, Karaikudi. Rice-flour kolam is Tamil, and different from coloured-powder rangoli. The avoid list blocks the usual postcard clichés: gopurams, piled jasmine and lamps, silk wedding sarees, posed smiles.

## Files

| File | What it is |
|---|---|
| `tamil_prompt.json` | Reusable base template (subject, setting, lighting, camera, palette, style, aspect_ratio, avoid) |
| `image1_base.json` | Image 1, the base template unchanged |
| `image2_golden_hour.json` | Image 2, only `setting.time_of_day` changed |
| `image3_close_up.json` | Image 3, only `camera.shot` changed |
| `video_motion.json` | Video, base template plus a `motion` block |
| `outputs/` | Generated results: `image1_base.png`, `image2_golden_hour.png`, `image3_close_up.png`, `video_motion.mp4`, `video_start_frame.png` |

## Captions for submission

- **Image 1:** Base template. Pre-dawn, wide shot.
- **Image 2:** Changed `setting.time_of_day` to golden hour after sunrise. Same scene, warmer light.
- **Image 3:** Changed `camera.shot` to a close-up of the hand and rice flour. Same scene, tighter.
- **Video:** Added the `motion` block (hand draws the line, slow push-in, 5 s). Scene unchanged.

## How to generate

Paste the full contents of each JSON file as the prompt. Use the same tool and the same seed for all three images if the tool exposes one, so only the changed field explains the difference.

- Images: ChatGPT image, Gemini (Nano Banana), Ideogram or Midjourney (set `--ar 3:2` in Midjourney, which ignores aspect_ratio text).
- Video: Google Veo, Kling or Runway. Best practice: generate Image 1 first and use it as the start frame for the video, so the scene stays identical.

## Design note

`lighting` says "natural available light for the time of day" instead of naming the light directly. That lets `time_of_day` drive the light by itself, so Image 2 really is a one-field change.

## Submission notes
- Tool: Google Gemini (images: Nano Banana, video: Veo), same chat for images 1-3 for consistency.
- Video was animated from `outputs/video_start_frame.png` (an earlier, medium-shot draft of Image 3) rather than Image 1; Veo output was 10 s, trimmed to 5 s to match `duration_seconds`.
- Gemini adds a visible watermark regardless of the `avoid` list (a prompting limit, not a prompt error).
- Image 2 was generated in a fresh Gemini chat (the original chat wasn't available), so the house and framing differ from Image 1; the one-field change is `setting.time_of_day`.

## Assignment note

**Objective.** Learn how structured JSON prompts give fine-grained control over AI image and video generation: build one reusable template, then change exactly one field per output and observe what that field controls.

**Why JSON over a plain-text prompt.** Each visual decision (subject, place, time, light, lens, palette, what to avoid) lives in its own named field. That makes the prompt reusable, easy to version in Git, and testable like code: change one key, compare the outputs, and you know what that key does.

**What worked**
- Specific cultural detail (Chettinad kandangi sari, white rice-flour pulli kolam, Mylapore row house, Margazhi month) produced an authentic Tamil scene instead of a generic "Indian" postcard.
- The `avoid` list kept common clichés out: no coloured rangoli, no silk wedding saree, no posed smile at the camera.
- Writing `lighting` as "natural available light for the time of day" let `time_of_day` drive the light by itself, so Image 2 stays a one-field change.

**Limitations observed**
- **Consistency across generations:** the model does not keep the same person and house between separate requests. Image 2 came from a fresh chat and shows a different house. A fixed seed or a reference image is needed for strict comparisons.
- **The `avoid` list is not absolute:** Gemini adds a visible watermark even though "watermark" is in the avoid list.
- **Video duration:** Veo produced 10 s regardless of `duration_seconds`, so the clip was trimmed to 5 s afterwards.
- **Shot size drift:** the first "close-up" attempt came out as a medium shot and needed a regeneration.

**Key takeaway.** JSON prompting turns image generation into a controlled experiment: the template is the baseline, each field is a variable, and the outputs are the results. Reproducibility still depends on the tool (seeds, reference images), not on the prompt alone.
