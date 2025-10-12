---
title: "OneMasterScene"
source: "https://chatgpt.com/c/68e7be83-a6d8-8325-8e3a-4486835b04a2"
author:
  - "[[ChatGPT]]"
published:
created: 2025-10-12
description: "ChatGPT helps you get answers, find inspiration and be more productive. It is free to use and easy to try. Just ask and ChatGPT can help with writing, learning, brainstorming and more."
tags:
  - "clippings"
---
## Recommended structure

- **Master “Stage” file (.blend)**
	- Holds the **World** (HDRI), cameras, global lights, and scene layout.
	- No high-poly rigs authored here—keep it light.
- **Asset files (.blend per character/prop)**
	- Each character rig lives in its own file.
	- Materials, textures, corrective shapes all local to the asset.

## Bring characters in the smart way

- Prefer **Link + Library Overrides** (not Append) for rigs and characters.
	- **Link** the character collection → **Create Library Override** → you can animate it in the Stage while upstream fixes in the asset file flow through.
	- Use **Append** only for one-off disposable copies.

## HDRI tips in a master scene

- Put the HDRI in **World → Environment Texture**.
- If you want control over the visible backdrop vs lighting:
	- World nodes: **Light Path → Is Camera Ray** to mix the HDRI with a solid color/gradient.
	- Or Render Properties → **Film → Transparent**, and use a plane/cyclorama/shadow catcher for ground.
- Add a large **shadow catcher** plane (Cycles) for grounding characters.
- Keep **HDRI rotation** controllable: add a **Mapping** node driven by a scene custom property so all shots can share a single control.

## Collections & View Layers

- Put each imported character in its **own Collection** (linked).
- Use **View Layers** to isolate passes (e.g., character only, ground only) if you’ll comp.

## Scale & color management

- Ensure all assets use the same **unit scale** (Meters) and **Apply Scale** before linking.
- Use **Filmic** (default) and keep exposure consistent; store look presets in the stage.

## Performance

- Keep heavy sims (cloth/hair) authored/baked in the **asset file** when possible; link caches to the stage.
- For many characters, consider **Collection Instances** of baked versions for crowd shots.

## When NOT to do this

- If the HDRI must vary shot-to-shot, consider one **Stage per sequence/shot**, each with its own World and linked characters.

---

### TL;DR

Your approach—one master scene with HDRI, importing characters as needed—is good practice. Just **Link + Library Override** characters instead of appending, keep the HDRI and global controls in the master file, and organize via Collections/View Layers. This keeps animation flexible while letting upstream asset fixes propagate automatically.