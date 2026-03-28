![[Pasted image 20260311204734.png]]
音频接口可以理解为那种圆孔耳机插口，使音视频得以输出。
micro HDMI接口指Micro High-Definition Multimedia Interface，即微型高清晰度多媒体接口，用于接显示器，输出视频和声音。有两个接口意味着可同时接两个屏。
此图中靠上处未标示的是40-pin GPIO（General Purpose Input/Output通用输入输出）40针脚扩展接口，可以用于连接LED灯带、电机、蜂鸣器、继电器、传感器和其他一些兼容的外设等。
SD卡插于背面。



SD卡：Secure Digital Memory Card（安全数码存储卡）
如果说树莓派是主机，SD卡就是树莓派的硬盘和系统盘，使用SD卡以存放操作系统和其他文件、数据

SD卡读卡器：SD Card Reader
一个小转接器，一头插 SD 卡，一头插电脑USB。让电脑能读写 SD 卡，以及把文件从电脑传到SD卡
主要用于给树莓派烧录操作系统。

建议入门搞个64G的sd卡，有需要再换更大的。注意树莓派用的是microSD即小sd卡，并且此时不用考虑sd卡的格式。

购买的sd卡，其在电脑上显示的大小比型号标榜的要小，是因为对于闪存和硬盘厂商，其在生产时的芯片容量是按正常的二进制计算，但他们在标称容量时选择十进制，以节约成本。比如购买标榜64GB的sd卡，实际使用时换算显示约 59.6 GB。此外，还有一部分空间用于存储文件分配表（FAT）、分区表等，用于文件系统和分区的开销。



第一步是烧录操作系统，即将操作系统文件写入sd卡，然后将sd卡作为系统盘插入树莓派使用。注意写入操作系统会**清空**sd卡中的所有文件。
将sd卡插入读卡器时，注意sd卡金属触点朝下
下载步骤可参考：[下载操作系统参考-51CTO博客](https://blog.51cto.com/u_12226/13748679)

树莓派官方操作系统为**Raspbian**，后更名为**Raspberry Pi OS**，均为基于Dibian、以Linux为内核的操作系统，Operating System。

系统镜像下载：[Raspberry Pi software – Raspberry Pi](https://www.raspberrypi.com/software/)
此处的镜像与镜像站不可混为一谈。系统镜像，指整盘拷贝已经打包压缩好的系统文件；而镜像站，指由于如官方服务器在国外，为加速下载而可能在国内搭建的复制服务器或网站。
可以在此网站下载辅助用的**烧录工具**Raspberry Pi Imager
![[P1 Raspberry Pi Imager.png]]

选择64位的安装![[P2 安装.png]]


勾选exclude system drives指排除系统盘，防止将电脑系统盘误选择，而导致其格式化
![[Pasted image 20260304215606.png]]


authentication n.验证
SSH指Secure Shell，安全外壳协议，是一种网络协议。简单来说就是可通过网络，在另一台设备上输入命令以控制树莓派。一般情况下都会勾选。
下面两个选项一个是使用密码登录，一个使用公钥密钥登录。利用密钥登录会更安全，不需要输入密码。然而其配置步骤会复杂一些。
![[Pasted image 20260310200752.png]]



下好后，sd卡更名为bootfs并呈现如下文件：![[Pasted image 20260311152327.png]]
不可新建一文件夹再放入它们，因树莓派的启动加载器（bootloader）会直接在bootfs根目录寻找内核、`.dtb`(如 `bcm2710-rpi-3-b-plus.dtb`，后缀全称Device Tree Blob，树莓派的硬件描述文件)以及`config.txt` 等文件。
目前，只能在bootfs根目录继续新建文件。


新建一名为ssh无后缀文件。
常态下树莓派OS默认关闭SSH，需要树莓派开机后接显示器、键盘，手动输入命令开启 SSH，才能远程连接。有此文件后，树莓派开机SSH会自动启动，直接在电脑上用 SSH 工具（PuTTY或终端等）就能控制树莓派。


接下来，新建一名为wpa_supplicant.conf文件。
.conf为configuration配置文件，是各类程序、系统的**配置文件后缀**，本质是**纯文本文件**，用来存储程序运行所需的参数、规则、设置等。
此文件作用在于，将连接的WiFi名称（SSID）、密码等信息提前写入文件，树莓派开机后会自动读取这个文件，连接指定的 WiFi（如家用 WiFi或手机热点），无需手动输入 WiFi 密码。只有当树莓派连上网后，才能在电脑上通过网络找到它的 IP 地址，并用 SSH 远程连接，否则无法远程操作。
如果没有配置此文件，则需要接网线和显示器以手动配置WiFi连接。
```
country=CN 
ctrl_interface=DIR=/var/run/wpa_supplicant 
GROUP=netdev 
update_config=1 
network={ 
ssid="替换成你的WiFi名称" 
psk="替换成你的WiFi密码" 
}
```
国家码参数用于指定WiFi的国家或地区代码，决定了树莓派使用的WiFi信道和功率限制。
还可添加优先级字段（priority），当树莓派同时搜到多个已配置的 WiFi 时，会优先连接 `priority` 数值更大的那个
![[Pasted image 20260311165851.png]]
注意建议==**不要使用校园网**==配置Wifi。校园网与家庭wifi、手机热点等普通的WPA-PSK个人认证不同，多是Portal 网页认证、802.1X 企业认证、客户端认证等，需要打开浏览器输入账号密码等相关验证信息，甚至绑定 MAC地址。这就意味着刚烧录完的无显示器和桌面的树莓派无法完成验证流程。



在通电前简单介绍一下树莓派灯语。需要知道的是，树莓派版本、操作系统版本和Debian小版本不同，灯语也可能不同。以下为Debian 13 trixie版。
通电后可观察其USB-C供电口附近有两个灯亮起，**一红一绿**。
PWR，电源状态灯，红灯灯语：常亮表示电源正常，供电稳定。闪烁或不亮表明供电异常。
ACT，系统活动灯，绿灯。其可能有三种情况，完全不亮，微弱闪亮（暗闪）和像红灯一样明显亮起。
绿灯**微弱亮光、偶尔暗闪**时，表示其正处于SD卡高速读写状态，一般发生在刚启动时。
正常情况下，也是多数情况，==**其1~2秒轻微闪烁一次**==，表明系统无大量SD读写，正在空闲待机。
绿灯**短暂明显闪烁后恢复微光暗闪**，表示临时小量SD读写，如执行单条命令、创建文件等。
其他情况下，出现规律的绿灯慢闪，如4次一组，需要对照灯语表排查具体问题。



将SD卡插入，通电。开启手机热点，确认手机、PC及树莓派均处于wpa_supplicant.conf配置中的同一个网络。
用网线连接树莓派与PC。网线物理连接成功后，可以看到网线接口附近的小灯常亮绿灯或黄灯。![[f000a7916864848c64f8bcf767279ea2_720.jpg]]
显然，使用**显示屏**连接树莓派后直接使用，并进行诸如查询IP地址的操作，会更为便捷。以下内容进作为**无显示屏**时查询IP地址的参考。

此时可以先拔掉网线，打开PuTTY，输入树莓派的本地域名，PuTTY可以直接搜索并连接。
![[Pasted image 20260323210845.png]]
在这步前，若绿灯灯语有问题，可先等待几分钟，如果持续有问题，插拔SD卡。如有必要，重新烧录系统。
随后输入用户名、密码登录。
![[d73a4608403ee33bbe4c6c18d73abf87.png]]
![[Pasted image 20260323211004.png]]此时，指令`hostname -I`即可查询树莓派本机IP
在Windows电脑无法使用ctrl c v复制，可以ctrl c复制后，左键终端的光标，即复制内容。
![[Pasted image 20260323211311.png]]



接下来可给树莓派包管理器换源。首先确认你当前的树莓派**Debian系统**是在使用哪个**小版本**，可能分为bookworm（Debian 12，稳定版）、trixie（Debian 13，最新、测试版）、bullseye（11）、buster（10）等。
可使用命令`cat /etc/os-release | grep VERSION_CODENAME`查询版本。
cat为Linux读取文件命令。
os-release文件里记录了系统的版本、昵称、发行商等信息。
| 管道符可将前一命令的输出传给后一命令处理。
grep为搜素、过滤命令，此处即只检索版本昵称。
![[Pasted image 20260323212652.png]]
可得到当前版本为==**trixie**==。


接着先备份原始源，有备无患。
![[Pasted image 20260323212803.png]]
接着使用nano修改**主源文件**source.list
`sudo nano /etc/apt/sources.list`
```
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ trixie main contrib non-free non-free-firmware
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ trixie main contrib non-free non-free-firmware

deb https://mirrors.tuna.tsinghua.edu.cn/debian-security/ trixie-security main contrib non-free non-free-firmware
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian-security/ trixie-security main contrib non-free non-free-firmware

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ trixie-updates main contrib non-free non-free-firmware
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ trixie-updates main contrib non-free non-free-firmware
```
换行可按下箭头。
编辑后，ctrl o保存，回车，ctrl x退出。

随后修改树莓派专用源raspi.list
`sudo nano /etc/apt/sources.list.d/raspi.list`
写入`deb https://mirrors.tuna.tsinghua.edu.cn/raspberrypi/ trixie main ui`

接着输入命令`sudo apt update`更新源。没有出现大量报错就算成功。可以用AI分析报错信息。
![[Pasted image 20260323215948.png]]



通过指令`sudo shutdown now`关机树莓派.
对于此trixie版本，当**绿灯完全熄灭、只剩红灯常亮**时，即可插拔电源。