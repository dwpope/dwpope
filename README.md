# Dave Pope

Designer who builds. I work across product design and iOS engineering, with a focus on on-device sensing and real-time signal processing.

## Current Project

### [Quant](https://github.com/dwpope/Quant)
A real-time posture monitoring iOS app that uses Apple Vision body pose detection to track positioning, detect drinking gestures for hydration logging, and nudge you when you slouch — all processed on-device.

**Stack:** SwiftUI, ARKit, Vision, Combine, on-device ML pipeline

**Highlights:**
- 40,000+ lines of Swift across 237 files with 585 tests
- Pure-logic Swift Package (`PostureLogic`) with zero platform dependencies — entire detection pipeline testable without a simulator
- Three-signal sip detection with training-mode data collection for a future CreateML classifier
- Traffic-light state machine with hysteresis for posture evaluation
- Dynamic thermal adaptation (frame rate scaling from 10 FPS down to 2 FPS based on device temperature)
- Protocol-oriented engine design with mock injection and runtime debug inspection

## About

I care about building things that work well at the intersection of design judgment and engineering depth. My approach: ship real software, not just mockups.
