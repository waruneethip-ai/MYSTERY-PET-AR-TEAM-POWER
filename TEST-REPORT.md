# Validation report

Tested September 7, 2026. This report distinguishes software checks from live classroom validation.

## Passed

- **15 automated Node tests**: all exact mission structures; three-chunk Goldfish; unique pools across 50 randomized full sessions; duplicate selection rejection; non-solved initial chunks; physical sentence order, missing players and separation; two-second reset; minimum palms and all-palm agreement; continuous five-second requirement despite forgiving visual charge; answer switching and long loss; badge-anchored crossing; brief temporal fallback and ambiguity rejection; duplicate colors; color classifier; mirror/letterbox coordinates; extended versus closed/rotated palms.
- **Real MediaPipe engines in Chromium**: Pose Landmarker lite downloaded, initialized and executed inference on a blank canvas. Pose closed before Hand Landmarker downloaded, initialized and executed inference. Final disposal passed. This validates the pinned runtime/model paths and detector lifecycle, not people-detection accuracy.
- **Browser game flow, simulation**: roulette, four-player Snake word stage, successful question transition, answer zones, colored beams, five-second wrong-answer monster and retry.
- **Complete Goldfish round**: exactly three word cards and captain guidance, question completion, split consensus rejection, correct charge, and a 10 + 10 = 20 result.
- **Teacher controls**: practice-mode confirmation, explicit mission selection, pause, restart (same group and pet), skips, next group and new class.
- **Final scoreboard**: one completed round plus eight explicitly skipped rounds reached nine unique groups and nine different pets; correct stage scores, total and winner highlighting. The full-session no-repeat logic also passed 50 randomized automated runs.
- **WebMCP read-only status tool**: registered correctly, returned current game state; unexpected properties rejected intentionally.
- **Visual inspection**: start screen, Stage 1 cards, Stage 2 beams, monster and final scores; desktop, 1024×768 and 390×844 viewport checks. A narrow-screen pet-image/title overlap was found and corrected. No horizontal document overflow at the checked widths.
- No application console errors in the completed simulated browser session.

## Remaining hardware checks

- The embedded browser's live-camera permission request did not complete in this environment. A real video stream, one-person detection, four students crossing with chest badges, and three or more real open palms have **not** been verified here.
- Camera permission error messages exist; native permission denial was not interactively confirmed in the embedded browser.
- No real school-laptop inference FPS benchmark, iPad Safari test, classroom lighting test, or complete nine-group live lesson was possible.
- The host shell's HTTPS certificate handling prevented independent HTTP checks, but the browser successfully downloaded and ran both actual models.

Before the lesson, use normal Chrome on the target laptop: allow the camera, calibrate four colored badges, swap places, test one broken sentence hold, and test agreeing/split palms. Use the three-player Goldfish mission in practice mode to check classroom instructions. The application is implemented and packaged for GitHub Pages; live classroom tracking is not represented as certified or guaranteed.
