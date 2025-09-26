# Power_ScreenSpacePlanarReflection
urp ScreenSpacePlanarReflections support lots graphics ap

中文文档与demo:
https://ti4z0mosnbo.feishu.cn/wiki/Xl6qwWIMziBZmpkYZD3cdS5enl1


articles: 
https://remi-genin.github.io/posts/screen-space-planar-reflections-in-ghost-recon-wildlands/
https://github.com/Steven-Cannavan/URP_ScreenSpacePlanarReflections
https://www.cnblogs.com/idovelemon/p/13184970.html


ref gits:
https://github.com/redcool/PowerUtilities.git
https://github.com/redcool/PowerShaderLib.git

Folder structure:
PowerXXX
    PowerScreenSpacePlanarReflection
    PowerUtilities
    PowerShaderLib


usage:
1 add SSPRFeature to UniversalRenderPipelineAsset_Renderer
    1.1 ssprFeature add ssprCore
    1.2 change params
2 add 3D plane to scene
    2.1 assign SSPRFeature/Shaders/ShowReflectionTexture.mat to plane
    2.2 chang plane'mat renderqueue > 2500