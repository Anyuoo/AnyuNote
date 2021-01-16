# Manjaro

###  初始设置

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
```

```bash
sudo pacman -S postman-bin

# 办公软件
sudo pacman -S google-chrome
sudo pacman -S foxitreader # pdf 阅读
sudo pacman -S bookworm # 电子书阅读
sudo pacman -S unrar unzip p7zip
sudo pacman -S goldendict # 翻译、取词
sudo pacman -S wps-office
yay -S typora # markdown 编辑
yay -S electron-ssr # 缺少我需要的加密算法
yay -S xmind

# 设计
sudo pacman -S pencil # 免费开源界面原型图绘制工具

# 娱乐软件
sudo pacman -S netease-cloud-music

# 下载软件
sudo pacman -S aria2
sudo pacman -S filezilla  # FTP/SFTP

# 图形
sudo pacman -S gimp # 修图

# 系统工具
sudo pacman -S albert #类似Mac Spotlight，另外一款https://cerebroapp.com/
yay -S copyq #  剪贴板工具，类似 Windows 上的 Ditto

# 终端
sudo pacman -S screenfetch # 终端打印出你的系统信息，screenfetch -A 'Arch Linux'
sudo pacman -S htop
sudo pacman -S bat
sudo pacman -S yakuake # 堪称 KDE 下的终端神器，KDE 已经自带，F12 可以唤醒
sudo pacman -S net-tools # 这样可以使用 ifconfig 和 netstat
yay -S tldr
yay -S tig # 命令行下的 git 历史查看工具
yay -S tree
yay -S ncdu # 命令行下的磁盘分析器，支持Vim操作
yay -S mosh # 一款速度更快的 ssh 工具，网络不稳定时使用有奇效
```



### JDK 安装

```bash
sudo cp -pr jdk-11.0.7 /opt
#
sudo ln -s /opt/jdk-11.0.7/bin/java /usr/bin/java
sudo ln -s /opt/jdk-11.0.7/bin/javac /usr/bin/javac
#环境变量设置
export JAVA_HOME=/opt/jdk-11.0.7
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=$JAVA_HOME/lib
#设置为默认
archlinux-java status
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

### 终端代理

> 使用时在需要代理的命令前加 proxychains4 就可以了，例如：proxychains4 ping www.google.com 

```bash
vim /etc/proxychains.conf # 添加如下内容
socks5 127.0.0.1 1080
```

### 微信\QQ

```bash
#kde先安装
sudo pacman -Sy xsettingsd pacaur  
#安装微信
pacaur -Sy com.qq.weixin.deepin   
#安装QQ
pacaur -Sy com.qq.im.deepin    
```

### vim 配置

```bash
"~/.vimrc
"vim config file
"""""""""""""""""""""""""""""""""""
"""=>全局配置<="""
"""""""""""""""""""""""""""""""""""
"关闭vi兼容模式"
set nocompatible

"设置历史记录步数"
set history=1000

"开启相关插件"
"侦测文件类型"
filetype on
"载入文件类型插件"
filetype plugin on
"为特定文件类型载入相关缩进文件"
filetype indent on

"当文件在外部被修改时，自动更新该文件"
set autoread

"激活鼠标的使用"
set mouse=a
set selection=exclusive
set selectmode=mouse,key

"保存全局变量"
set viminfo+=!

"带有如下符号的单词不要被换行分割"
set iskeyword+=_,$,@,%,#,-

"通过使用: commands命令，告诉我们文件的哪一行被改变过"
set report=0

"被分割的窗口间显示空白，便于阅读"
set fillchars=vert:\ ,stl:\ ,stlnc:\

"""""""""""""""""""""""""""""""""
"""=>字体和颜色<="""
"""""""""""""""""""""""""""""""""
"自动开启语法高亮"
syntax enable

"设置字体"
"set guifont=dejaVu\ Sans\ MONO\ 10
set guifont=Courier_New:h10:cANSI

"设置颜色"
"colorscheme desert

"高亮显示当前行"
set cursorline
hi cursorline guibg=#00ff00
hi CursorColumn guibg=#00ff00

"""""""""""""""""""""""""""""""
"""=>代码折叠功能<="""
"""""""""""""""""""""""""""""""
"激活折叠功能"
set foldenable
"set nofen（这个是关闭折叠功能）"

"设置按照语法方式折叠（可简写set fdm=XX）"
"有6种折叠方法：
"manual   手工定义折叠"
"indent   更多的缩进表示更高级别的折叠"
"expr     用表达式来定义折叠"
"syntax   用语法高亮来定义折叠"
"diff     对没有更改的文本进行折叠"
"marker   对文中的标志进行折叠"
set foldmethod=manual
"set fdl=0（这个是不选用任何折叠方法）"

"设置折叠区域的宽度"
"如果不为0，则在屏幕左侧显示一个折叠标识列
"分别用“-”和“+”来表示打开和关闭的折叠
set foldcolumn=0

"设置折叠层数为3"
setlocal foldlevel=3

"设置为自动关闭折叠"
set foldclose=all

"用空格键来代替zo和zc快捷键实现开关折叠"
"zo O-pen a fold (打开折叠)
"zc C-lose a fold (关闭折叠)
"zf F-old creation (创建折叠)
"nnoremap <space> @=((foldclosed(line('.')) < 0) ? 'zc' : 'zo')<CR>

"""""""""""""""""""""""""""""""""""
"""=>文字处理<="""
"""""""""""""""""""""""""""""""""""
"使用空格来替换Tab"
set expandtab

"设置所有的Tab和缩进为4个空格"
set tabstop=4

"设定<<和>>命令移动时的宽度为4"
set shiftwidth=4

"使得按退格键时可以一次删除4个空格"
set softtabstop=4
set smarttab

"缩进，自动缩进（继承前一行的缩进）"
"set autoindent 命令打开自动缩进，是下面配置的缩写
"可使用autoindent命令的简写，即“:set ai”和“:set noai”
"还可以使用“:set ai sw=4”在一个命令中打开缩进并设置缩进级别
set ai
set cindent

"智能缩进"
set si

"自动换行”
set wrap

"设置软宽度"
set sw=4

"行内替换"
set gdefault

""""""""""""""""""""""""""""""""""
"""=>Vim 界面<="""
""""""""""""""""""""""""""""""""""
"增强模式中的命令行自动完成操作"
set wildmenu

"显示标尺"
set ruler

"设置命令行的高度"
set cmdheight=1

"显示行数"
set nu

"不要图形按钮"
set go=

"在执行宏命令时，不进行显示重绘；在宏命令执行完成后，一次性重绘，以便提高性能"
set lz

"使回格键（backspace）正常处理indent, eol, start等"
set backspace=eol,start,indent

"允许空格键和光标键跨越行边界"
set whichwrap+=<,>,h,l

"设置魔术"
set magic

"关闭遇到错误时的声音提示"
"关闭错误信息响铃"
set noerrorbells

"关闭使用可视响铃代替呼叫"
set novisualbell

"高亮显示匹配的括号([{和}])"
set showmatch

"匹配括号高亮的时间（单位是十分之一秒）"
set mat=2

"光标移动到buffer的顶部和底部时保持3行距离"
set scrolloff=3

"搜索逐字符高亮"
set hlsearch
set incsearch

"搜索时不区分大小写"
"还可以使用简写（“:set ic”和“:set noic”）"
set ignorecase

"用浅色高亮显示当前行"
autocmd InsertLeave * se nocul
autocmd InsertEnter * se cul

"输入的命令显示出来，看的清楚"
set showcmd

""""""""""""""""""""""""""""""""""""
"""=>编码设置<="""
""""""""""""""""""""""""""""""""""""
"设置编码"
set encoding=utf-8
set fencs=utf-8,ucs-bom,shift-jis,gb18030,gbk,gb2312,cp936

"设置文件编码"
set fileencodings=utf-8

"设置终端编码"
set termencoding=utf-8

"设置语言编码"
set langmenu=zh_CN.UTF-8
set helplang=cn

"""""""""""""""""""""""""""""
"""=>其他设置<="""
"""""""""""""""""""""""""""""
"开启新行时使用智能自动缩进"
set smartindent
set cin
set showmatch

"在处理未保存或只读文件的时候，弹出确认"
set confirm

"隐藏工具栏"
set guioptions-=T

"隐藏菜单栏"
set guioptions-=m

"置空错误铃声的终端代码"
set vb t_vb=

"显示状态栏（默认值为1，表示无法显示状态栏）"
set laststatus=2

"状态行显示的内容"
set statusline=%F%m%r%h%w\ [FORMAT=%{&ff}]\ [TYPE=%Y]\ [POS=%l,%v][%p%%]\ %{strftime(\"%d/%m/%y\ -\ %H:%M\")}

"粘贴不换行问题的解决方法"
set pastetoggle=<F9>

"设置背景颜色"
set background=dark

"文件类型自动检测，代码智能补全"
set completeopt=longest,preview,menu

"共享剪切板"
set clipboard+=unnamed

"从不备份"
set nobackup
set noswapfile

"自动保存"
set autowrite

"显示中文帮助"
if version >= 603
        set helplang=cn
            set encoding=utf-8
endif

"设置高亮相关项"
highlight Search ctermbg=black ctermfg=white guifg=white guibg=black

""""""""""""""""""""""""""""""""
"""=>在shell脚本开头自动增加解释器以及作者等版权信息<="""
""""""""""""""""""""""""""""""""
"新建.py,.cc,.sh,.java文件，自动插入文件头"
autocmd BufNewFile *.py,*.cc,*.sh,*.java exec ":call SetTitle()"
"定义函数SetTitle，自动插入文件头"
func SetTitle()
    if expand ("%:e") == 'sh'
        call setline(1, "#!/bin/bash")
        call setline(2, "#Author:Anyu")
        call setline(4, "#Time:".strftime("%F %T"))
        call setline(5, "#Name:".expand("%"))
        call setline(7, "#Description:This is a production script.")
    endif
endfunc
```

### 疑难杂症

#### 手动挂载(liveUSB)

```bash
#将除home目录意外的系统挂在到当前任意目录
#一般选择/mnt
sudo mkdir /mnt/manjaro
sudo mount disk/01  /mnt/amnjaro
#对于虚拟目录手动绑定,/dev /proc /sys
sudo mount --bind /dev /mnt/manjaro/dev
#系统文件准备完成，chroot
cd /mnt/manjaro
chroot .
pacman -Sy linux
#报错，检查是否挂在成功，正确挂载
pacman -Syy archlinux-keyring
```

#### 系统亮度调节

```bash
#sys/class/backlight/intel_backlight 亮度配置
sudo vim max_brightness  #最大亮度120000
#修改亮度
echo xxxxx > brightness
#脚本
#!/usr/bin/bash
sudo su<<EOF
    echo $1 > /sys/class/backlight/intel_backlight/brightness
EOF
```

```bash
#!/bin/bash
doChangeBrightness(){
	sudo su<<EOF
    echo $1 > /sys/class/backlight/intel_backlight/brightness
EOF
}

bright=$1
#输入参数为空，设为50%
if [ -z "$bright" ]
	then 
	bright=50
	echo "input args is nil, system bright will be set ${bright}%"
fi
#输入不能小于30，大于100
if [ "$bright" -lt "30" ]
then 
	bright=30
	echo "input args too small,system bright will be set 30%"
elif [ "$bright" -gt "100" ]
then
	bright=100
	echo "input args too max,system bright will be set 100%"
else 
	echo "system bright will be set ${bright}%"
fi
#更改系统亮度
doChangeBrightness $((bright * 1200))
```

#### Linux 环境变量文件执行顺序

```bash
==> /etc/profile
==> ~/.bash_profile | ~/.bash_login | ~/.profile
==> ~/.bashrc
==> /etc/bashrc
#登录shell执行脚本的顺序
#登录shell执行/etc/profile
#/etc/profile执行/etc/profile.d目录下的脚本
#然后执行当前用户的~/.bash_profile
#~/.bash_profile执行当前用户的~/.bashrc
#~/.bashrc执行/etc/bashrc

#非登录shell执行脚本的顺序
#非登录shell首先执行~/.bashrc
#然后~/.bashrc执行/etc/bashrc
#/etc/bashrc 调用/etc/profile.d目录下的脚本
```

#### 解决idea中无法中文输入

```bash
#方法一：
#在idea安装目录bin目录下找到idea.sh文件，前面加上
export XMODIFIERS=@im=fcitx
export QT_IM_MODULE=fcitx
#重启IDEA

#方法二
#点击菜单 "Help | Edit Custom VM options..."
#添加 -Drecreate.x11.input.method=true 到最后一行
#重启IDEA
```

#### 关掉蜂鸣声

```bash
 rmmod pcspkr
 echo "blacklist pcspkr" | sudo tee /etc/modprobe.d/nobeep.conf
```



### 美化

####  终端zsh

```bash
sudo pacman -S zsh
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
# 切换到zsh
chsh -s /bin/zsh
# 安装3den主题
sudo vim ~/.zshrc
ZSH_THEME="3den"
```

#### 用户头像设置

```bash
#将头像放入该目录
/usr/share/pixmaps/faces
```



#### Dock

```bash
sudo pacman -Sy latte-dock
```

#### 窗口透明

```bash
yay -Sy devilspie
#创建配置文件
mkdir /home/$USER/.devilspie
vim transparent.ds
#捕获窗口应用名称
xprop | grep 'CLASS' #然后鼠标变成十字，点击窗口
#配置文件内容 
#窗口名称：systemsettings、yakuake、dolphin、netease-cloud-music、konsole
  if (contains (window_class) "dolphin")
    (begin
        (spawn_async (str "xprop -id " (window_xid) " -f _KDE_NET_WM_BLUR_BEHIND_REGION 32c -set _KDE_NET_WM_BLUR_BEHIND_REGION 0 "))
        (spawn_async (str "xprop -id " (window_xid) " -f _NET_WM_WINDOW_OPACITY 32c -set _NET_WM_WINDOW_OPACITY 0xccffffff"))
    )
#创建快捷方式并添加开机启动
```

### Docker

#### 安装

```bash
# Pacman 安装 Docker
sudo pacman -S docker
# 启动docker服务
sudo systemctl start docker 
# 查看docker服务的状态
sudo systemctl status docker
# 设置docker开机启动服务
sudo systemctl enable docker
```

#### 免sudo操作

```bash
# 如果还没有 docker group 就添加一个
sudo groupadd docker
# 将自己的登录名(${USER} )加入该 group 内。然后退出并重新登录就生效啦
sudo gpasswd -a ${USER} docker
# 重启 docker 服务
sudo systemctl restart docker
# 切换当前会话到新 group 或者重启 X 会话
# 注意，这一步是必须的，否则因为 groups 命令获取到的是缓存的组信息，刚添加的组信息未能生效，所以 docker images 执行时同样有错。
newgrp - docker
OR
pkill X
```

#### 镜像容器自定义位置

```bash
vi /usr/lib/systemd/system/docker.service  
#设置参数，追加--graph /new-path/docker
ExecStart=/usr/bin/dockerd --graph /new-path/docker 
#重启守护进程
systemctl daemon-reload 
#查看结果
docker info
```



#### 镜像加速

```bash
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://yeopv49g.mirror.aliyuncs.com"]
}
EOF
```

#### MySQL安装

```bash
docker pull mysql:last
#命令换行前加空格 -v 路径映射
docker run -d --name mysql -p 3306:3306 \
#设置登录密码,只支持5.7一下版本，8以上需要my.cnf文件
-e MYSQL_ROOT_PASSWORD=123456 \
-v /home/anyu/../mysql/data:/var/lib/mysql \
-v /home/anyu/S../mysql/log:/var/log/mysql \
-v /home/anyu/../mysql/config:/etc/mysql \
#指定容器
mysql:8.0.20
#进入容器登录MySQLdoc
docker exec -it mysql /bin/bash
mysql -uroot -p123456

#修改远程登录
use mysql;
select host,user from user;
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
flush privileges;

```

```bash
docker run -d --name mysql -p 3306:3306 \
#设置登录密码,只支持5.7一下版本，8以上需要my.cnf文件
-e MYSQL_ROOT_PASSWORD=123456 \
-v /home/anyu/../mysql/config/my.cnf:/etc/mysql/my.cnf \
-v /home/anyu/../mysql/data:/var/lib/mysql \
mysql:8.0.20
############################################################################
#可选参数
--restart=always                                            -> 开机启动容器,容器异常自动重启
-d                                                          -> 以守护进程的方式启动容器
-v /home/**/mysql/conf.d/my.cnf:/etc/mysql/my.cnf          -> 映射配置文件
-v /home/**/mysql/logs:/logs                               -> 映射日志
-v /home/**/mysql/data/mysql:/var/lib/mysql                -> 映射数据
-p 3306:3306                                                -> 绑定宿主机端口
--name mysql                                                -> 指定容器名称
-e MYSQL_ROOT_PASSWORD=123456   
##############################################################################
#my.cnf文件
[mysqld]
pid-file        = /var/run/mysqld/mysqld.pid
socket          = /var/run/mysqld/mysqld.sock
datadir         = /var/lib/mysql
secure-file-priv= NULL
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
 
# Custom config should go here
!includedir /etc/mysql/conf.d/
```



### Linux 其他操作

#### 查看和关掉某个端口

```bash
#Linux下端口被占用（例如端口3000），关掉端口占用的进程的方法：
netstat -tln | grep 3000
sudo lsof -i:3000
sudo kill -9 进程id
```

### 安装显卡