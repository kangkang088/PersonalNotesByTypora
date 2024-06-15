# Unity桌面精灵

<u>**转载自：B站：白菊花瓣丶[Unity 桌面精灵制作 一、 - 哔哩哔哩 (bilibili.com)](https://www.bilibili.com/read/cv6833637/?spm_id_from=333.999.collection.opus.click)**</u>

目录：
桌面精灵介绍

Hook简介

Hook 实现窗体透明

Hook 实现后台按键、鼠标事件响应

实现 2D 桌面精灵

## 桌面精灵介绍

最近看到B站很多主播都会整一个“桌面精灵”，即一个比较小巧卡通的角色形象，并且角色还会根据主播的按键和鼠标的移动还有鼠标按键做出同样的动作，来实时的显示主播的操作。我把这种角色形象称为桌面精灵。这个东西我第一次看到时就想用 Unity 实现一下，奈何自己的拖延症，一直到昨天才开始尝试。

## Hook 简介

Hook 是 桌面精灵制作中关键且核心的技术。

因为 桌面精灵 有两个特点：

1、需要游戏窗口没有边框，且游戏背景是透明的。 

2、需要能够在后台也可以接收到玩家的按键、鼠标输入信息。

对于窗体透明：在 Unity 中，我们游戏打包只有窗口、无边框窗口、全屏等选项，但是无法做到窗体背景透明，并且即使我们把摄像机的背景颜色的 alph 值设为 0，打包出的游戏，其背景也是 白色 或者 黑色。 而使用 Hook 我们就可以对窗体做一些个性化的设置。

对于后台检测输入：以QQ聊天框为例，如果我们点击了桌面上 QQ聊天框 以外的区域，那么我们敲键盘QQ聊天框是不会有任何响应的，我们没法在聊天框里打任何字。因为当我们点击了聊天框窗口以外的区域时，聊天框就进入了后台运行。窗口程序进入后台运行的目的就是停止掉窗口程序的部分功能来节省系统性能，所以对于窗口程序而言，当程序进入后台运行时，我们的窗口程序就无法检测用户的键盘、鼠标等的输入信号了，因为这一功能被系统停掉了。

但是，我们可以在QQ后台运行时通过 Ctrl + Alt + A 来进行QQ的截图功能，QQ这一快捷键的输入监测就是通过 hook 来实现的。

什么是 Hook？

Hook 翻译过来就是 “钩子”的意思，我们可以把 hook 理解为 电脑系统会去进行调用的一些 回调函数。

Windows 平台是基于事件驱动机制的，整个系统都是通过消息的传递来实现的。当进程有响应时(包括响应鼠标和键盘事件)，Windows 会向应用程序发送一个消息给应用程序的消息队列，应用程序进而从消息队列中取出消息并发送给相应窗口进行处理。

而 Hook 则是 Windows 消息处理机制的一个平台，应用程序可以在上面设置子程以监视指定窗口的某种消息，而且所监视的窗口可以是其他进程所创建的。当消息到达后，在目标窗口处理函数之前处理它。钩子机制允许应用程序接活处理 Windows 消息或特定事件。

引用自：https://www.cnblogs.com/Leo_wl/p/8574627.html

我自己将其简单理解为：Hook 是一系列系统默认的接口或者说回调函数，当用户进行某些操作触发某些系统事件时，就会调用对应的 hook。而我们的应用程序也可以主动向 hook 中添加自己的回调，这样当 hook 被系统调用时，应用程序注册的回调也就会被调用。

**这里需要注意的重点是：当系统事件触发时，该事件对应的 hook 就会被调用，因此 hook 是在应用程序之前执行的，hook 由系统调用不受应用程序影响，应用程序只能在 hook 中添加回调。**

还是以 QQ的截图快捷键为例，QQ在程序启动时就向 键盘输入 这一事件的 hook 注册了一个自己的回调函数用于检测快捷键。 因此，不管 QQ有没有在后台运行，只要我们 按下了键盘，Windows 系统就会去调用这个回调，从而执行 QQ的程序。

**Hook 还有哪些作用？**

除了上面提到的 QQ截图快捷键的检测 和 我们的桌面精灵制作。 Hook 还会被用来制作 盗号程序(不过现在大部分程序和网站都会对这个进行安全处理) 和 游戏外挂的制作。



## Hook 实现窗体透明

通过 hook 我们可以对窗体的绘制进行修改。

因为 hook 相关的 API 较为底层，且繁杂，所以笔者并没有多 C# 的 hook 相关API 做过多的研究，这里的 窗体设置程序 是笔者直接从网上找来的代码。

因为B站的专栏编辑格式不支持插入代码块，所以直接看的话会很难受，建议大家复制到自己的编辑器里再看。

**创建脚本 WindowSetting：**

```
using UnityEngine;

using System.Collections;

using System;

using System.Runtime.InteropServices;



/// <summary>

/// 一共可选择三种样式

/// </summary>

public enum enumWinStyle

{

/// <summary>

/// 置顶

/// </summary>

WinTop,

/// <summary>

/// 置顶并且透明

/// </summary>

WinTopApha,

/// <summary>

/// 置顶透明并且可以穿透

/// </summary>

WinTopAphaPenetrate

}

public class WindowSetting : MonoBehaviour

{



#region Win函数常量

private struct MARGINS

{

    public int cxLeftWidth;

    public int cxRightWidth;

    public int cyTopHeight;

    public int cyBottomHeight;

}



[DllImport("user32.dll")]

static extern IntPtr FindWindow(string lpClassName, string lpWindowName);

[DllImport("user32.dll")]

static extern int SetWindowLong(IntPtr hWnd, int nIndex, int dwNewLong);



[DllImport("user32.dll")]

static extern int GetWindowLong(IntPtr hWnd, int nIndex);



[DllImport("user32.dll")]

static extern int SetWindowPos(IntPtr hWnd, int hWndInsertAfter, int X, int Y, int cx, int cy, int uFlags);



[DllImport("user32.dll")]

static extern int SetLayeredWindowAttributes(IntPtr hwnd, int crKey, int bAlpha, int dwFlags);



[DllImport("Dwmapi.dll")]

static extern uint DwmExtendFrameIntoClientArea(IntPtr hWnd, ref MARGINS margins);

[DllImport("user32.dll")]

private static extern int SetWindowLong(IntPtr hWnd, int nIndex, uint dwNewLong);

private const int WS_POPUP = 0x800000;

private const int GWL_EXSTYLE = -20;

private const int GWL_STYLE = -16;

private const int WS_EX_LAYERED = 0x00080000;

private const int WS_BORDER = 0x00800000;

private const int WS_CAPTION = 0x00C00000;

private const int SWP_SHOWWINDOW = 0x0040;

private const int LWA_COLORKEY = 0x00000001;

private const int LWA_ALPHA = 0x00000002;

private const int WS_EX_TRANSPARENT = 0x20;

//

private const int ULW_COLORKEY = 0x00000001;

private const int ULW_ALPHA = 0x00000002;

private const int ULW_OPAQUE = 0x00000004;

private const int ULW_EX_NORESIZE = 0x00000008;

#endregion

//

public string strProduct;//项目名称

public enumWinStyle WinStyle = enumWinStyle.WinTop;//窗体样式

//

public int ResWidth;//窗口宽度

public int ResHeight;//窗口高度

//

public int currentX;//窗口左上角坐标x

public int currentY;//窗口左上角坐标y

//

private bool isApha;//是否透明

private bool isAphaPenetrate;//是否要穿透窗体

// Use this for initialization

void Awake()

{

    Application.runInBackground = true;

    Screen.fullScreen = false;



    switch (WinStyle)

    {

        case enumWinStyle.WinTop:

            isApha = false;

            isAphaPenetrate = false;

            break;

        case enumWinStyle.WinTopApha:

            isApha = true;

            isAphaPenetrate = false;

            break;

        case enumWinStyle.WinTopAphaPenetrate:

            isApha = true;

            isAphaPenetrate = true;

            break;

    }

    //

    IntPtr hwnd = FindWindow(null, strProduct);

    //

    if (isApha)

    {

        //去边框并且透明

        SetWindowLong(hwnd, GWL_EXSTYLE, WS_EX_LAYERED);

        int intExTemp = GetWindowLong(hwnd, GWL_EXSTYLE);

        if (isAphaPenetrate)//是否透明穿透窗体

        {

            SetWindowLong(hwnd, GWL_EXSTYLE, intExTemp | WS_EX_TRANSPARENT | WS_EX_LAYERED);

        }

        //

        SetWindowLong(hwnd, GWL_STYLE, GetWindowLong(hwnd, GWL_STYLE) & ~WS_BORDER & ~WS_CAPTION);

        SetWindowPos(hwnd, -1, currentX, currentY, ResWidth, ResHeight, SWP_SHOWWINDOW);

        var margins = new MARGINS() { cxLeftWidth = -1 };

        //

        DwmExtendFrameIntoClientArea(hwnd, ref margins);

    }

    else

    {

        //单纯去边框

        SetWindowLong(hwnd, GWL_STYLE, WS_POPUP);

        SetWindowPos(hwnd, -1, currentX, currentY, ResWidth, ResHeight, SWP_SHOWWINDOW);

    }

}

}
```

将该脚本挂载到场景物体上就ok了。

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\5322855d3d745cc3db5892233fbb0040be50035d.png@!web-article-pic.avif)

我习惯把一些初始化设置的脚本挂载主摄像机上(展示了我常用的 WindowSetting 设置)
注意：
如果你的 Unity 版本是 2018 版本以下，则这样子打包后的游戏窗体就是透明的

如果 Unity 版本是 2019 版本及以上，则在打包时还需要做以下设置：

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\b15419f720615d0cebce900e4da35b7e14c2cbdb.png@1256w_930h_!web-article-pic.avif)

在 PlayerSetting 中 取消 D3D11 的勾选

## Hook 实现后台键盘事件响应

上面已经介绍了，通过 hook 我们可以让程序在后台时也能够接收按键信号，那么接下来就开始实现它。

同样，由于笔者对于 hook 相关的 api 没有做过多研究，这里是笔者从网上找来的一个代码，然后在此基础上做了补充。

**模块化：**

我们的 hook 脚本可以捕捉玩家的按键输入，并且可以得到玩家按下的具体按键的键盘码。但是如果我们把 游戏中所有的按键事件 都写在 hook 脚本中，那么 hook 脚本的耦合性将会非常高。因此这里笔者进行了模块化，对 键盘响应 这一功能进行解耦。

功能的解耦核心是 MPlayerInput 脚本。MPlayerInput 是一个单例。 MPlayerInput 脚本存储了游戏中所有的 按键响应回调。 其它脚本通过 MPlayerInput 来注册自己的按键回调。 hook 脚本当 按键事件触发时 通过 MPlayerInput 来调用对应按键的响应回调。 这样就实现了 注册 与 调用之间的解耦。

**创建 MPlayerInput 脚本：**

```
using System;

using System.Collections;

using System.Collections.Generic;

using UnityEngine;



public class MPlayerInput : MonoBehaviour

{

// 单例

public static MPlayerInput Single;



/// <summary>

/// 存储键盘按下时的响应事件

/// 字典的键值为 响应事件对应的 按键

/// 字典的元素为 响应事件

/// 这里响应事件是 Action， 也可以通过 delegate 进行更丰富的定义。

/// </summary>

private Dictionary<KeyCode, Action> keyDownDic = new Dictionary<KeyCode, Action>();

/// <summary>

/// 存储按键抬起时的响应事件

/// </summary>

private Dictionary<KeyCode, Action> keyUpDic = new Dictionary<KeyCode, Action>();

/// <summary>

/// 存储鼠标移到时的回调

/// 该回调需要一个 Vector3 参数， 该参数在 hook 调用时会传入 鼠标在一帧中的移动量

/// </summary>

private Action<Vector3> mouseMoveList = (movement) => { };

/// <summary>

/// 存储鼠标响应事件

/// 这类事件是在鼠标 移动、点击左键、点击右键、点击中间等鼠标事件触发时都会被调用

/// </summary>

private Action mouseEventCall = () => { };

/// <summary>

/// 存储鼠标左键按下时的回调

/// </summary>

private Action mouseClickCall = () => { };

/// <summary>

/// 存储鼠标左键抬起时的回调

/// </summary>

private Action mouseReleaseCall = () => { };



// 简单单例的构造

private void Awake()

{

    if(Single != null)

    {

        Destroy(Single.gameObject);

    }



    Single = this;

}



/// <summary>

/// 注册鼠标移动时的回调

/// </summary>

/// <param name="callBack">回调</param>

public void RegisterMouseMoveCallBack(Action<Vector3> callBack)

{

    mouseMoveList += callBack;

}

/// <summary>

/// 注册鼠标事件回调

/// </summary>

/// <param name="callBack">回调</param>

public void RegisterMouseEventCallBack(Action callBack)

{

    mouseEventCall += callBack;

}

/// <summary>

/// 注册鼠标左键按下时的回调

/// </summary>

/// <param name="callBack">回调</param>

public void RegisterMouseClickCallBack(Action callBack)

{

    mouseClickCall += callBack;

}

/// <summary>

/// 注册鼠标左键抬起时的回调

/// </summary>

/// <param name="callBack"></param>

public void RegisterMouseRelaeaseCallBack(Action callBack)

{

    mouseReleaseCall += callBack;

}



/// <summary>

/// hook 用于调用鼠标移动回调的函数

/// </summary>

/// <param name="movement"></param>

public void MouseMoveCallBack(Vector3 movement)

{

    mouseMoveList.Invoke(movement);

}

/// <summary>

/// hook 用于调用鼠标事件回调的函数

/// </summary>

public void MouseEventCallBack()

{

    mouseEventCall.Invoke();

}

/// <summary>

/// hook 用于调用鼠标左键按下时回调的函数

/// </summary>

public void MouseClickCallBack()

{

    mouseClickCall.Invoke();

}

/// <summary>

/// hook 用于调用鼠标左键抬起时回调的函数

/// </summary>

public void MouseReleaseCallBack()

{

    mouseReleaseCall.Invoke();

}



/// <summary>

/// 注册按键按下时的回调

/// </summary>

/// <param name="key">检测的按键</param>

/// <param name="callBack">回调</param>

public void RegisterKeyDownCallBack(KeyCode key, Action callBack)

{

    if (!keyDownDic.ContainsKey(key)) keyDownDic[key] = callBack; 

    else keyDownDic[key] += callBack;

}

/// <summary>

/// 注册按键抬起时的回调

/// </summary>

/// <param name="key">检测的按键</param>

/// <param name="callBack">回调</param>

public void RegisterKeyUpCallBack(KeyCode key, Action callBack)

{

    if (!keyUpDic.ContainsKey(key)) keyUpDic[key] = callBack;

    else keyUpDic[key] += callBack;

}



/// <summary>

/// hook 用于调用按键按下时回调的函数

/// </summary>

/// <param name="key"></param>

public void KeyDownCallBack(KeyCode key)

{

    if(keyDownDic.ContainsKey(key)) keyDownDic[key].Invoke();

}

/// <summary>

/// hook 用于调用按键抬起时回调的函数

/// </summary>

/// <param name="key"></param>

public void KeyUpCallBack(KeyCode key)

{

    if (keyUpDic.ContainsKey(key)) keyUpDic[key].Invoke();

}

}
```

 解释说明都已经写在了代码的注释里，所以这里就不做过多解释了。

实际上 MPlayerInput 不需要继承 MonoBehaviour，构造成普通单例就可以了，当时图个方便就写成了简单的 mono 单例。

将 MPlayerInput 挂载到场景物体上，这里我同样是挂载到了主摄像机上：

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\d863c7aa0b1f6f8b8a3d840ce7dd1238ed8aab95.png@!web-article-pic.avif)

​																														MPlayerInput脚本
**接下来创建 KeyCodeController 脚本：**

```
using System.Collections;

using System.Collections.Generic;

using UnityEngine;

using System;

using System.Runtime.InteropServices;

using System.Diagnostics;



public class KeyCodeController : MonoBehaviour

{

//截获按钮

// 键盘事件码

private const int WH_KEYBOARD_LL = 13;

// 键盘按下 事件码

private const int WM_KEYDOWN = 0x0100;

// 键盘抬起 事件码

private const int WM_KEYUP = 0x0101;

private const int WM_SYSKEYDOWN = 0x0104;

private const int WM_SYSKEYUP = 0x0105;

private static LowLevelKeyboardProc _proc = HookCallback;

private static IntPtr _hookID = IntPtr.Zero;



/// <summary>

/// 信号存储字典

/// 因为在键盘处于被按下的状态，系统会一直调用 键盘按下的 hook

/// 而我们希望只在键盘被按下的一瞬间，调用一次回调

/// 所以在键盘被按下时 存储一个为 true 的信号

/// 在调用回调前判断对应键的信号，如果为 true 则不调用，为 false 则调用，调用完 信号转为 true

/// 在按键被抬起时，把信号设置回 false

/// </summary>

private static Dictionary<KeyCode, bool> keyDownDic = new Dictionary<KeyCode, bool>();



// Use this for initialization

void Start()

{

    // 向键盘事件 hook 注册回调

    _hookID = SetHook(_proc);

}

void OnApplicationQuit()

{

    // 注销 hook 回调

    UnhookWindowsHookEx(_hookID);

}

private static IntPtr SetHook(LowLevelKeyboardProc proc)

{

    using (Process curProcess = Process.GetCurrentProcess())

    using (ProcessModule curModule = curProcess.MainModule)

    {

        return SetWindowsHookEx(WH_KEYBOARD_LL, proc, GetModuleHandle(curModule.ModuleName), 0);

    }

}

private delegate IntPtr LowLevelKeyboardProc(int nCode, IntPtr wParam, IntPtr lParam);

// 键盘事件 hook 回调

private static IntPtr HookCallback(int nCode, IntPtr wParam, IntPtr lParam)

{

    // 如果当前有按键被按下

    if (nCode >= 0 && wParam == (IntPtr)WM_KEYDOWN)

    {

        int vkCode = Marshal.ReadInt32(lParam); // 按下的按键的键盘码



        KeyCode key = KeyCode.None;

        try

        {

            // 大部分按键的键盘码 与 Unity 的 KeyCokde 转换可以用 KeyCode key = (vkCode + 32)

            // 但是有些则不能，例如 回车键、TAB键等，因此为了防止程序崩溃，需要用 try catch 让程序稳定

            key = (KeyCode)(vkCode + 32);

        }

        catch { }



        // 判断是否是按下按键的瞬间

        if(!keyDownDic.ContainsKey(key) || keyDownDic[key] == false)

        {

            // 通过 MPlayerInput 调用游戏注册的按键回调

            MPlayerInput.Single.KeyDownCallBack(key);

            // 字典这样写，如果字典没有存储 key 这个键值，则会默认调用 dic.Add() 方法

            // 将按键信号设为 true

            keyDownDic[key] = true;

        }

    }



    // 如果按键有被抬起

    if (nCode >= 0 && wParam == (IntPtr)WM_KEYUP)

    {

        int vkCode = Marshal.ReadInt32(lParam);



        KeyCode key = KeyCode.None;

        try

        {

            key = (KeyCode)(vkCode + 32);

        }

        catch { }

        // 将按键信号恢复为 false

        keyDownDic[key] = false;

        // 通过 MPlayerInput 调用游戏注册的按键回调

        MPlayerInput.Single.KeyUpCallBack(key);

    }



    return CallNextHookEx(_hookID, nCode, wParam, lParam);

}



[DllImport("user32.dll", CharSet = CharSet.Auto, SetLastError = true)]

private static extern IntPtr SetWindowsHookEx(int idHook,

    LowLevelKeyboardProc lpfn, IntPtr hMod, uint dwThreadId);



[DllImport("user32.dll", CharSet = CharSet.Auto, SetLastError = true)]

[return: MarshalAs(UnmanagedType.Bool)]

private static extern bool UnhookWindowsHookEx(IntPtr hhk);



[DllImport("user32.dll", CharSet = CharSet.Auto, SetLastError = true)]

private static extern IntPtr CallNextHookEx(IntPtr hhk, int nCode, IntPtr wParam, IntPtr lParam);



[DllImport("kernel32.dll", CharSet = CharSet.Auto, SetLastError = true)]

private static extern IntPtr GetModuleHandle(string lpModuleName);

}
```

同样，详细的解释已经写在注释里了，这里不做过多解释。

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\3f71c119c1dad2f784a61a60470c4ae1824941d7.png@!web-article-pic.avif)

​																												挂载 KeyCodeController 脚本

## Hook 实现后台接收鼠标事件

**创建 MouseController 脚本：**

```
using System.Collections;

using System.Collections.Generic;

using UnityEngine;

using System;

using System.Runtime.InteropServices;

using System.Diagnostics;



// 用于表示鼠标位置的结构体

[StructLayout(LayoutKind.Sequential)]

public class POINT

{

public int x;

public int y;

}



// 用于表示鼠标移动信息的结构体

[StructLayout(LayoutKind.Sequential)]

public class MouseHookStruct

{

public POINT pt;

public int hwnd;

public int wHitTestCode;

public int dwExtraInfo;

}



public class MouseController : MonoBehaviour

{

//截获按钮

// 鼠标事件吗

private const int WH_KEYBOARD_LL = 14;

// 鼠标移动事件码

private const int WM_MOUSEMOVE = 0x0200;

// 鼠标右键按下事件码

private const int WM_RBUTTONDOWN = 0x0204;

// 鼠标左键抬起事件码

private const int WM_RBUTTONUP = 0x0205;

// 鼠标左键按下事件码

private const int WM_LBUTTONDOWN = 0x0201;

// 鼠标左键抬起事件码

private const int WM_LBUTTONUP = 0x0202;



private static LowLevelMouseProc _proc = HookCallback;

private static IntPtr _hookID = IntPtr.Zero;



// 存储上一帧鼠标的位置

private static Vector3 lastMousePos = Vector3.zero;

// 上一帧到当前帧鼠标的移动量

public static Vector3 mouseDeltMove = Vector3.zero;

// 用于判断当前帧是否是游戏启动后的第一帧

private static bool first = true;



// Use this for initialization

void Start()

{

    // 注册 hook

    _hookID = SetHook(_proc);

}



void OnApplicationQuit()

{

    // 注销 hook

    UnhookWindowsHookEx(_hookID);

}

private static IntPtr SetHook(LowLevelMouseProc proc)

{

    using (Process curProcess = Process.GetCurrentProcess())

    using (ProcessModule curModule = curProcess.MainModule)

    {

        return SetWindowsHookEx(WH_KEYBOARD_LL, proc, GetModuleHandle(curModule.ModuleName), 0);

    }

}

private delegate IntPtr LowLevelMouseProc(int nCode, IntPtr wParam, IntPtr lParam);

// hook 回调

private static IntPtr HookCallback(int nCode, IntPtr wParam, IntPtr lParam)

{

    // 只要有鼠标事件被触发，该 hook 回调方法就会被系统调用

    // 所以直接在该方法中 通过 MPlayerInput 调用 游戏注册的鼠标事件回调

    MPlayerInput.Single.MouseEventCallBack();



    // 如果鼠标左键被按下

    if (nCode >= 0 && wParam == (IntPtr)WM_LBUTTONDOWN)

    {

        int vkCode = Marshal.ReadInt32(lParam);

        // 通过 MPlayerInput 调用鼠标左键被按下时的回调

        MPlayerInput.Single.MouseClickCallBack();

    }

    // 如果鼠标右键被按下

    if (nCode >= 0 && wParam == (IntPtr)WM_RBUTTONDOWN)

    {

        int vkCode = Marshal.ReadInt32(lParam);

        MPlayerInput.Single.MouseClickCallBack();

    }



    // 如果鼠标左键被抬起

    if (nCode >= 0 && wParam == (IntPtr)WM_LBUTTONUP)

    {

        int vkCode = Marshal.ReadInt32(lParam);

        MPlayerInput.Single.MouseReleaseCallBack();

    }

    // 如果鼠标右键被抬起

    if (nCode >= 0 && wParam == (IntPtr)WM_RBUTTONUP)

    {

        int vkCode = Marshal.ReadInt32(lParam);

        MPlayerInput.Single.MouseReleaseCallBack();

    }



    // 输入鼠标发生了移动

    if (nCode >= 0 && wParam == (IntPtr)WM_MOUSEMOVE)

    {

        int vkCode = Marshal.ReadInt32(lParam);

        // 获取鼠标移动信息

        MouseHookStruct M_MouseHookStruct = (MouseHookStruct)Marshal.PtrToStructure(lParam, typeof(MouseHookStruct));

        // 根据鼠标移动信息获取 当前帧 鼠标位置

        Vector3 curMousePos = new Vector3(-M_MouseHookStruct.pt.x, M_MouseHookStruct.pt.y, 0f);


            

        // 如果当前是游戏启动后的第一帧

        if (first)

        {

            // 则 lastMousePos = curMousePos

            // 即 认为当前鼠标的移动量为 0

            lastMousePos = curMousePos;

            first = false;

        }

        // 计算上一帧到当前帧鼠标的移动量

        mouseDeltMove = lastMousePos - curMousePos;

        // 通过 MPlayerInput 调用游戏的鼠标移动回调

        MPlayerInput.Single.MouseMoveCallBack(mouseDeltMove);

        // 更新 lastMousePos

        lastMousePos = curMousePos;

    }



    return CallNextHookEx(_hookID, nCode, wParam, lParam);

}





[DllImport("user32.dll", CharSet = CharSet.Auto, SetLastError = true)]

private static extern IntPtr SetWindowsHookEx(int idHook,

    LowLevelMouseProc lpfn, IntPtr hMod, uint dwThreadId);



[DllImport("user32.dll", CharSet = CharSet.Auto, SetLastError = true)]

[return: MarshalAs(UnmanagedType.Bool)]

private static extern bool UnhookWindowsHookEx(IntPtr hhk);



[DllImport("user32.dll", CharSet = CharSet.Auto, SetLastError = true)]

private static extern IntPtr CallNextHookEx(IntPtr hhk, int nCode, IntPtr wParam, IntPtr lParam);



[DllImport("kernel32.dll", CharSet = CharSet.Auto, SetLastError = true)]

private static extern IntPtr GetModuleHandle(string lpModuleName);

}
```

具体解释看代码注释，这里不做过多解释。

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\1540da84f9a30bd0b0fda68b65f63cc9cce7d459.png@!web-article-pic.avif)

​																												挂载 MouseController 脚本
至此 hook 部分基本结束。 接下来开始 按键精灵的制作。



## 2D桌面精灵的制作

### 2D桌面精灵的绘制

笔者的绘画技术很菜，所以这里就随便画画作为演示，这里我使用 Aseprite 作为绘画软件。

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\ad12ff9755c0268eebf8b9e7f552f0d66dc29f05.png@1256w_730h_!web-article-pic.avif)

​																			Aseprite 许多像素游戏制作者用它来制作像素画。可以在 Steam 上购买
到时我们需要用控制骨骼的方法来控制我们桌面精灵的动作，所以绘制时需要注意合理的分层。

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\d42727e668cfdb698f75ba8afdac2c3e0681cbcc.png@!web-article-pic.avif)

​																																分层一览
我绘制的角色是一只兔子。 这里我把兔子的 身体、左胳膊、右胳膊、左手、右手、脸部(后面导出时我其实又把脸部和身体合并了)、左眼、右眼、左边下半部分耳朵、左边上半部分耳朵、右边下半部分耳朵、右边上半部分耳朵 各分为一层。

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\3f143aa303a4799b3e8d761647f0fcb2ae1d065e.png@!web-article-pic.avif)

​																															绘制完的兔子
如果像上图一样，导出的图片是不行的。因为我们后面还要用 Unity 的 2D 工具来为 2D 图像进行骨骼绑定，所以我们要把各个图层的部分分开，以便于后续的骨骼绑定和权重绘制，在绑定完骨骼后再对整个图像进行拼接。

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\056575ba01fa7ad9c9033f644335e3ac48eb0be5.png@!web-article-pic.avif)

​																														各图层分开后的样子
将上面这样各图层部分分开后的图像导出，然后放入到 Unity 中。

### 使用 Unity 2D 工具进行骨骼绑定

在以前，如果我们要在 Unity 中使用 2D 骨骼动画，通常会选择一些专业软件来进行 2D骨骼绑定，这里列举两个 2D 骨骼动画制作软件：

1、Spine：Spine 是一款专业的 2D骨骼动画制作软件，国际上许多 2D 游戏制作者都有使用。支持 Unity、虚幻、cocos 等众多引擎和平台。

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\7b69a90a4379e29e37f9f5ebe853544a1c42f4cb.png@1256w_734h_!web-article-pic.avif)

​																																Spine的官网截图
2、Dragon Bones(龙骨)：Dragon Bones 是国产游戏引擎——白鹭引擎 的制作公司，制作的一款专门用于制作 2D骨骼动画 的软件。 其与 Spine 相比的一个优点就是 Dragon Bones 是开源免费的。 Dragon Bones 同样支持 Unity、Cocos 等游戏引擎和平台。

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\52b669502294b905f2b8c550b8c774e89fcbd5f0.png@1256w_476h_!web-article-pic.avif)

​																															Dragon Bones 官网截图
随着 Unity 2019 版本的更新，Unity 对于 2D 创作的工具包也进行了一系列升级。现在，使用 Unity 原生的 2D 工具包绑定和制作 2D骨骼动画 也不失为一种选择。 这里，我就使用 Unity 的 2D工具 进行 2D骨骼 绑定。

打开 Package Manager 窗口，确保自己安装了以下套件：

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\1e38b892146c9b7a296106959304863c316a389a.png@1256w_1074h_!web-article-pic.avif)

​																															Unity 2D 工具包
实际上只要安装了 2D Animation 和 2D Sprite 这两个套件就好了。因为我是直接创建了 2D 项目，这些套件都是默认安装好的。

把刚才绘制好的桌面精灵图像导入 Unity，我们观察 图像的 Inspector 面板：

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\2d2396d5a68e5143af57e4a9ebf1bb20d6db15c3.png@!web-article-pic.avif)

​																												导入的图像的 Inspector 面板
注意把 Sprite Mode 设置为 Single，然后点击 Sprite Editor 按钮，就会打开 Sprite Editor窗口：

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\cd71190e3b68a5663d8d51b093e0094b2a7b95b5.png@1256w_804h_!web-article-pic.avif)

​																																			SpriteEditor 窗口
点击左上角的 Sprite Editor 然后选择 Skinned Editor，进入蒙皮编辑窗口：

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\06ac2b812c950ea14a5390e5eae6e06089f1744a.png@1256w_918h_!web-article-pic.avif)

​																														Skinned Editor 窗口
然后我们可以点击左侧的 Create Bone 按钮，然后再在图像上双击鼠标左键创建出一根骨头，之后可以点击 骨骼的根部 或者 头部，新生出 虚连接的骨骼 或者 实连接的骨骼。

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\974aa6f7925647456497f70201824e245a5cdd7b.png@1256w_918h_!web-article-pic.avif)

​																															绑定的骨骼
可以发现，这里我绑定的骨骼是存在错误的。因为这里我所有的骨骼连接都是使用的 “虚连接”，像胳膊和手掌、身体和耳朵 应该是直接连接的 “实连接”。这里我所有连接都用“虚连接”的原因是，这样子做在后面可以直接自动蒙皮而不出什么差错。

之后，我们点击左侧的 Auto Geometry 再点击右下角的 Generate For Selected 进行自动蒙皮：

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\729039df4eee43e0a90d741779763005aceace93.png@1256w_876h_!web-article-pic.avif)

​																																				自动蒙皮
最后，点击左侧的 Auto Weight 再点击 右下角的 Generate 来自动绘制权重：

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\8547db27ef88ea203f78547f3290d89e59b44558.png@1256w_874h_!web-article-pic.avif)

​																																				自动绘制权重
最后点击窗口右上角的 Apply 应用我们的设置，然后关闭窗口，我们的 2D骨骼 就绑定完成了。

现在，我们可以直接把这个 桌面精灵图像 拖拽到场景中。并给该物体添加 Unity 内置的 Sprite Skine 脚本：

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\5d24ceeed7a89552c2ba5726a4ead19dea91b2f8.png@!web-article-pic.avif)

​																											添加 Unity 内置的 Sprite Skine 脚本
然后点击脚本的 Create Bones 按钮，Unity 就会自动的为我们创建出骨骼：

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\14251e85c9d9699db9bd4f884ecbfeefbe2adb2c.png@!web-article-pic.avif)

​																															Sprite Skin 脚本

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\4058167cc780b770a55a3b0b07f05733c9a2df0e.png@!web-article-pic.avif)

​																														自动创建出的骨骼
之后，我们来摆放这些骨骼，把零散的角色部位拼接为一体：

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\9b918e0e5a9154c478fafc0e65e5cb8b4746f5e4.png@!web-article-pic.avif)

​																																	拼接后的样子

### 制作键盘 与 鼠标物体

从上图可以看出，我摆放了 WASD 这四个键盘按键 和 一个圆形作为鼠标。

它们所用的图像(Sprite)都是 Unity 自带的，一个是 UISprite、一个是 Knob

注意：

这里我创建的是 2D Gameobject 中的 Sprite，而不是 UI 中的 Image

知道吗：

Unity 2D 项目，虽然是 2D，但是场景的 X、Y、Z 三个轴都是存在的。我们可以让物体绕 X 轴旋转：

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\a766f9da69c1c9a0a0386d5686acb4fae1ac59e3.png@!web-article-pic.avif)

​																											可以在 3D 视图下观察和旋转物体

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\21c644895f96657ca80ccbd6716d6584f9333550.png@!web-article-pic.avif)

​																															2D 视图下的效果

### 代码原理

原理其实非常简单，现在我们已经有了角色骨骼，当我们按下按键 例如 W 键时，我们可以计算出 手部骨骼 和 胳膊的骨骼 位置 与 W按键 的指向向量，通过向量计算来旋转和移动骨骼，来使手 和 手臂处于按键的姿势。

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\e3c6bc619b369733d20d2bd488132dff1682548f.png@1256w_810h_!web-article-pic.avif)

​																															骨骼与按键间的向量
视觉残留效应：

当一个物体快速移动时，人类大脑和眼睛会自动补充一系列残影来模拟该物体的移动。

因此，我们可以让精灵的手和手臂在一帧中直接移动到相应的按键位置。 由于视觉残留效应，我们的大脑会自动脑补出一个运动动画。

当然，我们可以通过代码实现缓动(如 Vector3.Slerp)。但我我觉得通过立马将位置移至目标位置，我们角色的动作会更加的卡通与可爱。 

### 撸代码！！！

这里我们兔子的左手(对于兔子而言实际是右手，但是本文所选参考皆为我们自身)，兔子的左手是负责对键盘事件的相应，而兔子的右手是负责对鼠标事件的响应。 所以，我把这两个功能给分离开，分别处理。

**创建 LeftController 脚本，控制兔子的左半部分，对于键盘事件进行响应：**

B站专栏编辑不支持代码格式，这里我大部分解释都写在了代码的注释里，所以建议大家把代码复制到自己的编辑器中，这样看起来会比较舒服。

```
using System.Collections;

using System.Collections.Generic;

using UnityEngine;



public class LeftController : MonoBehaviour

{

// 存储键盘按键Sprite

[SerializeField] private SpriteRenderer wPoint = null;

[SerializeField] private SpriteRenderer aPoint = null;

[SerializeField] private SpriteRenderer sPoint = null;

[SerializeField] private SpriteRenderer dPoint = null;



// 存储我们要控制的角色骨骼

[SerializeField] private Transform arm = null; // 左胳膊

[SerializeField] private Transform hand = null; // 左手

[SerializeField] private Transform leftEarDown = null; // 左耳朵下半部分

[SerializeField] private Transform leftEarUp = null; // 左耳朵上半部分

[SerializeField] private Transform rightEarDown = null; // 右耳朵下半部分

[SerializeField] private Transform rightEarUp = null; // 右耳朵上半部分



// 响应时耳朵旋转的角度

[Range(-90, 0)]

[SerializeField] private float leftEarDownRotAngle = -10f;

[Range(-90, 0)]

[SerializeField] private float leftEarUpRotAngle = -60f;

[Range(0, 90)]

[SerializeField] private float rightEarDownRotAngle = 10f;

[Range(0, 90)]

[SerializeField] private float rightEarUpRotAngle = 60f;



// 存储骨骼初始的位置和角度

private Vector3 startArmPos;

private Quaternion startArmRot;

private Vector3 startHandPos;

private Quaternion startHandRot;



// 存储耳朵骨骼初始的位置和角度

private Vector3 leftEarDownPos;

private Quaternion leftEarDownRot;

private Vector3 leftEarUpPos;

private Quaternion leftEarUpRot;



// 存储耳朵骨骼初始的位置和角度

private Vector3 rightEarDownPos;

private Quaternion rightEarDownRot;

private Vector3 rightEarUpPos;

private Quaternion rightEarUpRot;





private void Start()

{

     // 记录初始化数据

     startArmPos = arm.position;

     startArmRot = arm.rotation;

     startHandPos = hand.position;

     startHandRot = hand.rotation;



     leftEarDownPos = leftEarDown.position;

     leftEarDownRot = leftEarDown.rotation;

     leftEarUpPos = leftEarUp.position;

     leftEarUpRot = leftEarUp.rotation;



     rightEarDownPos = rightEarDown.position;

     rightEarDownRot = rightEarDown.rotation;

     rightEarUpPos = rightEarUp.position;

     rightEarUpRot = rightEarUp.rotation;



     // 注册按键按下时要调用的回调

     MPlayerInput.Single.RegisterKeyDownCallBack(KeyCode.W, () => Move(wPoint));

     MPlayerInput.Single.RegisterKeyDownCallBack(KeyCode.A, () => Move(aPoint));

     MPlayerInput.Single.RegisterKeyDownCallBack(KeyCode.S, () => Move(sPoint));

     MPlayerInput.Single.RegisterKeyDownCallBack(KeyCode.D, () => Move(dPoint));



     // 注册按键抬起时要调用的回调

     MPlayerInput.Single.RegisterKeyUpCallBack(KeyCode.W, () => Resume());

     MPlayerInput.Single.RegisterKeyUpCallBack(KeyCode.A, () => Resume());

     MPlayerInput.Single.RegisterKeyUpCallBack(KeyCode.S, () => Resume());

     MPlayerInput.Single.RegisterKeyUpCallBack(KeyCode.D, () => Resume());

}



// 按键抬起时 恢复我们兔子的耳朵和手的位置与角度

private void Resume()

{

     ResumeHand();

     ResumeEar();



     // 将按键的颜色重置为白色

     // 其实可以缓存一个当前按下的按键对象，然后只恢复该对象的颜色

     wPoint.color = Color.white;

     aPoint.color = Color.white;

     sPoint.color = Color.white;

     dPoint.color = Color.white;

}



// 恢复手的位置与角度

private void ResumeHand()

{

     // 直接将 胳膊 和 手的骨骼位置与角度赋值为初始的位置与角度

     // 由视觉残留效应 让人脑补出动作

     arm.position = startArmPos;

     arm.rotation = startArmRot;

     hand.position = startHandPos;

     hand.rotation = startHandRot;

}



// 恢复耳朵的位置与角度

private void ResumeEar()

{

     // 直接将 耳朵 骨骼位置与角度赋值为初始的位置与角度

     // 由视觉残留效应 让人脑补出动作

     leftEarDown.position = leftEarDownPos;

     leftEarDown.rotation = leftEarDownRot;

     leftEarUp.position = leftEarUpPos;

     leftEarUp.rotation = leftEarUpRot;



     rightEarDown.position = rightEarDownPos;

     rightEarDown.rotation = rightEarDownRot;

     rightEarUp.position = rightEarUpPos;

     rightEarUp.rotation = rightEarUpRot;

}



// 键盘按下时，移动我们的手和胳膊

private void Move(SpriteRenderer target)

{

     MoveHand(target);

     MoveEar();



     // 将按下的按键对象的颜色设置为 蓝色

     target.color = Color.blue;

}



// 移动手臂

private void MoveHand(SpriteRenderer target)

{

     // 计算 胳膊 指向 按键的向量

     Vector3 tar = target.transform.position - arm.position;

     tar.z = 0; // 因为是 2D 的，所以我们把 Z 轴值设为 0

     // 计算骨骼当前的指向或者说骨骼的前方(与一般物体不同，骨骼的前方不是 transform.forward，而是 transform.right)

     // 计算 胳膊骨骼前方 与 胳膊指向按键的向量 的夹角

     float angle = Vector3.SignedAngle(arm.right, tar, Vector3.forward);



     // 根据计算出的角度，旋转胳膊。 

     // 旋转绕的轴是 世界坐标的 forward 轴，即由屏幕内指向屏幕外的轴。

     arm.Rotate(Vector3.forward, angle);

     // 直接把手的位置赋值为 按键的位置再偏向胳膊一点的位置

     // 因为我们的手与胳膊的连接是 "虚连接" 所以直接移动手的位置 不会产生胳膊被拉伸的情况

     // 但是这样子做，如果手的移动量比较大时，会出现手脱离胳膊的情况

     hand.position = target.transform.position - arm.right * 0.5f;



     // 同样，上面是直接旋转和移动我们的手与胳膊。由视觉残留效应脑补动作。

}



// 移动耳朵

private void MoveEar()

{

     // 让耳朵按照我们设置的角度去做一个旋转

     // 同样是由视觉残留效应脑补动作

     leftEarDown.Rotate(Vector3.forward, leftEarDownRotAngle);

     leftEarUp.Rotate(Vector3.forward, leftEarUpRotAngle);



     rightEarDown.Rotate(Vector3.forward, rightEarDownRotAngle);

     rightEarUp.Rotate(Vector3.forward, rightEarUpRotAngle);

}

} 
```

代码的注释已经写的比较详尽，配合上一篇专栏所写的原理应该很好理解，所以不做过多赘述。这里再复述一下代码的核心原理：

通过代码获取到角色胳膊的骨骼指向向量 并计算出 胳膊指向目标按键的指向向量，然后计算二者之间的夹角，之后再把胳膊骨骼按照这个角度进行旋转，最后再把手指骨骼移到目标按键上就 ok 了，至于中间的动作动画，通过视觉残留效应，我们的大脑就会脑补出来。

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\748b5a792d41d4bc89d1e92f3c621af240f4a7f7.png@!web-article-pic.avif)

​																				绿色的胳膊骨骼指向向量，红色的是胳膊指向目标按键的向量。

**Tip：**

说道打击感，其实就是当玩家操作角色攻击到敌人时，游戏能够给予玩家的反馈程度。

相比于打击感，这里我们可以产生一个 “按键感” 的概念，即当我们的桌面精灵进行按键动作时，桌面精灵能够给予用户的反馈强度。

如果桌面精灵执行按键动作只是把手掌和胳膊移动到对应位置，那么这样子的反馈强度是不够的。所以在我的代码里，桌面精灵进行按键动作时，代码还会改变对应按键的颜色，同时兔子的耳朵也会做出动作，这样就可以让用户更轻易更明显更准确地感知到桌面精灵进行了按键。

而在游戏设计中对于打击感是同样的道理。对于打击感，我们常说动画、音效、特效和摄像机抖动，这些其实只是给予玩家打击反馈的基础手段。仔细观察动作游戏大作会发现，当角色的攻击方向和攻击方式不同时，敌人的受击动画也会不同，这是为了让玩家能更精确的感知到打击、在打击到时的攻击音效中会夹杂砍肉音效，这是为了区分出击中和未击中时的音效，让玩家更轻易的感知到打击，敌人受击时“啊啊大叫”也是同样道理、血溅特效会根据攻击方向改变喷射方向、除了血溅特效还会在敌人身上产生刀痕特效、屏幕模糊后处理特效等、让目标锁定环产生变色等，实际上我们有非常多的手段来提升打击感。

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\83be55f113a9483130279389ea2826a4a6acf2de.png@!web-article-pic.avif)

​										把LeftController脚本挂载到桌面精灵物体(即我们的兔子)上，然后把对应的 按键物体 和 骨骼拖拽到对应的格子里。

**接下来创建 RightController 脚本，控制角色的右半部分对鼠标事件进行响应：**

```
using System.Collections;

using System.Collections.Generic;

using UnityEngine;



public class RightController : MonoBehaviour

{

// 存储骨骼

[SerializeField] private Transform arm = null; // 右手胳膊骨骼

[SerializeField] private Transform hand = null; // 右手手掌骨骼



// 存储鼠标Sprite物体

[SerializeField] private SpriteRenderer mouse = null;



// Start is called before the first frame update

void Start()

{

      // 注册鼠标事件

      MPlayerInput.Single.RegisterMouseEventCallBack(() => MoveHand(mouse));

      // 注册点击鼠标事件

      MPlayerInput.Single.RegisterMouseClickCallBack(OnClick);

      // 注册松开鼠标事件

      MPlayerInput.Single.RegisterMouseRelaeaseCallBack(OnRelease);

}



// 移动手臂

private void MoveHand(SpriteRenderer target)

{

      // 计算胳膊指向鼠标的向量

      Vector3 tar = target.transform.position - arm.position;

      tar.z = 0; // 因为是 2D 的，所以 Z 轴值设为0

      // 计算 胳膊朝向向量 与 胳膊指向鼠标的向量 二者之间的夹角 

      float angle = Vector3.SignedAngle(arm.right, tar, Vector3.forward);



      // 根据计算得到的夹角对胳膊进行旋转

      arm.Rotate(Vector3.forward, angle);

      // 将手掌移动到 鼠标位置 再 偏向胳膊一点的位置

      hand.position = target.transform.position - arm.right * 0.5f;

}



// 当鼠标被点击时的回调(不论点击的是左键还是右键)

private void OnClick()

{

      // 设置鼠标Sprite 的颜色为红色

      mouse.color = Color.red;

}



// 当鼠标被抬起时的回调(不论抬起的是左键还是右键)

private void OnRelease()

{

      // 恢复颜色到白色

      mouse.color = Color.white;

}

}
```

注释写的相当详尽，且原理与键盘的一样，这里就不做过多赘述了。

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\c382ebdf46a18c9a4c9e4cb64824063a5ddb7c3c.png@!web-article-pic.avif)

​																							绿色为胳膊的朝向向量，红色为胳膊指向鼠标的向量

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\c19b381dfcd3c41212c15cea974d64ee43521bf4.png@!web-article-pic.avif)

​																							挂载 Right Controller 脚本，并给对应物体赋值
至此，我们的 2D桌面精灵 的制作就完毕了。

补充一下漏掉的东西，如果我们要窗体透明，还需要把摄像机设置为 Solid Color 背景颜色设置为 黑色：

![img](D:\Unity资料和笔记\个人笔记\Unity2DDesktopSprite-Image\57192a93adc1ea5c72ccb1dcaf6d1ec87d24a19d.png@!web-article-pic.avif)

​																															摄像机的设置