# MYSTERY PET AR: TEAM POWER

A complete, static, cooperative English game for Grade 6: nine groups, nine unique pets and categories, body-based question building, and open-palm team answers. HTML5, CSS3, vanilla JavaScript, MediaPipe Tasks Vision and Canvas. No backend, account, database or recording.

## Quick start

1. Extract the ZIP.
2. Open a terminal in `mystery-pet-ar` (the folder containing `index.html`).
3. With Node.js installed, run `node tools/serve.mjs`.
4. Open **http://localhost:4173** in Chrome. Click **START CAMERA**, allow Camera, then **START GAME**.

Alternatively use any static HTTP server on localhost, such as `python -m http.server 4173`. Opening `index.html` directly with `file://` will not support this game’s camera/module flow. Another device accessing your laptop by its network IP needs HTTPS; localhost is an exception only on that same device.

## Deploy to GitHub Pages

No build step is required. Upload `index.html`, `css/`, `js/`, `assets/`, and `.nojekyll` to the root of your GitHub repository (not inside a nested ZIP folder). In **Settings → Pages**, choose **Deploy from a branch**, your branch (usually `main`), and **/(root)**. Save, wait for GitHub's deployment, then open the HTTPS Pages URL. Project URLs such as `https://username.github.io/mystery-pet-ar/` work: all local asset paths are relative.

You can also run `node tools/build.mjs` and publish the contents of `dist/`. Do not publish the enclosing `dist` folder as your root. This project has not been uploaded to a GitHub account or publicly published for you.

## Classroom setup

- Use Chrome on a laptop, a landscape projector, and a front webcam. Safari on iPad is a best-effort target; test the actual device before class. Keep iPad in landscape, allow camera access, and tap a game button to enable sound.
- Bright, even light and a plain background help. Avoid bright windows behind children.
- Prepare four large, matte, solid-color chest badges: **P1 red, P2 blue, P3 green, P4 yellow**. About 12–15 cm across is a useful starting point. Color names and IDs in the UI mean students do not need to rely on color alone.
- Place the badges between shoulders and waist. Keep faces, shoulders, hips, and badges visible. Stand far enough back to fit all word players. Leave clear floor space and swap places slowly without overlapping bodies.
- Calibration requires all necessary badge identities to be visible for 1.5 seconds. Captains stand outside the camera during calibration. The goldfish color mission uses **three** word players: red, blue, green. Other teammates help the Question Captain.
- Press **RANDOM GROUP**. The group and a mission are removed from their pools immediately and can never be selected again in that session. Restarting retains that group and pet; skipping also consumes them.
- Stage 1: students retain their word and move left/right. The correct sentence must remain unambiguous and complete for two seconds. The question mark is fixed on the sentence rail.
- Stage 2: discuss for 3.5 seconds, then show open palms. Move palms horizontally under A, B or C. **At least three open palms** must agree; every currently valid open palm counts, including extra hands. One child may contribute two hands: this is palm consensus, not verification of distinct students.
- Charge is visually forgiving during a brief break, but winning always requires **five continuous seconds** of agreement. Changing answer starts a new charge. After a break, the continuous-seconds label is authoritative even if the visual meter retains charge. Loss longer than 2.2 seconds clears it.
- Wrong answers trigger a silly monster for about two seconds and allow retries. No points are deducted. Each completed stage earns 10 points; all 20-point teams tie as winners.

## Teacher controls

Open **⚙ Teacher** for Pause/Resume, Restart Current Group, Skip Group, Camera Recalibrate, retry camera, and New Class Session. Restart and skip ask for confirmation to avoid accidental classroom score loss. Recalibrating during Stage 1 preserves the group and word assignment; during Stage 2 it reloads hand tracking and restarts the discussion. If tracking failed during initial loading, retry may restart Stage 1.

Switching tabs pauses the game. Use Resume when you return. Timers do not advance while paused or after a long frame stall. Scores live in memory for this session only; refreshing or closing the page clears them. The final scoreboard lists all groups, pets, points and skipped rounds.

## Testing without students

Debug Mode is **off by default**. Open Teacher → Teacher Debug Mode → Continue → Start simulation. Entering or leaving simulation clears the current session so practice scores cannot mix with live scores.

- Select an unused group or mission, or leave both random.
- Drag simulated word cards, or focus one and use arrow keys. The **Arrange words correctly** helper positions the players; the real two-second hold still runs.
- **Aim palms at correct/wrong answer** creates four simulated palms. Drag them or use arrow keys. **Split team** and **Hide all palms** test consensus loss.
- All scoring, roulette, timers, retries and final scoreboard use the same game code as real play. Pose/Hand detectors are stopped during simulation.
- **Show landmarks** shows model landmarks and performance information. In live mode inference is throttled to a maximum of 20 Hz and adapts down if a frame is expensive. Main-thread MediaPipe inference can still reduce rendering FPS on slower hardware.

Run automated logic checks with `node --test tests/game.test.mjs`. Run a static build with `node tools/build.mjs`. Both require only Node's built-in modules; no npm installation is necessary.

While the local server is running, open `http://localhost:4173/tools/diagnostics.html` for a real MediaPipe loading/switching check on a blank frame. This is a diagnostic, not evidence of accurate multi-person tracking.

## Player identity strategy and limits

Pose landmarks provide torso centers. The game samples saturated colors in a chest region in a small unmirrored camera canvas, then maps each color to a fixed player ID. The preview and overlay coordinates are mirrored once, and both use the same contained-video geometry, including letterboxing.

Unique confident color observations override pose result order, so crossing left/right does not deliberately reassign words. During brief badge loss, predicted position and nearest-distance gating retain identity. Ambiguous matches, overlapping bodies, duplicate badge colors and stale observations are rejected; their cards fade and confirmation stops. Fallback expires after 1.4 seconds without a color anchor. This conservative strategy reduces swaps but cannot guarantee identity through complete occlusion, similarly colored clothing, or poor lighting. Re-show badges and recalibrate if necessary. Do a four-student crossing check before the lesson.

The palm heuristic requires three of four non-thumb fingers to be geometrically extended, with straight joints and tips farther from the wrist. It tolerates hand rotation, but bent fingers, tiny distant hands and edge-on palms can be missed. Keep palms facing the camera and separated. Model confidence thresholds are configured at the detector; handedness confidence is not incorrectly treated as detection confidence.

## Camera / loading troubleshooting

- Camera blocked: enable Camera for the site in browser settings, reload, and retry. HTTPS or localhost is required.
- Camera permission prompt pending: respond to the browser's camera prompt. Some embedded browsers cannot complete permission requests; open the HTTPS page in normal Chrome or Safari.
- No webcam: connect one and retry. Camera busy: close other applications or tabs using it.
- Model loading failed: check the internet and the school network allowlist. MediaPipe JS/WASM comes from `cdn.jsdelivr.net`; model files come from `storage.googleapis.com`. Google Fonts is optional; system fonts are the fallback. These asset downloads are required on first load; this is not an offline-bundled game.
- Models slow: use a smaller camera view, good lighting, close other apps, and keep only the required students in frame. The lightweight pose model is used. GPU initialization falls back to CPU.
- Hand not detected: face open palms toward the lens, spread fingers, move closer, and separate overlapping hands.
- Badge not detected: use saturated matte colors, avoid similar colors on shirts/backgrounds, and keep the entire chest region in view.
- iPad: low-memory browsers can fail with multiple people/hands. Restart the tab and use the school laptop if the detector cannot load reliably.

The browser keeps video locally. No microphone permission is requested and no camera frames are uploaded by this app. Third-party asset providers receive normal asset requests.

## Change the learning content

Edit `js/game-data.js`. Each mission contains pet name, category, an array of exact question chunks, correct answer, two distractors and fallback emoji. The correct answer is the first item in source data; answer positions are shuffled for each round. Keep nine unique pets/categories for the nine-round lesson. Three or four chunks are supported; never add a fake chunk or a question-mark chunk.

`assets/pets.png` is a generated 3×3 illustration sheet: hamster, dog, goldfish / frog, snake, guinea pig / gecko, parrot, hedgehog. CSS crops by mission ID. Replace it with a matching 3×3 sheet to update the animals. No external image hotlinks are used.

## Project map

`index.html` — page shell and teacher dialogs  
`css/style.css` — responsive theme and animation  
`js/app.js` — camera, UI, state transitions, simulation and render loop  
`js/game-data.js` — missions, no-repeat session, sentence and consensus rules  
`js/pose-stage.js` — chest color sampling and temporal player identity  
`js/hand-stage.js` — open palms and answer zones  
`js/vision.js` — exclusive MediaPipe lifecycle and coordinate transform  
`js/effects.js` — Canvas beams/particles and synthesized sounds  
`tests/` — deterministic logic tests  
`tools/` — dependency-free server, static build and engine diagnostic

MediaPipe implementation follows Google's [Pose Landmarker web guide](https://developers.google.com/edge/mediapipe/solutions/vision/pose_landmarker/web_js) and [Hand Landmarker web guide](https://developers.google.com/edge/mediapipe/solutions/vision/hand_landmarker/web_js). Runtime version is pinned in `js/vision.js`. See `TEST-REPORT.md` for what was actually verified and the remaining hardware checks.
