---
layout: post
read_time: true
show_date: true
title: "First Mamba chiplet in GF 22nm"
date: 2025-05-29
img: posts/20210324/mamba_ssm.gif
tags: [state space model, low computation complexity, edge-computing]
author: Miao Sun
description: "Midlife career change: a disaster or an opportunity?"
---

The field of machine learning has seen a recent surge of interest in **State Space Model (SSM)**-based architectures for handling sequential data. For an extended period, **Transformer models** were the de facto standard in sequence modeling, lauded for their powerful representational abilities. However, the continuous expansion of model scales has highlighted a critical limitation: the Transformers' inherent quadratic complexity with respect to token length. This characteristic translates directly into considerable computational resource and energy consumption overhead, which has become a significant bottleneck for their continued development and deployment.

Amidst these challenges, **Mamba**, a newly introduced sequence-to-sequence SSM architecture, has demonstrated exceptional capabilities, surpassing existing model frameworks. It not only achieves comparable accuracy to Transformer models on diverse tasks but also offers a substantial leap in computational efficiency, leading to significant reductions in training and inference costs.

![complexity](.\assets\img\posts\20210324\complexity_table.png)
<center> Computational Complexity: $D$ is the number of states and $L$ is the sequence length. Transformers' complexity expands quadratically ($O(L^2)$) with the length of input tokens, while Mamba's complexity is linear ($O(L)$).</center>

Leveraging their high computational intensity and robust sequential modeling capabilities, the Mamba family of architectures has found widespread application, ranging from lightweight detection tasks to large language models (LLMs). Recently, Alab21 launched a Mamba-Transformer-based hybrid LLM model, which strategically combines the attention mechanism of the Transformer with the SSM sequential modeling of Mamba.

However, despite Mamba's algorithmic breakthroughs, fully realizing its potential and ensuring broad applicability demands dedicated hardware acceleration. In this context, the research team led by [Prof. Umit Yusuf Ogras](https://elab.ece.wisc.edu/staff/ogras-umit/) and [Assistant Prof. Akhilesh Jaiswal](https://directory.engr.wisc.edu/ece/Faculty/Jaiswal_Akhilesh/) has developed the first ASIC-targeted Mamba-Chiplet, incorporating an accelerator specifically designed for SSM in sequence model processing. Diverging from traditional accelerator designs for Transformer blocks and CNNs, this work implements a dedicated pipeline for hidden state updating and serial scanning. Furthermore, to efficiently process the non-linear activation functions and exponent calculations inherent to the Mamba block, a hardware-friendly approximation optimization method is proposed to address associated computational and area overheads.

The main contributions of this work are focused on:

- A scale-aware quantization strategy for the SSM layer, coupled with general 8-bit quantization for other layers, aiming to achieve a more balanced quantization approach.
- The design of a heterogeneous Mamba accelerator integrated with a RISC-V based MCU controller and a compute-in-memory (CIM) block, intended for a chiplet-based, scale-up high-performance computing architecture.
- A comparative analysis of the Mamba sequential block against CNN and Vision Transformer (ViT) implementations, presenting hardware measurements obtained on GF 22 nm technology.

![echip-V2](.\assets\img\posts\20210324\mamba_arch.svg)
<center>The designed architecture for MAMBA block.</center>{: .img-responsive width="900" height="500"}

When compared to **Transformer** and **CNN models** with similar modeling capacity, the proposed design exhibits **higher energy efficiency** and **lower area overhead**. A dedicated pipeline design implements the complex dataflow and scanning mechanism, which has been verified through front-end simulation. To achieve full ASIC verification and implementation, the designed accelerator successfully completed its first tape-out process in early May using the **GF 22nm technology node**. The core design measures **0.5913 mm × 0.5917 mm**. Our eMamba implementation operates at **100MHz** with a **0.8V** supply voltage, consuming a total power of **13.49mW**.
![echip-V2](.\assets\img\posts\20210324\layout.png){: .img-responsive width="300" height="300"}
<center>Layout of the e-chip-V1. </center>

In parallel, to lay the groundwork for a chiplet design within a scale-up roadmap incorporating heterogeneous Network-on-Chip (NoC) for an upcoming tape-out in November, a CIM based Mamba accelerator featuring a superscalar RISC-V core (designed to support high-computation-intensity instruction sets) is currently under development. To enable a truly efficient, high-performance solution for the Mamba-Transformer hybrid model, a scale-out domain-specific accelerator NoC is actively being pursued.

![echip-V2](.\assets\img\posts\20210324\echip_v2.svg)
<center>The conception design of e-chip-V2.</center>

