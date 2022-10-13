<p align="center">
<a href = "https://hello.kexie.space/"><img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210111634004.png" width="250" /></a>
</p>
[TOC]

# 前言

在很久以前参加了一次比赛，当时策划提出一个比较特殊的需求，要求玩家能动态地把特定图片折角与复原。当时的我技术力还不够解决这个问题，只能由主程出解决方案。他通过操作mesh解决了这个问题，而我当时还不会mesh，这给了我很深的挫败感。之后将这一块查漏补缺，发现确实有很多需求可以通过mesh来实现，值得一学。

# 前置知识

因为我希望即使是刚入门游戏开发的纯小白也能看懂这篇文章，所以我在这里会相对通俗地解释我认为需要掌握的前置知识，方便大家对这一块有比较全面的了解。相比其他文档，本文的前置知识信息量比较大，烦请耐着性子看完。

你有没有想过，游戏场景、游戏角色等，究竟是凭借什么才能呈现在屏幕上。计算机图形学领域的前辈们在很多年前就找到了解决方案。



## 渲染

早在很多年前，就有人想让计算机能够模拟并呈现三维世界，但是应该怎么做呢？

经过前人探索与沉淀后的解决方案如下：

1. 首先在计算机中构建一个三维世界，在里面放置各种各样的模型。
2. 然后假定有一个摄像机在这个虚拟世界中拍摄，就像现实中你用眼睛或者摄像机观察这个世界一样。
3. 最后把摄像机在虚拟世界中拍到的图片显示在电脑屏幕上。

![image-20221012012323529](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130828662.png)

这个过程就是渲染。现在的游戏引擎或建模软件仍然在使用这个思路。

思路听起来挺简单的，但实现起来会遇到各种各样的问题，比如：三维世界要有多大？该怎么组织数据来表示三维模型？怎么才能让摄像机拍出来的模型遮挡与透视关系正确？......

即使一个问题解决了，还可能会从解决方案中衍生出另一些问题，令人头大。

如果想了解更多图形学相关内容，可以观看[GAMES101](https://www.bilibili.com/video/BV1X7411F744/)。

下面继续推进有关mesh的知识点。



## 模型的表示方法

上面我们提到了一个问题：应该怎么组织数据来表示三维模型？

前人们提出了很多种解决方案：

1. 点云（`point cloud`）：既然构建出了三维世界，那么可以把模型中每一个点的三维空间位置保存起来，称为点云。渲染时，只需把点云中的所有点全部加载一遍，就能显示一个模型的全貌。

   毫无疑问，这个方法非常暴力。利用这个方法加载精细的模型会是一场灾难，有一种终极吟诗的美。

2. 多边形网格（`mesh`）：我们可以把一个模型的表面分割成很多个小三角形面。当一个模型的面数越多，它看起来就越精细，如下。

   ![image-20221011231104270](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130828891.png)

   显然，我们无需像点云一样，精确地记录每一个点的位置，而只需存储相对较少的三角形面即可表示出较精细的模型。网格法虽然在理论上不如点云精细，但在应用上，网格法的代价更小，在面数足够的情况下，其效果也能符合预期。

   大部分游戏引擎和建模软件中都是使用这种方案。

3. 构造型立体几何表达法（`CSG`）：这种方式利用的是数学概念上的交、并、补。先取相对简单的几何体，比如立方体、圆柱、圆锥等，再把它们按照交、并、补的方式组合起来，构成一个新的几何体，依此往复，从而构建出我们想要的模型，如下。

   ![image-20221012011611244](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130828165.png)

   显然，我们无法得知该模型的最终信息，例如边界、顶点等。而且因为CSG法受简单几何体种类和运算操作种类的限制，用它表示模型的覆盖域有较大的局限性。

4. 隐式几何法：用抽象的数学公式或函数来记录模型。它有几种形式，如：函数表示、几何的交并差表达式、距离函数、分形等。正因如此，它不够直观。

   不过隐式几何法也有好处，好处在于它存储方便，只需要记录对应公式或函数即可。因为它是公式或函数，所以也很方便处理光线与模型之间的运算。

接下来我们终于要专门讨论mesh了。



# 引擎中的mesh

Unity引擎中，三维模型、二维精灵（`Sprite`）、游戏UI等，都是基于mesh来渲染的。上下文中的“模型”不仅仅是指三维模型，任何可被渲染的都可以称为模型。



## mesh的属性

上文提到了，mesh是由一堆小三角形共同构成的多边形网格，这里来介绍一下它的基本属性：

1. 顶点数组（vertices-Vector3[]）：每一个元素都是mesh中的某个顶点的坐标。若该mesh有n个顶点，则该数组的长度为n。
2. 顶点颜色数组（colors-Color[]）：一个三角形网格的颜色会受它的三个顶点的颜色影响。数组长度同上。
3. 三角形数组（triangles-int[]）：该数组为三角形的列表，包含顶点数组的索引。因为一个三角形有三个顶点，所以三角形数组的大小必须始终是 3 的倍数。这个对应关系会在[实际操作](# 实际操作)中解释。
4. 网格的法线数组（normals-Vector3[]）：三角形的垂直向外的法线，网上有博客说这是顶点的法线，但Unity官方文档中说这是网格的法线，它会分配给每个顶点。

可能有人看到这么多属性，一时难以理解，但其实不难理解，因为mesh就是由一堆小三角形构成的，它势必要存储和这些小三角形相关的数据，依此来构建整个模型。

要注意的有两点：

1. 除三角形数组外，第 i 个顶点的数据在每个数组中都处于索引“i”处。这意味着数组元素间的顺序不能轻易修改。
2. 三角形数组中要按照顺时针设置顶点。在Unity引擎中规定，当屏幕中要绘制一个三角形时，若它的三个顶点排列顺序为顺时针，则将它视为正面朝屏幕；若为逆时针，则背面朝屏幕，而在引擎默认不会渲染背朝屏幕的面片，那么就看不到这个三角形。

Unity引擎提供了相关API，让我们能设置这些属性的值。实际上，mesh的属性远不止这些，想详细了解指路：[UnityEngine.Mesh - Unity 脚本 API](https://docs.unity.cn/cn/current/ScriptReference/Mesh.html)。



## Mesh相关组件

1. MeshFilter 网格过滤器：它内置一个mesh对象。其他脚本可以访问或修改它的mesh对象。要和Mesh Renderer一起使用。

2. Mesh Renderer 网格渲染器：从MeshFilter中获取mesh，把它所表示的模型渲染出来，所以要和MeshFilter 一起使用。

   除此之外，Mesh Renderer还提供了一系列属性，如材质、光照探针、反射探针等，能实现更复杂的效果。这里为了不让人头大，就不展开讲了，想了解的指路→[网格渲染器 (Mesh Renderer) - Unity 手册 (unity3d.com)](https://docs.unity3d.com/cn/2018.4/Manual/class-MeshRenderer.html)。

3. Text Mesh Pro：TextMesh Pro 是 Unity 的最终文本解决方案。它是 Unity UI Text 和旧版 Text Mesh 的完美替代方案。它虽然是UI相关的组件，但名字里带了个mesh，所以把它拿出来当典型。如何调整UI的mesh这里不展开讲，感兴趣可自行查阅，指路→[(UI Mesh_81192_csdn的博客-CSDN博客](https://blog.csdn.net/qq_37833413/article/details/104158747)。



有人认为mesh是一个组件，我认为这样划分不合适。因为引擎中的mesh仅在MeshFilter中充当一个内部对象，由MeshFilter组件管控，而不作为一个相对独立的组件。从它们的继承关系也可以看出来：mesh继承于Object，而MeshFilter继承于Component，如下。

![image-20221012112244534](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130828367.png)

![image-20221012112056414](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130828385.png)

# 实际应用

## mesh的简单操作

光说不练假把式，下面在引擎中进行mesh的简单操作。

首先新建空游戏对象，挂载MeshFilter和Mesh Renderer组件，再挂载自定义脚本，我这里命名为MeshTriangle。

![image-20221012130815276](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130827368.png)

这里我想用四个顶点绘制两个三角形，所以如下声明顶点数组和三角形数组。

```c#
        Vector3[] vertices = new Vector3[4]
        {
            new Vector3(2, 0, 0),
            new Vector3(0, -1, 0),
            new Vector3(-2, 0, 0),
            new Vector3(0, 1, 0)
        };

        int[] triangles = new int[6]
        {
            3, 0, 1,//第一个三角形，这三个数代表vertices数组中元素的位置，分别对应(0, 1, 0)，(2, 0, 0)，(0, -1, 0)，此处有意将它们按顺时针排列
            1, 2, 3//第二个三角形
        };

```

全部代码如下。

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class MeshTriangle : MonoBehaviour
{
    private MeshFilter meshFilter;
    private Mesh mesh;

    private void Awake()
    {
        meshFilter = this.GetComponent<MeshFilter>();
        mesh = new Mesh();
        meshFilter.mesh = mesh;

        Vector3[] vertices = new Vector3[4]
        {
            new Vector3(2, 0, 0),
            new Vector3(0, -1, 0),
            new Vector3(-2, 0, 0),
            new Vector3(0, 1, 0)
        };

        int[] triangles = new int[6]
        {
            3, 0, 1,//第一个三角形，这三个数代表vertices数组中元素的位置，分别对应(0, 1, 0)，(2, 0, 0)，(0, -1, 0)，此处有意将它们按顺时针排列
            1, 2, 3//第二个三角形
        };

        mesh.vertices = vertices;//根据一些资料，赋值顶点信息一定要赋值三角信息之前，见下文资料
        mesh.triangles = triangles;
    }
}

```

运行后结果如下。

![image-20221012134006627](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130828626.png)

由于我们之前没在Mesh Renderer组件中设置材质，这个三角形呈现出紫色，在一般项目中意味着材质丢失。

设置材质后运行结果如下。

![image-20221012134258038](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130828137.png)

如果你自己实现时遇到了一些报错，可以查找其他资料，指路→[Unity3d 创建Mesh报错 Mesh.vertices is too small_LuckyDog阿祥的博客-CSDN博客](https://blog.csdn.net/luckydogyxx/article/details/83302823)。





## 通过mesh动态调整形状

拿上面的例子继续举例。这次我们重新设置顶点位置，让这个菱形逐渐变换成一个矩形。

如果你有思路，不妨先自己动手试试看。

如果你动手实现过的话，你可能会遇到一些坑，我们一起来看。

我下面提供了三种协程方法，它们都试图对顶点与目标点进行插值，以改变顶点的位置，你猜猜哪些方法能成功，哪些方法会失败？

```c#

    IEnumerator DoTransformWay1(Vector3[] targetVertices)
    {
        for(float t = 0; t < 1f; t += Time.deltaTime)
        {
            yield return null;

            List<Vector3> res = new List<Vector3>();

            for (int i = 0; i < mesh.vertices.Length; i++)
            {
                res.Add(Vector3.Lerp(mesh.vertices[i], targetVertices[i], t));
            }

            mesh.SetVertices(res);
        }

        yield break;
    }
```

```c#

    IEnumerator DoTransformWay2(Vector3[] targetVertices)
    {
        for (float t = 0; t < 1f; t += Time.deltaTime)
        {
            yield return null;

            Vector3[] res = mesh.vertices;

            for (int i = 0; i < mesh.vertices.Length; i++)
            {
                res[i] = Vector3.Lerp(res[i], targetVertices[i], t);
            }

            mesh.vertices = res;
        }

        yield break;
    }



```

```c#
    IEnumerator DoTransformWay3(Vector3[] targetVertices)
    {
        for (float t = 0; t < 1f; t += Time.deltaTime)
        {
            yield return null;

            for (int i = 0; i < mesh.vertices.Length; i++)
            {
                mesh.vertices[i] = Vector3.Lerp(mesh.vertices[i], targetVertices[i], t);
                //UnityEngine.Debug.Log(Vector3.Lerp(mesh.vertices[0], targetVertices[0], t));
                //UnityEngine.Debug.Log(mesh.vertices[0]);
            }
        }

        yield break;
    }
```

揭晓答案：前两种方法能成功，第三种方法会失败。

现象是：每一次对mesh.vertices的整体赋值都能成功；每一次对单个顶点的赋值都将失效。

暂时未能寻找到相关资料解释这个现象。

全部代码如下。

```c#
using System.Collections;
using System.Collections.Generic;
using System.Diagnostics;
using System.Security.Cryptography;
using Unity.VisualScripting;
using UnityEngine;

public class MeshTriangle : MonoBehaviour
{
    private MeshFilter meshFilter;
    private Mesh mesh;

    private void Awake()
    {
        meshFilter = this.GetComponent<MeshFilter>();
        mesh = new Mesh();
        meshFilter.mesh = mesh;

        Vector3[] vertices = new Vector3[4]
        {
            new Vector3(2, 0, 0),
            new Vector3(0, -1, 0),
            new Vector3(-2, 0, 0),
            new Vector3(0, 1, 0)
        };

        int[] triangles = new int[6]
        {
            3, 0, 1,//第一个三角形，这三个数代表vertices数组中元素的位置，分别对应(0, 1, 0)，(2, 0, 0)，(0, -1, 0)，此处有意将它们按顺时针排列
            1, 2, 3//第二个三角形
        };

        mesh.vertices = vertices;
        mesh.triangles = triangles;//根据一些资料，赋值顶点信息一定要赋值三角信息之前，见下文资料
    }


    private void Update()
    {
        if(Input.GetKeyDown(KeyCode.E))
        {
            UnityEngine.Debug.Log("E pressed!");

            Vector3[] vertices = new Vector3[4]
            {
                new Vector3(1, 2, 0),
                new Vector3(1, -2, 0),
                new Vector3(-1, -2, 0),
                new Vector3(-1, 2, 0)
            };

            StartCoroutine(DoTransformWay1(vertices));
        }
    }

    IEnumerator DoTransformWay1(Vector3[] targetVertices)
    {
        for(float t = 0; t < 1f; t += Time.deltaTime)
        {
            yield return null;

            List<Vector3> res = new List<Vector3>();

            for (int i = 0; i < mesh.vertices.Length; i++)
            {
                res.Add(Vector3.Lerp(mesh.vertices[i], targetVertices[i], t));
            }

            mesh.SetVertices(res);
        }

        yield break;
    }
}

```

结果如下。

![MeshTransform 00_00_00-00_00_30](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210131046105.gif)

## 基于mesh的拖尾组件

如果你了解或使用过Unity中的Trail Renderer组件，你可能会注意到，下图红框中的属性就是Mesh Renderer中的。它实际上就是对mesh的应用之一，你甚至可以自己手搓一个Trail Renderer组件。

![image-20221012162734992](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130828445.png)



mesh的通用性非常强，只要是基于mesh的做的渲染，这一套基本属性和工作流程是不变的。即使学习其他引擎，mesh用的仍然是同一套东西。



## 在mesh上绘制图片

本不想增加这一小节。但思索一番，觉得还是有必要讲一讲关于uv的事情。

上午提到过，我们可以用mesh来渲染模型的。上文的例子中，我们使用的是Unity默认的材质Default-Line，所以渲染出来的三角形面是白色的。接下来我们想让网格面上显示我们自己的图片。

首先创建新的材质（Material），然后把我们导入的图片作为材质的Albedo，如下。

![image-20221012205738838](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130829967.png)

把该材质转递给Mesh Renderer，如下。

![image-20221012205943844](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130829165.png)

像之前一样编辑好操作mesh的脚本。

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class MeshPicture : MonoBehaviour
{
    private MeshFilter meshFilter;
    private Mesh mesh;

    private void Awake()
    {
        meshFilter = this.GetComponent<MeshFilter>();
        mesh = new Mesh();
        meshFilter.mesh = mesh;

        Vector3[] vertices = new Vector3[4]
        {
            new Vector3(-5, 5, 0),
            new Vector3(5, 5, 0),
            new Vector3(5, -5, 0),
            new Vector3(-5, -5, 0)
        };

        int[] triangles = new int[6]
        {
            0, 1, 2,
            2, 3, 0
        };

        mesh.vertices = vertices;
        mesh.triangles = triangles;
    }
}
```

如果不设置uv，直接点击运行，会发现网格上没有出现我们预期的图片。

![image-20221012210101145](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130829319.png)



### 什么是材质

渲染时，如果没有材质，那么模型就会显示出紫色，就像上文中的例子一样。

这意味着，材质中存储着网格面上的颜色等信息。

每一个材质都是一个二维平面。当我们提取三维模型的材质时，软件总是会把它导出为一个二维图片，就像二向箔一样，如下。

![image-20221012215057286](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130829614.png)

有了网格，一个模型就有了形状；有了材质，一个模型就有了可见的外观。



### 什么是uv

之前我们已经设置好了材质，但还没有建立起材质和网格的对应关系。

uv是一个二维坐标轴，它的引入，正可以解决这个问题。可以注意到在上一张图中，已经有两个坐标轴`u-v`在度量材质图了。

`u-v`坐标轴的四个角对应着四个坐标，人们规定原点坐标为（0，0），原点的对角点坐标为（1，1），如下。

![image-20221012220220033](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130829297.png)

建立起了二维坐标系，那么材质中的任意一小块三角形都能用`u-v`坐标表示出来。

我们之前提到：在mesh中，除三角形数组外，第 i 个顶点的数据在每个数组中都处于索引“i”处。mesh中的uv数组也遵守此规则。

当我们为某个顶点所对应的uv赋值，就相当于在二维材质图上点出一个点，点出三个点就意味着在材质图上划出了一个三角形，正对应着三个顶点所围成的小三角形面片。

我们的问题得到解决。



### 实际过程

我导入的是这张图片。按照uv的规则，它的左下角默认为（0，0），右上角为（1，1）。

![picture](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130829095.jpg)

我们如下设置mesh中的uv坐标。

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class MeshPicture : MonoBehaviour
{
    private MeshFilter meshFilter;
    private Mesh mesh;

    private void Awake()
    {
        meshFilter = this.GetComponent<MeshFilter>();
        mesh = new Mesh();
        meshFilter.mesh = mesh;

        Vector3[] vertices = new Vector3[4]
        {
            new Vector3(-5, 5, 0),
            new Vector3(5, 5, 0),
            new Vector3(5, -5, 0),
            new Vector3(-5, -5, 0)
        };

        int[] triangles = new int[6]
        {
            0, 1, 2,
            2, 3, 0
        };

        Vector2[] uv = 
        {
            new Vector2(0, 0),
            new Vector2(0, 1),
            new Vector2(1, 1),
            new Vector2(1, 0)
        };

        mesh.vertices = vertices;
        mesh.triangles = triangles;
        mesh.uv = uv;
    }
}

```

运行结果如下。

![image-20221012221930228](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130829114.png)

结果发现这张图片是横过来的。

修改uv，使其和vertices具有正确的对应关系，如下。

```c#
        Vector2[] uv = 
        {
            new Vector2(0, 1),
            new Vector2(1, 1),
            new Vector2(1, 0),
            new Vector2(0, 0)
        };
```

再次运行结果如下。

![image-20221012222339989](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130829458.png)

结果和预期符合得很好。

# 更多的可能性



## 双面网格

![SinglePlaneMesh 00_00_00-00_00_30](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210131047765.gif)

我们可以再多设置两个三角形，使它们的顶点顺序按逆时针排列，并设置好对应的uv坐标。这样当我们移动到背面时，也能看到背面相应的材质图了。

![DoublePlaneMesh 00_00_00-00_00_30](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210131048565.gif)

## 基于mesh的绳索

项目演示指路→[Unity WebGL Player | rope (yellowjump.github.io)](https://yellowjump.github.io/project/rope_unityh5_0.3/)



## 视觉效果

你甚至可以用mesh玩一些更加花里胡哨的操作，已经有人这么做了，仓库指路→[keijiro/Skinner: Special Effects with Skinned Mesh in Unity (github.com)](https://github.com/keijiro/Skinner)。

![skinner01](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130829264.gif)

![skinner2](https://camo.githubusercontent.com/4ef2907bc27b83ee1b41a6184a67c2a88982ca97786dc3853bc9f7161bc79bc2/687474703a2f2f692e696d6775722e636f6d2f456c66643851452e676966)



# 可参考资料

[UnityEngine.Mesh - Unity 脚本 API](https://docs.unity.cn/cn/current/ScriptReference/Mesh.html)

[UnityEngine.MeshFilter - Unity 脚本 API](https://docs.unity.cn/cn/current/ScriptReference/MeshFilter.html)

[网格渲染器 (Mesh Renderer) - Unity 手册 (unity3d.com)](https://docs.unity3d.com/cn/2018.4/Manual/class-MeshRenderer.html)

[Mesh Filter 组件 - Unity 手册](https://docs.unity.cn/cn/current/Manual/class-MeshFilter.html)

[材质 - Unity 手册](https://docs.unity.cn/cn/2020.3/Manual/Materials.html)

[Lecture 10 Geometry 1 (Introduction)_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1X7411F744?p=10&vd_source=3239e1e7b2000f4dda469ce95c02d761)

[hugoscurti/mesh-cutter: Simple mesh cutting algorithm that works on simple 3d manifold objects with genus 0 (github.com)](https://github.com/hugoscurti/mesh-cutter#examples)

[Unity WebGL Player | rope (yellowjump.github.io)](https://yellowjump.github.io/project/rope_unityh5_0.3/)

[coposuke/TextMeshProAnimator: TextMeshProの文字アニメーション (github.com)](https://github.com/coposuke/TextMeshProAnimator)

[Unity 之图形渲染（二）Mesh_angry_youth的博客-CSDN博客](https://blog.csdn.net/angry_youth/article/details/117885173)