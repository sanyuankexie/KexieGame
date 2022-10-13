<p align = "center"> 
<a href = "https://hello.kexie.space/"><img 
src = "https://hellokexie.obs.cn-north-4.myhuaweicloud.com/images/logo.png" width="250"></a>

# Rigidbody 2D（Unity 2D 刚体组件）

## 组件介绍

​	在各种各样游戏中，不同的物体会有不同的运动方式：玩家可以跳跃，敌人可以奔跑，子弹轨迹在空中划出优美的弧线。要实现上述的效果，都需要使用到一个对游戏开发至关重要的工具—— `物理引擎`。

​	而想让物体能够展现出我们的物理效果，就需要给场景中的物体加上我们的 **==Rigidbody 2D==（Unity2D 刚体组件）**。

## 组件的使用

### 如何添加

​	要学会怎么使用 <u>**Rigidbody 2D**</u>，就需要知道怎么给物体添加该组件。

> 1. 首先在 **Hierarchy** 窗口中 <u>选中</u> 需要添加 **Rigidbody 2D** 的 GameObject 
> 2. 在 **Inspector** 窗口中,点击 **AddComponent** ,并搜索 **Rigidbody 2D**
> 3. 点击组件完成添加

![image-20221012091949184](https://unity-note-image.oss-cn-guangzhou.aliyuncs.com/img/202210120919212.png)

### 组件效果

​	在给物体添加了Rigidbody 2D 组件后，该物体就 <u>可以产生相应的 `物理效果`</u>了。例如会受到重力的影响，自动向下移动，或是在受到其他物体的碰撞时，自身也被冲击力推出。

<img src="https://unity-note-image.oss-cn-guangzhou.aliyuncs.com/img/202210121416481.png" alt="image-20221012141601447" style="zoom: 67%;" />

​							*（可以看到椅子和酒瓶在炸弹爆炸后受到冲击，并朝着两侧飞出去*

### 如何使用

​	**Rigidbody 2D** 组件可以实现的功能多种多样:可以通过 Rigidbody 2D 移动物体,持续检测各个物体之间的碰撞,还可以通过给 Rigidbody 2D 添加 **Physical Material(物理材质)** 来模拟真实的物体碰撞效果(弹性,摩擦力等).

​	在游戏开发中,更改 **Rigidbody 2D** 参数的方法有两种:`第一种`是直接在 Inspector窗口 中进行修改;`第二种`也是最为重要的一种，是通过代码动态的对 **Rigidbody 2D** 进行修改.

​	下面将对上述的两种修改方式进行详细的介绍.




>**直接在 Inspector窗口 中进行修改**

​	直接修改的好处是,可以 <u>更直观</u> 的看到 Rigidbady 2D 被修改的参数的数值以及 <u>修改参数后的效果</u>,方便我们在 **UnityEditor** 运行游戏时 <u>对游戏进行测试</u>.

​	举个例子,当我们在点击游戏上方的开始按钮后,游戏将在 UnityEditor 运行,此时我们在 Inspector窗口 选中某个物体并对其 Rigidbody 2D 进行修改,就可以 **立刻在 Scene窗口 中看到物体被修改后的效果** ,就不需要再点击暂停进行修改,或是通过代码进行控制了.



​	**下面是一些常用参数的介绍：**

> 1. Body Type：修改此参数为 static 可以将物体固定
> 2. Material：修改此参数可以将物体改为拥有 **摩擦属性** 或 **碰撞时有弹性**
> 3. Mass：**刚体质量**，通过修改此参数可以提升物体的稳定程度
> 4. Gravity Scale：**刚体重力**，修改此参数可以修改物体的自由落体速度
> 5. Collision Dectection：可以选择物体是 **持续检测碰撞** 还是 **离散检测碰撞**
> 6. Sleeping Mode：选择刚体的唤醒模式（**游戏开始就唤醒** / **第一次碰撞时唤醒** / .......）
> 7. Constraints：可以 **冻结** 物体在某个方向上的移动

![image-20221012164703266](https://unity-note-image.oss-cn-guangzhou.aliyuncs.com/img/202210121647299.png)



​	当然,这样的修改方式也是有缺点的.首先在 Inspector窗口 中进行修改的方法,将会变成游戏开始后物体的<u>初始状态</u>.且在游戏进程中的数据修改在游戏结束后将会<u>清空</u>.

​	这就意味着,<u>这样的修改方式将 **只能对物体的初始状态进行修改** ,且不能在游戏运行的状态中对物体的各项数值进行动态的修改.</u>

​	再举个例子,角色再冲刺过程中,要是 **Rigidbody 2D** 一直产生作用那么角色的冲刺将会变得非常别扭:角色在冲刺过程中的运动轨迹将会变成诡异的抛物线,但因为你目前使用的对 Rigidbody 2D 的修改方式只是再 Inspector窗口 中进行修改,所以对这种情况毫无办法.

<img src="https://unity-note-image.oss-cn-guangzhou.aliyuncs.com/img/202210121344491.png" alt="image-20221012134408231" style="zoom: 50%;" />

<img src="https://unity-note-image.oss-cn-guangzhou.aliyuncs.com/img/202210121326405.png" alt="image-20221012132633072" style="zoom: 50%;" />

​	 *（由上图可知，当我们使用代码控制Rigidbody 2D时，就可以在游戏运行时控制物体的物理状态*



​	而此时你应该能想到:“我直接在冲刺过程中,把物体y轴上的移动冻结,等冲刺完成后再将其解冻不就行了吗?”

​	确实如此,当我们在角色进行某个行为的时候,对他的Rigidbody 2D组件进行适当的修改,就能够轻松的达到我们想要的游戏效果.这就是下面我要进行介绍的 <u>**通过代码控制动态的对 Rigidbody 2D 进行修改**</u> 的方法.



> **通过代码控制动态的对 Rigidbody 2D 进行修改**

​	通过代码对 **Rigidbody 2D** 进行控制时，首先需要 <u>**获得该组件**</u>。获得组件的第一种方式，是在脚本中写下一个 **RIgidbody 2D**  类型的公有（**public**）变量，然后到Inspector窗口右边将Rigidbody 2D组件拖拽到该变量上给他赋值。

![image-20221012182814494](https://unity-note-image.oss-cn-guangzhou.aliyuncs.com/img/202210121828536.png)

​	通过这种方式获取组件其实是 <u>十分不安全</u> 的做法，因为这样需要将 Rigidbody 2D 声明为 public 类型的变量，这样就会导致在任何地方都可以访问到该变量，并且可以更改其中的参数，导致程序可维护性降低。



​	而第二种方式就是通过代码获得组件，该方法可以在代码内部直接获取组件，不需要外部赋值，外部也无法访问该物体的 Rigidbody 2D 组件，相比于第一种获取方法是 <u>相对安全</u> 的。

~~~c#
public class myself
{
    public Rigidbody2D rb;	//创建一个Rigidbody2D类型的变量
    
    void start()	//Unity生命周期中的函数,在游戏开始时执行一遍
    {
        //获得Rigidbody2D组件的语句
        rb = GetComponet<Rigidbody2D>();
    }
}
~~~

​	在获得该组件后,我们终于可以 <u>通过代码</u> 对 Rigidbody 2D 进行操作了!!! 这真的太好了不是吗?!!



​	那么,该怎么样通过代码对 Rigidbody 2D 进行操作呢?下面将对常用的方法进行举例.

​	首先,我们最常用的就是通过 Rigidbody 2D 给物体施加 <u>力</u> 或是给物体一个 <u>速度</u>.代码如下:

~~~c#
//给物体施加一个水平方向的速度
rb.velocity = new Vector2(speed,rb.velocity.y);	

//给物体施加一个力,函数第二个形参表示施加力的方式
rb.AddForce(new Vector2(), ForceMode2D.Impulse);
~~~

​	

​	再有就是开头我们提及的,通过代码动态修改物体某个方向上的移动冻结

~~~c#
//将物体竖直方向上与旋转冻结
rb.constraints = RigidbodyConstraints2D.FreezePositionY | 	RigidbodyConstraints2D.FreezeRotation;
~~~

​	

​	其实关于 Rigidbody 2D 组件的控制,Unity官方还提供了很多公有的变量与方法,有相关需求的话可以自行查阅 [Unity官方中文文档](https://docs.unity.cn/cn/current/Manual/index.html) 中有关于 [Rigidbody 2D](https://docs.unity.cn/cn/current/ScriptReference/Rigidbody2D.html) 的部分.



## 关于Rigidbod 2D

​	如果你足够细心,就会发现到目前为止,我都在使用 Rigidbody 2D 来称呼Unity2D刚体组件,那么你一定会有一个问题:为什么不将其简称为 Rigidbody 或者是 rb 呢?

​	这是因为 Unity2D 与 Unity3D 所使用的物理引擎,是完全不同的两套物理引擎,而Unity已经将这两套物理引擎封装完善,供我们这些游戏开发者使用,所以看起来 Unity2D 与 3D 所使用的组件只有名字不同.将U3D的组件添加到2D项目中无法展现出对应的效果就是因为这个原因.

​	关于物理引擎的知识就点到这了,这里只是介绍Rigidbody 2D 这个组件,对物理引擎感兴趣的还可以到广大的互联网上寻找答案了.

