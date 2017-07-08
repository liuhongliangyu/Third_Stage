# XML操作案例

## 数据操作

### XML创建内容

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86%E5%8F%8AHTTP%E5%BA%94%E7%94%A8/XML%E7%BC%96%E5%86%99%E8%A7%84%E8%8C%83/XML%E6%95%B0%E6%8D%AE%E6%93%8D%E4%BD%9C%E5%8F%8A%E5%BA%8F%E5%88%97%E5%8C%96%E5%92%8C%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96/images/Image.png)

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86%E5%8F%8AHTTP%E5%BA%94%E7%94%A8/XML%E7%BC%96%E5%86%99%E8%A7%84%E8%8C%83/XML%E6%95%B0%E6%8D%AE%E6%93%8D%E4%BD%9C%E5%8F%8A%E5%BA%8F%E5%88%97%E5%8C%96%E5%92%8C%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96/images/Image1.png)

点击CrateXml会在Assets路径中创建一个Xml文件，并且写入相应内容所上图所示。添加示例代码如下

```XML
using System.Xml;
public class CreateXml : MonoBehaviour {
  public string filepaht;
  void Start () {
   	filepaht = Application.dataPath+"/xiayifan.xml";    //xml文本存放路径
  }
  void OnGUI()
	{
		if (GUI.Button (new Rect (0, 0, 100, 40),"CreatXml"))
		{	 
		    CrateXmlFile();
		}
  }

  void CrateXmlFile()
  {
    XmlDocument xmldoc = new XmlDocument ();           //创建XmlDocument对象
		XmlElement root = xmldoc.CreateElement ("root");   //创建本文的节点对象（root）
    XmlElement Book = xml.CreateElement ("book");
		XmlElement Title = xml.CreateElement ("title");
    XmlElement Author = xml.CreateElement ("author");
		XmlElement Year = xml.CreateElement ("year");
    XmlElement Price = xml.CreateElement ("price");

    Book.SetAttribute ("category", "悬疑");      //设置book节点的属性
		Title.InnerText =  "盗墓笔记";                    //定义title节点内容
		Author.InnerText = "南派三叔";
		Year.InnerText = "2007";
		Price.InnerText = "198.99";

		Book.AppendChild (Title);                    //让book成为title的父节点
		Book.AppendChild (Author);
		Book.AppendChild (Year);
		Book.AppendChild (Price);
		root.AppendChild (Book);
    xml.AppendChild (root);

		xml.Save (filepaht);                          //保存XML到指定路径
  }

}
```

### XML添加内容

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86%E5%8F%8AHTTP%E5%BA%94%E7%94%A8/XML%E7%BC%96%E5%86%99%E8%A7%84%E8%8C%83/XML%E6%95%B0%E6%8D%AE%E6%93%8D%E4%BD%9C%E5%8F%8A%E5%BA%8F%E5%88%97%E5%8C%96%E5%92%8C%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96/images/Image2.png)

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86%E5%8F%8AHTTP%E5%BA%94%E7%94%A8/XML%E7%BC%96%E5%86%99%E8%A7%84%E8%8C%83/XML%E6%95%B0%E6%8D%AE%E6%93%8D%E4%BD%9C%E5%8F%8A%E5%BA%8F%E5%88%97%E5%8C%96%E5%92%8C%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96/images/Image3.png)

点击AddXml会在原有的内容基础上添加一段新内容，效果如上图所示.添加示例代码如下：

```XML
void OnGUI()
{
    if (GUI.Button (new Rect (0, 50, 100, 40), "AddXml"))
    {
     AddXmlFile();   
    }
}
void AddXmlFile()
{
    XmlDocument xmldoc = new XmlDocument ();
		xmldoc.Load (filepaht);                     //读取以保存的Xml文件
		XmlElement root = xmldoc.SelectSingleNode ("root") as XmlElement; //选择root根节点
    XmlElement Book = xml.CreateElement ("book");  //创建本文的节点对象(book)
    XmlElement Title = xml.CreateElement ("title");
    XmlElement Author = xml.CreateElement ("author");
    XmlElement Year = xml.CreateElement ("year");
    XmlElement Price = xml.CreateElement ("price");

    Book.SetAttribute ("category", "历史");      //设置book节点的属性
    Title.InnerText =  "三国演义";                    //定义title节点内容
    Author.InnerText = "罗贯中";
    Year.InnerText = "1366";
    Price.InnerText = "298";

    Book.AppendChild (Title);                    //让book成为title的父节点
    Book.AppendChild (Author);
    Book.AppendChild (Year);
    Book.AppendChild (Price);
    root.AppendChild (Book);
    xml.AppendChild (root);

    xml.Save (filepaht);                          //保存XML到指定路径
}
```

### XML更新内容

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86%E5%8F%8AHTTP%E5%BA%94%E7%94%A8/XML%E7%BC%96%E5%86%99%E8%A7%84%E8%8C%83/XML%E6%95%B0%E6%8D%AE%E6%93%8D%E4%BD%9C%E5%8F%8A%E5%BA%8F%E5%88%97%E5%8C%96%E5%92%8C%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96/images/Image4.png)

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86%E5%8F%8AHTTP%E5%BA%94%E7%94%A8/XML%E7%BC%96%E5%86%99%E8%A7%84%E8%8C%83/XML%E6%95%B0%E6%8D%AE%E6%93%8D%E4%BD%9C%E5%8F%8A%E5%BA%8F%E5%88%97%E5%8C%96%E5%92%8C%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96/images/Image5.png)

点击UpdateXml会更新Xml文件里的year选项，效果如上图所示.添加示例代码如下：

```XML
void OnGUI()
{
  if (GUI.Button (new Rect (0, 100, 100, 40), "UpdateXml"))
  {
  	 UpdateXmlFile();   
  }

}
void UpdateXmlFile()
	{
		XmlDocument xmldoc = new XmlDocument ();
		xmldoc.Load (filepaht);
		XmlNodeList nodelist = xmldoc.SelectSingleNode("root").SelectNodes("book"); //获取root下的所有book节点
		/*
		for (int i = 0; i < nodelist.Count; i++)
		{
			for (int j= 0; j < nodelist[i].ChildNodes.Count; j++) {
				if(nodelist[i].ChildNodes[j].Name == "price")
				{
					nodelist[i].ChildNodes[j].InnerText = "998";
				}
			}
		}*/
		foreach (XmlNode node in nodelist)
    {
			foreach (XmlNode item in node.ChildNodes)
		  {
  	    if(item.Name=="year")
				{
           item.InnerText = "2017";  
           print(item.InnerText);
				}
			}
		}
		xmldoc.Save (filepaht);
	}

```

### XML删除内容

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86%E5%8F%8AHTTP%E5%BA%94%E7%94%A8/XML%E7%BC%96%E5%86%99%E8%A7%84%E8%8C%83/XML%E6%95%B0%E6%8D%AE%E6%93%8D%E4%BD%9C%E5%8F%8A%E5%BA%8F%E5%88%97%E5%8C%96%E5%92%8C%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96/images/Image6.png)

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86%E5%8F%8AHTTP%E5%BA%94%E7%94%A8/XML%E7%BC%96%E5%86%99%E8%A7%84%E8%8C%83/XML%E6%95%B0%E6%8D%AE%E6%93%8D%E4%BD%9C%E5%8F%8A%E5%BA%8F%E5%88%97%E5%8C%96%E5%92%8C%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96/images/Image7.png)

点击DeleXml会删除掉Xml文件里的year选项中的内容，效果如上图所示.添加示例代码如下：

```XML
void OnGUI()
{
  if (GUI.Button (new Rect (0, 150, 100, 40), "DeleXml"))
  	{
  		DeleXmlFile();   
  	}
}
void DeleXmlFile()
	{
		XmlDocument xmldoc = new XmlDocument ();
		xmldoc.Load (filepaht);
		XmlNodeList nodelist = xmldoc.SelectSingleNode("root").SelectNodes("book");
		foreach (XmlNode node in nodelist)
		{
			foreach (XmlNode item in node.ChildNodes) {
				if(item.Name == "year")
				{
					item.RemoveAll();   //删除节点内容
					//item.RemoveChild(item);//删除节点
				}
			}
		}
		xmldoc.Save (filepaht);
	}
```

优化（综合、简单）

```XML
using UnityEngine;
using System.Collections;
using System.Xml;   //引用命名空间

public class CreateXml : MonoBehaviour
{

    string path;  //持有生成文档路径
    XmlDocument doc;  //持有XML类型变量
    // Use this for initialization
    void Start()
    {
        path = Application.dataPath + "/bookstores.xml";  //声明XML文档路径

    }

    void OnGUI()
    {
        if(GUI.Button(new Rect(0, 0, 100, 30), "Create"))
        {
            CreateBookstores(doc, new XmlElement[6], new string[] { "bookstore", "book", "title", "author", "year", "price"}, "悬疑", "CN", "盗墓笔记", "南派三叔", "2005", "198");
        }

        if (GUI.Button(new Rect(0, 40, 100, 30), "Add"))
        {
            AddXml(doc, "历史", "CN", "三国演义", "罗贯中", "1345", "298");
        }
        if (GUI.Button(new Rect(0, 80, 100, 30), "Remove"))
        {
            RemoveXml(doc);
        }
        if (GUI.Button(new Rect(0, 120, 100, 30), "Update"))
        {
            UpdateXml(doc,  "悬疑", "author", "旺财");
        }
    }

    void UpdateXml(XmlDocument xmldoc, string title, string text, string str)
    {
        xmldoc = new XmlDocument();
        xmldoc.Load(path);   //加载本地XML文档

        XmlElement root = xmldoc.DocumentElement;  //获取根节点
        XmlNodeList elements = root.ChildNodes;    //获取根节点下的子节点
        for (int i = 0; i < elements.Count; i++)
        {
            XmlNodeList children = elements[i].ChildNodes;   //获取elements下的所有子节点
            // 修改具体某个book内的内容
            XmlElement e = elements[i] as XmlElement;
            if (e.GetAttribute("ceteory") == title)
            {
                for (int j = 0; j < children.Count; j++)
                {
                    if (children[j].Name == text)
                        children[j].InnerText = str;
                }
            }

            /*  //更改所有相同的内容
            for (int j = 0; j < children.Count; j++)
            {
                if (children[j].Name == text)
                {
                    children[j].InnerText = str;
                }
            }*/
        }
        xmldoc.Save(path);

    }

    void RemoveXml(XmlDocument xmldoc)
    {
        xmldoc = new XmlDocument();   
        xmldoc.Load(path);   //加载本地XML文档
        XmlElement root = xmldoc.DocumentElement;   //获取根节点
        
        XmlNodeList elements = root.SelectNodes("book");
        for(int i = 0; i < elements.Count; i++)
        {
            XmlNodeList xmls = elements[i].ChildNodes;
            //删除所有book内的title
            for (int j = 0; j < xmls.Count; j++)
            {
                if (xmls[j].Name == "title")
                    elements[i].RemoveChild(xmls[j]);
            }
            /* //删除具体book内的title
            XmlElement e = elements[i] as XmlElement;
            if (e.GetAttribute("ceteory") == "悬疑")
            {
                for (int j = 0; j < xmls.Count; j++)
                {
                    if (xmls[j].Name == "title")
                        elements[i].RemoveChild(xmls[j]);
                }
            }*/
           
        }
        xmldoc.Save(path);
    }

    void AddXml(XmlDocument xmldoc, params string[] text)
    {
        xmldoc = new XmlDocument();
        xmldoc.Load(path);   //加载本地XML文档
        XmlElement root = xmldoc.DocumentElement;
        XmlElement[] elements = CreateElement(xmldoc, new string[] { "book", "title", "author", "year", "price" }, new XmlElement[5]);
        root.AppendChild(elements[0]);        
        for (int i = 1; i < elements.Length; i++)
        {
            elements[0].AppendChild(elements[i]);
            elements[i].InnerText = text[i + 1];
        }
        elements[0].SetAttribute("ceteory", text[0]);
        elements[1].SetAttribute("lang", text[1]);
        xmldoc.Save(path);
    }


    void CreateBookstores(XmlDocument xmldoc, XmlElement[] elements, string[] elementNames, params string[] TextNames )
    {
        xmldoc = new XmlDocument();   //创建XML文档
        for (int i = 0; i < elements.Length; i++)   //循环创建节点
        {
            elements[i] = xmldoc.CreateElement(elementNames[i]);
        }
        elements[1].SetAttribute("ceteory", TextNames[0]);
        elements[2].SetAttribute("lang", TextNames[1]);

        for (int i = 2; i < elements.Length; i++)
        {
            elements[i].InnerText = TextNames[i];
            elements[1].AppendChild(elements[i]);
        }
        xmldoc.AppendChild(elements[0]);
        elements[0].AppendChild(elements[1]);
        
       xmldoc.Save(path);
    }

    XmlElement[] CreateElement(XmlDocument doc, string[] elementNames, params XmlElement[] elements)
    {
        for(int i = 0; i < elements.Length; i++)
        {
            elements[i] = doc.CreateElement(elementNames[i]);
        }
        return elements;
    }
}
```

