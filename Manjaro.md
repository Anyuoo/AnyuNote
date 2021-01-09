# Manjaro

####  初始设置

```bash
#将系统时间设置为硬件时间
sudo timedatectl set-local-rtc 1 #配置镜像源
sudo pacman-mirrors -i -c China -m rank 
#升级系统
sudo pacman -Syyu
```

```bash
##打开配置文件
sudo nano /etc/pacman.conf
[archlinuxcn]
SigLevel = Optional TrustedOnly
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```

```bash
#导入GPG Key
sudo pacman -Syy && sudo pacman -S archlinuxcn-keyring
现在可以安装软件了，比如 chrome 和搜狗拼音输入法/googlepinyin
#安装chrome
sudo pacman -S google-chrome
添加了中科大源后，也可以直接在添加/删除软件里搜索直接安装
#配置AUR源 使用yaourt
sudo pacman -S yaourt
sudo gedit /etc/yaourtrc
AURURL=“https://aur.tuna.tsinghua.edu.cn”
pacman -S screenfetch  ##查看系统信息
```

```bash
#查看显卡状态
lsmod | grep usb 查看usb工作状态
usb-storange u盘无法识别
sudo mhwd   - -usb  #u盘无法识别 可能是usb驱动未成功
mhwd-tui #管理工具
```

### 软件安装

#### 基础软件

```bash
sudo pacman -S vim git rpm yay unzip snapd you-get annie
sudo systemctl enable --now snapd.socket
sudo pacman -S zsh
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
# 切换到zsh
chsh -s /bin/zsh
# 安装3den主题
sudo vim ~/.zshrc
ZSH_THEME="3den"
```



### JDK 安装

```bash
sudo cp -pr jdk-11.0.7 /opt
sudo ln -s /opt/jdk-11.0.7/bin/java /usr/bin/java
sudo ln -s /opt/jdk-11.0.7/bin/javac /usr/bin/javac
```

**错误：** **Cannot find the strip binary required for object file stripping.**

```bash
sudo pacman -S base-devel 
```

### Pacman

#### 更新

```bash
#全面更新
pacman -Syyu 
#更新所有包
pacman -Syu
#更新报数据源
pacman -Sy
#更新已安装的包
pacman -Su
```

#### 搜索安装

```bash
# 安装
pacman -S
# 搜索含关键字的包
pacman -Ss
# 同步包后再执行安装
pacman -Sy
# 安装本地包 (扩展名：pkg.tar.gz)
pacman -U
# 搜索已安装的包
pacman -Qs
# 升级全部包
pacman -Syu
# 只下载，不安装
pacman -Sw
```

#### 删除

```bash
# 删除包，不会删除其依赖
pacman -R
# 删除包，及其所有没有被其它包使用的依赖
pacman -Rs
# 删除一个包，包括所有依赖
pacman -Rsc
# 清理未安装的包 (包文件目录：/var/cache/pacman/pkg/)
pacman -Sc
# 清理所有缓存文件
pacman -Sccpa
# 显示包信息
pacman -Si
# 查询本地包的详情信息
pacman -Qi
# 列出所有不再作为依赖的包
pacman -Qdt
# 列出所有明确安装而且不被其他包依赖的包
pacman -Qet
```

