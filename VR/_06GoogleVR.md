
# Google VR SDK下载

* 官方GitHub下载:GoogleVR SDK For Unity

链接：http://pan.baidu.com/s/1dEPxGJ3 密码：joji

# 全景实现

1.导入GoogleVR SDK，及全景图片

![](https://nts.newbieol.com/static/k25/04_%E8%99%9A%E6%8B%9F%E7%8E%B0%E5%AE%9E%E5%BC%80%E5%8F%91/06_GoogleVR%E5%85%A8%E6%99%AF/images/20170315120117.jpg)

2.创建一个材质球，并将全景图片拖动到Albedo中。

![](https://nts.newbieol.com/static/k25/04_%E8%99%9A%E6%8B%9F%E7%8E%B0%E5%AE%9E%E5%BC%80%E5%8F%91/06_GoogleVR%E5%85%A8%E6%99%AF/images/20170315115903.jpg)

3.在场景中创建一个Sphere 3D游戏对象，将其放大5倍，并将刚刚建好的材质球赋给Sphere。

![](https://nts.newbieol.com/static/k25/04_%E8%99%9A%E6%8B%9F%E7%8E%B0%E5%AE%9E%E5%BC%80%E5%8F%91/06_GoogleVR%E5%85%A8%E6%99%AF/images/20170315120311.jpg)

4.拖动GvrViewerMain到场景中，并把默认的Main Camera设置到球体中心位置(0,0,0),使摄像机从球内部进行拍摄。

![](https://nts.newbieol.com/static/k25/04_%E8%99%9A%E6%8B%9F%E7%8E%B0%E5%AE%9E%E5%BC%80%E5%8F%91/06_GoogleVR%E5%85%A8%E6%99%AF/images/20170315120507.jpg)

但此时并不能看到全景图片，是因为当前的材质只有球体外面能显示图片，内部不能显示。接下来，我们来通过修改Shader使球体两面都 能显示全景图片 。

5.双面显示Shader.
在Project面板中，创建Shader文件，并粘贴双面显示Shader。

```C#
Shader "VRShader/DoubleSided"
{
    Properties
    {
         _Color("Main Color", Color) = (1,1,1,1)
        _MainTex("Texture", 2D) =  "white" {}
    }
        SubShader
    {
        //Ambient pass  
        Pass
        {
            Name "BASE"
            Tags  {"LightMode" = "Always" /* Upgrade NOTE: changed from PixelOrNone to Always */}  
            Color[_PPLAmbient]
            SetTexture[_BumpMap]  
            {
                constantColor(.5,.5,.5)  
                combine constant lerp(texture) previous
            }  
        SetTexture[_MainTex]
    {
        constantColor[_Color]  
        Combine texture * previous DOUBLE, texture *constant
    }

        }

        //Vertex lights
        Pass{
        Name "BASE"
         Tags {"LightMode" = "Vertex"}
        Material
    {
         Diffuse[_Color]
        Emission[_PPLAmbient]
         Shininess[_Shininess]
        Specular[_SpecColor]
    }

        SeparateSpecular On
        Lighting On
        cull off
        SetTexture[_BumpMap]  
        {
            constantColor(.5,.5,.5)
            combine  constant lerp(texture) previous
        }
        SetTexture[_MainTex]  
        {
            Combine texture *previous DOUBLE, texture *primary  
        }
    }

    }

        FallBack "Diffuse", 1
}
```

6.将原来创建的材质球的Shader替换为VRShader/DoubleSided

![](https://nts.newbieol.com/static/k25/04_%E8%99%9A%E6%8B%9F%E7%8E%B0%E5%AE%9E%E5%BC%80%E5%8F%91/06_GoogleVR%E5%85%A8%E6%99%AF/images/20170315121257.jpg)

更换后，运行案例，可以看到全景图片了。
通过Alt+鼠标移动可以移动视角，Ctrl+鼠标可以旋转视角。

![](https://nts.newbieol.com/static/k25/04_%E8%99%9A%E6%8B%9F%E7%8E%B0%E5%AE%9E%E5%BC%80%E5%8F%91/06_GoogleVR%E5%85%A8%E6%99%AF/images/20170315121641.jpg)

# GoogleVR视频

## Easr Move Texture Video插件

下载地址： 链接：http://pan.baidu.com/s/1gfvhFDx 密码：c3xu

1. 导入插件，和视频（视屏可能有要求，普通MP4...），在上面的项目中新建一个场景

2. 创建一个Sphere，将Scale扩大3倍即可。

3. 拖动GvrViewerMain到场景中，并把默认的Main Camera设置到球体中心位置(0,0,0),使摄像机从球内部进行拍摄。

4. 在Sphere上添加AudioSource组件，用来播放声音，

5. 在Sphere上添加VideoPlay组件，将视频资源拖放到VideoClip下面，并将Sphere拖放到VideoPlayer/AudioSource中。

6. 将上面的Shader文件拖放到Sphere上面，并设置Sphere的Shader属性为VRShader/DoubleSided

7. 运行，即可观看视频。
