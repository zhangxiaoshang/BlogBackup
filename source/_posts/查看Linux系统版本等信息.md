---
title: 查看Linux系统版本等信息
date: 2018-01-04 22:37:34
tags: Linux
categories: log
toc: true
---

输出信息为当前测试环境信息,命令只保证在当前系统环境有效

## 1. 查看系统内核信息

```bash
uname -a
# Linux ip-172-31-22-34 4.4.0-1044-aws #53-Ubuntu SMP Mon Dec 11 13:49:57 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux
```

## 2. 查看系统版本信息

```bash
cat /proc/version
# Linux version 4.4.0-1044-aws (buildd@lgw01-amd64-013) (gcc version 5.4.0 20160609 (Ubuntu 5.4.0-6ubuntu1~16.04.5) ) #53-Ubuntu SMP Mon Dec 11 13:49:57 UTC 2017
```

## 3. 查看发行版信息

```bash
cat /etc/issue
# Ubuntu 16.04.3 LTS \n \l
lsb_release -a
# No LSB modules are available.
# Distributor ID: Ubuntu
# Description:    Ubuntu 16.04.3 LTS
# Release:    16.04
# Codename:   xenial
```

## 4. 查看CPU相关信息，包括型号、主频、内核信息等

```bash
cat /proc/cpuinfo

# processor   : 0
# vendor_id   : GenuineIntel
# cpu family  : 6
# model       : 63
# model name  : Intel(R) Xeon(R) CPU E5-2676 v3 @ 2.40GHz
# stepping    : 2
# microcode   : 0x3b
# cpu MHz     : 2400.060
# cache size  : 30720 KB
# physical id : 0
# siblings    : 1
# core id     : 0
# cpu cores   : 1
# apicid      : 0
# initial apicid  : 0
# fpu     : yes
# fpu_exception   : yes
# cpuid level : 13
# wp      : yes
# flags       : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology eagerfpu pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand # hypervisor lahf_lm abm fsgsbase bmi1 avx2 smep bmi2 erms invpcid xsaveopt
# bugs        :
# bogomips    : 4800.12
# clflush size    : 64
# cache_alignment : 64
# address sizes   : 46 bits physical, 48 bits virtual
# power management:
```

## 5. 查看当前CPU运行模式 32/64位

```bash
getconf LONG_BIT
# 64
```

