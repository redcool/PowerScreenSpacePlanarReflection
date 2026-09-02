# Power_ScreenSpacePlanarReflection

[English](README.md) | [简体中文](README.zh-CN.md)

## 简介

Power_ScreenSpacePlanarReflection 是 URP（通用渲染管线）的屏幕空间平面反射（SSPR）实现，支持完整的图形 API（全图形 API 支持）。它通过 `SSPRFeature`（ScriptableRendererFeature）将平面反射 Pass 注入 URP 渲染器，使用计算着色器完成反射的哈希解析与双 Pass 处理，并提供模糊、拉伸、渐隐、渲染缩放等可配置参数，可通过 `runModeAuto` 根据平台自动选择运行模式，广泛适用于 PC、主机与移动端。

文档与 Demo（中文）：
https://ti4z0mosnbo.feishu.cn/wiki/Xl6qwWIMziBZmpkYZD3cdS5enl1

## 功能特性（Features）

- `SSPRFeature : ScriptableRendererFeature`（SSPRFeature.cs，命名空间 PowerUtilities.SSPR）—— 添加到 URP 渲染器（UniversalRenderPipelineAsset_Renderer）中；创建 `SSPRPass : SRPPass`（来自 PowerUtilities.RenderFeatures）并将其排入 `renderEvent + renderEventOffset`（默认 AfterRenderingSkybox）；强制关闭 MSAA（asset 的 msaaSampleCount = 1）；仅在匹配 `gameCameraTag`（默认 "MainCamera"）的相机上运行
- `SSPRSettingSO : ScriptableObject`（SSPRSettingSO.cs）—— 设置资源（见 Demo/_SSPRSettingSO_2.asset）：planeLocation/planeRotation、fadingRange、拉伸选项（isApplyStretch/stretchThreshold/stretchIntensity）、renderScale、runMode/runModeAuto、isFixedHoleInHashMode、模糊（blurMat/blurSize/stepCount/blurRenderScale）、reflectionTextureName、renderEvent + offset
- 基于计算着色器的平面反射（SSPR.compute）：内核 `CSClear`、`CSMain`、`CSHashClear`、`CSHash`、`CSResolve`
    - RunMode.Hash —— 哈希解析模式（InterlockedMin 哈希，可选 `isFixedHoleInHashMode` 4 角最小值），用于 PC/主机（D3D11/12、Metal、Vulkan、GLES3.1+）
    - RunMode.CS_PASS_2 —— 使用高度缓冲 `_ReflectionHeightBuffer` 的两遍 `CSMain`，用于移动端 GLES 3.0
    - `runModeAuto` 会根据平台自动选择；Metal/Vulkan 使用 RWBuffer `_HashResult`
- 模糊：`Hidden/SSPR/Blur`（来自 PowerShaderLib 的 BoxBlur），模式为 OffsetHalfPixel / SinglePass / TwoPasses
- 显示着色器：`SSPRFeature/Shaders/ShowReflectionTexture.shader`（Unlit/ShowReflectionTexture）—— 按粗糙度将 `_ReflectionTexture` 与 IBL 立方体贴图混合
- `PowerSSPR.asmdef` —— 程序集 `PowerSSPR`（引用 PowerUtilities）
- Demo 的 URP 资源启用了深度纹理与不透明纹理（urp_pipeline_sspr.asset：m_RequireDepthTexture=1、m_RequireOpaqueTexture=1）

## 目录结构（Folder structure）

    PowerXXX
    ├── PowerScreenSpacePlanarReflection
    │   ├── SSPRFeature/
    │   │   ├── PowerSSPR.asmdef
    │   │   ├── SSPRFeature.cs, SSPRPass.cs, SSPRSettingSO.cs
    │   │   ├── SSPR.compute
    │   │   └── Shaders/ (ShowReflectionTexture.shader, HashResolve.shader, Blur.shader, Hidden_HashResolve.mat, Hidden_SSPR_Blur.mat)
    │   └── Demo/
    │       ├── sspr_demo.unity
    │       ├── urp_pipeline_sspr.asset, urp_pipeline_sspr_Renderer.asset（已添加 SSPRFeature）
    │       ├── _SSPRSettingSO_2.asset
    │       └── plan.mat
    ├── PowerUtilities
    └── PowerShaderLib

## 使用说明（Usage）

1. 将 SSPRFeature 添加到 UniversalRenderPipelineAsset_Renderer
    1.1 创建 / 指定一个 SSPRSettingSO 资源（`SSPRSettingSO`，例如 Demo/_SSPRSettingSO_2.asset）并赋给 ssprFeature.settingSO —— 该设置会自动加载 SSPR.compute / Hidden_SSPR_Blur.mat
    1.2 修改参数（plane、fading、stretch、blur、runMode、renderScale...）
2. 在场景中添加 3D 平面
    2.1 将 SSPRFeature/Shaders/ShowReflectionTexture.mat 赋给平面
    2.2 将平面材质的渲染队列改为大于 2500（demo 的 plan.mat 使用 3000）

## 参考仓库 / 依赖（Reference Gits）

参考文章：
https://remi-genin.github.io/posts/screen-space-planar-reflections-in-ghost-recon-wildlands/
https://github.com/Steven-Cannavan/URP_ScreenSpacePlanarReflections
https://www.cnblogs.com/idovelemon/p/13184970.html

依赖仓库：
https://github.com/redcool/PowerUtilities.git
https://github.com/redcool/PowerShaderLib.git

## 备注 / 更新记录（Notes / Changelog）

原英文 README 未提供更新记录（changelog）。
