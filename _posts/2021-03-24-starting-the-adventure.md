---
layout: post
read_time: true
show_date: true
title: "First chiplet for mamba-based nerual networks in GF 22nm technology"
date: 2025-05-29
img: posts/20210324/mamba_ssm.gif
tags: [state space model, low computation complexity, edge-computing]
author: Miao Sun
description: "Midlife career change: a disaster or an opportunity?"
---

State Space Model (SSM)-based machine learning architectures have recently gained significant attention for processing sequential data. For a long time, Transformer models dominated sequence modeling due to their powerful capabilities. However, as model scales continuously expand, the Transformers' inherent quadratic complexity leads to significant computational resource and energy consumption overhead, which has become a bottleneck for their further development.

Against this backdrop, Mamba, a recently introduced sequence-to-sequence SSM architecture, demonstrates superior capabilities beyond existing model frameworks. It achieves comparable accuracy to Transformer models on various tasks while significantly improving computational efficiency, substantially reducing training and inference costs.



Benifits from the high computation intensity and the sequential modeling ablity, the mamba family has been employed widely in multiple applications from the light-weight detection  tasks to LLMs. Recently, the mamba-transformer based hybrid LLM model has been lanched by Alab21, which combine the abilities of the attention mechinism from transformer model and the SSM sequencing modelling from MAMBA. However, while Mamba has made algorithmic breakthroughs, fully unleashing its potential and making it widely applicable requires hardware acceleration. Recently, the research team led by Prof. Umit Yusuf Ogras and Assistant Prof. Ahkilesh Jasiwal has designed the fisrt ASIC targered MAMBA-Chiplet with the accelerator for SSM in sequencing model processing. Differnet from the traditional accelertors design for transformer block and CNN, a dedicated pipeline design for hidden state updating and serial scanning is implemented. Meanwhile, to process the non-linear activation function and exponents calculation from the MAMBA block, a hardware frinedly approximation optimizaiton method is proposed for addressing the computation and area overhead. The main contributions in this works foucs on:
- a scale-aware quantization for the SSM layer coupled with general 8-bit
quantization for the other layers, aiming to achieve a more balanced quantization strategy. Unlike
- the existing two studies, we compare eMamba against CNN and ViT implementations and present
hardware measurements on GF 22 nm. 

![echip-V2](.\assets\img\posts\20210324\mamba_arch.png)
[^2]: The designed architecture for MAMBA block.