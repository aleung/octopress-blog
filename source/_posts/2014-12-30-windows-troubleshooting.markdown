---
layout: post
title: "Trouble shooting: Windows响应缓慢"
date: 2014-12-30 16:24:15 +0800
comments: true
tags:
- SysAdmin 
- Windows
---
最近一个星期，电脑出现了缓慢的现象：打开任何应用，窗口都要等待上一段时间才能出现。

在 [Process Explorer](http://technet.microsoft.com/en-us/sysinternals/bb896653) 中看到进程已经出现，也没有什么CPU占用，但窗口就是不出来，好像是要等到什么操作超时。这里如果能够查看调用栈，应该就能知道程序卡在什么地方。

上网搜索一下，看到一些文章提到 troubleshoot Windows 问题，都用到了 WinDbg 来分析进程调用栈。这篇[文章](http://www.codeproject.com/Articles/6084/Windows-Debuggers-Part-A-WinDbg-Tutorial)简要介绍了 WinDbg 的使用方法（不过可能文章年代过于久远，有些细节已经对不上了）。于是安装相应软件，实操一下。

只是用到 WinDbg 的一些皮毛功能，包括这些命令：

| Command | Description |
|---------|-------------|
|~        | 列出所有线程  |
|k _5_    | 显示当前线程的stack trace，如果附带数字则限制打印条数 |
|kb       | 显示stack trace，包含前三个调用参数 |
|~n[cmd]  | 对n号线程执行 _cmd_ 操作 |
|~*[cmd]  | 对所有线程执行操作，如 `~*k 5` |
|~ns      | 切换到n号线程 |

WinDbg 可以 attach 到一个执行中的进程。但我只是想看看某个时刻那个进程在干什么，用 Process Explorer 创建进程的 minidump 更加简单，在 GUI 中选择进程，右键菜单选择 dump 就能创建出 dump 文件，可以多 dump 几次，然后用WinDbg 打开 dump 文件查看。

~~~
Loading Dump File [C:\tmp\2014-12\notepad++.dmp]
User Mini Dump File: Only registers, stack and portions of memory are available

Symbol search path is: srv*c:\LocalSymbols* http://msdl.microsoft.com/download/symbols
Executable search path is:
Windows 7 Version 7601 (Service Pack 1) MP (4 procs) Free x86 compatible
Product: WinNt, suite: SingleUserTS
Machine Name:
Debug session time: Tue Dec 30 15:53:27.000 2014 (UTC + 8:00)
System Uptime: not available
Process Uptime: 0 days 0:00:06.000
~~~

这是 notepad++ 启动6秒钟后的 dump，看看这时有些什么线程，分别在干什么：

~~~
0:000> ~*k 6

.  0  Id: 1894.d54 Suspend: 0 Teb: 7efdd000 Unfrozen
ChildEBP RetAddr 
00137980 74c314ab ntdll!NtWaitForSingleObject+0x15
001379ec 764b1194 KERNELBASE!WaitForSingleObjectEx+0x98
00137a04 764b1148 kernel32!WaitForSingleObjectExImplementation+0x75
00137a18 74a5098c kernel32!WaitForSingleObject+0x12
00137a28 74a4fcdc msctf!CCicEvent::Wait+0x15
00137ca4 74a4fbd9 msctf!CAssemblyList::LoadFromCache+0x94

   1  Id: 1894.107c Suspend: 0 Teb: 7efda000 Unfrozen
ChildEBP RetAddr 
WARNING: Frame IP not in any known module. Following frames may be wrong.
00b4ff88 764b338a 0x1e02d1
00b4ff94 76ecbf32 kernel32!BaseThreadInitThunk+0xe
00b4ffd4 76ecbf05 ntdll!__RtlUserThreadStart+0x70
00b4ffec 00000000 ntdll!_RtlUserThreadStart+0x1b

   2  Id: 1894.4bc Suspend: 0 Teb: 7efd7000 Unfrozen
ChildEBP RetAddr 
02ecfe28 76ee1ad0 ntdll!NtWaitForWorkViaWorkerFactory+0x12
02ecff88 764b338a ntdll!TppWorkerThread+0x216
02ecff94 76ecbf32 kernel32!BaseThreadInitThunk+0xe
02ecffd4 76ecbf05 ntdll!__RtlUserThreadStart+0x70
02ecffec 00000000 ntdll!_RtlUserThreadStart+0x1b

   3  Id: 1894.f30 Suspend: 0 Teb: 7ef9f000 Unfrozen
ChildEBP RetAddr 
033afe28 76ee1ad0 ntdll!NtWaitForWorkViaWorkerFactory+0x12
033aff88 764b338a ntdll!TppWorkerThread+0x216
033aff94 76ecbf32 kernel32!BaseThreadInitThunk+0xe
033affd4 76ecbf05 ntdll!__RtlUserThreadStart+0x70
033affec 00000000 ntdll!_RtlUserThreadStart+0x1b

   4  Id: 1894.2034 Suspend: 0 Teb: 7ef9c000 Unfrozen
ChildEBP RetAddr 
04d1fdf4 76ecc6c5 ntdll!NtWaitForMultipleObjects+0x15
04d1ff88 764b338a ntdll!TppWaiterpThread+0x33d
04d1ff94 76ecbf32 kernel32!BaseThreadInitThunk+0xe
04d1ffd4 76ecbf05 ntdll!__RtlUserThreadStart+0x70
04d1ffec 00000000 ntdll!_RtlUserThreadStart+0x1b
~~~

共有5个线程，只有0号线程在干活。看0号线程的完整调用栈：

~~~
0:000> k
ChildEBP RetAddr 
00137980 74c314ab ntdll!NtWaitForSingleObject+0x15
001379ec 764b1194 KERNELBASE!WaitForSingleObjectEx+0x98
00137a04 764b1148 kernel32!WaitForSingleObjectExImplementation+0x75
00137a18 74a5098c kernel32!WaitForSingleObject+0x12
00137a28 74a4fcdc msctf!CCicEvent::Wait+0x15
00137ca4 74a4fbd9 msctf!CAssemblyList::LoadFromCache+0x94
00137cd0 74a4e045 msctf!CAssemblyList::Load+0x39
00137d00 74a4e863 msctf!EnsureAssemblyList+0xe9
00137d28 74a4e30a msctf!CLangBarItemMgr::GetCurrentCategoryList+0x14
00137d44 74a4e67d msctf!CLangBarItemMgr::_Init+0xea
00137d58 74a7d0c4 msctf!CLangBarItemMgr::CreateInstance+0xea
00137d78 74a57f99 msctf!CLangBarItemMgr_Ole::CreateInstance+0x6e
00137d8c 76038ca6 msctf!CClassFactory::CreateInstance+0x14
00137e14 76053170 ole32!CServerContextActivator::CreateInstance+0x172 [d:\w7rtm\com\ole32\com\objact\actvator.cxx @ 1000]
00137e54 76038dca ole32!ActivationPropertiesIn::DelegateCreateInstance+0x108 [d:\w7rtm\com\ole32\actprops\actprops.cxx @ 1917]
00137ea8 76038d3f ole32!CApartmentActivator::CreateInstance+0x112 [d:\w7rtm\com\ole32\com\objact\actvator.cxx @ 2268]
00137ec8 76038ac2 ole32!CProcessActivator::CCICallback+0x6d [d:\w7rtm\com\ole32\com\objact\actvator.cxx @ 1737]
00137ee8 76038a73 ole32!CProcessActivator::AttemptActivation+0x2c [d:\w7rtm\com\ole32\com\objact\actvator.cxx @ 1630]
00137f24 76038e2d ole32!CProcessActivator::ActivateByContext+0x4f [d:\w7rtm\com\ole32\com\objact\actvator.cxx @ 1487]
00137f4c 76053170 ole32!CProcessActivator::CreateInstance+0x49 [d:\w7rtm\com\ole32\com\objact\actvator.cxx @ 1377]
00137f8c 76052ef4 ole32!ActivationPropertiesIn::DelegateCreateInstance+0x108 [d:\w7rtm\com\ole32\actprops\actprops.cxx @ 1917]
001381ec 76053170 ole32!CClientContextActivator::CreateInstance+0xb0 [d:\w7rtm\com\ole32\com\objact\actvator.cxx @ 685]
0013822c 76053098 ole32!ActivationPropertiesIn::DelegateCreateInstance+0x108 [d:\w7rtm\com\ole32\actprops\actprops.cxx @ 1917]
001389fc 76059e25 ole32!ICoCreateInstanceEx+0x404 [d:\w7rtm\com\ole32\com\objact\objact.cxx @ 1334]
00138a5c 76059d86 ole32!CComActivator::DoCreateInstance+0xd9 [d:\w7rtm\com\ole32\com\objact\immact.hxx @ 343]
00138a80 76059d3f ole32!CoCreateInstanceEx+0x38 [d:\w7rtm\com\ole32\com\objact\actapi.cxx @ 157]
Unable to load image C:\Windows\System32\kunlun.ime, Win32 error 0n2
*** WARNING: Unable to verify timestamp for kunlun.ime
*** ERROR: Module load completed but symbols could not be loaded for kunlun.ime
00138ab0 71a86801 ole32!CoCreateInstance+0x37 [d:\w7rtm\com\ole32\com\objact\actapi.cxx @ 110]
WARNING: Stack unwind information not available. Following frames may be wrong.
00138ad4 71a86722 kunlun+0xf6801
00138b0c 71a8648c kunlun+0xf6722
00138bf0 71a4fb9e kunlun+0xf648c
00138c58 71a51e63 kunlun+0xbfb9e
00138c8c 71a51efc kunlun+0xc1e63
00138cc4 71a51b90 kunlun+0xc1efc
00138d34 71a4fc81 kunlun+0xc1b90
00138d74 762d459a kunlun+0xbfc81
00138d90 762d2900 imm32!CreateInputContext+0x195
00138db8 762d1e8c imm32!InternalImmLockIMC+0xca
00138dc8 762d34ae imm32!ImmLockIMC+0xf
00138dec 750cd9e7 imm32!ImmSetActiveContext+0x62
00138e08 750cadf9 user32!FocusSetIMCContext+0x28
0013905c 750c75b7 user32!ImeSystemHandler+0x31f
00139084 750c75ed user32!ImeWndProcWorker+0x2c9
001390a4 750c62fa user32!ImeWndProcW+0x29
001390d0 750c6d3a user32!InternalCallWinProc+0x23
00139148 750c6de8 user32!UserCallWinProcCheckWow+0x109
001391a4 750c6e44 user32!DispatchClientMessage+0xe0
001391e0 76ea010a user32!__fnDWORD+0x2b
001391f4 00fe7ae0 ntdll!KiUserCallbackDispatcher+0x2e
001392d8 750f10d3 0xfe7ae0
001392fc 750f1125 user32!CreateDialogIndirectParamAorW+0x33
*** ERROR: Module load completed but symbols could not be loaded for notepad++.exe
00139328 00516a73 user32!CreateDialogParamW+0x49
0013934c 750caa3c notepad__+0x116a73
00139400 750c8a5c user32!_CreateWindowEx+0x210
0013943c 0041ee1c user32!CreateWindowExW+0x33
0013a7cc 00584fd4 notepad__+0x1ee1c
0013a7e4 00584fa0 notepad__+0x184fd4
0013a7e8 00400000 notepad__+0x184fa0
0013a7ec 00040d60 notepad__
0013a7f0 00110d3c 0x40d60
0013a7f4 00000000 0x110d3c
~~~

最后的操作在msctf中等待某个事件，msctf 是 Microsoft Text Service；回溯调用栈，看见imm32（Input Method Manager）相关的调用，看来这个无响应的现象与输入法有关系。而 stack 中出现的 kunlun.ime 是必应输入法。

试试把正在使用的必应输入法卸载后重启，系统就正常了。重新安装必应输入法，重启后系统又出现无响应现象。

但比应输入法已经使用了一段时间了，为什么开始没有出现问题呢？回忆了一下，大概是在安装了 [Synergy](http://synergy-project.org/) 之后出现的，Synergy共享鼠标键盘，也许有可能与输入法产生冲突。尝试一下，卸载synergy后，即使安装必应，系统也正常。看来可以确认是两者冲突，试了 Synergy 1.4.16/17/18 几个64bit版本都不能与 Bing IME 1.6.98.04 工作。

Ok，对我来说问题解决了，进一步的不再深究。这次学会了使用 WinDbg 查看调用栈的简单操作。
