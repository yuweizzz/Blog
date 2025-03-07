---
date: 2020-07-20 23:00:00
title: 配置 GitHub 认证密钥
tags:
  - "GitHub"
  - "SSH"
  - "Git"
draft: false
---

通过配置 GitHub SSH Key 实现 GitHub 免密推送，并且使用 GPG 密钥对 commit 进行签名。

<!--more-->

```bash

                                       (@@) (  ) (@)  ( )  @@    ()    @     O     @     O      @
                                  (   )
                              (@@@@)
                           (    )

                         (@@@)
                       ====        ________                ___________
                   _D _|  |_______/        \__I_I_____===__|_________|
                    |(_)---  |   H\________/ |   |        =|___ ___|      _________________
                    /     |  |   H  |  |     |   |         ||_| |_||     _|                \_____A
                   |      |  |   H  |__--------------------| [___] |   =|                        |
                   | ________|___H__/__|_____/[][]~\_______|       |   -|                        |
                   |/ |   |-----------I_____I [][] []  D   |=======|____|________________________|_
                 __/ =| o |=-~O=====O=====O=====O\ ____Y___________|__|__________________________|_
                  |/-=|___|=    ||    ||    ||    |_____/~\___/          |_D__D__D_|  |_D__D__D_|
                   \_/      \__/  \__/  \__/  \__/      \_/               \_/   \_/    \_/   \_/

```

## 前言

我们通过本地 Git 来远程管理 GitHub 上的仓库，在每次推送都需要输入账户和密码，可以通过配置 GitHub 公钥，省去重复输入账户密码的麻烦。

## 创建秘钥对

GitHub 的推送和 SSH 的登陆原理非常相似，通常是本地使用私钥，GitHub 端保存公钥信息，我们使用 Git 推送时，以私钥加密的信息可以被对应的公钥解析，通信就能够建立了。

我们需要在本地先生成一个新的秘钥对。

```bash
$ ssh-keygen -t rsa -C "注册 GitHub 时的邮箱"
# -t 代表秘钥对的加密算法，rsa是最常见的非对称加密算法
# -C 可以指定公钥的文本信息，这一步是 GitHub 要求的
```

上述的命令会产生一个交互对话，按照对话提示输入对应信息即可。

在交互式生成模式下，会提示我们指定生成秘钥对的文件名和路径，通常在 Linux 环境下，保持默认则为 /home/user/.ssh/id_rsa ，user 为当前使用的用户。

此外，在交互式生成模式下，还会提示使用 passphrase 再度加密，这个功能一般不使用，否则每次使用密钥都需要再次输入 passphrase 。

所以，我比较推荐使用非交互式的命令生成秘钥对。

```bash
$ ssh-keygen -t rsa -C "注册 GitHub 时的邮箱" -P "" -f ~/.ssh/id_rsa
# 非交互生成的参考命令
# -P passphrase为空
# -f 指定路径名称
```

执行完命令后，我们可以得到 id_rsa 和 id_rsa.pub 两个文件，其中 id_rsa 是私钥，id_rsa.pub 是公钥。

## 在 GitHub 账户中添加公钥

我们需要把公钥信息上传到自己的 GitHub 账户。

打开自己的[GitHub 账户](https://github.com/settings/keys)，点击 New SSH key ，复制 id_rsa.pub 的内容到 Key 中，Title 按照自己的需求命名，然后 Add SSH Key ，完成公钥添加。

## 测试公钥

完成云端公钥信息的添加后，我们可以在本地测试公钥是否正确设置。

```bash
# 使用生成秘钥的账户测试
$ ssh git@github.com

# 出现 kex_exchange_identification: Connection closed by remote host 报错的解决方法：
# 通过 ~/.ssh/config 重新指定 ssh 信息
# 应该尽量通过写入 ssh_config 的方式实现，尤其是私钥不是以默认方式命名的情况
$ cat > ~/.ssh/config << EOF
Host github.com
    HostName ssh.github.com
    User git
    Port 443
    IdentityFile ~/.ssh/id_rsa
EOF

# 出现报错并且尚未写入 ssh_config 时可以通过以下的命令行测试
$ ssh git@ssh.github.com -i ~/.ssh/id_rsa -p 443
```

在第一次使用免密登录时会需要我们确认 GitHub 的主机信息，输入 yes 即可。

如果一切正常，会返回下面的信息：

```bash
$ ssh git@github.com
PTY allocation request failed on channel 0
Hi yuweizzz! You've successfully authenticated, but GitHub does not provide shell access.
Connection to github.com closed.
```

其实从这部分可以得出 GitHub 推送仓库是基于 OpenSSH 来实现的。

更多信息可以参考 GitHub 的[官方文档](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh)。

## 利用 GPG 密钥对 commit 进行签名

```bash
# 生成 GPG 密钥
$ gpg --full-generate-key

# 查看生成后的密钥信息
$ gpg --list-keys

# 导出对应的公钥，并将它添加到对应的 Github 账户中
$ gpg --armor --export <key-id>

# 在对应仓库中设置 signingkey 后，就可以在 commit 时进行签名
# GPG_TTY 用于交互式获取 GPG 密钥的 passphrase ，没有正常设置时可能会无法进行签名
$ git config user.signingkey <key-id>
$ export GPG_TTY=$(tty)
$ git commit -S -m "..."
```
