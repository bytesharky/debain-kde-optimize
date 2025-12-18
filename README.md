# 这是一个我自己在的KDE优化方案

## 主题优化

#### 全局主题

全局主题可以直接在系统设置里下载主题，也可以下载其他主题，下面我在使用的几个主题

MacTahoe: [https://github.com/vinceliuice/MacTahoe-kde](https://github.com/vinceliuice/MacTahoe-kde)

MacVentura: [https://github.com/vinceliuice/MacVentura-kde](https://github.com/vinceliuice/MacVentura-kde)

Layan-kde: [https://github.com/vinceliuice/Layan-kde](https://github.com/vinceliuice/Layan-kde)

💡这里说一下手动修改主题文件，找到全局主题目录：

> 系统级在 /usr/share/themes/[你的主题]
>
> 用户级在  ~/.local/share/themes/[你的主题]

### ICON主题

图标主题也可以直接在系统设置里下载主题，我这里使用就是系统里下载的 `Papirus` ，个人不喜欢 MAC 那种圆角的方形图标。

💡这里说一下手动修改主题文件，找到ICON主题目录：

> 系统级在 /usr/share/icons/[你的主题]
> 
> 用户级在  ~/.local/share/icons/[你的主题]


### 窗口装饰元素

这里我使用的是 MacVentura-Dark，安装全局主题可以选这个，由于长时间使用电脑，个人比较喜欢暗色主题，其次这个窗口圆角比较小，个人感觉MacTahoe主题的窗口圆角比较大

💡这里说一下手动修改主题文件，找到窗口装饰元素目录：

> 系统级在 /usr/share/aurorae/themes/[你的主题]
> 
> 用户级在  ~/.local/share/aurorae/themes/[你的主题]

### 桌面壁纸

桌面壁纸可以直接在系统设置里选择自己喜欢的图片

💡这里说一下手动修改主题文件，找到桌面壁纸目录：

> 系统级在 /usr/share/wallpapers/[你的主题]
> 
> 用户级在 ~/.local/share/wallpapers/[你的主题]

### 欢迎屏幕

欢迎屏是开机进入桌面时的过渡画面

💡这里说一下手动修改主题文件,找到欢迎屏幕主题目录：

> 系统级在 /usr/share/plasma/look-and-feel/[你的主题]
> 
> 用户级在 ~/.local/share/plasma/look-and-feel/[你的主题]

替换主题目录下的背景图，或者编辑 `contents/splash/Splash.qml` 调整图片路径。

### 登录屏幕（SDDM）

SDDM 是 KDE 现代版默认登录管理器

💡这里说一下手动修改主题文件，找到登录屏幕主题目录：

> 系统级在 /usr/share/sddm/themes/[主题名]/
> 
> 没有用户级，此时用户还没有登录
> 
> 替换主题目录下的背景图，或者编辑 `theme.conf`，修改 `background=` 路径指向新图片。


## 应用程序图标

💡这里说一下配置开始菜单或桌面的应用程序图标

> 系统级在：/usr/share/applications/
> 
> 用户级在：~/.local/share/applications/

为应用程序创建一个`*.desktop文件`，示例如下

```ini
# Desktop Entry 分是必须的内容Exec是要执行命令
[Desktop Entry]
Name=Visual Studio Code
Comment=Code Editing. Redefined.
GenericName=Text Editor
Exec=/usr/share/code/code --ozone-platform=x11 %F
Icon=vscode
Type=Application
StartupNotify=false
StartupWMClass=Code
Categories=TextEditor;Development;IDE;
MimeType=application/x-code-workspace;
Actions=new-empty-window;
Keywords=vscode;

# Desktop Action 这是在图标上点击右键的菜单选项，这是可选的
# new-empty-window 对应上方的Actions，Actions可以多个用分号分隔

[Desktop Action new-empty-window]
Name=New Empty Window
Name[cs]=Nové prázdné okno
Name[de]=Neues leeres Fenster
Name[es]=Nueva ventana vacía
Name[fr]=Nouvelle fenêtre vide
Name[it]=Nuova finestra vuota
Name[ja]=新しい空のウィンドウ
Name[ko]=새 빈 창
Name[ru]=Новое пустое окно
Name[zh_CN]=新建空窗口
Name[zh_TW]=開新空視窗
Exec=/usr/share/code/code --ozone-platform=x11 --new-window %F
Icon=vscode

```

## 文件管理器

### 在右键菜单增加“以管理员身份打开”

在终端中运行`dolphin --sudo`，安装`dolphin admin`插件。

### 在右键菜单增加自定义选项

> 系统级在：/usr/share/kio/servicemenus/
> 
> 用户级在：~/.local/share/kio/servicemenus/

为应用程序创建一个`*.desktop文件`，示例如下

```ini
[Desktop Entry]
Name = Open in VS Code
Type=Service
ServiceTypes=KonqPopupMenu/Plugin
MimeType=all/all;all/allfiles;inode/directory;
X-KDE-Priority=TopLevel
Actions=openInVSCODE;
Icon=vscode

[Desktop Action openInVSCODE]
Name=Open in VS Code
Name[nl]=Openen in VS Code
Name[cs]=Otevřít ve VS Code
Name[es]=Abrir en VS Code
Name[pl]=Otwórz za pomocą VS Code
Name[zh]=在 VS Code 中打开
Name[zh_CN]=在 VS Code 中打开
Name[de]=In VS Code öffnen
Name[hi]=VS Code में खोले
Name[ml]=VS Code ൽ തുറക്കുക
Name[mr]=VS Code मध्ये उघडा
Name[kn]=VS Code ನಲ್ಲಿ ತೆರೆಯಿರಿ
Icon=vscode
Exec=code --ozone-platform=x11 %f
```

## 解决 VSCode 无法输入中文的问题

关于这个问题我查阅了很多资料，基本确定导致这个问题的原因是 `Fcitx5`、`Wayland`、`Electron`兼容问题导致，

而`VSCode` 就是基于 `Electron`开发的，有下面几种方案

### 方案一

这个方案是我验证成功的，我的环境是`Debain 13`系统，`KDE`桌面，`Fcitx5`输入法

这里建议使用官方的deb包安装，下载地址：https://code.visualstudio.com/

先关闭所有的`VSCode`窗口，然后在终端输入 `/usr/share/code/code --ozone-platform=x11`，强制`VSCode`运行在`x11`模式

执行这个会打开`VSCode`窗口，测试是否可以输入中文，如果可以则说明该方案有效，接下来修改`.desktop`文件，启动命令加上`--ozone-platform=x11`参数即可

> 系统级在：/usr/share/applications/
>
> 用户级在：~/.local/share/applications/

### 方案二

这个方案我尝试了并没有解决问题，如果之前的方法无效可以尝试这个

先关闭所有的`VSCode`窗口，然后在终端输入下面的命令

```bash

# fcitx5 输入法
export GTK_IM_MODULE=fcitx5
export QT_IM_MODULE=fcitx5
export XMODIFIERS=@im=fcitx5

# ibus 输入法
export GTK_IM_MODULE=ibus
export QT_IM_MODULE=ibus
export XMODIFIERS=@im=ibus

# 启动VSCode
/usr/share/code/code

```

执行这个会打开`VSCode`窗口，测试是否可以输入中文，如果可以则说明该方案有效。

接下来修改`~/.profile`或者`~/.bashrc`，在文件末尾添加上面的环境变量，使用`source`重新加载即可。

## 常用软件

**VSCode** ： https://code.visualstudio.com/ (VSCode官方版)

**IDEA**: https://www.jetbrains.com/idea/download/?section=linux (新版是订阅制，不部分社区版，可以免费使用基础功能)

**WeChat Dev Tools**: https://github.com/msojocs/wechat-web-devtools-linux (这是一个第三方维护的Linux移植版，暂无官方版本)

**HBuilderX**:  https://www.dcloud.io/hbuilderx.html (严重阉割的官方版，没有图形化，非IDE，仅是CLI)

**ApiPost**:  https://www.apipost.cn/ (ApiPost官方版)


## 我的配置备份

**local.gz.tar** 这是用户级配置，解压到 `~/.local`

**usr.gz.tar** 这是系统级配置，解压到 `/usr`，仅包含 `SDDM`
