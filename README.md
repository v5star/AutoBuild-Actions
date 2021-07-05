# 使用Actions编译OpenWrt固件或更新

![GitHub Stars](https://img.shields.io/github/stars/Hyy2001X/AutoBuild-Actions.svg?style=flat-square&label=Stars&logo=github)
![GitHub Forks](https://img.shields.io/github/forks/Hyy2001X/AutoBuild-Actions.svg?style=flat-square&label=Forks&logo=github)

测试通过的设备: `lenovo_newifi-y1` ，能使用，但是有些问题不会自动挂载U盘，还在研究中。

支持的源码: `coolsnowwolf/lede`、`immortalwrt/immortalwrt`、`openwrt/openwrt`

## 部署环境(STEP 1):

1.  **Fork** 我的`AutoBuild-Actions`仓库,然后进入你的`AutoBuild-Actions`仓库进行之后修改和定制，看注释。


## 定制固件(STEP 2):

1. 进入你的`AutoBuild-Actions`仓库,**下方所有操作都将在你的`AutoBuild-Actions`仓库下进行**

   **提示: 下文中所有的 TARGET_PROFILE 均为你的设备名称,可以在 .config 中找到没有自己建一个,例如： lenovo_newifi-y1**

   **在本地的 .config 文件中获取设备名称:** `egrep -o "CONFIG_TARGET.*DEVICE.*=y" .config | sed -r 's/.*DEVICE_(.*)=y/\1/'`
   
   **或者:** `grep 'TARGET_PROFILE' .config`,名称中不应含有 `DEVICE_`

2. 把本地的 `.config` 文件重命名为你设备的 **TARGET_PROFILE** 并上传到`/Configs`目录

3. 编辑`.github/workflows/*.yml`文件,修改`第35 行`为你设备的 **TARGET_PROFILE**  后面的值 

4. 按照需求编辑并定制`Scripts/AutoBuild_DiyScript.sh`文件（还在研究中）

   **注意: 为了更方便地同步最新改动,不建议修改 Scripts/AutoBuild_Function.sh 文件**

   **第三方软件包列表** 编辑`CustomPackages`目录下对应的 **TARGET_PROFILE** 的文件,按照现有语法为该设备添加第三方软件包 (可选，自行研究)

**AutoBuild_DiyScript.sh: Diy_Core() 函数中的变量解释:**
```
   Author 作者名称,若该项留空将自动获取

   Default_TARGET_PROFILE 设备名称,获取方法见上方
   
   Short_Firmware_Date 固件日期样式,当设置为 true: [20210601] false: [202106012359]
   
   Default_LAN_IP 固件 LAN IP 地址,默认为 192.168.1.1

   INCLUDE_AutoBuild_Features 自动添加 AutoBuild 特性到固件

   INCLUDE_DRM_I915 自动勾选 Intel Graphics 驱动

   INCLUDE_Argon 自动添加 luci-theme-argon 主题和控制器

   INCLUDE_Obsolete_PKG_Compatible 优化原生 OpenWrt-19.07、21.02 支持 (测试特性)
   
   注: 若要启用某项功能,请将该值修改为 true,禁用某项功能则修改为 false 或留空
```
**其他指令:** 编辑`Scripts/AutoBuild_DiyScript.sh`,参照下方语法:
```
   [使用 git clone 拉取文件]  AddPackage git 存放位置 软件包名 仓库地址 分支

   [使用 svn co 拉取文件]  AddPackage svn 存放位置 软件包名 仓库地址/branches/分支/路径

   [复制 /CustomFiles 文件到源码] Copy 文件(夹)名称 目标路径 新名称[可选]
```
## 编译固件(STEP 3):

   **一键编译** 先删除`第 26-27 行`的注释并保存,单(双)击重新点亮右上角的 **Star** 即可一键编译

   **定时编译** 先删除`第 23-24 行`的注释,然后按需修改相关参数并保存,[使用方法](https://www.runoob.com/w3cnote/linux-crontab-tasks.html)

   **手动编译** 点击上方`Actions`,选择你要编译的设备名称,点击右方`Run workflow`,点击绿色按钮即可开始编译
   
   **临时修改 IP 地址** 该功能仅在**手动编译**时生效,点击`Run workflow`后即可输入 IP 地址(优先级**高于** `Default_LAN_IP`)

## 使用 AutoUpdate 一键更新脚本:

   首先需要打开`TTYD 终端`或者在浏览器输入`IP 地址:7681`,按需输入下方指令:

   检查并更新固件(保留配置),输入: `autoupdate`或`bash /bin/AutoUpdate.sh`

   更新固件(FastGit 镜像加速): `autoupdate -P`

   更新固件(不保留配置): `autoupdate -n`或`autoupdate -n -P`

   查看更多使用方法: `autoUpdate --help`

   **注意: 该功能需要在 Diy-Core() 函数中设置`INCLUDE_AutoBuild_Features`为`true`**

## 使用 AutoBuild 固件工具箱:

   打开`TTYD 终端`,输入`tools`或`bash /bin/AutoBuild_Tools.sh`

   **注意: 该功能需要在 Diy-Core() 函数中设置`INCLUDE_AutoBuild_Features`为`true`**

## 鸣谢

   - [Lean's Openwrt Source code](https://github.com/coolsnowwolf/lede)

   - [P3TERX's Actions-OpenWrt Project](https://github.com/P3TERX/Actions-OpenWrt)

   - [P3TERX's Blog](https://p3terx.com/archives/build-openwrt-with-github-actions.html)

   - [ImmortalWrt's Source code](https://github.com/immortalwrt)

   - [eSir 's workflow template](https://github.com/esirplayground/AutoBuild-OpenWrt/blob/master/.github/workflows/Build_OP_x86_64.yml)

   - 测试与建议: [CurssedCoffin](https://github.com/CurssedCoffin) [Licsber](https://github.com/Licsber) [sirliu](https://github.com/sirliu) [teasiu](https://github.com/teasiu)
