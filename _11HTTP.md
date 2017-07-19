# Unity中使用HTTP

## 认识Unity中的www

www是Unity开发中非常常用的工具类，主要提供一般HTTp的访问功能，以及动态从网上下载图片、声音、视频等Unity资源。

主要支持的协议有：

* http://

* https://

* file://

* ftp://(只支持匿名账号)

其中file://便是访问本地文件。

## WWW加载网络资源

这里指的网络资源指图片、文本、视频、音频等各种格式的资源。实际上WWW类具有很多使用的属性与资源对应，如下：

* AssetBundle:AssetBundle的数据流，可以包含项目文件夹中的任何类型的资源。

* audioClip（声音）：从下载的数据，返回一个AudioClip。（只读）

* bytes（字节数组）：以字节组的形式返回获取到的网络页面中的内容（只读）。

* movie（视频）：从下载的数据，返回一个MovieTexture（只读）。

* text（文本，XML， Json）：通过网页获取并以字符串的形式返回内容（只读）。

* texture（图片）：从下载的数据返回一个Texture2D

## 使用WWW实现资源加载

在上面提到的所有资源，加载的思路都是一样的，所以这里我们通过加载图片为例来学习资源的加载。

### 图片加载

现在得到图片的网络地址，现需要将此图片加载到Unity中，并显示到界面上。（当然图片也可自己在网络上查找，复制图片地址即可）

图片地址：https://ss0.bdstatic.com/5aV1bjqh_Q23odCf/static/superman/img/logo/bd_logo1_31bdc765.png

使用UGUI在界面上添加一个RawImage组件，并创建一个LoadTexture.cs脚本组件。

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86%E5%8F%8AHTTP%E5%BA%94%E7%94%A8/Http%E5%9C%A8Unity%E4%B8%AD%E4%BD%BF%E7%94%A8/images/20170116131040.jpg)

在LoadTexture中编写代码：

```C#
using UnityEngine;
using System.Collections;
using UnityEngine.UI;

public class LoadTexture : MonoBehaviour {
    // 图片地址
    private string url = "https://ss0.bdstatic.com/5aV1bjqh_Q23odCf/static/superman/img/logo/bd_logo1_31bdc765.png";
    private RawImage img;
	// Use this for initialization
	void Start () {
        //获取RawImage组件
        img = this.transform.FindChild("RawImage").GetComponent<RawImage>();
        StartCoroutine(DoLoad());
	}
    IEnumerator DoLoad()
    {
        WWW www = new WWW(url);
        //挂起程序段，等资源下载完成后，继续执行下去  
        yield return www;

        //判断是否有错误产生  
        if (string.IsNullOrEmpty(www.error))
        {
            //把下载好的图片显示到RawImage中  
            img.texture = www.texture;
            //释放资源  
            www.Dispose();
        }
    }
}
```

程序执行后效果：

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86%E5%8F%8AHTTP%E5%BA%94%E7%94%A8/Http%E5%9C%A8Unity%E4%B8%AD%E4%BD%BF%E7%94%A8/images/20170116131544.jpg)

## WWW实现GET请求

在上节课程内容中，我们已经编写的用于模拟登录的GET方式页面，此案例将通过WWW在Unity客户实现Get请求。

编写Unity客户端代码，用于模拟GET请求。

```C#
using UnityEngine;
using System.Collections;

public class HttpGetDemo : MonoBehaviour
{

    //用于处理GET请求的页面地址
    private string url = "http://localhost/login.php?username=admin&password=123";

    // Use this for initialization
    void Start()
    {
        StartCoroutine(DoLoad());
    }

    IEnumerator DoLoad()
    {
        WWW www = new WWW(url);
        yield return www;

        //判断是否有错误产生  
        if (string.IsNullOrEmpty(www.error))
        {
            //输出服务器返回的内容
            print(www.text);
        }
    }
}
注：使用服务器页面时要注意，将文档中的POST给为GET（两个文档中都要改）
```

服务器返回结果

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86%E5%8F%8AHTTP%E5%BA%94%E7%94%A8/Http%E5%9C%A8Unity%E4%B8%AD%E4%BD%BF%E7%94%A8/images/20170116135746.jpg)

## WWW实现POST请求

在Unity中实现POST请求稍与GET有一些不同，地址中，只需要指定需要请求的页面地址即可，需要传递的参数通过WWWForm来进行构建。

同样，使用上节课中的服务器页面，编写客户端程序

```C#
using UnityEngine;
using System.Collections;

public class HttpPostDemo : MonoBehaviour
{

    //用于处理GET请求的页面地址
    private string url = "http://localhost/login.php";

    // Use this for initialization
    void Start()
    {
        StartCoroutine(DoLoad());
    }
    IEnumerator DoLoad()
    {
        //创建WWWForm对象
        WWWForm form = new WWWForm();
        //添加内容
        form.AddField("username", "admin");
        form.AddField("password", "123");

        //通过地址，表单构建WWW对象
        WWW www = new WWW(url,form);

        yield return www;

        //判断是否有错误产生  
        if (string.IsNullOrEmpty(www.error))
        {
            //输出服务器返回的内容
            print(www.text);
        }
    }
}

注：使用服务器页面时要注意，将文档中的GET给为POST（两个文档中都要改）

返回的结果与GET请求的无差异
```

## WWW实现声音的下载

在Unity引擎中，创建场景，在Main Camera上添加LoadAudioSource组件

```C#
using UnityEngine;
using System.Collections;
using UnityEngine.UI;
public class LoadAudioSource : MonoBehaviour
{
    public AudioClip clip;
    private string AudioPath = "http://localhost/Fade.wav";  //请求声音  声音文件格式要是.wav格式的
    void Start ()
	  { 
      StartCoroutine(LoadImage(AudioPath));
    }
    
    IEnumerator LoadImage(string url)
    {
        WWW www = new WWW(url);  //get请求和请求图片时使用
        yield return www;  //等待加载
        if (www.isDone)
        {
            clip = www.audioClip;  //加载声音
            AudioSource source = this.gameObject.AddComponent<AudioSource>();  //动态添加组件
            source.clip = clip;
            source.Play();
        }
        www.Dispose();  //释放资源
    }
}
```

# 课后作业

1.请使用www从服务器下载图片到本地，并显示在界面上。

2.从网上下载一个XML文件，并将xml内容显示到界面上.

3.使用www从服务器下载一首mp3歌曲文件，下载完成后在unity中播放这首歌曲。

4.利用http的POST请求，实现账户的登录。

请求地址: http://localhost/crazyphrase2/login.php

需要信息:

* user 用户名

* password 密码

5.在连网的状态下，通过地址查询身份证归属地信息，请求方式采用GET方式.

http://api.k780.com/?app=idcard.get&appkey=10003&sign=b59bc3ef6191eb9f747dd4e83c99f2a4&format=xml&idcard=身份证号码 

# 课后练习——疯狂猜成语游戏

与数据库关联，进行登录和注册

* 注意：要有已做好的数据库。

登录请求

```C#
using UnityEngine;
using System.Collections;
using System.Linq;

public class UILoginHandler : UIHandler
{

    private UIButton login_btn;  //登录按钮
    private UIButton register_btn;  //注册按钮
    private UIInput username;   //用户名输入 
    private UIInput password;   //密码输入
    private string pathPost = "http://localhost/crazyphrase/login.php";  //POST请求 登录
	// Use this for initialization
	void Start ()
	{
	    login_btn = GetComponentByName<UIButton>("login_btn");
            EventDelegate.Add(login_btn.onClick, OnLogin_btnClick);   //给按钮注册事件
	    register_btn = GetComponentByName<UIButton>("register_btn");
	    EventDelegate.Add(register_btn.onClick, OnRegister_btnClck);  //给按钮注册事件
	    username = GetComponentByName<UIInput>("username");
            password = GetComponentByName<UIInput>("password");
	}
	

    void OnLogin_btnClick()
    {
        StartCoroutine(LoadImage(pathPost, username.value, password.value));   //开时调用协程 
    }

    void OnRegister_btnClck()
    {
        UIGameManager.Instance.ShowUI(UIName.UIRegister);  //切换UI
        UIGameManager.Instance.HideUI(UIName.UILogin);
    }
    IEnumerator LoadImage(string url, string Text, string psw)     //协程
    {
        WWWForm form = new WWWForm();   //创建WWWForm对象
        form.AddField("username", Text);  //添加内容
        form.AddField("password", psw);
        WWW www = new WWW(url, form);   //通过地址，表单构建WWW对象
        yield return www;  //等待加载
        if (www.isDone)  //如果加载完成
        {
            //print(www.text);
            string[] str= www.text.Split('|');  //将字符串以'|'分割开，成多个单字符串
            for (int i = 0; i < str.Length; i++)
            {
                if (str[i] == "登录成功")   //如果str中的对象有“登录成功”
                {
                    UIGameManager.Instance.ShowUI(UIName.UIHome);  //切换UI
                    UIGameManager.Instance.HideUI(UIName.UILogin);
                }
            }
        }
        www.Dispose();   //释放资源
    }
}

```

注册请求

```C#
using UnityEngine;
using System.Collections;

public class UIRegisterHandler : UIHandler {

    private UIButton login_btn;
    private UIButton return_btn;
    private UIInput username;
    private UIInput password;
    private string pathPost = "http://localhost/crazyphrase/reg.php";  //注册请求地址
    // Use this for initialization
    void Start()
    {
        login_btn = GetComponentByName<UIButton>("login_btn");
        EventDelegate.Add(login_btn.onClick, Onlogin_btnClick);
        return_btn = GetComponentByName<UIButton>("return_btn");
        EventDelegate.Add(return_btn.onClick, OnReturn_btnClck);
        username = GetComponentByName<UIInput>("username");
        password = GetComponentByName<UIInput>("password");
    }

    void Onlogin_btnClick()
    {
        StartCoroutine(Register(pathPost, username.value, password.value));
    }

    void OnReturn_btnClck()
    {
        UIGameManager.Instance.ShowUI(UIName.UILogin);
        UIGameManager.Instance.HideUI(UIName.UIRegister);
    }

    IEnumerator Register(string url, string Text, string psw)
    {
        WWWForm form = new WWWForm();
        form.AddField("username", Text);
        form.AddField("password", psw);
        WWW www = new WWW(url, form); 
        yield return www;
        if (www.isDone)
        {
            print("注册成功");  //注册成功后会在数据库中显示注册内容
        }
    }
}

```

