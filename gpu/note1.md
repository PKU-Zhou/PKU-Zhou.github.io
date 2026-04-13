---
layout: default
title: GPU 笔记 1：CUDA 入门与执行模型
---

# GPU 笔记 1：CUDA 入门与执行模型

CUDA 程序通常由 Host 端和 Device 端组成，核心在于线程层次化调度。

## 关键概念
- Grid / Block / Thread 的三级并行结构。
- Kernel 通过 `<<<grid, block>>>` 启动。
- 全局内存、共享内存与寄存器的访问代价不同。

[← 返回 GPU 列表](/gpu/)
