---
layout: post
title: "Reading Report - Meltdown"
date: 2021-12-07
tags: [operating system]
comments: false
author: keyinwu
---

[Meltdown: Reading Kernel Memory from User Space](https://meltdownattack.com/meltdown.pdf)

This paper introduces Meltdown which is a software-based attack that reads arbitrary kernel memory from an unprivileged user space program based on out-of-order execution and side channels on current processors. Firstly, the authors illustrate how Meltdown functions in detail, from out-of-order execution and side channels to the necessary building blocks for an attack to exploit out-of-order execution. Moreover, they point out that the severe information leakage caused by Meltdown is not dependent on any software vulnerabilities or the operating system. Therefore, the software-based KAISER countermeasure, which was originally designed for KASLR protection but also obstructs Meltdown, must be deployed on every operating system until Meltdown is fixed in hardware.

## Strengths

- The paper is well organized: it gives a clear outline of the whole paper and follows a well-structured flow and storyline, with the consistent focus on Meltdown.
- It provides enough context before introducing the important notions like out-of-order execution, side channels, and building blocks. It also uses simple but valid examples to illustrate notions such as out-of-order execution.
- The paper delivers essential takeaway points from previous research works:
  - out-of-order execution can change the microarchitectural state in a way that leaks information
  - there are two building blocks making up the full Meltdown attack: the first is to make the CPU execute one or more instructions that would never occur in the executed path; the second is to transfer the microarchitectural side effect of the transient instruction sequence to an architectural state to further process the leaked secret
  - Meltdown can successfully leak kernel memory on different operating systems without the patches introducing the KAISER mechanism
  - there are two countermeasures against Meltdown attack: 1) microcode updates and general changes in the hardware design;  2) the KAISER countermeasure for software workarounds

## Weaknesses

- The paper doesn’t have enough discussion about the countermeasures against Meltdown attack on the software side. It only evaluates KAISER, which makes the following proposal for mandatory KAISER deployment on every operating system not that convincing.
- The paper could have explained more details in some aspects that help illustrate Meltdown, for example:
  - the differences and similarities between Meltdown Attacks and Spectre Attacks
  - the detection of  Meltdown Attacks on different operating systems.
