<p align="center">
<a href = "https://hello.kexie.space/"><img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210111634004.png" width="250" /></a>
</p>

# Typora使用教程

**目录**

[toc]

## ——简介：Typora和Markdown是什么

简单来说，**Markdown是一种语言，Typora是翻译和编辑Markdown的软件**

+ **Markdown**

  Markdown 是一种轻量级标记语言，它允许人们使用易读易写的纯文本格式编写文档。

  Markdown 编写的文档可以导出 HTML 、Word、图像、PDF、Epub 等多种格式的文档。

  Markdown 编写的文档后缀为 `.md`, `.markdown`。

+ **Typora**

  Typora是一款轻便简洁的Markdown编辑器，支持即时渲染技术，这也是与其他Markdown编辑器最显著的区别。即时渲染使得你写Markdown就想是写Word文档一样流畅自如，不像其他编辑器的有编辑栏和显示栏。



## 关于Typora的安装

（不是原创，作者：hackett）

Typora目前是需要收费的，所以除非钱多没地方花，都用破解版（悄悄咪咪）

先下载此压缩包并解压⬇️

链接：https://pan.baidu.com/s/1FnkoFNN1KeoskKbB9ztKQw?pwd=6666 
提取码：6666

运行解压后文件里面的Typora.exe进行安装，安装的时候记住自己安装的路径（安装位置）

typora安装后

如果解压后的文件当中的文件是这样的 `app.asar.txt` ，删除后缀 `.txt`

如果是 `app.asar` 就不用管了



---

>  提示：若文件名中未展示扩展名（.txt），请打开计算机，查看，勾选文件扩展名，使文件显示扩展名

![img](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210072142959.png) 

---



将此文件拷贝到typora安装路径下替换（建议是删除原来的 `app.asar` 再拷贝过来）

我的路径是：D:\Typora\resources 根据自己的安装路径进行替换



![img](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210072137855.png)     

运行Typora，输入序列号

邮箱一栏中任意填写（但须保证邮箱地址格式正确），输入序列号(**在key.txt文件中，任选一条**)，点击“激活”。

出现已激活即可

![image-20220601173354172](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210072146738.png) 





## Typora食用方法

- 加粗： `Ctrl + B`
- 标题： `Ctrl + H`
- 插入链接： `Ctrl + K`
- 插入代码： `Ctrl + Shift + C` -- 无法执行
- 行内代码： `Ctrl + Shift + K`
- 插入图片： `Ctrl + Shift + I`
- 无序列表：`Ctrl + Shift + L` -- 无法执行
- 撤销： `Ctrl + Z`
- 一级标题： `Ctrl + 1` -- 以此类推

````
Typora快捷键整合
```
Ctrl+1  一阶标题    Ctrl+B  字体加粗
Ctrl+2  二阶标题    Ctrl+I  字体倾斜
Ctrl+3  三阶标题    Ctrl+U  下划线
Ctrl+4  四阶标题    Ctrl+Home   返回Typora顶部
Ctrl+5  五阶标题    Ctrl+End    返回Typora底部
Ctrl+6  六阶标题    Ctrl+T  创建表格
Ctrl+L  选中某句话   Ctrl+K  创建超链接
Ctrl+D  选中某个单词  Ctrl+F  搜索
Ctrl+E  选中相同格式的文字   Ctrl+H  搜索并替换
Alt+Shift+5 删除线 Ctrl+Shift+I    插入图片
Ctrl+Shift+M    公式块 Ctrl+Shift+Q    引用

注：一些实体符号需要在实体符号之前加”\”才能够显示
```
````



### 换行符

在markdown中，段落由多个空格分隔。在Typora中，只需回车即可创建新段落。

### 标题级别

\# 一级标题 快捷键为 Ctrl + 1
\## 二级标题 快捷键为 Ctrl + 2
......
\###### 六级标题 快捷键为 Ctrl + 6

### 引用文字

\> + 空格 + 引用文字

### 无序列表

使用 * + - 都可以创建一个无序列表

- AAA
- BBB
- CCC

### 有序列表

使用 1. 2. 3. 创建有序列表

1. AAA
2. BBB
3. CCC

### 任务列表

? -[ ] 不勾选
? -[x] 勾选

### 代码块

在Typora中插入程序代码的方式有两种：使用反引号 `（~ 键）、使用缩进（Tab）。

- 插入行内代码，即插入一个单词或者一句代码的情况，使用 `code` 这样的形式插入。
- 插入多行代码输入3个反引号（`） + 回车，并在后面选择一个语言名称即可实现语法高亮。

```
def helloworld():
    print("hello, world!")
```

### 数学表达式

当你需要在编辑器中插入数学公式时，可以使用两个美元符 $$ 包裹 TeX 或 LaTeX 格式的数学公式来实现。根据需要加载 Mathjax 对数学公式进行渲染。

按下 `$$`，然后按下回车键，即可进行数学公式的编辑。

```
$$
\mathbf{V}_1\times\mathbf{V}_2 = \mathbf{X}_3
$$
```

### 插入表格

输入 `| 表头1 | 表头2 |`并回车。即可创建一个包含2列表。快捷键 `Ctrl + T`弹出对话框。

| id   | number |
| ---- | ------ |
|      |        |

- 不管是哪种方式，第一行为表头，第二行为分割表头和主体部分，第三行开始每一行为一个表格行
- 列与列之间用管道符号`|` 隔开
- 还可设置对齐方式(表头与内容之间)，如果不使用对齐标记，内容默认左对齐，表头居中对齐
  - 左对齐 ：|
  - 右对齐 |：
  - 中对齐 ：|：
- 为了美观，可以使用空格对齐不同行的单元格，并在左右两侧都使用 | 来标记单元格边界
- 为了使 Markdown 更清晰，| 和 - 两侧需要至少有一个空格（最左侧和最右侧的 | 外就不需要了）。

### 脚注

这个例子的脚注为[1](http://www.mamicode.com/info-detail-2794030.html#fn1)。

注意：该例子脚注标识是1，脚注标识可以为字母数字下划线，但是暂不支持中文。脚注内容可为任意字符，包括中文。

### 分割线

输入 `***` 或者 `---` 再按回车即可绘制一条水平线，如下：

------

### 目录（TOC）

输入 `[ toc ]` 然后回车，即可创建一个“目录”。TOC从文档中提取所有标题，其内容将自动更新。

> Typora支持TOC自动生成目录，博客园不支持？



### 内部链接

`[链接](http://example.net/)`

[链接](http://example.net/)

### 网址

Typora允许用<括号括起来>, 把URL作为链接插入。

Typora还会自动链接标准网址。

www.baidu.com

### 图片

```
![显示的文字](C:\Users\Hider\Desktop\echart.png "图片标题")
![显示的文字](C:\Users\Hider\Desktop\echart.png)
```

除了以上2种方式之外，还可以直接将图片拖拽进来，自动生成链接。

### 斜体

使用 `*单个星号*` 或者 `_单下划线_` 可以字体倾斜。快捷键 `Ctrl + I`

*斜体*

### 加粗

使用 `**两个星号**` 或者 `__两个下划线__` 可以字体加粗。快捷键 `Ctrl + B`

**加粗**

### 加粗斜体

使用`***加粗斜体***`可以加粗斜体。

***加粗斜体\***

### 代码标记

标记代码使用反引号，即在英文输入法下，ESC键下面和1键左边的符号。

> 使用该 `printf()`功能

### 删除线

使用`~~删除线~~` 快捷键 `Alt + Shift + 5`

~~删除线~~

### 下划线

\下划线 -- 无法执行

参考另一篇文章，可执行。

通过`<u>下划线的内容</u>` 或者 快捷键`Ctrl + U`可实现下划线

下划线的内容

### 表情符号

Github的Markdown语法支持添加emoji表情，输入不同的符号码（两个冒号包围的字符）可以显示出不同的表情。

:smile -- 无法显示

??

### 下标

(需在设置中打开该功能)

H~2~O

### 上标

(需在设置中打开该功能)

X^2^

### 高亮

`==高亮==`(需在设置中打开该功能)

==我是最重要的==

### 文本居中

使用 `<center>这是要居中的内容</center>`可以使文本居中这是要居中的文本内容

### 转义

Markdown 使用了很多特殊符号来表示特定的意义，如果需要显示特定的符号则需要使用转义字符，Markdown 使用反斜杠转义特殊字符：

**文本加粗**
** 正常显示星号 **

Markdown 支持以下这些符号前面加上反斜杠来帮助插入普通的符号：

```
\   反斜线
`   反引号
*   星号
_   下划线
{}  花括号
[]  方括号
()  小括号
#   井字号
+   加号
-   减号
.   英文句点
!   感叹号
```



### 总结：

可能会用到的不止这些东西，想了解更多自己查去吧你，我才懒得给你复制粘贴了⛔







## Typora图床

### 关于图床

在使用Typora上传笔记时，我们的截图在上传后无法显示只出现一个链接，或者直接把图片上传到github上面时每次打开博客图片都会加载一段时间，为了**让图片加载的时间更快，并且可以在上传的笔记当中显示自己的图片**，我们需要用到 **图床**。

+ 阿里云OSS图床需要付费，但是价格非常便宜，一年可能10块钱左右。



### 安装

#### Typora： [Typora 官方中文站 ](https://typoraio.cn/)

最新版本的Typora需要付费，但是可以安装历史版本

> 安装的版本需要高于**0.9.86**



#### Picgo: [PicGo ](https://github.com/Molunerfinn/PicGo/releases)

下滑找到这个文档安装

<img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210111656474.png" alt="image-20220514141147756" style="zoom: 67%;" /> 



#### 阿里云OSS

+ 进入 [阿里云](https://www.aliyun.com/product/oss)

+ 注册账号

+ 进入控制台

  ![image-20220514141748130](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210111656441.png )

+ 选择对象储存并开通

  <img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210111656485.png" alt="image-20220514142301211" style="zoom: 50%;" /> 

+ 创建Bucket

  随便创建名字

  服务器选 **离自己近的**

  图床选 **标准储存**

  读写权限选 **公共读**

  <img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210111656658.png" alt="image-20220514142426982" style="zoom: 67%;" /> 

  <img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210111656656.png" alt="image-20220514142833992" style="zoom: 67%;" /> 

+ 找到地域节点

  在刚刚创建的Bucket概览中找到 **访问域名** 

  复制 **地域节点**  （只需要复制到地点）

  > 如图只需要复制 `oss-cn-hangzhou`

  <img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210111656569.png" style="zoom:67%;" /> 

+ 找到Key

  鼠标放右上角头像点击Key管理

  ![image-20220514144341687](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210111656394.png) 

  

  弹出的框选继续

  <img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210111656507.png" alt="image-20220514144618765" style="zoom: 67%;" /> 

  

  创建一个AccessKey ，在弹出的界面里，记住你的`accessKeyId`和`accessKeySecret`

  <img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210111656533.png" alt="image-20220514144734301" style="zoom:67%;" /> 

  

+ 给阿里云账户充值

  可以先充值个10块钱试试

  刚开始不需要购买容量包储存包之类的

  >0.12元/1GB/1个月，一年就是1.44元，远低于40GB的9元收费！
  >
  >截图/照片以平均0.5mb/张估算，1gb可存放超过1600张图片！
  >
  >数据低于6GB的情况下直接充值，以GB付费其实比购买储存包更加值得！

  <img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210111656396.png" alt="image-20220514145025157" style="zoom:67%;" /> 



### 配置



#### PicGo

打开picgo后，在你windows的**状态栏**里找到picgo的图标，打开picgo的主界面

<img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210111656746.png" style="zoom:67%;" /> 

在图床设置里面选择`阿里云OSS`，依照以下步骤填写信息

- **设定Keyld**：填写刚刚获得的`AccessKeyID`

- **设定KeySecret**：填写`AccessKeyIDSecret`

- **设定储存空间名**：填写**bucket名称**

  这里填写的是`bucket名称`，不是浏览器里的域名

- **确认存储区域**：填写你的`地域节点`，注意复制的格式

- **指定存储路径**：其实就是自定义一个**文件夹**的名字，以`/`结尾

  它会**自动**在你的bucket里面创建一个文件夹，并把图片上传进去

  ·<img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210111656899.png" alt="image-20220514145803053" style="zoom:67%;" /> 

**弄完之后，记得“确定”，并点击“设置为默认图床”！**



+ 在设置中打开这两项

  <img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210111656578.png" alt="image-20220514145942982" style="zoom: 67%;" /> 





#### Typora

进入typora主界面，点击左上角的“文件-偏好设置”

- 选择`图像`

- 插入图片时`上传图片`

- 下面的选项全勾上【更新22.03.05: 第二个`网络位置的图片`可以不勾，避免已经上传到图床的图片重复上传】

- 上传服务选择`PicGo(app)`

- PicGo路径：找到picgo的安装路径
  **不是安装包的路径！！！！**

  <img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210111656515.png" alt="image-20220514150042541" style="zoom:67%;" /> 

  设置完毕后，我们点击验证图片上传选项

  如果弹出以下弹窗，我们的图床就搞定了！

  <img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210111656486.png" alt="image-20220514150119179" style="zoom:67%;" /> 