# 第一题

1.（20分)剧情实现，通过Unity读取Excel文件，将表格中剧情文字显示在Unity界面上，点击鼠标左键，显示下一 句,效果如下。

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Unity3D%E9%AB%98%E7%BA%A7%E7%BC%96%E7%A8%8B/images/cut/20170115173822.jpg)

解题：

首先，给定Excel文件，cut.xls，第一种方式是直接读取.xls格式文档，第二种方式是要转换一下格式，将.xls转换为.xlsx格式。

转换格式方式：打开cut.xls, 将其另存为，这时在保存类型中选择.xlsx类型再保存，将转换好的Excel文档拖放到Unity中的Project面板中

其次，我们需要插件，在网上找Excel.dll应用程序扩展、ICSharpCode.SharpZipLib.dll应用程序扩展和System.Data.dll应用程序扩展，将下载好的三个插件放在Unity的Project面板中，存放在Plugins文件夹中。

然后，搭建UI，创建UGUI，Text，在Text中显示Excel中的内容

最后，写代码

```C#
using UnityEngine;
using System.Collections.Generic;
using System.Text;
using System.Text.RegularExpressions;
using Excel;
using System.Data;
using System.IO;
using UnityEngine.UI;

public class UnityExcle : MonoBehaviour {

    public Text readData;
    int rows;
    string nvalue;
    int i = 0;
    int index = 0;
    // Use this for initialization
    void Start()
    {

    }
    void Update()
    {
        List<string> temp = Loadxlsx();
        if (Input.GetMouseButtonDown(0))
        {
            readData.text = temp[index++];
            //print(index);
            if (index == 7)
                index = 0;
            //当不用List<string>返回值时可以使用
	    //readData.text = nvalue;
            //i++;
            //if (i == 7)
            //{
            //    i = 0;
            //}
        }
    }

    List<string> Loadxlsx()
    {
        //读取.xlsx格式文档                                             //打开filepath路径中的XML文件（文件流对象）
        FileStream stream = File.Open(Application.dataPath + "/cut.xlsx", FileMode.Open, FileAccess.Read);
        IExcelDataReader excelReader = ExcelReaderFactory.CreateOpenXmlReader(stream);  //读取.xlsx文档
        
        //读取.xls格式文档
        //FileStream stream = File.Open(Application.dataPath + "/cut.xls", FileMode.Open, FileAccess.Read);
        //IExcelDataReader excelReader = ExcelReaderFactory.CreateBinaryReader(stream);   //使用流读取.xls格式文档
        DataSet result = excelReader.AsDataSet();  //作为数据集进行储存

        int columns = result.Tables[0].Columns.Count;  //列
        int rows = result.Tables[0].Rows.Count;   //行
        
	//当不用List<string>返回值时可以使用
        //for (int j = 0; j < columns; j++)
        //{
        //    nvalue = result.Tables[0].Rows[i][j].ToString();
        //}

        List<string> strList = new List<string>();
        for (int j = 0; j < rows; j++)
        {
            strList.Insert(j, result.Tables[0].Rows[j][0].ToString());  //将0列j行的数据插入到列表中
        }
        return strList;
    }
}
```

# 第二题

2.（20分)在Unity编程连接本地数据库，数据库user表中内容有ID,Name,Age,Maill,尝试输入内容进行增删改查等操作。

```
数据库IP地址: 127.0.0.1
端口:3306
数据库账号: root
数据库密码: 123456
数据库名: U3D_1702班
```

事先创建好数据库内容，然后直接读取数据库，对数据库中的数据进行增删改查


写代码

```C#
using UnityEngine;
using System.Collections;
using System.Xml.Serialization;
using MySql;
using MySql.Data.MySqlClient;
public class UseMySQL : MonoBehaviour
{
    //public string ServerIp = "127.0.0.1";  //数据库IP地址

    public string dataName = "liu";  //数据库名
    public string ServerIp = "localhost"; 
    public string UserId = "root";   //用户名
    public string password = "liuhongliang";  //密码

    private string connetionStr = "";   //链接数据库的字符串

    //对数据库的操作
    private string selectText = "select * from user"; //查
    private string addText = "insert into user values (10, '小强', 25, 'xiaoqiang@163.com')";  //增
    private string updateText = "update user set age = 30 where name = '小强'"; //改
    private string deleteText = "delete from user where name = '小强'";  //删
    private MySqlConnection mySqlConnection;

	// Use this for initialization
	void Start ()
	{
        connetionStr = string.Format("Server = {0}; Database = {1}; User ID = {2}; Password = {3}", ServerIp, dataName, UserId, password);
        OpenMSQL(connetionStr);  //打开数据库
	    Select(selectText);
        //InsertInto(addText);
        //Select(selectText);
        //UpdateSQL(updateText);
        //Select(selectText);
        //DeleteSQL(deleteText);
        //Select(selectText);
	    //CloseMySQL();

	}
	
	//打开数据库
    void OpenMSQL(string info)
    {
        mySqlConnection = new MySqlConnection(info); //使用connetionStr关联数据库
        if (mySqlConnection != null)
        {
            mySqlConnection.Open();   //打开数据库
        }
    }

    //关闭数据库
    void CloseMySQL()
    {
        if (mySqlConnection != null)
        {
            mySqlConnection.Close();   //关闭数据库
            mySqlConnection = null;
        }
    }
    //查
    void Select(string info)
    {
        MySqlCommand command = new MySqlCommand(info, mySqlConnection);
        MySqlDataReader reader = command.ExecuteReader();  //反馈信息调用ExecuteReader
        if (reader.HasRows)
        {
            while (reader.Read())
            {
                string dataStr = "";
                dataStr += reader["Age"];
                print(dataStr);
            }
        }
    }
    //增
    void InsertInto(string info)
    {
        MySqlCommand command = new MySqlCommand(info, mySqlConnection);
        int rows = command.ExecuteNonQuery();
    }
    //改
    void UpdateSQL(string info)
    {
        MySqlCommand command = new MySqlCommand(info, mySqlConnection);
        int rows = command.ExecuteNonQuery();
    }
    //删
    void DeleteSQL(string info)
    {
        MySqlCommand command = new MySqlCommand(info, mySqlConnection);
        int rows = command.ExecuteNonQuery();
    }

}

```

# 第三题

3.（20分）利用XML序列化反序列知识储存PlayerData玩家信息类，(exp)经验值每分钟自动增长50，（level）等级每提一级,升级所需（needExp初始化值100）经验提高百分之25。

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Unity3D%E9%AB%98%E7%BA%A7%E7%BC%96%E7%A8%8B/images/xml/xml.png)


写代码

```C#
using UnityEngine;
using System.Collections;
using System.Xml.Serialization;
using System.IO;

//创建一个XMLPlayer类操作序列化过程
public class XmlPlayer : MonoBehaviour {

    XmlSerializer xmlserializer;    // 持有XmlSerializer对象类型
    public GameObject Player;  //创建玩家对象
    PlayerData data;   //定义玩家数据
    string path;  //路径
	void Start () {
        data = new PlayerData();  //创建玩家数据
        Player = GameObject.Find("Cube");
        path = Application.dataPath + "/Player.xml";    //创建xml文件的路径
        InvokeRepeating("Upgrade", 0f, 60f);   //异步调用方法Upgrade，0秒开始调用，之后每隔60秒调用
	}
	void OnGUI()
    {
        if(GUI.Button(new Rect(0,0,100,30),"SaveData"))
        {                 
            XmlData(data);    
        }
        if(GUI.Button(new Rect (0,40,100,30), "LoadData"))
        {
            LoadData();
        }
    }
	void XmlData(PlayerData data)
    {
        StreamWriter str;   //流/写入器对象
        FileInfo info = new FileInfo(path);   //创建filepath路径下的文件对象
        if(!info.Exists)   // 判断路径是否存在，不存在则创建，存在则删除再创建
        {
            str = info.CreateText();
        }
        else
        {
            info.Delete();
            str = info.CreateText();
        }
        xmlserializer = new XmlSerializer(typeof(PlayerData));    // XmlSerializer对象并且指定序列化类型

        xmlserializer.Serialize(str, data);                 //  序列化方法（传递流/写入器对象，和玩家数据对象）
        str.Close();                                          //  关闭流
    } 
    void LoadData()
    {
        xmlserializer = new XmlSerializer(typeof(PlayerData));         

        FileStream fs = new FileStream(path, FileMode.Open);             //  打开filepath路径中的XML文件（文件流对象）

        PlayerData player = (PlayerData)xmlserializer.Deserialize(fs);   //  反序列化成一个object对象然后强制转化成PlyerData类型
       
        print(player.Exp + "," + player.Level + "," + player.NeedExp);
       
    }

    void Upgrade()
    {
        data.Exp += 50;
        if (data.Exp >= data.NeedExp)
        {
            data.Level += 1;
            data.NeedExp *= 1.25f;
            data.Exp = 0;
        }
    }
}

//创建一个PlayerData类
public class PlayerData
{
    private int exp =0;  //经验

    public int Exp  //属性
    {
      get { return exp; }
      set { exp = value; }
    }

    private int level =0;  //等级
  
    public int Level  //属性
    {
      get { return level; }
      set { level = value; }
    }

    private float needExp = 100;  //升级所需的经验值

    public float NeedExp  //属性
    {
      get { return needExp; }
      set { needExp = value; }
    }
}
```

# 第四题

4.(20分)尝试将一个地形（可以通过资源商店下载）打包AssetBundle到本地路径Prefab下，然后放置到WAMP服务器上，最后再把该地形加载到新建场景中。

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Unity3D%E9%AB%98%E7%BA%A7%E7%BC%96%E7%A8%8B/images/assetbundle/assetbundle.png)

在Project面板中创建Editor文件夹，在文件夹中创建脚本组件BundleAsset，不用挂载，只要编译，脚本就会自动运行

```C#
using UnityEngine;
using System.Collections;
using UnityEditor;

public class BundleAsset : MonoBehaviour
{

    [MenuItem("Build/ModeAsset")]
    static void BundleAssetMainAsset()
    {
        //输出路径
        string path = Application.dataPath;
        //打包的资源
        AssetBundleBuild[] AssetBundles = new AssetBundleBuild[1];
        //打包的文件名称
        AssetBundles[0].assetBundleName = "modle";
        //要打包的预制体路径名称
        string[] AssetName = new string[1] {"Assets/Prefabs/Terrain.prefab" };
        AssetBundles[0].assetNames = AssetName;
        //构建AssetBundles文件的方法                         //选择资源包的关系类型     //打包的目标平台
        BuildPipeline.BuildAssetBundles(path, AssetBundles, BuildAssetBundleOptions.DeterministicAssetBundle,
            BuildTarget.StandaloneWindows64);

    }
}
```

下载资源，将资源加载到Unity中，打开资源中的场景，然后，将所有物体对象全部拖放到Terrain中，以Terrain为父集，将其做成预制体。然后点击菜单栏中的Build/ModeAsset按钮。

等待打包，打包好后会在Project面板中生成4个文件，将这4个文件复制到D:\wamp\www\Text目录下（当然，绕注意自己的wamp装在那个磁盘上），删除Project面板中的4个文件，预制体，创建新场景，在此场景中加动态载资源。

写代码

```C#
using UnityEngine;
using System.Collections;

public class LoadAssetBundle : MonoBehaviour {
    private string url = "http://127.0.0.1/Text/modle";
	// Use this for initialization
	void Start ()
	{
	    StartCoroutine(Load(url));
	}

    IEnumerator Load(string url)
    {
        WWW www = new WWW(url);
        yield return www;
        if (www.isDone)
        {
            GameObject clone = www.assetBundle.LoadAsset("Terrain.prefab") as GameObject;
            GameObject.Instantiate(clone);
        }
        www.Dispose();
    }
}
```















