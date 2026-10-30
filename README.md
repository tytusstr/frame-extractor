# Frame Extractor

Upload a video and download one picture for every moment something moved. Duplicate frames are skipped.

Open `index.html` in a browser (or turn on GitHub Pages for this repo). Everything runs locally, and the video is never uploaded.

## How it works

1. **Find the real frame rate.** The video plays briefly while `requestVideoFrameCallback` records when each new frame appears.
2. **Visit every frame.** The app seeks to the middle of each frame (`(i + 0.5) / fps`) so it never lands on the neighbouring frame.
3. **Compare frames cheaply.** Each frame is shrunk to 160 px wide and turned to greyscale. Shrinking it removes compression noise and grain.
4. **Skip duplicates.** A pixel counts as moved if its brightness changed by more than 24 out of 255. A frame is kept when any 10×10 tile of the thumbnail has 12% of its pixels moved, so small objects anywhere in the picture are caught. A frame is also kept when 1% of the whole picture has moved. Grain and compression noise stay below both limits.
5. A frame is kept when the share of moved pixels reaches the Sensitivity slider (2% by default).
5. **Compare against the last kept frame, not the previous frame.** Slow movement adds up until it passes the threshold, so it isn't missed. Static stretches produce no pictures.
6. **Export.** Kept frames are saved at full resolution. You can download them one at a time or all together as a ZIP (built with JSZip).
