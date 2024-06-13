# 1.Editor-graphic-渲染轴向

改变自定义渲染轴向Z为Y轴，避免频繁设置Order in layer选项。也可以改变图片根据轴心点而不是中心渲染，用Sprite Editor自定义轴心点

![image-20240329193640483](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329193640483.png)

![image-20240329193719978](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329193719978.png)

![image-20240329193837353](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329193837353.png)

# 2.地图遮挡物设置

一些遮挡物额外制作一直图片覆盖到原图上，Order in layer设置大一点，用于经过时遮挡游戏对象

![image-20240329193925577](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329193925577.png)

![image-20240329193955029](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329193955029.png)

![image-20240329194121344](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329194121344.png)

![image-20240329194137782](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329194137782.png)

# 3.抖动的解决

把自己通过计算控制游戏对象的移动，改为控制游戏对象的刚体进行移动，解决自己的计算移动和物理系统的碰撞冲突

![image-20240329194215239](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329194215239.png)

# 4.动画的Exit Time

退出时间表示的是播放到整个动画什么比例就退出，进行过渡，过渡时间就是从一个动画过渡到里一个动画的时间

![image-20240329194255244](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329194255244.png)

# 5.动画混合

一个动作可能由多个动画组合而成，连线复杂，可以用混合树坐标系来解决

![image-20240329194403838](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329194403838.png)

![image-20240329194439267](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329194439267.png)

# 6.Cinemachine插件

建立虚拟相机CinemachineVirtualCamera组件(虚拟相机拍摄到画面，发送给主摄像机渲染)，实现摄像机移动(Follow属性：跟随...移动)
防止移动露出地图范围外，限制摄像机移动范围，CinemachineConfiner2D组件，设置地图碰撞器(触发器)，地图加Polygon2D碰撞器。

![image-20240329194816025](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329194816025.png)

![image-20240329194845971](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329194845971.png)

![image-20240329194606652](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329194606652.png)

![image-20240329194652757](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329194652757.png)

![image-20240329194731544](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329194731544.png)

# 7.攀爬系统

特定区域添加Polygon2D碰撞器，设为触发器，开始触发时显示攀爬动作，结束触发时显示正常动作

![image-20240329194922098](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329194922098.png)

![image-20240329194949216](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329194949216.png)

![image-20240329195014792](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329195014792.png)

# 8.头顶血条

Canvas改为World Space模式，Canvas设置为跟随的人物的子对象，Canvas缩放100倍，，创建一个血条图片

![image-20240329195227878](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329195227878.png)

![image-20240329195241107](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329195241107.png)

![image-20240329195252517](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329195252517.png)

![image-20240329195319671](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329195319671.png)

# 9.Mask遮罩组件

填充异形图片：给需要遮罩的图片添加父对象，父对象添加Image组件（用于设置图片：遮罩成？样式的图片），Mask组件（实现遮罩样式）

![image-20240329195430588](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329195430588.png)

![image-20240329195441931](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329195441931.png)

![image-20240329195501808](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329195501808.png)

![image-20240329195527310](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329195527310.png)

![image-20240329195551534](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329195551534.png)

# 10.UI管理者

管理UI

![image-20240329195628161](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329195628161.png)

# 11.数据管理者

管理Luna角色数据，任务数据等

![image-20240329195652756](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329195652756.png)

# 12.战斗界面搭建

触发战斗，显示；离开战斗，隐藏。

![image-20240329195733338](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329195733338.png)

![image-20240329195752485](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329195752485.png)

# 13.DoTween插件

## 1.方法

这些方法不需要放在Update函数里面，也会实现效果。
1.移动到指定目标点（相对世界坐标系）this.transform.DoMove(targetTrans,moveTime)。一个方法直接完成移动，指定目标点和移动时间，不需要自己去实现移动逻辑。DoLocalMove（相对本地坐标系）

2.旋转指定角度this.transform.DoRotate(Vector3,rotateTime)。一个方法直接完成旋转，指定欧拉角度和旋转时间，不需要自己去实现旋转逻辑

3.震动DOShakePosition(duration,intensity,...)。震动，持续时间和强度，其他为默认值

4.这些都是动画，返回值可以用Tween类接收对象object。object.Pause()暂停，object.Play()播放。通过对象可以在Update()里面监听指定按键再播放，object.PlayForward()正播，object.PlayBackwards()倒播。不能连续正播或倒播

5.object.OnCompleted(function)：动画完成事件函数，传入一个回调函数，动画播放完毕后执行。

6.object.Kill()。销毁动画对象。释放内存。

7.Sequence 动画序列对象。Sequence sequence = DOTween.Sequence();创建一个动画序列池，sequence.Append()往里面依次加入动画，依次播放。

8.SetEase()，设置动画函数，使动画更加真实

9.DoFade(),渐变到某个透明值。SpriteRender可以调用这个方法，渐隐渐显的效果。

# 14.跳跃系统(DoTween插件实现移动)

-指定区域为跳跃区域，添加BoxCollider2D组件，设为触发器。添加两个子对象作为跳跃落点（注意不要在碰撞器内部）。触发时，播放跳跃动画，跳跃到对面的落点。
跳跃时，刚体休眠，利用DoTween的DoMove方法进行跳跃位移，结束后，利用OnCompleted函数，开启刚体，切换到正常动画(刚体不休眠，会受到物理系统影响，跳不过去，有碰撞器阻碍。)
DoMove时，如何确定哪个是目标点？换句话说，如何确定是向上跳还是向下跳？通过距离判断！距离远，说明就是DoMove的目标点。

![image-20240329195901338](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329195901338.png)

![image-20240329195934317](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329195934317.png)

![image-20240329195948466](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329195948466.png)

![image-20240329200002214](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329200002214.png)

![image-20240329200026476](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329200026476.png)

![image-20240329200110259](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329200110259.png)

![image-20240329200153672](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240329200153672.png)

# 15.战斗系统

通过协程，分时分步实现回合制。

按钮事件监听通过Inspector面板完成。

![image-20240330131601987](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240330131601987.png)

## 1.通过协程，完成攻击实现

Luna攻击，攻击完成；再怪物攻击，攻击完成。

记录好初始位置。

![image-20240330130102221](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240330130102221.png)

Luna攻击的协程（yield return分时，根据上面的逻辑执行需要消耗的时间，返回指定的时间，决定下面的逻辑要等待多少s后才可以执行。）

![image-20240330125843510](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240330125843510.png)

Luna攻击完成。怪物攻击。

![image-20240330130020225](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240330130020225.png)

## 2.通过协程，完成防御实现

![image-20240330131816556](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240330131816556.png)

## 3.HP,MP的改变。通过MP判定技能的释放

![](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240330140521962.png)

![image-20240330133425292](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240330133425292.png)

## 4.通过协程，完成攻击技能、回血技能实现(技能系统)。

![image-20240330140339989](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240330140339989.png)

![image-20240330140404180](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240330140404180.png)

![image-20240330140251843](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240330140251843.png)

![image-20240330140304778](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240330140304778.png)

## 5.完善怪物和玩家的扣血机制

玩家扣血。

![](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240330160003237.png)

怪物扣血。

![image-20240330155847094](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240330155847094.png)

![image-20240330160318297](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240330160318297.png)

## 6.逃跑实现

![image-20240330161751987](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240330161751987.png)

每次进入战斗时（激活对象），初始化一些信息。避免上次战斗（失活对象）遗留下来的数据。比如Luna逃跑时（向后退五格）的位置遗留。怪物的渐变效果等。

![image-20240330161740729](D:\Unity资料和笔记\个人笔记\2DRPG-Luna's Adventure_Image\image-20240330161740729.png)

# 16.任务和对话系统

通过一个对话列表管理相关对话和任务信息。通过索引遍历显示这些信息。通过游戏管理器管理任务条件和更新。对话时根据任务条件显示合适的对话。
