## 起

### 起因

在学习 Redis 的时候，学到 Redis 在进行内存快照时会使用 bgsave 命令

> bgsave：创建一个子进程（使用 fork 函数），专门用于写入 RDB 文件，避免了主线程的阻塞，这也是 Redis RDB 文件生成的默认配置。
>
> bgsave 子进程需要通过 fork 操作从主线程创建出来。虽然，子进程在创建后不会再阻塞主线程，但是，fork 这个创建过程本身会阻塞主线程，而且主线程的内存越大，阻塞时间越长。如果频繁 fork 出 bgsave 子进程，这就会频繁阻塞主线程了。

对于”fork 这个创建过程本身会阻塞主线程，而且主线程的内存越大，阻塞时间越长”这句，当时读完很愣逼，虽然能感觉出内味儿，但是还是对这个阻塞时间没有一个明显的感知。

最终还是自己实践一下比较实在。

## 承

### 实验目标

在 Linux 上测试占内存1个G的进程进行 fork 操作需要多少时间

1. 如何写一个占内存1G的程序？
2. 在 Windows 的 Visual Studio 写出来的 C++ 程序如何跑到 Linux 上？
3. 如何来衡量 fork 函数的执行时间？

## 转

接下来是过程：

### 如何在 Windows 上使用 C++ 的 fork 函数？

众所周知，Windows 上的 C++ 是不支持 fork 这个函数的，所以想使用 fork 函数的话，有两个选择：

1. 找个 Linux 虚拟机跑程序
2. 使用微软爸爸的 Windows Subsystem for Linux

对于第一个选项来讲，成本很大，除非有现成的可用的 Linux 虚拟机（我的 NAS 里面就运行的有），否则从头部署一台 Bare Metal Linux 还是有点耗时的。

对于第二个选项，要看自己的 Win10 版本：

- 如果是 Win10 Version 1903 以上（最好是最新版本），那直接用 WSL2，不需要安装 Hyper-V 虚拟机组件
- 如果是低于1903这个版本，只能用 WSL1，需要安装 Hyper-V

我使用的是 WSL2，感觉很舒服，不安装 Hyper-V 感觉还是蛮好的（Hyper-V 和其它虚拟机软件会有冲突）。

在 Visual Stuido 开发 C++ 运行到 WSL2 的相关流程，这里就暂时略过，这个内容不是本文重点。

### 如何来衡量 fork 函数的执行时间？



### 如何写一个占内存1G的程序



### 如何测试



## 结

### 结论



## 附

官方的 [WSL 命令的配置文档](https://docs.microsoft.com/en-us/windows/wsl/wsl-config#set-a-default-distribution)

[fork(2) — Linux manual page](https://man7.org/linux/man-pages/man2/fork.2.html#top_of_page)

ben佬：

对了我给你说这个的原因就是，你要是了解这个，那你就要摸到cpp的巨坑了

编译器与你的实际效果相关

给你几个关键字你搜一下

c++  std::string cow sso

cpp在g++5.0还是4.5之前都是cow的实现，但是如果你编译器是这个版本之上的