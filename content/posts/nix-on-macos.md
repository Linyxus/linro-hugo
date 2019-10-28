---
title: "Nix on macOS, Made Easy"
date: 2019-10-28T15:14:00+08:00
showDate: true
draft: false
tags: ["tutorial", "nix"]
---

# What is Nix?

简单来说，Nix是一个纯函数式包管理器。它将函数式的思想应用到了包管理中——每一个package都是一条语句的值，安装、编译包的过程是对表达式求值，而不是借助副作用。一个包一旦编译完成，就是不可变的。所有编译好的包都会被存储于`/nix/store/`中。

Nix的思想其实很值得体会，也的确能为实际应用带来莫大的方便与效率，但对这些话题的探讨不在本篇文章的范畴之内。

本文的主要陈述了怎样在macOS Catalina下，安装使用`nix`。

# Installation

## 主要问题

事实上，在macOS Catalina之前，`nix`的安装都不是一件困难的事情，简简单单，一条指令就能解决：

```shell
$ sh <(curl https://nixos.org/nix/install)
```

然而在Calalina之后，官方默认的安装脚本便失效了。主要原因是：**Apple改变了macOS的文件结构，将系统与用户数据挂载到了两个分区，`/`变为了只读目录。**

然而，`nix`的所有文件都需要放到`/nix`下，`/`变为只读直接导致`nix`无法安装到Catalina上。

## 解决方法

解决方法参照了[这个issue](https://github.com/NixOS/nix/issues/2925)中的讨论与一个[Github repo](https://github.com/steshaw/catalina-nix-upgrade)。

简单来说，既然安装脚本无法在根目录中创建文件夹，那我们就先为他创建好`/nix`，再进行安装。

具体来说，只需要新建一个分区并挂载到`/nix`即可。

刚刚提到的repo是针对升级Catalina前已安装`nix`进行recover用的，稍作修改，可以得到在Catalina上安装`nix`的解决方案。[Install Nix on Catalina](https://github.com/Linyxus/install-nix-on-catalina)

## 具体步骤

首先，clone刚刚给出的[repo](https://github.com/Linyxus/install-nix-on-catalina)。

```shell
git clone https://github.com/Linyxus/install-nix-on-catalina.git
cd install-nix-on-catalina
```

执行第一个脚本

```shell
./step-1
```

重启电脑，随后执行第二个脚本

```shell
./step-2
```

Voila!

# Multi-user installation 使用Proxy的问题

如果在之前的安装中，选择了multi user，也即使用daemon的选项，proxy的使用就会成为一个问题。

为什么一定要用proxy？因为至少在我的环境下，直连cache.nixos.org下载binary的速度实在是太！慢！了！

而Proxy的使用为什么会成为问题呢？因为nix multi user的实现中，各项操作事实上都是由daemon完成的，这意味着即使在安装时设置了proxy，事实上daemon还是直连下载的：

```shell
export http_proxy=http://127.0.0.1:1087;export https_proxy=http://127.0.0.1:1087;
nix-env -f '<nixpkgs>' -iA cabal-install
```

这很不友好。

事实上，关于这个问题，在官方的[Nix Mannual](https://nixos.org/nix/manual/#sec-installer-proxy-settings)中有所提及。

> The Nix installer has special handling for these proxy-related environment variables: `http_proxy, https_proxy, ftp_proxy, no_proxy, HTTP_PROXY, HTTPS_PROXY, FTP_PROXY, NO_PROXY`.

> If any of these variables are set when running the Nix installer, then the installer will create an override file at `/etc/systemd/system/nix-daemon.service.d/override.conf` so nix-daemon will use them.

不幸的是，这并不适用于macOS：`launchd`没有办法很容易地override服务配置文件中的一些字段。为了解决这一问题，最简单的方法是新建一个service，将原来的daemon service删除。

```XML
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>EnvironmentVariables</key>
    <dict>
      <key>OBJC_DISABLE_INITIALIZE_FORK_SAFETY</key>
      <string>YES</string>
      <key>http_proxy</key>
      <string>http://127.0.0.1:1087</string>
      <key>https_proxy</key>
      <string>http://127.0.0.1:1087</string>
    </dict>
    <key>Label</key>
    <string>org.nixos.nix-daemon</string>
    <key>KeepAlive</key>
    <true/>
    <key>RunAtLoad</key>
    <true/>
    <key>ProgramArguments</key>
    <array>
      <string>/bin/sh</string>
      <string>-c</string>
      <string>/bin/wait4path /nix/store/6639l9815ggdnb4aka22qcjy7p8w4hb9-nix-2.3.1/bin/nix-daemon &amp;&amp; /nix/store/6639l9815ggdnb4aka22qcjy7p8w4hb9-nix-2.3.1/bin/nix-daemon</string>
    </array>
    <key>StandardErrorPath</key>
    <string>/var/log/nix-daemon.log</string>
    <key>StandardOutPath</key>
    <string>/dev/null</string>
  </dict>
</plist>
```

修改的配置文件在相比于原来的配置，唯一的区别是在 `EnvironmentVariables` 下增加了两个字段：`http_proxy`与`https_proxy`。将这个文件保存为`org.nixos.nix-daemon-proxy.plist`，然后执行

```shell
sudo launchctl unload org.nixos.nix-daemon.plist
sudo launchctl load org.nixos.nix-daemon-proxy.plist
rm org.nixos.nix-daemon.plist
```

至此，`nix-env -i`应该可以通过proxy下载binary了。

这个方案最大的问题在于，每一次`nix`更新时，理应都要重新修改一次配置。配置文件中有hard-coded的，包含hash与版本好的路径。产生这样问题的主要原因是，我们把impure引入到了一个pure的系统中。

但：能用就行。
