# 交换技术排错

## 介绍
交换技术（VLAN/GVRP/RSTP/单臂路由）故障排查实验记录，附带初始错误配置与最终修复配置。

本仓库基于华为 eNSP 仿真环境，针对给定的初始配置（SW1~SW4, R1）进行故障注入与修复。实验覆盖二层与三层交换的常见故障点，适合网络初学者进行排错思路训练。

## 软件架构（实验环境与目录结构）
本仓库主要由初始故障配置、修复参考配置及排障文档组成：

```text
jiaohuanjishu/
├── configs/           # 初始错误配置文件 (S1.cfg ~ S4.cfg, R1.cfg)
├── solutions/         # 修复后的最终配置文件
├── docs/              # 实验拓扑图、排障分析记录、验收清单
└── README.md          # 实验总览及排错指引

实验拓扑说明：
- 包含 4 台交换机（SW1~SW4）、1 台路由器（R1）及 3 台 PC。

- PC1 (VLAN 10) 和 PC2 (VLAN 10) 需实现同 VLAN 互通。

- PC3 (VLAN 20) 需通过 R1 单臂路由与 VLAN 10 实现互通。

安装教程（环境准备）
1. 安装华为 eNSP 仿真软件，并配置好 VirtualBox 及 Wireshark 抓包组件。

2. 参考 docs/LAB_TOPOLOGY.md 中的拓扑图，在 eNSP 中搭建好设备连线。

3. 将 configs/ 目录下的初始错误配置，分别导入到对应的 SW1~SW4 及 R1 设备中。

使用说明（排错实战）
1. 复现故障：

    PC1 和 PC2 无法互通（VLAN 10 内不通）。

    PC1 无法 Ping 通 PC3（VLAN 10 和 VLAN 20 无法通信）。

    4 台交换机 GVRP 无法同步 VLAN 信息。

    SW4 未被选举为 RSTP 的根桥。

2. 排查与修复：

    参考 docs/PITFALLS.md 中的排错思路，检查 SW1 的接口划分、GVRP 全局开启情况。

    检查 SW4 的 STP 禁用配置，修改 STP 优先级。

    检查 R1 子接口的 dot1q termination vid 是否正确匹配 VLAN。

3. 验证与验收：

    执行 display vlan 检查 GVRP 动态注册情况。

    执行 display stp brief 确认 SW4 为 ROOT 根桥。

    PC1 执行 ping 10.1.1.3 及 ping 20.1.1.3 测试连通性。


说明与避坑


避坑提示：在 eNSP 中，交换机接口配置 Trunk 后建议使用 display port vlan 确认放通列表。GVRP 依赖底层 Trunk 链路的连通性，务必先确保 Trunk 放通了所需的 VLAN。