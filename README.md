###### 

# 照片 UWP 去更新版

自己修改过的安装包，可共存，不会升级到WinUI3版。目前只有x64版本。

[蓝奏云](https://firedoge.lanzoue.com/i1Yju1vglgcd)

已知问题：

- 最小化后不会自动挂起

- 磁贴会炸

## 打包方法：
安装包获取可以到[这个网站](https://store.rg-adguard.net)

解包与去更新请参考[从网易云UWP说起：修改AppManifest阻止UWP自动更新/多版本共存](https://zhuanlan.zhihu.com/p/146393154)

首先安装[Windows SDK](https://developer.microsoft.com/zh-cn/windows/downloads/windows-sdk/)和[WSAppBak](https://github.com/Wapitiii/WSAppBak)。

打开PowerShell，运行以下命令来生成证书：

`New-SelfSignedCertificate -Type Custom -Subject "CN=PandaFiredoge" -KeyUsage DigitalSignature -FriendlyName "友好名称" -New-SelfSignedCertificate -Type Custom -Subject "CN=你的证书名" -KeyUsage DigitalSignature -FriendlyName "友好名称" -CertStoreLocation "Cert:\CurrentUser\My" -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.3", "2.5.29.19={text}") -NotBefore (Get-Date) -NotAfter (Get-Date).AddYears(生效年数)`



生成之后，按Win+R输入certmgr.msc（勿以管理员身份运行），进入个人—证书，右键你生成的证书，所有任务—导出，点击下一步—是，导出私钥—下一步—随便设个密码（记下来，后面要用到）—下一步—选择导出的目录和文件名—下一步—完成，导出证书备用



打开`WSAppBak.exe`，把解包出来的路径输入（拖动也可以）到命令行，回车，把想要输出安装包的路径输入（拖动）到命令行，回车

，在弹出的窗口中选择“None”，按任意键退出`WSAppBak.exe`，打开输出安装包的路径，把除了.appx后缀名之外的两个文件删掉。



进入`C:\Program Files (x86)\Windows Kits\10\bin\（版本号）\x64`，右键空白处，在终端中打开，在命令行中键入

`.\signtool.exe sign /debug /fd sha256 /a /f 路径\你的证书.pfx /p 证书的密码 路径\刚才输出的安装包.appx`

回车。



右键`刚才输出的安装包.appx`，属性，转到“数字签名”选项卡，选中签名列表里的签名，点击详细信息，点击查看证书，点击安装证书，

选择本地计算机，点击下一步（如果需要管理员权限，请允许），选择将所有的证书都放入下列存储—浏览—受信任的根证书颁发机构—确定—下一步—完成—确定—确定—确定。



双击安装包，即可安装。

大功告成！



