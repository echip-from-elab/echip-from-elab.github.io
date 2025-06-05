---
layout: post
read_time: true
show_date: true
title: "Paving the way from the P<sup>2</sup>M sensing paradigm to heterogeneous SoC design"
date: 2025/05/06
img: posts/20210420/frontpage.jpg
tags: [copyright, creativity, neural networks, machine learning, artificial intelligence]
category: opinion
author: Miao Sun
description: "Paving the way from the P<sup>2</sup>M sensing paradigm to heterogeneous SoC design"
---
In recent decades, smart sensing technology has evolved considerably to meet performance demands and address resource limitations in edge devices. This evolution has shifted from traditional separate Sensor-NPU-CPU architectures [1-2] to in-sensor processing [3]. However, data movement between the sensor and peripheral logic continues to incur substantial energy and bandwidth costs. With the increasing deployment of AI on edge devices, lightweight neural networks for tasks like object classification, eye tracking, and hand gesture recognition are becoming viable for intelligent devices. For energy-sensitive applications such as smart rings and wearable glasses, an energy-efficient sensing system is crucial.
Therefore, the research group from UW-Madison ECE led by [Assistant Prof. Akhilesh Jaiswal](https://directory.engr.wisc.edu/ece/Faculty/Jaiswal_Akhilesh/) proposed the P2M paradigm, which enables processing-in-pixel-in-memory [4]. In collaboration with [Prof. Umit Yusuf Ogras](https://elab.ece.wisc.edu/staff/ogras-umit/), an integral SoC imaging system was designed and verified. This system features a P2M-based sensing front-end and minimizes resource overhead for completing the entire neural network using the small data output from the P2M sensor array. The design was sign-off verified in GF 22nm technology on May 1st. This approach enables the initial layers of sensing and computation to be completed directly within the sensor array, eliminating the need to transfer data to off-sensor boundary logic. The trained weights for these first few network layers are mapped to specific transistor sizes within the pixel array. This design choice does not compromise the feasibility of downstream applications because the initial layers of modern CNNs typically function as high-level feature extractors, which are common across numerous vision tasks. As Fig. 1 depicts, relocating a portion of the neural network computation to the sensor array effectively reduces the area and power overhead associated with a standalone NPU, thereby easing NPU requirements.
<br>
<div align="center">
  <img src="https://github.com/echip-from-elab/echip-from-elab.github.io/blob/4b43623669c85fceff6b7f999af56577153d213c/assets/img/posts/20210420/p2m_fig1.png?raw=true" width="400" height="150">
</div>
<center> Figure 1. Evolutionary Roadmap of Advanced Sensing Technologies. </center>
<br>

The P<sup>2</sup>M pixel array significantly advances deep learning hardware by embedding critical Convolutional Neural Network (CNN) functionalities directly within each pixel's architecture. As detailed in Fig. 2, each pixel is meticulously designed to support analog multi-channel, multi-bit convolution, along with crucial operations like batch normalization and ReLU. During a convolution operation, only one weight transistor (Wi) within a pixel is activated at any given time. This transistor corresponds to a specific input channel and, through its driving strength, effectively represents the multi-bit weight for that channel. The pixel's inherent photosensitivity means that the input activation is provided by the photodiode current. This current is then analogously modulated by the activated Wi transistor, performing the essential multiplication step of the convolution. The output of this analog computation is then manifested as a voltage on the bit lines, which directly represents the convolution result. To achieve high throughput, the system can concurrently activate a large number of pixels, specifically X×Y×3 pixels. Here, X and Y represent the spatial dimensions of the input, and '3' refers to the RGB (red, green, blue) color channels, enabling efficient parallel processing of image data.
<br>
<div align="center">
  <img src="https://github.com/echip-from-elab/echip-from-elab.github.io/blob/4b43623669c85fceff6b7f999af56577153d213c/assets/img/posts/20210420/p2m_arch.png?raw=true" width="380" height="200">
</div>
<center> Figure 2. Circuit techniques for P<sup>2</sup>M based sensing architecture. </center>
<br>

Fig. 3 illustrates the complete algorithm flow and module mapping for a P<sup>2</sup>M based imaging system. Beyond the P<sup>2</sup>M block, the inferencing model incorporates a near-sensor processing (NSP) module for the remaining neural network layers and an MCU equipped with robust co-processing communication and interrupt capabilities. This heterogeneous integration of P<sup>2</sup>M and PIS enables direct processing of raw sensor data into an extracted latent feature map, ready for downstream tasks. By verificating on the VWW [5] dataset, the P<sup>2</sup>M-enabled custom model significantly reduces computational demands. It cuts Multiply-Accumulate operations (MAdds) by approximately 7.15$\times$ and peak memory usage by about 25.1$\times$ for 560 × 560 image resolution. This comes with a minimal 1.47% drop in test accuracy compared to the uncompressed baseline model. This substantial memory reduction allows our P<sup>2</sup>M model to even run on tiny microcontrollers with just 270KB of on-chip SRAM. While both baseline and custom model accuracies decrease with reduced image resolution (with a more notable drop for the custom model), this highlights the importance of high-resolution images for optimal performance.
<br>
<div align="center">
  <img src="https://github.com/echip-from-elab/echip-from-elab.github.io/blob/main/assets/img/posts/20210420/dataflow.png?raw=true" width="320" height="140">
</div>
<center> Figure 3. The proposed SoC diagram for sensing-to-computation integration. </center>
<br>

Comparing standard models with P<sup>2</sup>M-implemented counterparts reveals significant energy efficiencies. Specifically, P<sup>2</sup>M can reduce energy consumption by up to 7.81$\times$. This energy saving becomes even more pronounced when feature maps would otherwise need to be transferred from an edge device to the cloud for further processing, largely due to high communication overheads. Based on simulation verification from reference [4], a full silicon design has been launched in GF22nm technology. This design incorporates separate components, including a calibration module, comparator, and DAC and ADC arrays with linear slope characteristics. In parallel, a SoC for future integration, managing the remaining neural network layers outside the P<sup>2</sup>M block with minimal memory overhead, has also been implemented within the same GF 22nm shuttle. This push toward heterogeneous integration, particularly for energy and resource-constrained scenarios, aims to realize a truly intelligent edge-computing paradigm, evolving from novel sensing architectures to advanced AI-driven devices.
<br>
<div align="center">
  <img src="https://github.com/echip-from-elab/echip-from-elab.github.io/blob/main/assets/img/posts/20210420/p2m_layout.png?raw=true" width="360" height="160">
</div>
<center> Figure 4. Layout design of 3x3 P<sup>2</sup>M sensor array and integrated SoC. </center>
<br>
Reference<br>
[1] Pinkham, R., Berkovich, A. & Zhang, Z. Near-sensor distributed dnn processing for augmented and virtual reality. IEEE J. Emerg. Sel. Top. Circuits Syst. 11, 663–676. https://doi.org/10.1109/JETCAS.2021.3121259 (2021).
[2] Chen, Z. et al. Processing near sensor architecture in mixed-signal domain with CMOS image sensor of convolutional-kernelreadout method. IEEE Trans. Circuits Syst. I Regul. Pap. 67, 389–400 (2020).
[3] Song, R., Huang, K., Wang, Z. & Shen, H. A reconfgurable convolution-in-pixel cmos image sensor architecture. IEEE Trans. Circuits Syst. Video Technol.https://doi.org/10.1109/TCSVT.2022.3179370 (2022).
[4] Datta G, Kundu S, Yin Z, et al. A processing-in-pixel-in-memory paradigm for resource-constrained tinyml applications. Scientific Reports J, 2022, 12(1): 14396.
[5] Han, S., Lin, J., Wang, K., Wang, T. & Wu, Z. Solution to Visual Wakeup Words Challenge'19 (First Place). https://github.com/mithan-lab/VWW (2019).