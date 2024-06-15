# Unity3D桌面精灵

## 3D桌面精灵制作的探索

![1](D:\Unity资料和笔记\个人笔记\Uniry3DDesktopSprite-Image\1.png)

![2](D:\Unity资料和笔记\个人笔记\Uniry3DDesktopSprite-Image\2.png)

## 3D桌面精灵的制作

![3](D:\Unity资料和笔记\个人笔记\Uniry3DDesktopSprite-Image\3.png)

![4](D:\Unity资料和笔记\个人笔记\Uniry3DDesktopSprite-Image\4.png)

## 代码部分

首先是 hook 实现窗体透明 和 hook 实现后台检测按键输入。这一部分与 2D桌面精灵文章中使用的代码没有什么区别可以使用。

唯一的区别在于，之前可以注册的键盘回调函数只能是 Action 类型，而这里我又添加了一种 Action<KeyCode> 类型，即当我们按下键盘时，可以把按下的键盘名传入到回调函数中，让回调函数自身去判断然后再做进一步处理。之前是检测到按键后对按键名进行判断和分析再调用对应回调函数，这种方式现在仍有保留。

这里把修改后的 KeyCodeController  和 MPlayerInput 脚本给出，如果已经理解了 讲解2D桌面精灵时给出的那两个脚本，那么对于里面做出的修改应该不难理解。

**KeyCodeController脚本：**

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

private const int WH_KEYBOARD_LL = 13;

private const int WM_KEYDOWN = 0x0100;

private const int WM_KEYUP = 0x0101;

private const int WM_SYSKEYDOWN = 0x0104;

private const int WM_SYSKEYUP = 0x0105;

private static LowLevelKeyboardProc _proc = HookCallback;

private static IntPtr _hookID = IntPtr.Zero;



private static Dictionary<KeyCode, bool> keyDownDic = new Dictionary<KeyCode, bool>();



// Use this for initialization

void Start()

{

     _hookID = SetHook(_proc);

}

void OnApplicationQuit()

{

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

private static IntPtr HookCallback(int nCode, IntPtr wParam, IntPtr lParam)

{



     int vkCode = Marshal.ReadInt32(lParam);

     KeyCode key = KeyCode.None;

     try

     {

         key = (KeyCode)(vkCode + 32);

     }

     catch { }



     if (nCode >= 0 && wParam == (IntPtr)WM_KEYDOWN)

     {


             

         key = (KeyCode)(vkCode + 32);

         if(!keyDownDic.ContainsKey(key) || keyDownDic[key] == false)

         {

             MPlayerInput.Single.KeyDownCallBack(key);

             keyDownDic[key] = true;

         }

     }



     if (nCode >= 0 && wParam == (IntPtr)WM_KEYUP)

     {

         key = (KeyCode)(vkCode + 32);

         keyDownDic[key] = false;

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

**MPlayerInput 脚本：**

```
using System;

using System.Collections;

using System.Collections.Generic;

using UnityEngine;



public delegate void KeyDownCallBack(KeyCode key);

public delegate void KeyUpCallBack(KeyCode key);



public class MPlayerInput : MonoBehaviour

{

public static MPlayerInput Single;



private Dictionary<KeyCode, Action> keyDownDic = new Dictionary<KeyCode, Action>();

private Dictionary<KeyCode, Action> keyUpDic = new Dictionary<KeyCode, Action>();

private List<KeyDownCallBack> keyDownSelfKeyList = new List<KeyDownCallBack>();

private List<KeyUpCallBack> keyUpSelfKeyList = new List<KeyUpCallBack>();



private Action<Vector3> mouseMoveList = (movement) => { };

private Action mouseEventCall = () => { };

private Action mouseClickCall = () => { };

private Action mouseReleaseCall = () => { };



private void Awake()

{

      if(Single != null)

      {

          Destroy(Single.gameObject);

      }



      Single = this;

}



public void RegisterMouseMoveCallBack(Action<Vector3> callBack)

{

      mouseMoveList += callBack;

}

public void RegisterMouseEventCallBack(Action callBack)

{

      mouseEventCall += callBack;

}

public void RegisterMouseClickCallBack(Action callBack)

{

      mouseClickCall += callBack;

}

public void RegisterMouseRelaeaseCallBack(Action callBack)

{

      mouseReleaseCall += callBack;

}



public void MouseMoveCallBack(Vector3 movement)

{

      mouseMoveList.Invoke(movement);

}

public void MouseEventCallBack()

{

      mouseEventCall.Invoke();

}

public void MouseClickCallBack()

{

      mouseClickCall.Invoke();

}

public void MouseReleaseCallBack()

{

      mouseReleaseCall.Invoke();

}



public void RegisterKeyDownCallBack(KeyCode key, Action callBack)

{

      if (!keyDownDic.ContainsKey(key)) keyDownDic[key] = callBack; 

      else keyDownDic[key] += callBack;

}

public void RegisterKeyDownCallBackSelfKey(KeyDownCallBack callBack)

{

      keyDownSelfKeyList.Add(callBack);

}

public void RegisterKeyUpCallBack(KeyCode key, Action callBack)

{

      if (!keyUpDic.ContainsKey(key)) keyUpDic[key] = callBack;

      else keyUpDic[key] += callBack;

}

public void RegisterKeyUpCallBackSelfKey(KeyUpCallBack callBack)

{

      keyUpSelfKeyList.Add(callBack);

}



public void KeyDownCallBack(KeyCode key)

{

      if(keyDownDic.ContainsKey(key)) keyDownDic[key].Invoke();

      KeyDownCallBackSelfKey(key);

}

public void KeyUpCallBack(KeyCode key)

{

      if (keyUpDic.ContainsKey(key)) keyUpDic[key].Invoke();

      KeyUpCallBackSelfKey(key);

}

private void KeyDownCallBackSelfKey(KeyCode key)

{

      for(int i = 0; i < keyDownSelfKeyList.Count; ++i)

      {

          keyDownSelfKeyList[i].Invoke(key);

      }

}

private void KeyUpCallBackSelfKey(KeyCode key)

{

      for (int i = 0; i < keyUpSelfKeyList.Count; ++i)

      {

          keyUpSelfKeyList[i].Invoke(key);

      }

}

}
```

WindowSetting 和 MouseController 脚本是没有变化的。

我们把这些脚本挂载到场景物体上，这里我仍是挂载到了主摄像机上：

![img](D:\Unity资料和笔记\个人笔记\Uniry3DDesktopSprite-Image\45e518eae944e8818b0fad8fab4f9c4fd4a9801f.png@!web-article-pic.avif)

​																																主摄像机

接下来就是我们控制桌面精灵的脚本了。

**创建 Character Controller 脚本：**

```
using System.Collections;

using System.Collections.Generic;

using UnityEngine;



public class CharacterController : MonoBehaviour

{

// 存储所有键盘按键位置的根物体

// 所有的键盘按键都作为该物体的子物体

[SerializeField] private Transform keyBoardRoot = null;



// 存储 按键名 和 KeyBoard对象 构成的字典表

// 每个按键物体身上都会挂载一个 KeyBoard 对象

private Dictionary<string, KeyBoard> keyBoadDic = new Dictionary<string, KeyBoard>();

// 角色的 animator

private Animator animator;

// PlayeInput

private MPlayerInput playerInput;



// 当前手掌要移动到的位置

private Transform target;

// 手掌的引用

private Transform rightHand;

private Rigidbody rightHandRig;





private void Awake()

{

       // 初始化，获取所有按键物体身上的 KeyBoadr 脚本

       KeyBoard[] keyBoards = keyBoardRoot.GetComponentsInChildren<KeyBoard>();

       // 初始化，建表

       for (int i = 0; i < keyBoards.Length; i++)

       {

           keyBoadDic.Add(keyBoards[i].name, keyBoards[i]);

       }

}



// Start is called before the first frame update

void Start()

{

       // 获取 MplayerUnput、Animator 和 右手的 Transform、Rigidbody 的引用

       playerInput = MPlayerInput.Single;

       animator = GetComponent<Animator>();

       rightHand = animator.GetBoneTransform(HumanBodyBones.RightHand);

       rightHandRig = rightHand.GetComponent<Rigidbody>();



       // 注册按键按下 和 松开的回调

       playerInput.RegisterKeyDownCallBackSelfKey((key) => EnterKeyBoard(key));

       playerInput.RegisterKeyUpCallBackSelfKey((key) => ReleaseKeyBoard(key));

}



// 设置当前手掌要移动到的目标

// 为什么这么一句话要装成一个函数？

// 因为这个方法是当时还没构思完整时就写上了，后面也懒得删掉了

private void SetTarget(Transform target)

{

       this.target = target;

}



// 键盘按下时的回调

private void EnterKeyBoard(KeyCode key)

{

       // 通过 ToString 获取键盘名

       // 待优化：ToString 会产生 GC，虽然对于桌面精灵这样的小程序来说没什么大问题

       // 可以在 字典初始化时使用 Enum.Parse 来把字符串转成枚举

       // 这样在这里就不用 ToString 了

       // 话说我有时间在这里写这些注释，干嘛不直接改了。 唉，就是这么懒

       string keyStr = key.ToString();

       // 安全判断

       if (keyBoadDic.ContainsKey(keyStr))

       {

           SetTarget(keyBoadDic[keyStr].transform); // 设置当前的手掌移动目标

           keyBoadDic[keyStr].EnterKeyBoard(); // 调用对应按键的 KeyBoard 的 按下键盘方法(这么做是为了解耦，键盘就做键盘的事，角色就做角色的事)

       }

}



// 键盘抬起时的回调

private void ReleaseKeyBoard(KeyCode key)

{

       // ToString 获取键盘名

       // 与上面一样，可以进行优化

       string keyStr = key.ToString();

       // 安全判断

       if(keyBoadDic.ContainsKey(keyStr)) keyBoadDic[keyStr].ReleaseKeyBoard(); // 调用对应按键的方法

}



// LateUpdate 中负责执行对于骨骼控制的代码

private void LateUpdate()

{

       // 安全判断，如果 target == null 就没必要移动骨骼

       if (target == null) return;

       // 计算 目标按键 指向 手掌的向量

       Vector3 t = rightHand.position - target.position;



       // 因为我们采用的是布娃娃系统来控制骨骼的运动

       // 所以这里需要通过刚体来控制手掌的移动，让手掌朝着目标进行移动

       // 这里采用设置速度(Velocity) 的方式

       // 大家也可以试着用 AddForce 的方式

       rightHandRig.velocity = (target.position - rightHand.position) * 200f * Time.fixedDeltaTime;

       // 将手掌缓动旋转至手掌朝向目标

       rightHand.right = Vector3.Slerp(rightHand.right, t.normalized, Time.fixedDeltaTime);



}



private void OnAnimatorMove()

{



}



}
```



代码中有 keyBoardRoot 这个引用。 keyBoardRoot 是一个在 键盘模型下的子物体：

![img](D:\Unity资料和笔记\个人笔记\Uniry3DDesktopSprite-Image\43c0aaa833e26ba454fb1d319da4e1e55ad3b051.png@1256w_666h_!web-article-pic.avif)

​																														KeyBoardPos物体

![5](D:\Unity资料和笔记\个人笔记\Uniry3DDesktopSprite-Image\5.png)

**KeyBoard 脚本**

说回到按键物体本身，我们还有个 KeyBoard 脚本没有介绍：

```
using System.Collections;

using System.Collections.Generic;

using UnityEngine;

using UnityEngine.VFX;



public class KeyBoard : MonoBehaviour

{

// 存储粒子系统

private VisualEffect vfx;



// Start is called before the first frame update

void Start()

{

         // 初始化粒子系统

         vfx = GetComponentInChildren<VisualEffect>();

}



// 该键被按下时调用

public void EnterKeyBoard()

{

         vfx.Play(); // 开启粒子系统

}



// 该键被松开时调用

public void ReleaseKeyBoard()

{

         vfx.Stop(); // 关闭粒子系统

}

}
```

代码非常简单，就是开启和关闭粒子系统。

至此，我们的 3D桌面精灵 的制作就讲解完毕了。

^_^ 