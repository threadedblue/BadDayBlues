---
title: AVWorkflow
source: https://chatgpt.com/c/68e7be83-a6d8-8325-8e3a-4486835b04a2
author:
  - "[[ChatGPT]]"
published:
created: 2025-10-12
description: ChatGPT helps you get answers, find inspiration and be more productive. It is free to use and easy to try. Just ask and ChatGPT can help with writing, learning, brainstorming and more.
tags:
  - clippings
---
Here’s a tight, reliable workflow:

# 1) Lock your music first

- In your DAW, finalize the mix.
- Export **one master WAV** (48 kHz, 24-bit is typical for video).
- Optional but pro: add a **2-pop** (1 kHz, 1-frame beep) and a 1-frame **flash** exactly 2 seconds before the first frame of action. It’s a universal sync check.

# 2) Set up Resolve (master timeline)

- Create a project with your **final FPS** (e.g., 24.000 or 23.976—pick one and never change it).
- Drop the **full master WAV** at **01:00:00:00** (or 00:00:00:00—just be consistent).
- Place timeline markers at musically important moments (downbeats, section starts).
- Decide segment in/out **using timecode** (note exact timecodes or frame numbers for each segment).

# 3) Mirror settings in Blender for each segment

- **Frame rate:** Scene → **FPS** exactly matches Resolve.
- **Audio sync:** Timeline header → Playback → **AV-Sync**.
- **Audio sample rate:** Preferences → System → Audio → **Sample Rate 48000 Hz** (to match your WAV).
- **Reference audio in Blender:**
	- Add the **same full master WAV** to the VSE or as a Sequencer strip in the File → Video Sequencer workspace.
	- Slide the strip so that **frame 0 (or 1)** in Blender corresponds to the same moment you chose in Resolve (you can simply line up by ear/beat or compute frames: `start_frame = seconds * fps`).
	- For Segment N, position the playhead at that segment’s **in frame** (from Resolve) and animate there.
	- Alternatively, make **trimmed copies** of the master WAV for each segment *without changing sample rate* and line them up at frame 0. (I still prefer the full WAV for consistency.)

> Tip: Add **markers** in Blender at the same musical beats (M key). Label them to match Resolve.

# 4) Animate to music without drift

- Don’t time-stretch audio in Blender.
- Keep **Time Remapping** off (Scene Properties) unless you know why you need it.
- If you “Bake Sound to F-Curves,” always bake from the **same master WAV** and consistent FPS/sample rate.

# 5) Render segments with handles

- Render **image sequences** (PNG/EXR) to avoid A/V mux issues per segment.
- Give yourself **handles** (e.g., +12 frames head/tail) so you can crossfade or slip by a few frames in Resolve.
- You can export temp H.264 previews for review, but do your final conform from the image sequences.

# 6) Conform in Resolve

- Put the **master WAV** on Track 1 and **leave it uncut**.
- Lay each segment’s picture on video tracks at the exact timecode/frames you planned.
- Mute or delete any guide audio from segments.
- Do transitions/crossfades in picture only; the **single WAV keeps music continuous**.
- If something looks a frame off, slip the shot—never stretch the audio.

# 7) Common pitfalls (and how to dodge them)

- **Sample-rate mismatch (44.1 vs 48 kHz)** → tiny drifts over time. Use **48 kHz everywhere** for video.
- **Changing FPS mid-project** → guaranteed sync pain. Lock FPS on day one.
- **VFR footage** → not your case if you render from Blender, but keep final deliverables **constant frame rate**.
- **Cutting the music per segment** → creates clicks/gaps. Keep one master track.

# Quick cheat sheet

- Master WAV: **48 kHz / 24-bit**
- FPS: **locked** (e.g., 24) in both Blender and Resolve
- Blender playback: **AV-Sync**
- Segments: **image sequences** + **12-frame handles**
- Final conform: **one continuous music track** in Resolve

If you share your FPS and the exact timecode ranges you want for Segment 1–N, I can spit back the **Blender frame ranges** and a tiny “Blender marker pack” script that drops markers at those frames so you’re perfectly aligned.