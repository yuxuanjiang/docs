# Windows 系统如何生成SSH

<p class="warn">Windows系统本身自带的 Command 并不能直接生成 SSH，需要使用 Git Bash 或者 Windows Terminal 来生成 SSH。因此，前提需要在你的 Windows 系统上安装 Git Bash 或者 Terminal。本文将使用 Windows Terminal 来实现 SSH 的生成以及查看。</p>



## 1. 安装 Windows Terminal

`Windows Terminal` 是 Windows 官方发布的命令行工具，如果想要安装 Window Terminal，只能在 `Microsoft Store` 中进行安装。打开 Microsoft Store 之后，直接搜索 \"Windows terminal\" 就能进行下载。

![image-20200807225453424](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghj7tz9sn7j31650u0tzh.jpg)



## 2. 使用 Windows Terminal 生成 SSH keygen

Windows Terminal 的指令差不多和 Linux 的指令差不多，但是还是有很多用法不同的地方，因此，Windows Terminal 本身并不适合用于学习 Linux 的指令。



打开 Windows Terminal 之后，当前的路径就是处于当前的用户目录下 `C:\Users\username`,  因此直接在此输入下面的指令：

```nginx
ssh-keygen
```



这时候 Terminal 会显示反馈信息:

```nginx
Generating public/private rsa key pair.
Enter file in which to save the key (C:\Users\username/.ssh/id_rsa):
```



看到上面的信息之后，就一直点回车 (差不多4次)。最终会给出下面的信息, 就说明 `SSH Keygen` 生成成功

```nginx
PS C:\Users\username> ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (C:\Users\username/.ssh/id_rsa):
Created directory 'C:\Users\username/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\username/.ssh/id_rsa.
Your public key has been saved in C:\Users\username/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:rOZkRaJ20/zMXwtOtFmrBpf+iTx6N4hBqWvcI7J+r6w username@DESKTOP-CVQH04E
The key's randomart image is:
+---[RSA 2048]----+
|                 |
|                 |
|      . .  .     |
|     . *  o      |
|    o o So  ...  |
|   . . +.+o.o+ . |
|      =. o+*=.o  |
|     =..* +=*=oo |
|     .E*o+o=*++. |
+----[SHA256]-----+
```



## 3. 查看系统上的公钥

当 SSH Keygen 都生成成功之后，公钥和密匙都是保存在 `C:\Users\username` 目录下的 `.ssh` 目录中，其中会有两个文件，分别是 `id_rsa` 和 `id_rsa.pub`。其中 id_rsa.pub 的内容就是公钥。



在 `C:\Users\username` 目录下执行下面两行指令:

```nginx
cd .ssh
cat id_rsa.pub
```



Terminal 上就会输出 id_rsa.pub 的公钥内容 (以 ssh-rsa 开头)：

```bash
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDE4y8YZOWP1jAKQVX6+ge9DDeskiNQDZVSZIWpXCbKi3DxXqedwgTTWygcp6j9Fx+418OuHMNkyw45cOUaRD7OVbeWqWmawZN3eoNON4Hd/LB2sILOAaJBv3sT9VDzzFm5rabSt16Dsq/XAXSBb5OGwuokAbIATwsnWPPaN3ET2t/mVh71NIRTps6wvgtY5/kO/RPuyeHKnvqUErPQCjz8KNcMXKOZijI1BQxAJOQZD7E6+kA79/zmg9fNkJJo5DIUML8gNuMNGw9qcbiydpDjussqsq4V9YT/wykPoPgUvr7jw7iMK0CU51DLat+IeuT/bLHbALXvo1iUcNhi/1IV username@DESKTOP-CVQH04E
```



## 操作演示

<video controls width="100%">
    <source src="https://yuxuanjiang.github.io/boozhi/_media/widnows_sshkeygen.mp4" type="video/mp4">
</video>

