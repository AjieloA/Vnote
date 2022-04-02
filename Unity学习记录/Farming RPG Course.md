# Farming RPG Course
[toc]
** **
** **
** **
## 2022.04.02

### 导入包：
* <font color=red>**2DSprite**</font>,<font color=red>**2D Pixel Perfect**</font>,<font color=red>**Cinemachine**</font>,<font color=red>**Timeline**</font>,<font color=red>**TextMesh Pro**</font>,<font color=red>**Starterassets**</font>
### 新建主场景：
* <font color=red>**PersistentScene**</font>
* <font color=red>**Project Settings**</font>
    * <font color=Coral>**Graphics**</font>:
        * <font color=BlueViolet>**Transparency Sort Mode**</font>:Custom Axis
        * <font color=BlueViolet>**Tansparency Sort Axis**</font>:X>0,Y>1,Z>0
    * <font color=Coral>**Quality**</font>:Anti Aliasing(抗锯齿):Disabled
### 相机设置
* 新建相机：Cinemachine>Create Virtual Camera>(Rename)PlayerFollowVirtualCamera
    * <font color=red>**CinemachineVitualCamera**</font>常用模式属性解析
        * <font color=Coral>**Status:Live [Solo]**</font>：用于调试。选中这个按钮时，此虚拟相机会直接控制Unity相机显示在Game窗口中，用于相机的调试。（忽略优先级，但是需要所在GameOjbect是激活状态）
        * <font color=Coral>**Game Window Guides**</font>：勾选时，Game窗口会显示辅助线，用于设置虚拟相机的各个属性。需要注意的是，仅在以下任一属性赋值时显示辅助线：
            * <font color=BlueViolet>**Look At**</font>:设置了物体，Aim设置为Composer或Group Composer
            * <font color=BlueViolet>**Follow**</font>:设置了物体，Body设置为Framing Composer
        * <font color=Coral>**Save During Play**</font>：虚拟相机的属性在运行时的修改可以被保存下来，退出Play状态时不会被重置。 
        * <font color=Coral>**Priority**</font>:虚拟相机的重要程度，用于Live镜头的选择。数值越高代表优先级越高。Cinemachine Brain会根据这个属性从所有激活的虚拟相机中选择活动的虚拟相机。在Timeline上使用时这个属性不生效。
        * <font color=Coral>**Follow**</font>:虚拟相机会跟随这个物体移动。Body属性基于这个目标物体更新Unity相机的位置。如果你想让相机保持它自己的位置不变，不要设置这个属性。
        * <font color=Coral>**Look At**</font>:镜头瞄准的物体目标。Aim属性使用这个属性来更新Unity相机的旋转。如果你想相机保持原来的角度，不要设置这个属性。
        * <font color=Coral>**Standby Update**</font>:待命时的更新方式，当虚拟相机物体没有被禁用，但是优先级不足时，虚拟相机处于待命状态。这个属性会影响性能，通常设置为Never是最好的，但是有时候可能需要虚拟相机更新来做一些镜头相关的计算判断。
            * <font color=BlueViolet>**Never**</font>: 不更新
            * <font color=BlueViolet>**Always**</font>:每帧更新
            * <font color=BlueViolet>**Round Robin**</font>:循环更新，所有的待命虚拟相机循环更新，每帧更新一个相机（例如有10个待命的相机，第一帧更新第一个相机，第2帧更新第2个相机，….，第11帧更新第1个相机，以此类推）
        * <font color=Coral>**Lens**</font>:镜头设置
            * <font color=BlueViolet>**Orthographic Size**</font>:size的值是正交摄像机高度的一半。当你拉伸屏幕窗口的时候，摄像机的高度不变，宽度改变。size=分辨率宽度/Pixels Per Unit/2
            * <font color=BlueViolet>**Near Clip Plane**</font>:近裁剪平面，摄像机最近的渲染位置
            * <font color=BlueViolet>**Far Clip Plane**</font>:远裁剪平面，摄像机最远的渲染位置，摄像机会渲染处于近裁剪平面与远裁剪平面之间的物体
            * <font color=BlueViolet>**Dutch**</font>:摄像机Z轴的旋转角度
        * <font color=Coral>**Transitions**</font>:
            * <font color=BlueViolet>**Blend Hit**</font>:混合方式
                * None 无，默认线性混合
                * Spherical Position 根据Look At的物体球面旋转混合
                * Cylindrical Position 根据Look At的物体柱面旋转混合（水平方向圆弧，垂直方向线性）
                * Screen Space Aim When Target 在屏幕空间瞄准目标
            * <font color=BlueViolet>**Inherit Positon**</font>:下一个相机变成活动相机时，从上一个相机继承位置，即保持两个相机位置相同。
            * <font color=BlueViolet>**On Camera Live**</font>:相机变为活动时会触发对应的事件。
        * <font color=Coral>**Body**</font>:
            * <font color=BlueViolet>**Do Nothing**</font>：虚拟相机激活时，会控制Unity相机会固定在当前虚拟相机的位置，不会移动。用于固定位置的镜头，也可以通过自定义脚本来控制相机的位置。通常和Look At配合使用，模拟固定位置的跟随镜头。
            * <font color=BlueViolet>**Framing Transposer**</font>：为2D和正交相机设计的，主要用于2D情况。但是对于透视相机和3D环境也可以使用。Cinemachine会在屏幕空间将相机和跟随物体保持固定的相对位置关系。只会改变相机的位置，不会改变相机的旋转。
                * 特别注意：使用Framing Transposer时，Look At属性必须为空。
                * Lookahead Time：根据目标的运动，调整虚拟相机与“跟随”目标的偏移量。Cinemachine预测目标在未来数秒之内到达的位置并提前设置Unity相机的位置。这个功能对微动的动画敏感，并且会放大噪点，导致非预期的相机抖动。如果不能接受目标运动时的相机抖动，减小这个属性可能会使相机动画更流畅。
                * Lookahead Smoothing：预测算法的平滑度。较大的值可以消除抖动但会使预测滞后。
                * Lookahead Ignore Y：如果选中，在预测计算时会忽略沿Y轴的移动。

                * X Damping：相机在X轴上移动的阻力系数。较小的值会使相机反应更快。较大的值会使相机的反应速度变慢。每个轴使用不同的设置可以制造出各种类型相机的行为。
                * Y Damping：相机在Y轴上移动的阻力系数。较小的值会使相机反应更快。较大的值会使相机的反应速度变慢。
                * Z Damping：相机在Z轴上移动的阻力系数。较小的值会使相机反应更快。较大的值会使相机的反应速度变慢。
                * Screen X：目标的水平屏幕位置。相机移动的结果是使目标处于此位置。
                * Screen Y：目标的垂直屏幕位置，相机移动的结果是使目标处于此位置。
                * Camera Distance：沿摄像机Z轴与跟随目标保持的距离。
                * Dead Zone Width：当目标在此位置范围内时，不会水平移动相机。
                * Dead Zone Height：当目标在此位置范围内时，不会垂直移动相机。
                * Dead Zone Depth：当跟随目标距离相机在此范围内时，不会沿其z轴移动相机。

                * Unlimited Soft Zone：如果选中，                Soft Zone没有边界
                * Soft Zone Width：当目标处于此范围内时，会水平移动相机，将目标移回到Dead Zone中。Damping属性会影响摄像机的运动速度。
                * Soft Zone Height：当目标处于此范围内时，会垂直移动相机，将目标移回到Dead Zone中。Damping属性会影响摄像机的运动速度。
                * Bias X：Soft Zone的中心与目标位置的水平偏移。
                * Bias Y：Soft Zone的中心与目标位置的竖直偏移。
                * Center On Active：选中时，虚拟相机激活时会将镜头中心对准物体。不选中时，虚拟相机会将目标物体放置在最近的dead zone边缘。
            * <font color=BlueViolet>**Hard Lock to Target**</font>：虚拟相机和跟随目标使用相同位置。
                * Damping：相机追赶上目标位置的时间。如果为0，那就是保持同步，如果大于0，相当于经过多少秒相机和目标位置重合。
            * <font color=BlueViolet>**Orbital Transposer**</font>：相机和跟随目标的相对位置是可变的，还能接收用户的输入。常见于玩家控制的相机。
                * Orbital Transposer引入了一个新的概念叫heading，代表了目标移动的方向或面朝的方向。
                * Orbital Transposer会尝试移动相机，让镜头朝向heading的方向。默认情况下，相机的位置会在target的正后面。也可以通过Heading Bias属性设置。
            * <font color=BlueViolet>**Tracked Dolly**</font>：相机沿着预先设置的轨道移动。
            * <font color=BlueViolet>**Transposer**</font>：跟随目标移动，并在世界空间保持相机和跟随目标的相对位置固定。
    * 项目属性设置:
        * Lens>Orthographic Size>8.4375
        * Body>Framing Transposer
            * X/Y/Z Damping>0.7
            * Dead Zone Width/Height>0.1
        * Aim>Do nothing
        * Main Camera
            * Background>黑色
            * 添加Pixel Perfect Camera
                * Assets Pixels Per Unit>16
                * Reference Resolution X>480 Y>270
                * Crop Frame X✔ Y✔
                    * Stretch Fill ✔
***
***
***
## 2022.04.02
### 角色构建
* >Player
    * Rigidbody 2D
        * Collision Detection>Continuous
        * Interpolate>Interpolate
        * Constraints
            * Freeze Rotation ✔Z
    * Sorting Group
        * Sorting Layer>Instances
    * >Body
        * Sprite Renderer>customised_farmer_0
        * Sorting Layer[Instances]>1
    * >Hair
        * Sprite Renderer>customised_hair_0
        * Sorting Layer[Instances]>2
    * >Hat
        * Sprite Renderer>Transparent16X16
        * Sorting Layer[Instances]>3
    * >Ams
        * Sprite Renderer>customised_farmer_6
        * Sorting Layer[Instances]>4
    * >Emquippedltem
        * Sprite Renderer>Transparent16X16
        * Sorting Layer[Instances]>5
    * >Tool
        * Sprite Renderer>Transparent16X16
        * Sorting Layer[Instances]>6
    * >ToolEffect
        * Sprite Renderer>Transparent16X16
        * Sorting Layer[Instances]>7
### 脚本创建
* SingletonMonobehaviour
    * abstract>抽象类，被继承后可以使用virtual关键字被重写；
    * SingletonMonobehaviour<T> >泛型；
    * MonoBehaviour where T:MonoBehaviour >泛型约束，使用泛型指定类型时，有一定的规则；
```
public abstract class SingletonMonobehaviour<T> : MonoBehaviour where T:MonoBehaviour
{
   //单例
   private static T instance;
   //通过Instance间接访问instance，只提供get方法，保证了instance的安全性；
   public static T Instance
   {
       get
       {
           return instance;
       }
   }
   //保证单例的唯一性；
   protected virtual void Awake()
   {
       if(instance==null)
       {
           instance = this as T;
       }
       else
       {
           Destroy(gameObject);
       }  
   }
}
```
* Player
```
public class Player : SingletonMonobehaviour<Player>
{
}
```


