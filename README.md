# VISION SENTINEL / Browser Camera Tracker

A tactical-looking, **local on-device** camera app for phones and computers. Detects faces with Google's MediaPipe Face Detector and tracks moving *regions* with background subtraction. Works as a static website; no database or backend required.

## Features

- Live camera (asks for permission); front/rear camera switch on supported devices.
- Real face detection, persistent temporary session IDs, bounding boxes, confidence and keypoints.
- Motion region detection, boxes, IDs, trails and adjustable sensitivity.
- Click/tap tracks in video or in the object register to highlight them.
- Snapshot PNG and annotated video recording (where `MediaRecorder` and `canvas.captureStream` are supported).
- Fullscreen, mirror mode, event log, controls, responsive screen.
- Video analysis occurs in your own browser; this project does not send live video to a server. MediaPipe may collect library usage/performance metrics (see Google's privacy notice).

**Important distinctions:** Motion detection tracks changed parts of the scene. It does **not** classify all objects (car, person, etc.) or determine their real-world identity. It is most reliable with the phone/camera held still. Face detection recognizes *where faces are*, not *who people are*. Temporary IDs may change when targets leave the view or overlap. Camera autofocus/lighting/exposure changes can cause motion false positives. 'Tactical' graphics are decorative and do not represent real thermal, night-vision, or security-grade tracking.

## Deploy with GitHub -> Render

1. Download the ZIP and **extract it**. Create a new GitHub repository and upload the project files **inside** the extracted folder (`site/`, `render.yaml`, and `README.md`). Don't upload only the ZIP to GitHub.
2. In [Render](https://dashboard.render.com/) choose **New > Static Site** and connect the GitHub repo.
3. Set the **Build Command** to `echo ready`, and **Publish Directory** to `site`. Deploy. Or choose **New > Blueprint** and use the included `render.yaml`.
4. Open the resulting HTTPS `.onrender.com` URL, tap **START CAMERA**, and approve camera access.
5. For face tracking, allow the AI face model to load. The initial download needs internet; motion tracking does not need the model.

## Run locally

Open a terminal at the extracted project folder and run:

```bash
python -m http.server 8000 -d site
```

Open `http://localhost:8000`. Camera access requires HTTPS or localhost. Opening `index.html` as a file isn't recommended.

## Dependencies / caveats

- No build tooling or server packages required.
- Face module is loaded on demand from a pinned `@mediapipe/tasks-vision@0.10.22` jsDelivr CDN URL; the face model is loaded from the official Google MediaPipe model hosting. Internet access and those endpoints must be reachable. If unavailable, a retry control appears; motion tracking still works. Files are not bundled for offline face detection.
- Camera permissions and whether hardware provides both cameras depend on browser and device. Browser/OS may prevent camera switching or recording. Microphone audio is not recorded (video-only clips).
- Safari on some iPhones may lack `canvas.captureStream` or certain recording codecs; screenshots still work in supporting browsers.
- For performance, motion is processed on a 160×96 frame approximately every 110 ms; faces approximately every 180 ms. Both may run slower on old phones.

## Privacy / responsible use

Use your own camera or get the informed permission of anyone being recorded. Do not use for identifying or secretly monitoring other people. By default this app never uploads video, snapshots, or recordings; saving a file is a local browser action. Check Google's [MediaPipe privacy notice](https://www.npmjs.com/package/@mediapipe/tasks-vision) for diagnostics/usage data collected by the underlying library.

## File map

- `site/index.html` — interface
- `site/styles.css` — military-inspired, sharp-corner layout
- `site/app.js` — camera, MediaPipe, overlays, recordings, controls
- `site/motion.js` — simple motion segmentation
- `site/tracking.js` — temporary region-ID matching
- `render.yaml` — Render Static Site configuration
