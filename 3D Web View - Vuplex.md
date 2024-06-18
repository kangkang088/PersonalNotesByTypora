# 3D Web View - Vuplex

## 网站证书无效(SSL/TLS)：

![image-20240612093346799](D:\Unity资料和笔记\个人笔记\3D Web View - Vuplex_Image\image-20240329193640483.png)

解决方法：

![image-20240612093544994](D:\Unity资料和笔记\个人笔记\3D Web View - Vuplex_Image\image-20240612093544994.png)

## 网站内嵌视频无法播放：

**点击工具栏Vuplex-Enable Proprietary Video Codecs，勾选后点击下方按钮即可。**

![image-20240617152445928](D:\Unity资料和笔记\个人笔记\3D Web View - Vuplex_Image\image-20240617152445928.png)

![image-20240617152609314](D:\Unity资料和笔记\个人笔记\3D Web View - Vuplex_Image\image-20240617152609314.png)

## 网站加载失败重新加载（回调函数）：

![image-20240617174745479](D:\Unity资料和笔记\个人笔记\3D Web View - Vuplex_Image\image-20240617174745479.png)

## 一些其他常见问题

转载自：[哈哈，骗你的！ヾ(ﾟ∀ﾟゞ) (horse7.cn)](http://horse7.cn/2023/11/02/unity-使用3d-webview-for-windows-加载网页/)

官网上有很多实用的文章和教程，能够解决遇到的大部分问题。官方文档https://support.vuplex.com/search 其他零碎问题：输入法、中文输入、开启上传文件、开启下载文件、文件保存位置、光标状态、页面内拖拽或拖拽滑动页面、cookie的增删改查。

![img](D:\Unity资料和笔记\个人笔记\3D Web View - Vuplex_Image\webview2.jpg)

```c#
using System;
using System.Collections;
using System.Collections.Generic;
using System.Diagnostics;
using System.IO;
using System.Runtime.InteropServices;
using UnityEngine;
using Vuplex.WebView;
using Vuplex.WebView.Demos;
using Debug = UnityEngine.Debug;

/// <summary>
/// 解决webview输入问题 没有这个脚本，不能正常输入
/// （解决中文输入是在CanvasWebViewPrefab.cs Input.imeCompositionMode = IMECompositionMode.On;）
/// 解决在Windows平台上传文件，打开选择文件对话框
/// </summary>
public class WebViewKeyBoardInput : MonoBehaviour
{
    private CanvasWebViewPrefab canvasWebView;
    HardwareKeyboardListener _hardwareKeyboardListener;

    public System.Action<ProgressChangedEventArgs> OnLoadProgressChanged;

    async void Start()
    {
        canvasWebView = transform.GetComponent<CanvasWebViewPrefab>();
        #region 输入框输入功能
        _setUpHardwareKeyboard(canvasWebView);
        #endregion
        await canvasWebView.WaitUntilInitialized();

        #region 开启上传功能
        var standaloneWebView = canvasWebView.WebView as StandaloneWebView;
        standaloneWebView.SetNativeFileDialogEnabled(true);
        #endregion

        #region 开启下载功能
        var webViewWithDownloads = canvasWebView.WebView as IWithDownloads;
        if (webViewWithDownloads == null)
        {
            Debug.Log("This 3D WebView plugin doesn't yet support IWithDownloads: " + canvasWebView.WebView.PluginType);
            return;
        }
        webViewWithDownloads.SetDownloadsEnabled(true);
        webViewWithDownloads.DownloadProgressChanged += (sender, eventArgs) => {
            Debug.Log($@"DownloadProgressChanged:
                    Type: {eventArgs.Type},
                    Url: {eventArgs.Url},
                    Progress: {eventArgs.Progress},
                    Id: {eventArgs.Id},
                    FilePath: {eventArgs.FilePath},
                    ContentType: {eventArgs.ContentType}"
            );
            if (eventArgs.Type == ProgressChangeType.Finished)
            {
                Debug.Log("Download finished");
                // 默认下载路径是 Application.temporaryCachePath(Appdata/Local/Temp/公司名/应用名)
                string[] filePath = eventArgs.FilePath.Split('\\');
                string fileName = filePath[filePath.Length - 1];
                //如果原来路径上存在该文件 则需要删除后在进行移动
                if(File.Exists(Application.dataPath + "/../Download/" + fileName))
                {
                    File.Delete(Application.dataPath + "/../Download/" + fileName);
                }
                //移动到指定的固定位置
                File.Move(eventArgs.FilePath, Application.dataPath + "/../Download/" + fileName);
                //打开下载文件的目录
                Application.OpenURL($"File://{Application.dataPath}/../Download/");

                //只能打开绝对路径
                //string newPath = Application.dataPath + "/../Download/" + fileName;
                //WindowsTools.ExplorerFile(newPath);
            }
        };
        #endregion

        #region file选择 新版3dwebview4.2支持自定义文件对话框(也有bug，多次选择没有效果)
        //var webViewWithFileSelection = canvasWebView.WebView as IWithFileSelection;
        //if (webViewWithFileSelection != null)
        //{
        //    webViewWithFileSelection.FileSelectionRequested += (sender, eventArgs) =>
        //    {
        //        // Note: Here's where the application could use a system API or third party
        //        //       asset to show a file selection UI and then pass the selected file(s) to
        //        //       the Continue() callback.
        //        var filePaths = new string[] { OpenWindowDialog() };
        //        eventArgs.Continue(filePaths);
        //    };
        //}

        #endregion

        #region 页面加载失败回调
        canvasWebView.WebView.PageLoadFailed += WebView_PageLoadFailed;
        #endregion

        #region 加载进度 新版插件才支持
        canvasWebView.WebView.LoadProgressChanged += (sender, args) =>
        {
            if(OnLoadProgressChanged!=null)
            {
                OnLoadProgressChanged.Invoke(args);
            }
        };
        #endregion

        #region 光标状态 新版插件才支持

        #endregion
    }

    /// <summary>
    /// 页面加载失败回调  失败后显示提示，同时让浏览器加载空页面防止出现404界面
    /// </summary>
    /// <param name="sender"></param>
    /// <param name="e"></param>
    private void WebView_PageLoadFailed(object sender, System.EventArgs e)
    {
        WindowsTools.MessageBox(System.IntPtr.Zero, "数据加载失败，请检查是否可以连接到服务器！", "错误", 0);
        canvasWebView.WebView.LoadUrl("about:blank");
    }

    void _setUpHardwareKeyboard(CanvasWebViewPrefab webView)
    {

        _hardwareKeyboardListener = HardwareKeyboardListener.Instantiate();
        _hardwareKeyboardListener.KeyDownReceived += (sender, eventArgs) => {
            var webViewWithKeyDown = webView.WebView as IWithKeyDownAndUp;
            if (webViewWithKeyDown != null)
            {
                webViewWithKeyDown.KeyDown(eventArgs.Value, eventArgs.Modifiers);
            }
            else
            {
                webView.WebView.SendKey(eventArgs.Value);
            }
        };
        _hardwareKeyboardListener.KeyUpReceived += (sender, eventArgs) => {
            var webViewWithKeyUp = webView.WebView as IWithKeyDownAndUp;
            webViewWithKeyUp?.KeyUp(eventArgs.Value, eventArgs.Modifiers);
        };
    }

    

    float timer = 0;
    private void Update()
    {
        //让弹窗保持在最前端 因为不知道什么时候才有弹窗，所以循环查找
        timer += Time.deltaTime;
        if(timer >= 0.5f)
        {
            SetFileDialogFor();
            timer = 0;
        }
    }

    private void SetFileDialogFor()
    {
        IntPtr hWnd =WindowsTools.FindWindow(null,"Open Files");
        if (hWnd == IntPtr.Zero)
            hWnd = WindowsTools.FindWindow(null, "Open File");
        if(hWnd != IntPtr.Zero)
            WindowsTools.SetForegroundWindow(hWnd);
    }
}
```

**cookie操作**

```c#
//保存cookie
private async void SetWebCookie(string cookieValue)
{
    if (Web.CookieManager == null)
    {
        Debug.Log("Web.CookieManager isn't supported on this platform.");
        return;
    }
    string[] urlSplit = loginUrl.Split(':');

    var success = await Web.CookieManager.SetCookie(new Cookie
    {
        Domain = urlSplit[1].Replace("//", ""),
        Path = "/",
        Name = "Admin-Token",
        Value = cookieValue,
        //到期时间7天
        ExpirationDate = (int)DateTimeOffset.Now.ToUnixTimeSeconds() + 60 * 60 * 24* 7,
        HttpOnly = false,
        Secure = false
    });
    Debug.Log($"保存cookie {success}！");
}

//获取cookie
private async void GetCookie()
{
    if (Web.CookieManager == null)
    {
        Debug.Log("Web.CookieManager isn't supported on this platform.");
        return;
    }
    var cookies = await Web.CookieManager.GetCookies("http://192.168.32.227");
    if (cookies.Length > 0)
    {
        Debug.Log("Cookie: " + cookies[0]);
    }
    else
    {
        Debug.Log("Cookie not found.");
    }
}
```
