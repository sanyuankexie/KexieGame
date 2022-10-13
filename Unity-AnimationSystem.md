<p align="center">
<a href = "https://hello.kexie.space/"><img src="https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210111634004.png" width="250" /></a>
</p>

# `Unity-AnimationSystem`

[TOC]

## 前言

在各类游戏中，各种游戏对象的动画无疑会为游戏增色不少。Unity引擎已经封装好了动画系统，方便开发者统一管理游戏中的各种动画。

## 前置知识

### 何为动画

大家都看过动漫、电影，这些视频都是由一张张静止的图片排列而成的。在视频播放时，每一秒都会按顺序依次显示若干张图片。因为人眼有视觉暂留的特性，当我们在短时间内看到这些连续的图片，就会感觉视频中的角色动了起来。

Unity引擎中的2D动画也是利用了这个特性，我们把角色的动作图片按照顺序摆放好，那么播放时，就可以看见角色动了起来。

在上文提到，动画在每一秒会显示若干张图片。我们把一秒中显示图片的张数称为帧率（Frame Per Second，可简写为FPS），比如某2D角色一秒播放30张图片，则称该角色的动画帧率为30。

### 如何管理繁杂的动画

在各类游戏，尤其是大型游戏中，角色动画十分繁杂，如何管理这些动画是一大问题。Unity引擎中的动画系统采用分层有限状态机（Hierarchy Finite State Machine）来管理动画。在继续往下深入前，建议先了解相关知识，这样可以更好地理解其工作原理。

## 动画组件（Animator）

### 动画控制器（Animator Controller）

在Unity引擎中，每个需要播放动画的角色都需要添加一个动画组件（Animator），并且引用相关的动画控制器（Animator Controller），如下。

![image-20221009133516294](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130835365.png)

双击动画控制器，可以打开Animator面板，这里就是之前提到的HFSM，Unity在这里将它可视化了，方便开发者看到所有的状态和分层，如下。

![image-20221009134357936](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130835934.png)

如果你了解过了FSM，那么这里应该比较容易理解：上图中的每一个矩形都是一个状态，对应着一段动画片段（Animation Clip），即角色的某个动作；而上图中的箭头代表着状态之间的转换，即角色从某个动作转换到另一个动作。

显然，我们的角色动作的改变是有条件的。这里以Idle到Attack为例，设置按下鼠标左键进入Attack动画。

首先点击左上角的+创建条件参数，类型为trigger，重命名为Attack，如下。

![image-20221009135419552](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130835797.png)

你在点击+号创建时可以看到条件参数有四种类型，float、int、bool、trigger，前三者应该不难理解，第四个参数trigger类型有一个特性，是触发后会立刻自动复位，见[Animator-SetTrigger - Unity 脚本 API](https://docs.unity.cn/cn/2019.4/ScriptReference/Animator.SetTrigger.html)。

我们将Idle到Attack的转换条件设置为条件参数Attack，并将退出事件等参数设置为0，使得转换后动作立刻改变。

这意味着Attack被触发时，若当前状态为Idle，则会立刻切换至Attack，如下。

![image-20221009143049763](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130836732.png)

怎么改变条件参数呢？一般而言，我们会自己写一个脚本来改变条件参数。这里我在Player上创建了一个名为PlayerAnim的脚本，单击鼠标左键时会触发Attack，如下。

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerAnim : MonoBehaviour
{
    private Animator anim;
    void Awake()
    {
        anim = this.GetComponent<Animator>();
    }

    void Update()
    {
        if (Input.GetMouseButtonDown(0))
            anim.SetTrigger("Attack");
    }
}

```

运行演示如下。

![AnimatorConditionParamExample 00_00_00-00_00_30](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210131051332.gif)

在如上视频中，我们成功切换了动画状态，但Game窗口中的人物没有反应，这是因为我们还没有关联好动画片段（Animation Clip）。



### 动画片段（Animation Clip）

在Unity引擎中，真正存储角色动画信息的是动画片段（Animation Clip）中的关键帧，如下。

![image-20221009152907150](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130836683.png)

动画片段中可以存储很多信息，图片、位置、大小、旋转和颜色等。

根据动画片段的这个特性，游戏角色动画一般有两种实现方案：

一种是序列帧动画，它适用于2D游戏，在每个关键帧中只需要存储角色动画的图片即可，至于人物做什么动作，交给美术来画；

另一种是骨骼动画，需要配合，3D和部分2D游戏都可以用，它除了骨骼图片外，还要在每个关键帧中记录骨骼的位置、旋转、缩放等，如果想让角色的骨骼动画看起来不违和，K帧得交给专业人士，无经验者不容易做出合理的骨骼动画。



我们创建好一个动画片段后，双击它进入Animation窗口，这个界面有点像视频编辑器的视频轨道。我们可以把角色对应的动画图片素材拖入其中，调整好位置，如下。

![image-20221009150954050](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130836114.png)

角色处于Idle状态时，Idle动画应该设置为循环播放，这样看起来才不违和。

![image-20221009151711875](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130836243.png)

然后将动画片段关联到对应动画状态上，如下。

![image-20221009151241616](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130836808.png)

按照如上步骤设置好所有的动画状态后，再次运行，发现结果和预期符合得很好。

![AnimationClipRelated 00_00_00-00_00_30](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210131051644.gif)

如果想让角色从Attack自动转换为Idle，可以尝试如下设置参数，意为播放完Attack片段后，无需条件自动转换为Idle，如下。

![image-20221009155331173](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130837655.png)



Unity的动画系统中还有其他功能，像apply root motion、animator override controller、avatar mask等，可以查看[Animator 组件 - Unity 手册](https://docs.unity.cn/cn/2020.3/Manual/class-Animator.html)、[动画术语表 - Unity 手册](https://docs.unity.cn/cn/2020.3/Manual/AnimationGlossary.html)以及其他资料了解其功能。

## 更多的可能性

### 动画分层

如果把角色的动画状态全部摆放在同一层，那么随着状态数越来越多，Animator窗口中的箭头连线势必会越来越复杂，维护成本高。

![SpiderHell](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130837809.jpeg)

有句话说得好：软件开发中的任何结构性的问题，都可以通过分层来解决。

关于动画分层，可以参考[如何避免Unity动画状态机的蛛网地狱_真滋养的博客-CSDN博客](https://blog.csdn.net/zzy15627867866/article/details/109923727)，里面讲得比较清楚，要注意的是在Layer面板中越靠下的层优先级越高。

Unity中的动画状态切换条件和外部条件相分离，你可以认为它设置了一个作为中间层的条件数据层，使得内部动画状态与外部解耦。还有很多其他游戏引擎使用同一套思路来管理角色动画，比如虚幻。这种解耦无疑是结构良好的，便于维护的。

如果你想自己试着写一个敌人状态机，并自己写代码来管理动画片段，这也是可以的。我们可以使用代码来获取动画系统中的绝大部分参数，一般来说，做什么功能和操作都可以实现。相关参数及方法请查看[UnityEngine.Animator - Unity 脚本 API](https://docs.unity.cn/cn/current/ScriptReference/Animator.html)。

实际上，还有很多其他的动画解决方案，比如Playable动画系统等，可以自行了解，在此不多赘述。

### 录制动画

在上文的图片中，你肯定看见了播放键旁边的这个红色按钮，它就是录制按钮。

![image-20221009163657634](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130837641.png)

按下后，引擎会记录你在这一帧对对象参数进行的任何调整；再次点击按钮完成录制。

如果是录制角色的移动，一定要注意apply root motion的设置是否符合需求，可以参考[看懂unity的Animator中的Apply Rootmotion_mr.chenyuelin的博客-CSDN博客](https://blog.csdn.net/weixin_44453949/article/details/115273399)。

如果是录制UI动画，在某些需求量小的情况下，录制UI动画是一种可行的解决方案；而倘若需求量大，建议引入Dotween插件来写UI动画。

### 插入事件帧

如果想让动画播放到某一时刻时执行一个函数，可以点击如下按钮为某时刻插入一个事件帧。点击该事件帧选择好想要执行的函数方法即可。

![image-20221009164817577](https://shadow-fy.oss-cn-chengdu.aliyuncs.com/img/202210130837335.png)

这样做十分方便，但是不建议滥用事件帧，因为会增加维护成本。如果在你的需求或者策划案中，必须在动画播放到某时刻时调用某方法，且这样的需求量非常大时，推荐你自己获取对象的动画状态和动画播放时间，然后自己设计解决方案。获取这些数据并非难事，同上参考[UnityEngine.Animator - Unity 脚本 API](https://docs.unity.cn/cn/current/ScriptReference/Animator.html)。

## 动手实践 

1. 自己录制UI的渐入渐出、放大缩小、平移旋转的动画，调整curve，并使用代码修改动画器的播放速度（speed），以实现UI动画的正放和倒放，体会其过程与优缺点。
2. 单独写一个脚本来统一管理动画切换的条件参数，体会其过程。如果你没有角色图片素材，可以直接拿该文件同级目录下的Sprite中的角色图片素材使用。
3. 把角色动画分为若干层，体会分层的好处。

## 可参考资料

[Animator 组件 - Unity 手册](https://docs.unity.cn/cn/2022.1/Manual/class-Animator.html)

[Animator Controller - Unity 手册](https://docs.unity.cn/cn/2022.1/Manual/class-AnimatorController.html)

[Animation Clip - Unity 手册](https://docs.unity.cn/cn/2022.1/Manual/AnimationClips.html)

[动画常见问题解答 - Unity 手册](https://docs.unity.cn/cn/2022.1/Manual/MecanimFAQ.html)

[看懂unity的Animator中的Apply Rootmotion_mr.chenyuelin的博客-CSDN博客_apply motion root](https://blog.csdn.net/weixin_44453949/article/details/115273399)

[如何避免Unity动画状态机的蛛网地狱_真滋养的博客-CSDN博客](https://blog.csdn.net/zzy15627867866/article/details/109923727)

[Unity中的动画系统和Timeline(4) AvatarMask和IK动画 - 穆玄琅 - 博客园 (cnblogs.com)](https://www.cnblogs.com/lmx282110xxx/p/10798687.html)

[Playables API - Unity 手册](https://docs.unity.cn/cn/2022.1/Manual/Playables.html)

[[Unity Playable动画系统取代蜘蛛网Animator - 三页菌 - 博客园 (cnblogs.com)](https://www.cnblogs.com/sanyejun/p/16194217.html)

[UnityEngine.Animator - Unity 脚本 API](https://docs.unity.cn/cn/current/ScriptReference/Animator.html)