<p align="center">
<a href = "https://hello.kexie.space/"><img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210111634004.png" width="250" /></a>
</p>

#    `Tilemap` 绘制场景

## 简介

对于刚学习 Unity 的新手来说，都会从简单的 Unity2D 开始接触，`Tilemap（瓦片地图）` 无疑是Unity2D快速搭建一个关卡的利器。通过使用 `Tilemap` ，我们可以像泰拉瑞亚、我的世界等游戏中用方块搭建房子一样绘制我们的场景。要使用 `Tilemap` 绘制场景，首先你就需要一张图片素材作为你的场景，并且对图片进行部分的调整来使得图片可以在 `Tilemap` 中正常使用，我用下面这张图片举例示范

![main_lev_build](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210081301018.png)

## Sprite Editor

在使用 `Tilemap` 前，首先了解导入的图片的信息，并且需用用到 `Sprite Editor` 进行图片的切割，把图片切割成小方格，便于绘制

在选择一张图片后，`Inspector` 窗口就会显示图片的信息

![image-20221008130511663](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210081305755.png)  

### **关于窗口的具体信息**

| Texture Type（纹理类型）          | z**需要用于Tilemap的图片材质必须为 Sprite（2D and UI)**      |
| --------------------------------- | ------------------------------------------------------------ |
| **Sprite Mode（模式）**           | **需要用于切割的图片必须都设置为 Multiple（多个）**          |
| **Pixels per Unit（每单位像素**） | **数字越小，在场景当中显示出来的图片越大（一般设置为16）**   |
| **Filter Mode（过滤模式）**       | **Point用于像素画风的游戏，Bilinear是用于正常画风的游戏，自行调整** |

上述内容为需要调整设置，其他内容可以自行了解

![image-20221008150150396](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210081501427.png)

在调整好设置后滑动到最下方点击 `Apply` 保存设置，然后点击 `Sprite Editor` 进行图片切割

**注：此素材的每个格子尺寸为16，所以`Pixels Per Unit`设置为16，可以试试设置成其他大小例如32，看看效果会是什么样子**

### 关于图片切割

这里因为我们知道每个格子的大小，所以我们可以按照尺寸切割。`Type `改为 `Grid By Cell Size` 尺寸设置为16*16直接选择`Slice` 进行切割即可，最后选择 `Apply` 完成切割，素材就已经准备好了。

<img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210081511640.png" alt="image-20221008151126579" style="zoom:67%;" />  

**PS：在 `type` 中我们可以选择 `Grid By Cell Count` ，切割时候的参数不是 `Pixel Size` 而是 `Column & Row` ,这个可以根据自己的需求切割成几行几列**

### 创建画板

选择 `Window` 下 2D 中的 `Tile Palette` 打开调色板，选择`Create New Palette`创建自定义调色板。创建好后拖入我们准备好的素材。

<img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210081523222.png" alt="image-20221008152330173" style="zoom:67%;" />

<img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210081524259.png" alt="image-20221008152420219" style="zoom:67%;" />

<img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210081525747.png" alt="image-20221008152511686" style="zoom:67%;" />



### Tilemap绘制场景

在 `Hierarchy `面板下右键- >`2D Object`-> `Tilemap` 创建 `Tilemap` ，之后选择笔刷（快捷键B）开始绘画

<img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210081528952.png" alt="image-20221008152822905" style="zoom:67%;" />

<img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210081531242.png" alt="image-20221008153148148" style="zoom: 50%;" />

**注： 在进行绘制场景时，记得确实是否选取了 `Hierarchy` 窗口里面的 `Tilemap` ， 确认选取后再打开你的 `Tile Palette` 窗口进行绘画，在绘制场景时，一些 player 可以进行碰撞的地形和无需碰撞的背景图片，需要创建多个 `Tilemap` 进行绘制，例如 player 需要站在地图的地板上，此时这个地板是需要添加碰撞的（下面会说到），所以需要用一个 `Tilemap` 来绘制需要碰撞的地板，其次无需碰撞的一些场景，例如场景中的花草树木等，不会和 player 发生碰撞，就需要再创建一个没有添加碰撞的 `Tilemap` 单独绘制这类型的场景**





## 规则瓦片地图（快捷绘制场景）

(新版 unity 的 package 中会带有)

在 `Project `右键中创建一个 `Rule Tile` 点击使用

---

如果没有就按下面方法安装

windows->package manager 左上角加号添加git（输入此链接）：

https://github.com/Unity-Technologies/2d-extras.git

完成后 in project会显示2D Tilemap Extras

---

<img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210081549492.png" alt="image-20221008154955413" style="zoom: 50%;" />  

具体如何使用可以自己摸索也可以参考视频（[仙人指路👈](https://www.bilibili.com/video/BV1FE411f7VJ/?vd_source=8ebf6977c2a4fd938f57c4e2d108ae64)）



## 地图的Collider（碰撞）

在绘制好我们的场景之后，我们的 player 是需要站在我们的地形上，player 是添加了 `collider` 的，我们的地形也要添加 `collider`

选择你需要添加 `Collider` 的 `Tilemap`

在 `Inspector` 窗口中点击 `Add Component` 添加组件

可以直接搜索 `collider2d`，选择 `Tilemap Collider 2D`

添加成功后你的 `Scene` 窗口中绘制的每个图片都会有个绿色的框

![image-20221008161353364](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210081613473.png) 

![image-20221008162028087](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210081620134.png)

### Composite Collider 2D复合碰撞器

在添加了 `Tilemap Collider 2D`后 player已经可以实现在站在场景上了，但是实际上，在运行过程中，玩家在地图上移动会发现出现卡顿的问题（被地图给绊住了）或者直接穿过了地图的缝隙，因为地图目前的碰撞是一个一个格子，并没有和到一起，这个时候我们就需要 `Composite Collider 2D`  （复合碰撞器）

在添加了 `Tilemap Collider 2D` 的 `Tilemap`中，添加 `Composite Collider 2D`

添加了 `Composite Collider 2D` 后 会自动给此项目添加 `Rigdbody2D` ，所以需要更改一些设置

![image-20221008163111646](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210081631687.png)

在  `Tilemap Collider 2D` 中勾选 `Used By Composite` 启用复合碰撞器

在 `Rigidbody 2D` 中，将 `Body Type` 设置为 `Static ` 静态的，不然运行时绘制的场景会掉出屏幕

然后你绘制的场景的绿色方框会直接整合到一起，即添加成功

<img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210081634033.png" alt="image-20221008163432794" style="zoom:67%;" />

<img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210081637939.png" alt="image-20221008163749555" style="zoom: 67%;" />





## 可能遇到的问题

### 复合器问题

不同的脚本会有不同的效果，可能会有人遇到 player 明明站在平整的地板上，但是会显示 player 是处于下降或者上升的状态（可以查看 `Rigidbody2D`  里面的 `Velocity` 的x，y是否在变化，正常情况 player 静止时是没有变化的，如果有变化说明地板是有问题的（地板倾斜）

（小技巧：`Velocity` 可以认为是这个物体的移动速度，在某些时候说不定会有意想不到的作用🌟）

**解决办法**

在 `CompositeCollider2D` 中 需要**把 `Offset Distance` 数值改为0**



### 关于 `Tile Palate` 中素材和格子错位的问题

错位问题如下图所示：

<img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210081639752.png" alt="image-20220605230925921" style="zoom:67%;" />

素材没有完整布满格子

**解决办法**

问题出现的原因是因为在切割图片时 `pivot` 没有选择为 `center` ，如图改为 `center` 后正常显示

<img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210081639859.png" alt="image-20220605231315725" style="zoom: 50%;" />





### 方块碰撞体不均匀

如果你的场景中添加了斜面，仔细放大观察有些素材的斜面 `Collider` 是不均匀的，这会导致 player在斜坡上面移动时动画会出现一些bug

所以我们需要对这些特殊的方块的 `Collider` 进行更改

![image-20221008224214486](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210082242533.png)

打开你的图片的 `Sprite Editor` ，将你的  `Sprite Editor`  模式切换为 `Custom Physics Shape` 

选择需要更改的方块，点击 `Generate` 会出现此方块的 `Collider` 的具体显示，自己手动调整保存即可

<img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210082246186.png" alt="image-20221008224649126" style="zoom:67%;" />



