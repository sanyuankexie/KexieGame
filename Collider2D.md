<p align="center"><a href = "https://hello.kexie.space/"><img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210111634004.png" width="250" /></a></p>

# Collider2D

## 概述

顾名思义（顾不出来就翻译一下），**碰撞器**，就是检测物体与物体之间发生碰撞的组件。

当两个碰撞器组件接触时，会产生互相碰撞的效果，同时，函数`OnColliderEnter2D(collider2D collider)` 之类的函数会被调用，我们可以通过编写脚本以实现各种我们想要的功能。

<img src="https://raw.githubusercontent.com/qissqi/img/main/tmpImg/3.gif" alt="3" style="zoom:80%;" />

这`Collider2D`一类别下有许多形状，可以根据自己的需要选择不同的形状，也可以添加多个进行组合。

详细如下

## 组件内容

### 基本形状的Collider

虽然`Collider2D`有各种形状来区分，但是他们都同属于`Collider2D`类，不同的形状使用的效果就不同，比如使用圆形或椭圆的Collider就可能会产生滑动或滚动的情况。

![椭圆碰撞器](https://raw.githubusercontent.com/qissqi/img/main/tmpImg/image-20221012203052595.png)

这里就以Box Collider2D作为例子，其他形状也是类似的，有的形状D可能会少一点或多一点内容，不过都好理解。

![盒状碰撞器](https://raw.githubusercontent.com/qissqi/img/main/tmpImg/image-20221012212825928.png)

我们看看这里面的都是什么东西：



`Edit Collider`

这是一个按钮，点击时会变成白色，此时你可以在编辑窗口里**对碰撞器的形状大小进行直观修改**。

`Meterial`

碰撞器的物理材质，可以放入一个`Physics Meterial 2D`材质文件，通过材质改变其弹性和阻力。

`Is Trigger`

是否作为触发器。这个选项比较重要，如果不勾选，物体会和碰撞器产生碰撞效果（即两个物体无法穿过彼此），

但如果勾选上，物体则**失去碰撞效果**，但是可以作为检测范围内物体的工具，在脚本中搭配`OnTriggerEnter2D(Collider2D other)`等其他函数使用即可实现触发器的功能。比如做成陷阱的检测区域，当玩家经过Collider的区域时触发陷阱。

`Used By Effector`

由效果器使用。这里的效果器是指Unity中的一些组件，他们需要`Collider2D`的支持才能发挥作用，当你添加了这类组件时，记得把这个勾选上。

`Used By Composite`

由复合器使用。这个道理同上，也不细说了。

`Auto Tiling`

自动平铺。这个功能比较特殊，他需要在你游戏物体的`Sprite Renderer`组件中，将`DrawMode`选择为`Tiled`才能生效，作用是在你扩大图片尺寸时自动补上碰撞区域。

这一部分可能还听不太懂，涉及到了`SpriteRenderer`，不过这部分的确不重要可以跳过，感兴趣的可以自己整一个自己动手试试就知道了。

`Offset`、`Size`、`EdgeRadius`

这几个属性就是关于碰撞器面积的数值啦，通常情况下用`Edit Collider`手动调整即可，当然如果你有强迫症就在这里微调吧。

### Edge Collider2D

这个碰撞器比较特殊，是一条线（但也可以不只是一条线），检测时也只检测线的碰撞

在启用编辑时，鼠标放上线段中间时会出现一个点，拖动它，直线就会多一个点变成折线。按住Ctrl键，再点击线段，变红的线会被删除。

因为用的不多，所以不特别讲，够用就行。

![image-20221012202217849](https://raw.githubusercontent.com/qissqi/img/main/tmpImg/image-20221012202217849.png)

### Polygon Collider 2D

多边形碰撞器。看上去就是很多线段围成的多边形啦，使用的方法同上，需要时再用即可。

![image-20221012202610609](https://raw.githubusercontent.com/qissqi/img/main/tmpImg/image-20221012202610609.png)

### Tilemap Collider 2D

网格地图碰撞器。由专门的Tilemap笔记来讲~~



## 如何使用？

### 作为碰撞器

顾名思义中的顾名思义，就是让两个物体产生碰撞的效果。比如作为墙壁、地面使其与人物或其他物体产生碰撞。

不过这里还是需要注意，如果需要为碰撞使用，那么需要**两个物体中至少其中一个添加了`rigidbody2D`组件且为`Dynamic`类型**，否则是无法碰撞的。



### 作为触发器

正如上面提到的一样，当`Collider2D`勾选上`IsTrigger`后，它将不再有碰撞的效果，物体可以穿入`Collider2D`内以达到触发的效果。但**不勾选此选项也是可以做到**的。比如：玩家碰到敌人后掉血、玩家进入告示牌范围自动弹出提示框。玩家和敌人之间可以碰撞，一样可以用作受伤的触发效果，但我们不希望告示牌拦住我们的行动，此时就可以勾选`IsTrigger`来使用了。

所以作为触发器的话，需要`Collider2D`和`Collider2D`之间的接触来触发（不管有没有勾选IsTrigger都可以做到）。想要实现其功能就需要配合代码来实现了。常用的函数有以下几个：

|                  函数                  |                  触发时机                  |
| :------------------------------------: | :----------------------------------------: |
| OnTriggerEnter2D(Collider2D collision) | 进入触发器时（由未接触变为接触，执行一次） |
| OnTriggerStay2D(Collider2D collision)  |     在触发器内时（保持接触，每帧执行）     |
| OnTriggerExit2D(Collider2D collision)  | 离开触发器范围时（退出接触状态，执行一次） |

使用这几个函数时千万不要忘了有2D这两个字。



简单做个样例：我们写一个陷阱地板的代码，当玩家踏入触发器的范围时，让陷阱地板消失。

```
private void OnTriggerEnter2D(Collider2D collision)
{
	//传入的参数"collision"是接触了的的其他Collider2D
	if(collision.CompareTag("Player"))	 //这里我们通过检测物体Tag来判断玩家是否进入陷阱
	{
		gameObject.setActive(false);
	}
}
```

![2](https://raw.githubusercontent.com/qissqi/img/main/tmpImg/2.gif)



## 小拓展



*在实际开发的过程中可能会遇到这样的问题*：

如果我让玩家和敌人都有了`Collider2D`和`Rigidbody2D`使他们能站在地面上，并且**玩家接触敌人时会受伤**，但我**不希望敌人和玩家产生碰撞效果**该怎么做呢？



首先，在Unity界面上方中依次选择`Edit`、`Project Settings..`，在这个窗口中选择`Physics2D`选项，可以看到下方有很多框框勾勾。

![image-20221012211816254](https://raw.githubusercontent.com/qissqi/img/main/tmpImg/image-20221012211816254.png)

<img src="https://raw.githubusercontent.com/qissqi/img/main/tmpImg/image-20221012212015842.png" alt="image-20221012212015842" style="zoom:80%;" /> 

这些勾勾可以简单理解为**哪些物体可以和哪些物体发生碰撞**。当我们**取消**其中的勾选，那么勾选框左方和上方的图层将**不会发生碰撞（包括触发器效果也无法使用）**。

所以，我们要利用到**`Layer`(图层)**来区分碰撞是否发生，就是在Inspector窗口头顶右边的`Layer`选项，我们就可以利用这个东西实现我们的效果。（有想法的可以自己先思考一下）

![image-20221010174344038](https://raw.githubusercontent.com/qissqi/img/main/tmpImg/image-20221010174344038.png)



这时候，我们可以利用3个图层来区别。

第一个图层`Enemy`直接加给敌人就可以，敌人的碰撞器保持正常（可以发生碰撞）

<img src="https://raw.githubusercontent.com/qissqi/img/main/tmpImg/image-20221012212334408.png" alt="image-20221012212334408" style="zoom:80%;" /> 

然后我们给主角两个图层

一个可以命名为`Player`，直接加给玩家，此时玩家的`Collider2D`组件**勾选`IsTrigger`**。

<img src="https://raw.githubusercontent.com/qissqi/img/main/tmpImg/image-20221012212240022.png" alt="image-20221012212240022" style="zoom:80%;" /> 

然后为玩家添加一个子物体，可以将其图层命名为`Body`，并添加`Collider2D`组件，保持正常碰撞。

<img src="https://raw.githubusercontent.com/qissqi/img/main/tmpImg/image-20221012212444406.png" alt="image-20221012212444406" style="zoom:80%;" /> 

最后，在之前的`Physics2D`界面中**取消勾选`Body`和`Enemy`之间的框**，使这两个图层不会碰撞

<img src="https://raw.githubusercontent.com/qissqi/img/main/tmpImg/image-20221012212551465.png" alt="image-20221012212551465" style="zoom:80%;" /> 

这样，我们的玩家就可以不与敌人产生碰撞，但也能检测碰撞伤害了（如果需要用tag检测也不要忘记添加tag）。

```
private void OnTriggerEnter2D(Collider collision)
{
	if(collision.CompareTag("Enemy"))
	{
		Debug.Log("Hurt!");
	}
}
```



![001](https://raw.githubusercontent.com/qissqi/img/main/tmpImg/001.gif)
