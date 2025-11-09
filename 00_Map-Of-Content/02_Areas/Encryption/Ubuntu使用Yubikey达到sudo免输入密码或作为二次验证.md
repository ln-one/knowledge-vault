---
tags:
  - Tools/Security/YubiKey
created: 2025-08-13
author:
  - ln1
status: In Progress
---
### 1.安装所需的软件

打开终端，输入以下命令来安装所需

```js
sudo apt-get install libpam-u2f
```

### 2.将设备和您的计算机联系起来

打开终端并插入您的Canokey，然后创建存储key的文件夹（此处储存至~/.config/Canokey）

```js
mkdir -p ~/.config/Yubikey
```

接着运行以下命令（替换为您的路径）

```js
pamu2fcfg > ~/.config/Yubikey/u2f_keys
```

此时观察Canokey的灯是否闪烁，若闪烁请您轻触金属部分以继续。

如果您有多个Canokey（或Yubikey），类似的可输入以下命令继续设置（若没有则跳过，后续购入了可按此操作）

```js
pamu2fcfg -n >> ~/.config/Yubikey/u2f_keys
```

成功后会生成u2f\_keys。但此时（按照文章操作的话）处于一个都可以访问的路径，您可以移动到一个仅root用户有权限的路径。此处我移动到“/etc/Yubikey”（sudo mv ~/.config/Yubikey/u2f\_keys /etc/Yubikey/u2f\_keys）

### 3.设置sudo使用Yubikey

在终端中输入以下命令打开配置文件

```js
sudo nvim /etc/pam.d/sudo
```

在“@include common-auth”前或后按照您的需要加入内容

- 在插入Yubikey时免密码，未插入时输入密码：`auth sufficient pam\_u2f.so authfile=/etc/Yubikey/u2f\_keys`，在`@include common-auth`前；
- 必须插入Yubikey，同时也要输入密码（二次验证）：`auth required pam\_u2f.so authfile=/etc/Yubikey/u2f\_keys`，在`@include common-auth`后。