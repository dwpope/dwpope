# Quant — Real-Time Posture Monitor

**Role:** Solo designer-developer | **Platform:** iOS 17+ | **Stack:** SwiftUI, ARKit, Vision, Combine

## What It Is

Quant is an iOS app that sits on your desk and uses the front camera to monitor your posture in real time. It tracks five metrics (forward lean, head drop, shoulder rounding, lateral lean, twist), detects drinking gestures for hydration logging, and nudges you when you slouch — all processed entirely on-device with no server dependency.

## Why It's Interesting

This project demonstrates end-to-end ownership of a non-trivial sensing pipeline: from raw camera frames through body pose estimation, signal processing, state machine evaluation, and UI feedback — with a training-mode data collection path for a future ML classifier.

## Key Technical Decisions

**Pure-logic package separation.** All detection logic lives in `PostureLogic`, a Swift package with zero UIKit/SwiftUI/ARKit dependencies. The entire pipeline runs with `swift test` on macOS in seconds, no simulator. This made the difference between a project that's testable in theory and one with 585 tests that actually run in CI.

**Three-signal sip detection.** A simple proximity threshold produces too many false positives (chin-resting, phone calls, adjusting glasses). The SipDetector scores three independent signals — proximity, velocity profile, and duration band — with proximity normalised by shoulder width for scale invariance. The training-mode architecture runs a completely separate data path so ground-truth collection never touches production logic.

**State machine with hysteresis.** Posture evaluation uses time-accumulation transitions (good → drifting → bad) rather than instantaneous threshold crossings. The drifting state acts as a 60-second buffer. Accumulated drift freezes when tracking quality drops, preventing false alarms from camera occlusion.

**Thermal adaptation.** ARKit delivers 60 FPS but Vision body pose detection is expensive. A timestamp-based throttle runs detection at ~10 FPS, dynamically adjusted by device temperature (down to 2 FPS at serious thermal state, full pause at critical). This prevents the phone from overheating during long desk sessions.

**Protocol-oriented engines with debug introspection.** Each engine conforms to a protocol and implements `DebugDumpable`, exposing runtime state as a dictionary. This powers mock injection in tests and a live debug overlay in the app without coupling to engine internals.

## By the Numbers

- 40,000+ lines of Swift across 237 files
- 585 tests across 63 test files (unit, integration, golden recording replay, stability, migration)
- 60 UI variant designs exploring different monitoring visualisations
- watchOS companion app with live connectivity

## What It Shows

Judgment about where to draw architectural boundaries. The ability to build real sensing software, not just screens. Comfort with signal processing, state machines, and protocol-oriented design in Swift — applied to a problem I actually wanted to solve.
