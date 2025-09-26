# Power_ScreenSpacePlanarReflection
urp ScreenSpacePlanarReflections support lots graphics api

<img width="1318" height="910" alt="image" src="https://github.com/user-attachments/assets/96b2934e-cce0-4930-8dc0-ec5d20eef803" />


中文文档与demo:<br/>
https://ti4z0mosnbo.feishu.cn/wiki/Xl6qwWIMziBZmpkYZD3cdS5enl1<br/>


articles: 
https://remi-genin.github.io/posts/screen-space-planar-reflections-in-ghost-recon-wildlands/<br/>
https://github.com/Steven-Cannavan/URP_ScreenSpacePlanarReflections<br/>
https://www.cnblogs.com/idovelemon/p/13184970.html<br/>


ref gits:
https://github.com/redcool/PowerUtilities.git<br/>
https://github.com/redcool/PowerShaderLib.git<br/>

Folder structure:<br/>
PowerXXX <br/>
    PowerScreenSpacePlanarReflection<br/>
    PowerUtilities<br/>
    PowerShaderLib<br/>


usage:<br/>
1 add SSPRFeature to UniversalRenderPipelineAsset_Renderer<br/>
    1.1 ssprFeature add ssprCore<br/>
    1.2 change params<br/>
2 add 3D plane to scene<br/>
    2.1 assign SSPRFeature/Shaders/ShowReflectionTexture.mat to plane<br/>
    2.2 chang plane'mat renderqueue > 2500 <br/>
