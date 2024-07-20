---
title: CPU 的系统架构
date: 2022-05-18
author: LM
---

## 1.主流架构

目前市场上的 CPU 主要分为两大阵营，一个是 intel、AMD 为首的复杂指令集 CPU（ 如 X86 ），另一个是以 IBM、ARM 为首的精简指令集 CPU（ 如 ARM ）

## 2.X86

X86 又称 IA-32 是指美国 Intel 公司为其第一块 CPU i8086 专门开发的一种 32 位复杂指令集，主要应用于个人计算机、服务器的 CPU 设计中。

## 3.x86_64

在 64 位 CPU 系统中 Intel 首先设计出 IA-64，但由于其与 32 位指令集不兼容，因此渐渐被人们所淘汰。

但此时 AMD 公司基于 X86 设计出一个兼容 32 位的 64 位指令集，名字叫做 AMD64 。因此在后续的使用中，AMD64 逐渐被业界接受，成为了 x86 在 64 位平台的事实标准。

最后，在 AMD64 被大众接受之后，Intel 不得不兼容这个指令集。而 Intel 为了避嫌，将 AMD64 改为 x86_64，因此，AMD64 与 X86_64 本质上是同一个东西。

x86_64，x64，AMD64 基本上是同一个东西

## 4.ARM

ARM 架构，也称作进阶精简指令集机器（Advanced RISC Machine，更早称作：Acorn RISC Machine），是一个 32 位精简指令集（RISC）处理器架构，其广泛地使用在许多嵌入式系统设计。由于节能的特点，ARM 处理器非常适用于行动通讯领域，符合其主要设计目标为低耗电的特性。目前 ARM 主要市场是手机端 CPU 和 MCU，手机 CPU 市场，由高通骁龙系列、华为麒麟系列、苹果的 M1 系列以及三星猎户系列和联发科系列，在 MCU 端主要是 STM32 以及国产的 GD32。

## 5.AArch64（ARM64）

AArch64 不是一个单纯的 32 位 ARM 构架扩展，而是 ARMv8 内全新的构架，是ARMv8的一种执行状态，完全使用全新的 A64 指令集。AArch64 作为在 Fedora ARM 项目中被支持的 ARM 构架是一个很自然的过程： armv5tel、armv7hl、aarch64。



{{< details "参考文件" >}} 
1：[ CPU架构详细介绍_@MasterHu88 ](https://blog.csdn.net/qq_34160841/article/details/105744375)
{{< /details >}}