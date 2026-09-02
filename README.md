# Power_ScreenSpacePlanarReflection

[English](README.md) | [简体中文](README.zh-CN.md)

URP Screen Space Planar Reflections, supports full Graphics APIs

Docs & Demo (Chinese):<br/>
https://ti4z0mosnbo.feishu.cn/wiki/Xl6qwWIMziBZmpkYZD3cdS5enl1<br/>

<img width="1318" height="910" alt="image" src="https://github.com/user-attachments/assets/96b2934e-cce0-4930-8dc0-ec5d20eef803" />

## Features
- `SSPRFeature : ScriptableRendererFeature` (SSPRFeature.cs, namespace PowerUtilities.SSPR) — add to URP renderer (UniversalRenderPipelineAsset_Renderer); creates `SSPRPass : SRPPass` (from PowerUtilities.RenderFeatures) and enqueues it at `renderEvent + renderEventOffset` (default AfterRenderingSkybox); forces MSAA off (asset msaaSampleCount = 1); only runs on cameras matching `gameCameraTag` (default "MainCamera")
- `SSPRSettingSO : ScriptableObject` (SSPRSettingSO.cs) — settings asset (see Demo/_SSPRSettingSO_2.asset): planeLocation/planeRotation, fadingRange, stretch options (isApplyStretch/stretchThreshold/stretchIntensity), renderScale, runMode/runModeAuto, isFixedHoleInHashMode, blur (blurMat/blurSize/stepCount/blurRenderScale), reflectionTextureName, renderEvent + offset
- Compute-shader planar reflection (SSPR.compute): kernels `CSClear`, `CSMain`, `CSHashClear`, `CSHash`, `CSResolve`
    - RunMode.Hash — hash-resolve mode (InterlockedMin hash, optional `isFixedHoleInHashMode` 4-corner min), for PC/console (D3D11/12, Metal, Vulkan, GLES3.1+)
    - RunMode.CS_PASS_2 — two-pass `CSMain` with height buffer `_ReflectionHeightBuffer`, for mobile GLES 3.0
    - `runModeAuto` auto-selects by platform; Metal/Vulkan use RWBuffer `_HashResult`
- Blur: `Hidden/SSPR/Blur` (BoxBlur from PowerShaderLib), modes OffsetHalfPixel / SinglePass / TwoPasses
- Display shader: `SSPRFeature/Shaders/ShowReflectionTexture.shader` (Unlit/ShowReflectionTexture) — blends `_ReflectionTexture` with IBL cube by smoothness
- `PowerSSPR.asmdef` — assembly `PowerSSPR` (references PowerUtilities)
- Demo URP asset enables Depth & Opaque textures (urp_pipeline_sspr.asset: m_RequireDepthTexture=1, m_RequireOpaqueTexture=1)

## Folder structure
    PowerXXX
    ├── PowerScreenSpacePlanarReflection
    │   ├── SSPRFeature/
    │   │   ├── PowerSSPR.asmdef
    │   │   ├── SSPRFeature.cs, SSPRPass.cs, SSPRSettingSO.cs
    │   │   ├── SSPR.compute
    │   │   └── Shaders/ (ShowReflectionTexture.shader, HashResolve.shader, Blur.shader, Hidden_HashResolve.mat, Hidden_SSPR_Blur.mat)
    │   └── Demo/
    │       ├── sspr_demo.unity
    │       ├── urp_pipeline_sspr.asset, urp_pipeline_sspr_Renderer.asset (SSPRFeature added)
    │       ├── _SSPRSettingSO_2.asset
    │       └── plan.mat
    ├── PowerUtilities
    └── PowerShaderLib

## Usage / Setup
1. add SSPRFeature to UniversalRenderPipelineAsset_Renderer
    1.1 create / assign an SSPRSettingSO asset (`SSPRSettingSO`, e.g. Demo/_SSPRSettingSO_2.asset) to ssprFeature.settingSO — the setting auto-loads SSPR.compute / Hidden_SSPR_Blur.mat
    1.2 change params (plane, fading, stretch, blur, runMode, renderScale...)
2. add 3D plane to scene
    2.1 assign SSPRFeature/Shaders/ShowReflectionTexture.mat to plane
    2.2 change plane's mat renderqueue > 2500 (demo plan.mat uses 3000)

articles:
https://remi-genin.github.io/posts/screen-space-planar-reflections-in-ghost-recon-wildlands/<br/>
https://github.com/Steven-Cannavan/URP_ScreenSpacePlanarReflections<br/>
https://www.cnblogs.com/idovelemon/p/13184970.html<br/>

Ref Gits
https://github.com/redcool/PowerUtilities.git<br/>
https://github.com/redcool/PowerShaderLib.git<br/>