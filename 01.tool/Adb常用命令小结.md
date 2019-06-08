# Adb常用命令小结

### 基础
``` 
adb version  显示 adb 版本 
adb help  帮助 
adb start-server  启动 adb
adb kill-server  停止 adb 服务器
```

### 一.设备控制
```
-d	指定当前唯一通过 USB 连接的 Android 设备为命令目标
-e	指定当前唯一运行的模拟器为命令目标
-s <serialNumber>	指定相应 serialNumber 号的设备/模拟器为命令目标

adb devices 显示设备
adb reboot 重启
adb -s 选择设备

例:
adb -s emulator-5556 reboot   重启 emulator-5556

```
### 二.安装卸载
```
adb install  path_to_apk
adb uninstall packageName
```

### 三.文件复制到设备
```
adb pull remote local
adb push local remote

```
在上述命令中，local 和 remote 指的是开发计算机（本地）和模拟器/设备实例（远程）上目标文件/目录的路径。例如：

```
adb push foo.txt /sdcard/foo.txt
```

### 四.Shell 在目标模拟器/设备实例中启动远程 shell

不进入模拟器/设备实例上的 adb 远程 shell 发出设备命令:

```
adb [-d|-e|-s serial_number] shell shell_command
```
进入模拟器/设备实例上的远程 shell：

```
adb [-d|-e|-s serial_number] shell
```
当您准备退出远程 shell 时，按 Control + D 或输入 exit。

shell 命令二进制文件存储在模拟器或设备的文件系统中，其路径为 /system/bin/

#### 4.1 获取设备信息

```
adb shell getprop
```

#### 4.2 Window manager

```
adb shell wm
```
帮助

```

Window manager (window) commands:
  help
      Print this help text.
  size [reset|WxH|WdpxHdp]
    Return or override display size.
    width and height in pixels unless suffixed with 'dp'.
  density [reset|DENSITY]
    Return or override display density.
  overscan [reset|LEFT,TOP,RIGHT,BOTTOM]
    Set overscan area for display.
  scaling [off|auto]
    Set display scaling mode.
  dismiss-keyguard


```
例

```
获取屏幕尺寸命令:
adb shell wm size

返回结果:
Physical size: 1080x2160

```

#### 4.3 am Activity Manager

在 adb shell 中，您可以使用 Activity Manager (am) 工具发出命令以执行各种系统操作，如启动 Activity、强行停止进程、广播 intent、修改设备屏幕属性及其他操作

##### 4.3.1 常用命令:

```

启动 
adb shell am start -n 包名/activity名(全路径)
关闭
adb shell am force-stop 包名
例:打开浏览器
先 adb shell 再:
打开浏览器
am start -a android.intent.action.VIEW -d  http://www.google.cn/

```

##### 4.3.2 start 启动 intent 指定的 Activity

```
start [options] intent	启动 intent 指定的 Activity。
请参阅 intent 参数的规范。

选项包括：

-D：启用调试。
-W：等待启动完成。
--start-profiler file：启动分析器并将结果发送到 file。
-P file：类似于 --start-profiler，但当应用进入空闲状态时分析停止。
-R count：重复 Activity 启动 count 次数。在每次重复前，将完成顶部 Activity。
-S：启动 Activity 前强行停止目标应用。
--opengl-trace：启用 OpenGL 函数的跟踪。
--user user_id | current：指定要作为哪个用户运行；如果未指定，则作为当前用户运行。

```
##### 4.3.3 startService 启动 intent 指定的 Service

```
startservice [options] intent
```
##### 4.3.4 other

```
broadcast [options] intent	发出广播 intent
force-stop package	强行停止与 package（应用的包名称）关联的所有应用
kill [options] package	终止与 package（应用的包名称）关联的所有进程。此命令仅终止可安全终止且不会影响用户体验的进程
kill-all	终止所有后台进程
clear-debug-app	使用 set-debug-app 清除以前针对调试用途设置的软件包

...
...
```

##### 4.3.5 intent 参数的规范
```

对于采用 intent 参数的 Activity Manager 命令，您可以使用以下选项指定 intent：

-a action
指定 intent 操作，如“android.intent.action.VIEW”。此指定只能声明一次。
-d data_uri
指定 intent 数据 URI，如“content://contacts/people/1”。此指定只能声明一次。
-t mime_type
指定 intent MIME 类型，如“image/png”。此指定只能声明一次。
-c category
指定 intent 类别，如“android.intent.category.APP_CONTACTS”。
-n component
指定带有软件包名称前缀的组件名称以创建显式 intent，如“com.example.app/.ExampleActivity”。
-f flags
将标志添加到 setFlags() 支持的 intent。


```

#### 4.4 pm 调用软件包管理器

查看帮助

```
shell@JDtab:/ $ pm
usage: pm list packages [-f] [-d] [-e] [-s] [-3] [-i] [-u] [--user USER_ID] [FILTER]
       pm list permission-groups
       pm list permissions [-g] [-f] [-d] [-u] [GROUP]
       pm list instrumentation [-f] [TARGET-PACKAGE]
       pm list features
       pm list libraries
       pm list users
       pm path PACKAGE
       pm dump PACKAGE
       pm install [-lrtsfd] [-i PACKAGE] [--user USER_ID] [PATH]
       pm install-create [-lrtsfdp] [-i PACKAGE] [-S BYTES]
               [--install-location 0/1/2]
               [--force-uuid internal|UUID]
       pm install-write [-S BYTES] SESSION_ID SPLIT_NAME [PATH]
       pm install-commit SESSION_ID
       pm install-abandon SESSION_ID
       pm uninstall [-k] [--user USER_ID] PACKAGE
       pm set-installer PACKAGE INSTALLER
       pm move-package PACKAGE [internal|UUID]
       pm move-primary-storage [internal|UUID]
       pm clear [--user USER_ID] PACKAGE
       pm enable [--user USER_ID] PACKAGE_OR_COMPONENT
       pm disable [--user USER_ID] PACKAGE_OR_COMPONENT
       pm disable-user [--user USER_ID] PACKAGE_OR_COMPONENT
       pm disable-until-used [--user USER_ID] PACKAGE_OR_COMPONENT
       pm hide [--user USER_ID] PACKAGE_OR_COMPONENT
       pm unhide [--user USER_ID] PACKAGE_OR_COMPONENT
       pm grant [--user USER_ID] PACKAGE PERMISSION
       pm revoke [--user USER_ID] PACKAGE PERMISSION
       pm reset-permissions
       pm set-app-link [--user USER_ID] PACKAGE {always|ask|never|undefined}
       pm get-app-link [--user USER_ID] PACKAGE
       pm set-install-location [0/auto] [1/internal] [2/external]
       pm get-install-location
       pm set-permission-enforced PERMISSION [true|false]
       pm trim-caches DESIRED_FREE_SPACE [internal|UUID]
       pm create-user [--profileOf USER_ID] [--managed] USER_NAME
       pm remove-user USER_ID
       pm get-max-users

pm list packages: prints all packages, optionally only
  those whose package name contains the text in FILTER.  Options:
    -f: see their associated file.
    -d: filter to only show disbled packages.
    -e: filter to only show enabled packages.
    -s: filter to only show system packages.
    -3: filter to only show third party packages.
    -i: see the installer for the packages.
    -u: also include uninstalled packages.

pm list permission-groups: prints all known permission groups.

pm list permissions: prints all known permissions, optionally only
  those in GROUP.  Options:
    -g: organize by group.
    -f: print all information.
    -s: short summary.
    -d: only list dangerous permissions.
    -u: list only the permissions users will see.

pm list instrumentation: use to list all test packages; optionally
  supply <TARGET-PACKAGE> to list the test packages for a particular
  application.  Options:
    -f: list the .apk file for the test package.

pm list features: prints all features of the system.

pm list users: prints all users on the system.

pm path: print the path to the .apk of the given PACKAGE.

pm dump: print system state associated with the given PACKAGE.

pm install: install a single legacy package
pm install-create: create an install session
    -l: forward lock application
    -r: replace existing application
    -t: allow test packages
    -i: specify the installer package name
    -s: install application on sdcard
    -f: install application on internal flash
    -d: allow version code downgrade
    -p: partial application install
    -g: grant all runtime permissions
    -S: size in bytes of entire session

pm install-write: write a package into existing session; path may
  be '-' to read from stdin
    -S: size in bytes of package, required for stdin

pm install-commit: perform install of fully staged session
pm install-abandon: abandon session

pm set-installer: set installer package name

pm uninstall: removes a package from the system. Options:
    -k: keep the data and cache directories around after package removal.

pm clear: deletes all data associated with a package.

pm enable, disable, disable-user, disable-until-used: these commands
  change the enabled state of a given package or component (written
  as "package/class").

pm grant, revoke: these commands either grant or revoke permissions
    to apps. The permissions must be declared as used in the app's
    manifest, be runtime permissions (protection level dangerous),
    and the app targeting SDK greater than Lollipop MR1.

pm reset-permissions: revert all runtime permissions to their default state.

pm get-install-location: returns the current install location.
    0 [auto]: Let system decide the best location
    1 [internal]: Install on internal device storage
    2 [external]: Install on external media

pm set-install-location: changes the default install location.
  NOTE: this is only intended for debugging; using this can cause
  applications to break and other undersireable behavior.
    0 [auto]: Let system decide the best location
    1 [internal]: Install on internal device storage
    2 [external]: Install on external media

pm trim-caches: trim cache files to reach the given free space.

pm create-user: create a new user with the given USER_NAME,
  printing the new user identifier of the user.

pm remove-user: remove the user with the given USER_IDENTIFIER,
  deleting all data associated with that user

1|shell@JDtab:/ $ 
```

##### 4.4.1 list

```
list packages [options] filter	输出所有软件包，或者，仅输出包名称包含 filter 中的文本的软件包。
 
选项：

-f：查看它们的关联文件。
-d：进行过滤以仅显示已停用的软件包。
-e：进行过滤以仅显示已启用的软件包。
-s：进行过滤以仅显示系统软件包。
-3：进行过滤以仅显示第三方软件包。
-i：查看软件包的安装程序。
-u：也包括卸载的软件包。
--user user_id：要查询的用户空间。
例:
adb shell pm list packages
adb shell pm list packages -s 系统app
adb shell pm list packages -3 第三方app
```
##### 4.4.2 install and uninstall

```
install [options] path	将软件包（通过 path 指定）安装到系统。
选项：

-l：安装具有转发锁定功能的软件包。
-r：重新安装现有应用，保留其数据。
-t：允许安装测试 APK。
-i installer_package_name：指定安装程序软件包名称。
-s：在共享的大容量存储（如 sdcard）上安装软件包。
-f：在内部系统内存上安装软件包。
-d：允许版本代码降级。
-g：授予应用清单中列出的所有权限。


uninstall [options] package	从系统中移除软件包。
选项：

-k：移除软件包后保留数据和缓存目录。
```
##### 4.4.3 清除数据和缓存

```
adb shell pm clear 包名

例 清除qq数据
1|shell@JDtab:/ $ pm clear com.tencent.mobileqq
```



#### 4.5 ps 进程查看和操作

帮助

```
adb shell ps --help

usage: ps [-AadefLlnwZ] [-gG GROUP,] [-k FIELD,] [-o FIELD,] [-p PID,] [-t TTY,] [-uU USER,]

List processes.

Which processes to show (selections may be comma separated lists):

-A	All processes
-a	Processes with terminals that aren't session leaders
-d	All processes that aren't session leaders
-e	Same as -A
-g	Belonging to GROUPs
-G	Belonging to real GROUPs (before sgid)
-p	PIDs (--pid)
-P	Parent PIDs (--ppid)
-s	In session IDs
-t	Attached to selected TTYs
-T	Show threads
-u	Owned by USERs
-U	Owned by real USERs (before suid)

.....

```
常用进程查看操作

```
adb shell ps | grep 关键字  :显示进程
adb shell cat/proc/进程id/oom_adj  显示进程优先级
adb shell kill [pid]

```
例:显示所有腾讯app进程

```
adb shell ps | grep tencent

u0_a156        544   638 1313424  60448 0                   0 S com.tencent.tim:Daemon
u0_a156        623   638 1314488  60152 0                   0 S com.tencent.tim:assist
u0_a182        857   638 1385176  78028 0                   0 S com.tencent.mobileqq:MSF
u0_a165       1636   638 2473248 323928 0                   0 S com.tencent.mm
u0_a165       2204   638 2090388 117860 0                   0 S com.tencent.mm:push
system       10200   637 3924720  40540 0                   0 S com.tencent.soter.soterserver
u0_a156      10335   638 1334496  68572 0                   0 S com.tencent.tim:MSF
u0_a156      23472   638 1396960 107396 0                   0 S com.tencent.tim:mail


```


#### 4.6 输入 input 

```
input  <command> [<arg>...]

查看帮助
shell@JDtab:/ $ input help
Error: Unknown command: help
Usage: input [<source>] <command> [<arg>...]

The sources are: 
      mouse
      keyboard
      joystick
      touchnavigation
      touchpad
      trackball
      stylus
      dpad
      touchscreen
      gamepad

The commands and default sources are:
      text <string> (Default: touchscreen)
      keyevent [--longpress] <key code number or name> ... (Default: keyboard)
      tap <x> <y> (Default: touchscreen)
      swipe <x1> <y1> <x2> <y2> [duration(ms)] (Default: touchscreen)
      press (Default: trackball)
      roll <dx> <dy> (Default: trackball)
      3dtouch <x> <y> <z>(Default: touchscreen)
shell@JDtab:/ $ 

sources? 好像用不上 主要是commands的使用

模拟输入文字
adb shell input text "hahah" 注意输入中文时键盘模式需要是中文 同理英文

模拟点击屏幕
input tap 123 312     

模拟模拟按键
input keyevent 3 返回桌面
常用按键 event
1	menu	KEYCODE_MENU
3	home	KEYCODE_HOME
4	back	KEYCODE_BACK
21	光标左移	KEYCODE_DPAD_LEFT
22	光标右移	KEYCODE_DPAD_RIGHT
67	删除	KEYCODE_DEL


```

#### 4.7 截图和录屏

```
截图
adb shell screencap  /sdcard/我是截图.png  

录屏
如果不设置时间默认3分钟, 使用 --time-limit 设置截屏时间
adb shell screenrecord /sdcard/我是视频.mp4 --time-limit 12
录屏帮助
screedrecord --help

截图和录屏完毕后可退出 shell,使用  `adb pull /sdcard/我是视频.mp4` 把文件从手机复制到电脑


```

#### 4.8 monkey

```
查看帮助
shell@JDtab:/ $ monkey help                                                    
** Error: Count is not a number
usage: monkey [-p ALLOWED_PACKAGE [-p ALLOWED_PACKAGE] ...]
              [-c MAIN_CATEGORY [-c MAIN_CATEGORY] ...]
              [--ignore-crashes] [--ignore-timeouts]
              [--ignore-security-exceptions]
              [--monitor-native-crashes] [--ignore-native-crashes]
              [--kill-process-after-error] [--hprof]
              [--pct-touch PERCENT] [--pct-motion PERCENT]
              [--pct-trackball PERCENT] [--pct-syskeys PERCENT]
              [--pct-nav PERCENT] [--pct-majornav PERCENT]
              [--pct-appswitch PERCENT] [--pct-flip PERCENT]
              [--pct-anyevent PERCENT] [--pct-pinchzoom PERCENT]
              [--pct-permission PERCENT]
              [--pkg-blacklist-file PACKAGE_BLACKLIST_FILE]
              [--pkg-whitelist-file PACKAGE_WHITELIST_FILE]
              [--wait-dbg] [--dbg-no-events]
              [--setup scriptfile] [-f scriptfile [-f scriptfile] ...]
              [--port port]
              [-s SEED] [-v [-v] ...]
              [--throttle MILLISEC] [--randomize-throttle]
              [--profile-wait MILLISEC]
              [--device-sleep-time MILLISEC]
              [--randomize-script]
              [--script-log]
              [--bugreport]
              [--periodic-bugreport]
              [--permission-target-system]
              COUNT

255|shell@JDtab:/ $ 
例:
adb shell monkey -v -p your.package.name 500
```


### 五.dumpsys 将系统数据转储到屏幕
 
[developer.android.com](https://developer.android.com/studio/command-line/dumpsys)


```
命令格式
adb shell dumpsys [-t timeout] [--help | -l | --skip services | service [arguments] | -c | -h]

选项说明
-t timeout	指定时间 默认10秒
--help	 查看帮助
-l	列出可用参数列表
--skip services	Specifies the services that you do not want to include in the output.
service [arguments]	Specifies the service that you want to output. Some services may allow you to pass optional arguments. You can learn about these optional arguments by passing the -h option with the service, as shown below:
adb shell dumpsys procstats -h
    

-c	When specifying certain services, append this option to output data in a machine-friendly format.
-h	For certain services, append this option to see help text and additional options for that service.


常用
dumpsys activity AMS服务相关信息
dumpsys window  WMS服务相关信息
dumpsys cpuinfo CPU信息
dumpsys meminfo 内存信息

```

* dumpsys activity



```
查看帮助
adb shell dumpsys activity -h
 
Activity manager dump options:
  [-a] [-c] [-p PACKAGE] [-h] [WHAT] ...
  WHAT may be one of:
    a[ctivities]: activity stack state //查看activity
    r[recents]: recent activities state 
    b[roadcasts] [PACKAGE_NAME] [history [-s]]: broadcast state
    broadcast-stats [PACKAGE_NAME]: aggregated broadcast statistics
    i[ntents] [PACKAGE_NAME]: pending intent state
    p[rocesses] [PACKAGE_NAME]: process state
    o[om]: out of memory management
    perm[issions]: URI permission grant state
    prov[iders] [COMP_SPEC ...]: content provider state
    provider [COMP_SPEC]: provider client-side state
    s[ervices] [COMP_SPEC ...]: service state //服务
    as[sociations]: tracked app associations
    settings: currently applied config settings
    service [COMP_SPEC]: service client-side state
    package [PACKAGE_NAME]: all state related to given package
    all: dump all activities
    top: dump the top activity
  WHAT may also be a COMP_SPEC to dump activities.
  COMP_SPEC may be a component name (com.foo/.myApp),
    a partial substring in a component name, a
    hex object identifier.
  -a: include all available server state.
  -c: include client state.
  -p: limit output to given package.
  --checkin: output checkin format, resetting data.
  --C: output checkin format, not resetting data.
  --proto: output dump in protocol buffer format.
  --autofill: dump just the autofill-related state of an activity

```


```
例:
   	查看acitivty信息
   	adb shell dumpsys activity activities 或者 adb shell dumpsys activity a   
	查看微信所有服务
 	adb shell dumpsys activity s com.tencent.mm

```



[详细Adb常用命令](https://developer.android.com/studio/command-line/adb?hl=zh-cn)   