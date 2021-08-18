# V2ray Linux客户端v2rayA下载安装及使用教程 支持VMess/VLESS/SS/SSR/Trojan

> 本文转载自：https://ssr.tools/1827，如文中内容有错误请到原文查看原始版(最新版)

### 关于v2rayA

[V2ray](https://v2xtls.org/v2ray/)是一款大家常用的科学上网代理工具，在Ubuntu/Debian等Linux系统平台上，V2ray官方提供有相应内核，但是并没有图形化界面。

v2rayA则是在Linux官方内核的基础上，为大家提供了Web GUI，可以更直观的进行V2ray各项操作，可用于电脑、路由器、NAS等Linux平台设备。

除了用于V2ray客户端外，v2rayA还兼容其它几款主流的代理工具，具体包括：

- V2ray VMess
- V2ray VLESS
- [Shadowsocks](https://v2xtls.org/shadowsocks-ss/)
- [SSR](https://v2xtls.org/shadowsocksr-ssr/)
- [Trojan](https://v2xtls.org/shadowsocksr-ssr/)
- PingTunnel

v2rayA使用界面如下图所示：

![img](https://v2xtls.org/wp-content/uploads/2020/11/st10859.jpg)

### v2rayA下载地址

在使用v2rayA之前，建议首先完成V2ray服务器的搭建，具体可以参考：

[最流行的V2Ray一键安装脚本推荐汇总 包含WS+TLS+伪装网站](https://v2xtls.org/v2ray多合一脚本，支持vmesswebsockettlsnginx、vlesstcpxtls、vlesstcptls等组合/)

**最新版v2rayA下载地址：[v2ray客户端下载](https://v2xtls.org/v2ray客户端/)**

### v2rayA使用教程

注：以下使用教程来自v2rayA官方Wiki，包含多种安装方式可选。

**从软件源安装**

- debian/ubuntu

请确保已正确安装 v2ray-core，我们提供了 Linux 下的一键安装脚本：

运行下面的指令下载并安装 V2Ray。当 yum 或 apt-get 可用的情况下，此脚本会自动安装 unzip 和 daemon。这两个组件是安装 V2Ray 的必要组件。如果你使用的系统不支持 yum 或 apt-get，请自行安装 unzip 和 daemon

```shell
# download script
curl -O https://cdn.jsdelivr.net/gh/mzz2017/v2rayA@master/install/go.sh
# install v2ray-core from jsdelivr
sudo bash go.sh
```

准备完毕后：

```shell
# add public key
wget -qO - http://apt.v2raya.mzz.pub/key/public-key.asc | sudo apt-key add -
# add V2RayA's repository
sudo add-apt-repository 'deb http://apt.v2raya.mzz.pub/ v2raya main'
sudo apt-get update

# install V2RayA
sudo apt-get install v2raya
```

部署完毕后，访问该机器的2017端口即可使用，如http://localhost:2017。

- archlinux/manjaro

v2raya已发布于AUR中：

```shell
git clone https://aur.archlinux.org/v2raya.git /tmp/v2raya
cd /tmp/v2raya
makepkg -si
```

如果makepkg失败，运行以下命令再试：

```shell
sudo pacman -S base-devel
```

部署完毕后，访问该机器的2017端口即可使用，如http://localhost:2017。

**Docker方式**

- docker命令，仅使用 docker 命令部署。

```shell
# run v2raya
docker run -d \
--restart=always \
--privileged \
--network=host \
--name v2raya \
-v /etc/v2raya:/etc/v2raya \
mzz2017/v2raya
```

部署完毕后，访问该机器的2017端口即可使用，如http://localhost:2017。

如果你使用MacOSX或其他不支持host模式的环境，在该情况下无法使用全局透明代理，docker命令会略有不同：

```shell
# run v2raya
docker run -d \
-p 2017:2017 \
-p 20170-20172:20170-20172 \
--restart=always \
--name v2raya \
-v /etc/v2raya:/etc/v2raya \
mzz2017/v2raya
```

部署完毕后，访问该机器的2017端口即可使用，如http://localhost:2017。

- docker-compose

拉取源码，使用 docker-compose 部署。

```shell
git clone https://github.com/mzz2017/V2RayA.git
cd V2RayA
docker-compose up -d --build
```

如果出现ERROR: …Connot start service…container…is not running，尝试添加参数-V

部署完毕后，访问该机器的2017端口即可使用，如http://localhost:2017。

**二进制文件、安装包**

请确保已正确安装 v2ray-core，我们提供了 Linux 下的一键安装脚本：

运行下面的指令下载并安装 V2Ray。当 yum 或 apt-get 可用的情况下，此脚本会自动安装 unzip 和 daemon。这两个组件是安装 V2Ray 的必要组件。如果你使用的系统不支持 yum 或 apt-get，请自行安装 unzip 和 daemon

```shell
# download script
curl -O https://cdn.jsdelivr.net/gh/mzz2017/v2rayA@master/install/go.sh
# install v2ray-core from jsdelivr
sudo bash go.sh
```

准备完毕后，可下载[Releases](https://github.com/mzz2017/V2RayA/releases)中的二进制文件启动V2RayA服务端，或下载安装包进行安装。

部署完毕后，访问该机器的2017端口即可使用，如http://localhost:2017。

**自行编译运行**

当然，你也可以选择拉取源码，编译为二进制文件运行：

该方法同样需要正确安装v2ray-core，详情见上

```shell
git clone https://github.com/mzz2017/V2RayA.git
cd V2RayA/service
# set goproxy.io as the proxy of go modules
export GOPROXY=https://goproxy.io
# compile
go build -o v2raya
# run
sudo ./v2raya
```

注意，尽管 golang 具有交叉编译的特性，但由于项目使用了大量 linux commands，导致该方法仍然不支持 windows。若想在 windows 体验，可尝试借助 Docker 或 WSL。

**在NAS或路由器使用**

分为以下几种情况：

- daemon

v2ray能够以daemon存在即在正确安装v2ray后，使用下述命令之一能够得到正确的反馈：

```shell
# if systemctl is available
systemctl status v2ray
# else if service is available
service v2ray status
```

那么可从软件源安装，或下载[releases](https://github.com/mzz2017/V2RayA/releases)中的对应安装包进行安装。

- docker

若v2ray能够运行于docker，则可参照上文的Docker方式使用。

**通用方法**

请自行安装v2ray，并确保v2ray、v2ctl均被包含在PATH中，否则请将上述文件放于echo $PATH中的任一目录下。

下载releases中最新版本的对应CPU架构的二进制文件，或自行使用golang交叉编译。

使用以下参数启动V2RayA服务端，参数含义可执行–help查看。

```shell
--config=V2RAYA_CONFIG_PATH --mode=universal
```

请将上述V2RAYA_CONFIG_PATH替换为一个可读写的，并且你喜欢的路径。

### **其它常用代理工具 一键搭建脚本**

[Shadowsocks一键安装脚本使用教程](https://v2xtls.org/shadowsocks-ss一键脚本/)

[SSR一键安装脚本使用教程](https://v2xtls.org/shadowsocksr-ssr一键脚本/)

[Trojan一键安装脚本使用教程](https://v2xtls.org/trojan一键脚本/)

[trojan-go一键安装脚本使用教程](https://v2xtls.org/trojan-go一键脚本/)





ref

https://v2xtls.org/v2ray-linux%e5%ae%a2%e6%88%b7%e7%ab%afv2raya%e4%b8%8b%e8%bd%bd%e5%ae%89%e8%a3%85%e5%8f%8a%e4%bd%bf%e7%94%a8%e6%95%99%e7%a8%8b-%e6%94%af%e6%8c%81vmess-vless-ss-ssr-trojan-pingtunnel/

https://ssr.tools/1827

https://v2xtls.org/v2ray%e5%ae%a2%e6%88%b7%e7%ab%af/

https://zhen.bushini.de/23378.html