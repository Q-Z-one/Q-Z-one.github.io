#

## 清理缓存

1. 在正常进入系统之后，同时按住Windows键（微软图标）+ 字母X， 选择“命令提示符（管理员）”，之后输入Dism /online /cleanup-image /restorehealth  ，之后回车，等待此命令完成后，再输入Sfc /scannow ，之后回车，等待命令完成后重新启动设备。

2. 在正常进入系统之后，同时按住Windows键（微软图标）+ 字母X， 选择“命令提示符（管理员）”，之后输入以下命令

```
PowerShell -ExecutionPolicy Unrestricted 

Get-AppXPackage -AllUsers | Foreach {Add-AppxPackage -DisableDevelopmentMode -Register "$($_.InstallLocation)\AppXManifest.xml"}
```

回车，然后

```
PowerShell -ExecutionPolicy Unrestricted 

$manifest = (Get-AppxPackage Microsoft.WindowsStore).InstallLocation + '\AppxManifest.xml' ; Add-AppxPackage -DisableDevelopmentMode -Register $manifest
```
回车，重启设备。

## 关闭Windows　Defender开机自启

1. 专业版：
	1. ```win + R```打开运行框，输入```gpedit.msc```，打开组策略编辑器
	2. 在本地组策略编辑器上点击“计算机配置-管理模板-Windows组件-Windows Defender”，在窗口右侧找到“关闭Windows Defender” 
	3. 双击打开关闭Windows Defender，选择“已启用”点击确定，这时打开设置里的windows defender可以发现实时保护是关闭的
	4. 重启

2. 家庭版：
	1. 打开“命令提示符（管理员）”，然后输入：
	```
	reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows Defender" /v "DisableAntiSpyware" /d 1 /t REG_DWORD /f
	```
	重启电脑
	2. 想恢复开机自启：
	Win键+R，运行 regedit 打开注册表编辑器，定位到 HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows Defender 这儿，删掉 DisableAntiSpyware这个键值即可

## Reference

1. https://answers.microsoft.com/zh-hans/surface/forum/surfpro4-surfperf/surface-pro-4/4733c6bf-d9b8-431f-9494-3877b6a87c65
2. https://jingyan.baidu.com/article/4ae03de3ec67db3efe9e6b43.html
3. https://www.ithome.com/html/win10/279485.htm
