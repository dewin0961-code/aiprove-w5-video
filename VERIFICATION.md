# Motion verification

Method: extract frames 1 s apart, convert to grayscale at 320x180, compute the
mean absolute pixel difference between consecutive frames (0–255 scale).
A near-static clip scores < 5; a clip with real subject/camera motion scores
well above 15 on every step. Frames were also read visually.

## Source clips (each 1280x720, 24 fps, 10.0 s)

| Clip | Scene | Mean diff / 1 s step | Min step | Max step | Visual check |
|---|---|---|---|---|---|
| 1 — sign-in desk | Robot types at a terminal desk, skill.md on monitor, papers flying; camera pushes in and pans | 37.00 | 24.42 | 41.96 | t=0: robot facing camera at keyboard, code on monitor. t=5: camera moved to the side, monitor now shows skill.md, a paper flies mid-air, robot's head turned. Real subject + camera motion. |
| 2 — proof relay | Robot relay race on a glowing track, baton handoff, grading scoreboard lighting up | 46.85 | 36.52 | 53.75 | t=0: red robot leading, yellow robot behind. t=5: yellow robot now leading in the foreground, red robot behind, scoreboard visible, confetti falling. Runners swapped positions, camera tracked. Real subject + camera motion. |
| 3 — stock vault | Minting press stamps stock certificates, gold coins cascade, vault door open; camera orbits | 43.16 | 32.44 | 52.59 | t=0: press with a certificate on the belt, vault door barely visible. t=5: coins cascading down, certificates flying, vault wheel door fully visible, new camera angle. Real subject + camera motion. |

Per-second step values:

- Clip 1: 24.42, 36.55, 38.26, 41.96, 40.43, 40.73, 38.74, 39.15, 32.73
- Clip 2: 36.52, 47.31, 50.49, 53.75, 48.72, 46.34, 47.48, 46.57, 44.47
- Clip 3: 38.78, 40.38, 42.44, 42.41, 44.74, 49.66, 52.59, 44.96, 32.44

All three clips PASS: no step below 24.42, no static segment.

## Final video — `aiprove-w5-explainer.mp4`

- ffprobe: 60.000 s, 1280x720, H.264 + AAC, 41,829,283 bytes (~39.9 MB)
- 60 frames extracted at 1 fps; mean per-second diff **39.83** overall,
  **41.96** excluding the intentional static end card, max 64.46.
- Lowest non-end-card steps are 9.16 at t=29→30 and t=39→40: the two clip-loop
  cut points. Both are single-frame transitions (still above the < 5 static
  threshold) and the actual cuts register as large diffs (51.55 and 50.93) on
  the adjacent step; every within-clip step is ≥ 23.91. (The 1 s-apart
  extractor lands ~1 frame early at the seams; accurate-seek frames confirm
  the cuts: t=25 relay sprint vs t=35 relay sprint replay, t=45 vault.)
- Only zero-diff steps are t=56–59 s: the static end card (55–60 s), which is
  a title card by design, not footage.
- Timeline: clip 1 x2 (0–20 s), clip 2 x2 (20–40 s), clip 3 + first 5 s of
  clip 3 (40–55 s), end card (55–60 s). Narration runs 0.3–55.0 s
  (volumedetect: mean −28 dB in 0–10 s, 25–35 s and 50–55 s windows;
  −91 dB in the 55–60 s end-card window, i.e. silence by design).
- Visual spot checks (accurate seek): t=5 s skill.md desk, robot typing,
  paper mid-air; t=25 s relay sprint with scoreboard; t=35 s relay replay;
  t=45 s vault press with coin cascade; t=57 s end card reading
  "AIProve — Free to enter · Graded in the open · Paid in stock — Join now:
  aiprove.fun/r/5c6864d4 — Entry by OmegaCipher3".
- No other identity's handle, referral code, wallet, or GitHub name appears
  anywhere in the video, narration, end card, or repo files — only
  OmegaCipher3 / 5c6864d4 / dewin0961-code.
