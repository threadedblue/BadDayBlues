---
title: "HDRi Set up"
source: "https://chatgpt.com/c/68e7be83-a6d8-8325-8e3a-4486835b04a2"
author:
  - "[[ChatGPT]]"
published:
created: 2025-10-12
description: "ChatGPT helps you get answers, find inspiration and be more productive. It is free to use and easy to try. Just ask and ChatGPT can help with writing, learning, brainstorming and more."
tags:
  - "clippings"
---
1. **New file** → delete default cube.
2. **Add a large ground**: `Add > Mesh > Plane`, scale it big (e.g., 200–1000 m).
	- (Optional) Make a **cyclorama**: extrude back edge up, add a big bevel so floor blends into backdrop.
3. Add whatever you want “baked” into the HDRI: lights, emissive cards, softbox planes, props, a skyline, etc.
	- Keep things far apart and big to avoid super-hard shadows in the HDRI.

# 2) Set camera to panoramic/equirectangular

1. Select the **Camera** → **Object Data Properties (camera icon)**.
2. **Type:** *Panoramic* → **Panorama Type:** *Equirectangular*.
3. In **Output Properties**: set a **2:1 resolution** (e.g., 4096×2048 for 4K, 8192×4096 for 8K).
4. **Focal length** is irrelevant for equirectangular; position the camera roughly at eye height (e.g., Z=1.7 m) where you want the environment origin to be.

# 3) Use Cycles and 32-bit color

1. **Render Properties** → **Render Engine: Cycles**.
2. **Device:** GPU if available.
3. **Color Management:** *Filmic*, **View Transform** doesn’t affect saved EXR (but helps preview).
4. **Sampling:** Start with 512–1024 samples (more if noisy).
5. **Denoise:** **OFF** for final HDRIs (denoisers can smear dynamic range); okay to keep **Viewport Denoise ON** for lookdev only.
6. **Clamp** (optional): leave **0** for full range; if you get fireflies, try **Clamp Indirect: 2–5** (Direct = 0).

# 4) Light your “HDRI stage”

You’re *creating* the light, so don’t load another HDRI in **World**; keep World color neutral (mid-gray) unless you want a base tint.

- Use **Area Lights** (big rectangles) for soft key/fill/rim.
- Use **Emission planes** for softboxes (Material → Emission Strength 5–50).
- Add a **sun** if you want a distinct hard direction (Strength 1–5, Size 1–5°).
- Keep values realistic but not clipped; HDRI can handle bright values (100+), just avoid tiny ultra-hot pixels.

# 5) Test with a chrome ball

1. Add a **UV Sphere** (radius ~0.2 m), assign a **Glossy** material (Roughness ~0.05).
2. Place it near camera height.
3. Hit **Render View**—tune lights until reflections look the way you want.

# 6) (Optional) Add rolling hills / ground detail

- Add a **Geometry Nodes** modifier to your big plane and displace along the normal with Noise to create gentle hills (we can re-drop the node setup if you want).
- Alternative: keep it flat and add a **Shadow Catcher** plane later when using the HDRI.

# 7) Render the HDRI

1. **Output Properties** → **File Format:** *OpenEXR* (or **OpenEXR Multilayer** if you want passes).
	- **Color Depth:** *32-bit Float*.
	- **Compression:** *ZIP (Lossless)*.
2. **Film**: leave **Transparent** OFF if you want your modeled sky/backdrop baked in. Turn it **ON** if you’re planning to composite a sky later (the EXR will still hold lighting, but the visible background will be transparent).
3. **Render > Render Image** (F12).
4. **Image Editor** → **Image > Save As** → name it like `MyStudio_8k.exr`.

> You can also save **Radiance .HDR**: Image Editor → **Save As** → *Radiance HDR*. EXR is preferred (more robust 32-bit float), but .hdr is widely supported.

# 8) Use your HDRI in other scenes

1. Open your main production scene.
2. **World** → **Color** (yellow dot) → **Environment Texture** → load your EXR/HDR.
3. Add a **Mapping** node (Rotation Z) to turn the environment.
4. If you want the HDRI to light but not be seen:
	- **Render Properties → Film → Transparent** (composite background later), **or**
	- World Nodes: **Light Path → Is Camera Ray** → mix your HDRI with a solid color only for camera rays.

# 9) Ground your characters (two options)

- **Shadow Catcher** (Cycles): put a big plane under your character → Object Properties → **Visibility → Shadow Catcher**.
- **Visible ground** baked into HDRI: if you modeled your floor/hills, you already have grounding baked. For extra punch in the character scene, you can still add a local shadow plane.

# 10) Quality & troubleshooting

- **Banding:** render at higher res (8K) and downscale in comp; avoid saving to 8-bit formats.
- **Noisy reflections:** raise samples, reduce tiny ultra-bright emitters, use larger lights.
- **Too dark/bright in other apps:** that’s normal—HDRIs are linear; adjust environment strength in the new scene (World strength 0.5–2.0).
- **Seams at the poles:** keep important details near horizon; avoid tiny hot spots.