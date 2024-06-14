## UDP管理类

```
public class UdpStateEventArgs : EventArgs
{
    public IPEndPoint remoteEndPoint;
    public byte[] buffer = null;
}

public delegate void UdpReceivedEventHandler(UdpStateEventArgs args);

public class UdpReceiver
{
    private UdpClient udpClient;

public event UdpReceivedEventHandler UdpReceived;

public UdpReceiver(string localIp,int localPort)
{
    IPEndPoint localPoint = new IPEndPoint(IPAddress.Parse(localIp),localPort);
    udpClient = new UdpClient(localPoint);
    Task.Run(() =>
    {
        while(true)
        {
            UdpStateEventArgs args = new UdpStateEventArgs();
            if(udpClient != null)
            {
                IPEndPoint remotePoint = new IPEndPoint(IPAddress.Any,0);
                var received = udpClient.Receive(ref remotePoint);
                args.remoteEndPoint = remotePoint;
                args.buffer = received;
                UdpReceived?.Invoke(args);
            }
            else
            {
                break;
            }
        }
    });
}

}
```

## 解决Winform多线程访问UI控件问题

```
internal class ControlCross
{
    public static void ExchangeInterface(WebView2 web,PictureBox p,bool isOpenWeb,bool isOpenPic)
    {
        if(web.InvokeRequired && p.InvokeRequired)
        {
            MethodInvoker mi = delegate ()
            {
                web.Visible = isOpenWeb;
                p.Visible = isOpenPic;
            };
            web.Invoke(mi);
            p.Invoke(mi);
        }
        else
        {
            web.Visible= isOpenWeb;
            p.Visible= isOpenPic;
        }
    }
}
```

## 窗口逻辑

```
public partial class Form1 : Form
{
    private string content;

public Form1()
{
    InitializeComponent();
    CheckForIllegalCrossThreadCalls = false;
}

private void Form1_Load(object sender,EventArgs e)
{
    webView21.Visible = true;
    pictureBox1.Visible = false;
    webView21.Width = Screen.PrimaryScreen.Bounds.Width;
    webView21.Height = Screen.PrimaryScreen.Bounds.Height;
    pictureBox1.Width = Screen.PrimaryScreen.Bounds.Width;
    pictureBox1.Height = Screen.PrimaryScreen.Bounds.Height;
    webView21.Source = new Uri("***");

    UdpReceiver udpReceiver = new UdpReceiver(GetLocalIP(),8081);
    udpReceiver.UdpReceived += UdpReceiverProcess;
}
private string receiveBuffer = "";

private void UdpReceiverProcess(UdpStateEventArgs args)
{
    receiveBuffer = Encoding.UTF8.GetString(args.buffer);
    if(receiveBuffer == "Web")
        ControlCross.ExchangeInterface(webView21,pictureBox1,true,false);
    else if(receiveBuffer == "Idle")
        ControlCross.ExchangeInterface(webView21,pictureBox1,false,true);
}
private string GetLocalIP()
{
    IPAddress[] ips = Dns.GetHostAddresses(Dns.GetHostName());
    foreach(IPAddress ip in ips)
    {
        if(ip.AddressFamily == AddressFamily.InterNetwork)
            return ip.ToString();
    }
    return null;
}

}
```

