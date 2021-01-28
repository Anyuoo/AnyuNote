### 购买服务器

### 部署vps服务器

1. 使用远程工具链接到服务器，链接不到说明ip被墙了

2. CentOS 6和7

   - 脚本一：

     ```shell
     yum -y install wget
     
     wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/ssr.sh && chmod +x ssr.sh && bash ssr.sh
     ```

   - 脚本二：

     ```shell
     yum -y install wget 
     
     wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR.sh
     
     chmod +x shadowsocksR.sh
     
     ./shadowsocksR.sh 2>&1 | tee shadowsocksR.log
     ```

   安装ShadowsocketsR服务

   1. 设置端口

   2. 设置密码

   3. 设置加密方式

   4. 协议插件

      ```
      注意：如果协议是origin，那么混淆也必须是plain；如果协议不是origin，那么混淆可以是任意的。有的地区需要把混淆设置成plain才好用。因为混淆不总是有效果，要看各地区的策略，有时候不混淆（plain）或者（origin和plain一起使用），让其看起来像随机数据更好。（特别注意：tls 1.2_ticket_auth容易受到干扰！请选择除tls开头以外的其它混淆！！！）
      ```

      ```
      注意：如果创建的是centos7的服务器，需要使用命令关闭防火墙，否则无法使用代理。CentOS 7.0默认使用的是firewall作为防火墙。
      
      查看防火墙状态命令：firewall-cmd --state
      
      停止firewall命令：systemctl stop firewalld.service
      
      禁止firewall开机启动命令：systemctl disable firewalld.service
      ```

      ​	

   安装加速器

   **加速教程1：破解版锐速加速教程**

   - ```shell
     yum -y install wget
     
     wget --no-check-certificate https://blog.asuhu.com/sh/ruisu.sh && bash ruisu.sh
     ```

     ​	

   - ```shell
     wget -N --no-check-certificate https://raw.githubusercontent.com/91yun/serverspeeder/master/serverspeeder-all.sh && bash serverspeeder-all.sh
     ```

   - 卸载

     ```shell
     chattr -i /serverspeeder/etc/apx* && /serverspeeder/bin/serverSpeeder.sh uninstall -f
     
     
     ```

     **谷歌BBR加速，推荐**

     ```shell
     yum -y install wget
     wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh
     chmod +x bbr.sh
     ./bbr.sh
     ```

```
	加密 : chacha20-ietf
	协议 : auth_aes128_sha1
	混淆 : tls1.2_ticket_auth


```

>V2Ray 

**v2ray一键搭建脚本支持的系统有：Debian 8、Debian 7、Ubuntu 14、Ubuntu 16、CentOS 7。（注意：不支持CentOS 6和8系统！）**

1. **Debian8/Debian7/Ubuntu16/Ubuntu14/CentOS7 v2ray一键部署管理脚本**：

```shell
wget -N --no-check-certificate https://raw.githubusercontent.com/KiriKira/v2ray.fun/kiriMod/install.sh && bash install.sh
```

2. 卸载脚本命令：

```shell
wget -N --no-check-certificate https://raw.githubusercontent.com/KiriKira/v2ray.fun/kiriMod/uninstall.sh && bash uninstall.sh
```

3. 输入快捷管理命令v2ra

```
输入数字2进行更改配置，共有6个子选项，包括：更改UUID、更改主端口、更改加密方式、更改传输方式、更改TLS设置（有域名才行）、更改广告拦截功能。（更改TLS设置和更改广告拦截功能不用设置）

如上图，输入数字1来更改新的UUID号，弹出提示后，输入字母y来确认。

修改UUID号，界面会回到v2ray主界面，重新输入2进入更改配置选项，在输入数字2来更改主端口，主端口范围40～65535，理论上可以任意设置，但不要以0开头！

重新进入更改配置选项，输入数字3来更改加密方式，加密方式有4种，最后1种为不加密。
```



4. ```
    接着，进行传输方式的设置，传输方式共有7种，这个配置对v2ray的速度起着很大的作用，具体哪个最适合你那里的网络环境，需要你自己来尝试。**
      
      **注意：普通TCP、普通mKCP、mKCP伪装FaceTime通话、mKCP伪装BT下载流量、mKCP伪装微信视频流量可直接设置、不需要域名，HTTP伪装和WebSocket流量需要你有域名，且域名绑定了你的vps服务器ip。(在教程的最后“常见问题参考解决方法”里面增加了专门部署ws+tls的脚本)**
      
   v**接着，进行传输方式的设置，传输方式共有7种，这个配置对v2ray的速度起着很大的作用，具体哪个最适合你那里的网络环境，需要你自己来尝试。**
   
   **注意：普通TCP、普通mKCP、mKCP伪装FaceTime通话、mKCP伪装BT下载流量、mKCP伪装微信视频流量可直接设置、不需要域名，HTTP伪装和WebSocket流量需要你有域名，且域名绑定了你的vps服务器ip。(在教程的最
   ```

   后“常见问题参考解决方法”里面增加了专门部署
   
   )**

>unknown Socks version: 67
>这个是用错了socks和http的端口
>http端口=socks端口+1

```shell
#设置 Nginx 开机自启 完成
bash <(curl -L -s https://raw.githubusercontent.com/wulabing/V2Ray_ws-tls_bash_onekey/master/install.sh) | tee v2ray_ins.log
```

```
 地址（address）: www.anyuoo.club 
 端口（port）： 443 
 用户id（UUID）： 52ca706f-5ed1-4acf-ab6a-0edfb3c354cc
 额外id（alterId）： 68
 加密方式（security）： 自适应 
 传输协议（network）： ws 
 伪装类型（type）： none 
 路径（不要落下/）： /f07da979/ 
 底层传输安全： tls 
  URL导入链接:vmess://ewogICJ2IjogIjIiLAogICJwcyI6ICJ3dWxhYmluZ193d3cuYW55dW9vLmNsdWIiLAogICJhZGQiOiAid3d3LmFueXVvby5jbHViIiwKICAicG9ydCI6ICI0NDMiLAogICJpZCI6ICI1MmNhNzA2Zi01ZWQxLTRhY2YtYWI2YS0wZWRmYjNjMzU0Y2MiLAogICJhaWQiOiAiNjgiLAogICJuZXQiOiAid3MiLAogICJ0eXBlIjogIm5vbmUiLAogICJob3N0IjogInd3dy5hbnl1b28uY2x1YiIsCiAgInBhdGgiOiAiL2YwN2RhOTc5LyIsCiAgInRscyI6ICJ0bHMiCn0K 
```

>请注意,如果你使用 Quantaumlt X / 路由器 / 旧版 Shadowrocket / 低于 4.18.1 版本的 V2ray core 请选择 兼容模式