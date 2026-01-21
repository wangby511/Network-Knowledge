## PCI-e

PCIe (Peripheral Component Interconnect Express) 外部设备快速互联总线

NVIDIA GPUDirect Technology, P2P (Peer to Peer) 

GPU点对点直连，数据从一个GPU的显存直接复制到另一个GPU的显存，不需要CPU利用主机内存并使用PCI-e总线中转。适合需要频繁进行GPU之间数据交换的AI深度学习任务。

但随着AI深度学习任务规模的不断扩大，GPU之间数据交换量也大幅度增长，PCI-e总线带宽逐渐成为数据交换的性能瓶颈。

### 与NVLINK的区别

SXM适合百亿，千亿级别的大模型训练和大数据分析，这些场景对于GPU之间的互联带宽要求较高。

PCI-e适合工作负载较小的场景，比如深度学习的小模型训练。

PCIe 是“通用公路”，连接各种硬件（硬盘、显卡、网卡）；而 NVLink 是“高速专线”，专门用于 NVIDIA GPU 之间的高性能数据交换。

<img src="image/nvswitch.png" width="800">

## NVLINK

### NVLINK 1.0

NVLink 1.0可以达到双向带宽160 GB/s

但全互联NVLink架构的横向扩展能力仍存在问题：1）每个GPU的NVLink通道有限，2）每对GPU之间只有一条数据通路，存在单点故障隐患。

2014 - SXM2 P100: 4 links * 20 Gbps * 2(双向) = 160 Gbps (GPU间双向带宽)

2014 - SXM2 V100: 每条链路的带宽翻倍，达到25 GB/s (单向)，每个V100 GPU拥有6个NVLink端口，使得GPU-GPU互联总带宽达到300 GB/s (双向)。

### NVLINK 3.0 (2020)

其核心是为 Ampere 架构（A100 GPU）提供跨节点扩展能力，对应专为**NVIDIA A100 Tensor Core GPU**。

12 NVlinks per GPU

单链路双向带宽: 50 GB/s. (每个链路（Link）的单向带宽提升至 25 GB/s（双向 50 GB/s）)

每GPU总双向带宽: 600 GB/s. (Total bidirectional bandwidth: 600 GB/s.)

主要载体：DGX A100， NVSwitch 2.0

**跨节点NVLINK** - NVLink 3.0 的突破：通过 NVLink Switch System（一种外部机架式交换机），可以将多个 DGX A100 服务器（通常是 4台或 8台）通过其 GPU 上的 NVLink 端口直接连接起来，形成一个小型的、延迟极低的 GPU 集群。

实现方式：每台 DGX A100 的 8 个 GPU 通过内部的 NVSwitch 2.0 芯片互联。每台服务器通过专用线缆，将 GPU 的 NVLink 端口连接到机架中的 NVLink Switch System 上。该交换机构建了一个 NVLink 网络域，允许不同服务器中的 GPU 直接进行高速 P2P 通信，延迟远低于通过 InfiniBand 网卡。

#### 载体

* NVSwitch 2.0 - NVSwitch 2.0 芯片：每颗 NVSwitch 2.0 芯片提供 64 个 NVLink 端口。
* DGX(Deep GPU Xceleration) A100 - 每个A100GPU拥有 12 个NVLink 3.0 端口。

在 NVIDIA DGX A100 系统中设计 6 颗 NVSwitch 2.0 芯片。每颗 GPU 向 每颗 NVSwitch 连接 2 条 ( = 12/6) NVLink 链路，任何一颗 GPU 都有 2/12（约 16.7%）的带宽直接连接到任意一颗 NVSwitch 上。NVSwitch 2.0中总共拥有 36 个端口，其中 8 * 2 = 16 个端口连接GPU。

Each NVIDIA A100 GPU in a DGX A100 system has 12 third-generation NVLinks. These 12 links provide a total of 600 GB/s bidirectional bandwidth per GPU. Those 12 links per GPU, combined with 6 NVSwitches, provide 600 GB/s per GPU (600 GB/s x 8 GPUs = 4.8 TB/s total system bandwidth).

Each A100 GPU uses twelve NVLink interconnects to communicate with all six NVSwitches, which means there are two links from each GPU to each switch[3]. These six second-generation NVSwitches enable high-speed, bidirectional connectivity between all eight NVIDIA A100 GPUs in the system, providing 4.8 TB/s of aggregate bandwidth.

优先保证每个 GPU 拥有极高的单节点带宽（600 GB/s），并为跨节点扩展预留物理通道，而非单纯追求单节点内的 GPU 数量。因此，节点内 GPU 数量从 16 降为 8，但每个 GPU 的可用带宽和扩展性更强。

## 来源

[1] [GPU互联革命史】PCIe到NVLink，再到NVSwitch的成长故事——看GPU如何推动AI与高性能计算！](https://www.bilibili.com/video/BV193BBYYEaG)

[2] [Unveiling the Evolution of NVIDIA NVLink Technology](https://network-switch.com/blogs/networking/the-evolution-of-nvidia-nvlink-technology)

[3] [NVIDIA DGX A100 System Architecture](https://www.skyblue.de/uploads/Datasheets/nvidia_twp_dgx_a100_system_architecture.pdf)
