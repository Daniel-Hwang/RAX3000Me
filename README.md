欢迎支持一键三连，点颗星星~谢谢呢

**20251211,新硬件更新，增加新盐，如果无法导出配置，就只能进行flash硬件刷写了**

**20250426由于RAX3000M固件美化页面更新，T760（U20-F50）系列终结5G竞赛——RAX3000M开启飞猫U20轻NAS挂载详细教程，见RAX3000Me/20250426-RAX3000M-suc文件夹**

***20250417补充：uboot文件，请从telnet或者ssh里面进行刷写，也就是说“三、通过Telnet更新Uboot”步骤中的教程***

***20250415重要更新：20241111-RAX3000Me_Step12-TelnetUboot文件夹中增加新的uboot文件20250415_RAX3000me-nand-ddr3_HY_uboot.bin，全面支持.itb固件，按需索取，可替换mt7981_cmcc_rax3000m-fip-fixed-parts.bin***

***20250401重要更新：20250321-RAX3000Me_nessipk_installedpac文件夹中有必要的软件包（对应immortalwrt-23.05.3），用于玩家从非我提供的bin刷机包刷机，请使用软件包时注意！！！！！此页面中的ipk包是kernel=5.15.162版本的，相对应的factory和sysupgrade文件为0318版！！！***

***这些ipk包不支持任何factory.itb和sysupgrade.itb的openwrt刷机包！！！请对应kernel进行安装软件包***

***20250321重要更新：add new wonderful drivers and firmware for brigding eth2(RNDIS-CDC) to lan***

***20250319重要更新：ADD usbutils RNDIS USB CDC and so on for USB 3.0 network drivers and storage***

***file: RAX3000Me-250316-sysupgrade.bin for update.***

***20250318重要更新：20250314更新修正，修正window中telnet解密的前13个字符，去掉后再返回正确的mypassword，用于之后的生成加密配置文件。主要是对powershell文件进行修改***

RAX3000Me路由器开发与固件刷入详细教程

软件版本号：RAX3000Me-MTK.ZD.03
生产日期：20241111

**闪存型号FM25S01A**
复旦微FUDANmicro
![image](https://github.com/user-attachments/assets/46e4f646-953f-4275-9e8a-c9b714ef617c)


一、准备工作

1.2025.3.13版本realese-v1.0.0,说明需要使用linux虚拟机来生成解Telnet文件，推荐使用Ubuntu虚拟机，确保OpenSSL版本大于3.0（支持aes-256-cbc -pbkdf2）。
这一步已经在2025.3.14版本上更新windows上安装openssl，如下文件
![image](https://github.com/user-attachments/assets/35643fd4-8176-4e27-b678-2fcf4b462475)
直接安装，下一步下一步下一步，全部默认安装finish完成，搞定！

2.下载所需文件：https://www.123865.com/s/9JnLTd-18jhd?提取码:OI7L，或者直接下载release-v1.0.0
补充支持windows的生成解密conf的bat和ps1文件在https://github.com/Daniel-Hwang/RAX3000Me/tree/main/20241111-RAX3000Me_Step12-TelnetUboot

二、生成加密配置文件

1.在Ubuntu终端输入自己的SN码，获取设备序列号（SN位于设备背面）：
SN=5D11210006XXXXX

2.在Ubuntu终端中依次输入以下命令生成密码：

mypassword=$(openssl passwd -1 -salt aV6dW8bD "$SN")

mypassword=$(eval "echo $mypassword")

echo $mypassword

3.下载Telnet解锁配置文件：

wget https://github.com/Daniel-Hwang/RAX3000Me/raw/refs/heads/release-v1.0.0/20241111-RAX3000Me_Step12-TelnetUboot/RAX3000M_XR30_cfg-telnet-20240117.conf

4.加密并导入配置文件：

openssl aes-256-cbc -pbkdf2 -k "$mypassword" -in RAX3000M_XR30_cfg-telnet-20240117.conf -out cfg_import_config_file_new.conf


20250314重要更新：此1-4步，在2025.3.14版本上，安装完windows的openssl后，直接点击generate_config.bat，输入SN码后，直接生成cfg_import_config_file_new.conf。
![image](https://github.com/user-attachments/assets/813d030a-ef0c-4a65-8701-30d981e2b97c)

***20250321重要更新：add new wonderful drivers and firmware for brigding eth2(RNDIS-CDC) to lan***

将生成的cfg_import_config_file_new.conf文件上传到路由器后台的配置导入页面，重启完成Telnet解锁。有bug可以issus我

三、通过Telnet更新Uboot

使用网线连接路由器LAN口

Telnet登录路由器（默认IP地址192.168.10.1，无需账号密码）。

在Windows电脑中使用HTTP_File_Server.exe（Win8兼容模式），添加mt7981_cmcc_rax3000m-fip-fixed-parts.bin文件，并且开启http file server。

在Telnet终端下载Uboot文件至路由器/tmp文件夹下：

wget -P /tmp http://192.168.10.2/mt7981_cmcc_rax3000m-fip-fixed-parts.bin

烧录Uboot：

mtd write /tmp/mt7981_cmcc_rax3000m-fip-fixed-parts.bin FIP

完成后，路由器IP地址变为192.168.1.x段。

四、这才正式开始刷入OpenWrt固件

设置电脑静态IP为192.168.1.2

子网掩码255.255.255.0

网关192.168.1.1

用曲别针或者取卡针按住路由器RESET按钮，插电后约10秒，指示灯变色（蓝色或绿色，有些RAX路由器是绿色，可能是硬件的问题？）后松开。电脑网线连接路由器LAN口。

在浏览器中访问192.168.1.1，选择刷入20250318_RAX3000Me-HY-factory.bin固件，等待重启。

![image](https://github.com/user-attachments/assets/6fc9b8d3-db89-4123-8c4b-969e75234d2b)
（注意uboot页面中刷入factory.bin文件和openwrt页面中的升级sysupgrade.bin文件页面不同）

完成刷入并重启后，电脑改为自动获取IP，浏览器访问192.168.1.1(依照你所刷的固件和电脑IP网段而定)，在升级界面继续刷入20250318_RAX3000Me-HY-sysupgrade.bin固件。

至此，OpenWrt固件成功刷入RAX3000Me路由器。

**如果继续刷入RNDIS相关驱动，请参见文件夹20250321-RAX3000Me_nessipk_installedpac文件夹中readme**
![image](https://github.com/user-attachments/assets/63cfd93d-2b10-4dc1-9bfa-0dfcfc2475d0)

访问192.168.1.1开始愉快的immortal openwrt玩耍吧！

现在可以支持中兴F50，飞猫U20的type c转网口，网线连接RAX3000Me的WAN，跑满5G速度；或者使用RAX中继U20，也是稳定跑满，甚至比中兴飞猫的还要好一点点。
