---
title: gd32_in_vscode 
date: 2024-10-11 00:00:00
tags: embedded
---





使用 msys2 安装编译工具链

```bash
pacman -S base-devel
pacman -S mingw-w64-clang-x86_64-arm-none-eabi-toolchain
```



配置环境变量 env

```
driver:\msys64\usr\bin
driver:\msys64\clang64\bin
```



STM32CubeMX 生成 CMake 或者 MakeFile 项目

```
/STM32F407VETx_FLASH.ld:56: syntax error
[build] collect2.exe: error: ld returned 1 exit status
[build] ninja: build stopped: subcommand failed.
```



如果 make 出现以下问题时将以下保存为 `patch.patch` 文件并使用 `patch -p0 < patch.patch` 修改替换

```diff
--- ./STM32F407VETx_FLASH.ld	2024-10-11 20:11:30.799283500 +0800
+++ ./STM32F407VETx_FLASH.ld	2024-10-11 15:49:36.491106700 +0800
@@ -53,7 +53,7 @@
 ENTRY(Reset_Handler)
 
 /* Highest address of the user mode stack */
-_estack = ORIGIN() + LENGTH();    /* end of RAM */
+_estack = ORIGIN(RAM) + LENGTH(RAM);    /* end of RAM */
 /* Generate a link error if heap and stack don't fit into RAM */
 _Min_Heap_Size = 0x200;      /* required amount of heap  */
 _Min_Stack_Size = 0x400; /* required amount of stack */
@@ -144,7 +144,7 @@
 
     . = ALIGN(4);
     _edata = .;        /* define a global symbol at data end */
-  } > AT> FLASH
+  } >RAM AT> FLASH
 
   _siccmram = LOADADDR(.ccmram);
 
@@ -180,7 +180,7 @@
     . = ALIGN(4);
     _ebss = .;         /* define a global symbol at bss end */
     __bss_end__ = _ebss;
-  } >
+  } > RAM
 
   /* User_heap_stack section, used to check that there is enough RAM left */
   ._user_heap_stack :
@@ -191,7 +191,7 @@
     . = . + _Min_Heap_Size;
     . = . + _Min_Stack_Size;
     . = ALIGN(8);
-  } >
+  } >RAM
```

问题来自: https://community.st.com/t5/stm32cubemx-mcus/f303-generated-ld-missing-ram-symbol-won-t-compile-includes-fix/td-p/722915



烧录使用 pyocd 进行烧录

建议使用 pipx 进行安装

```sh
pipx install pyocd

pyocd pack find gd32f407
pyocd pack install gd32f407

pyocd flash --target gd32f407ve .\filename.elf
```

