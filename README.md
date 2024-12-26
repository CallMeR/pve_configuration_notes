# Proxmox VE 折腾手记

## 介绍
PVE 虚拟化平台的安装以及折腾手记。  

- PVE ISO 版本：8.3-1 (更新时间: 2024-11-21)

- 演示机：
    - CPU：N6005
    - 内存：16GB DDR4
    - 网卡：I226-V
    - 硬盘：500GB NVMe

- PVE 网络：
    - IPv4 网络
        - IP 地址：`172.16.1.254`
        - 子网掩码：`255.255.255.0` ( 即 `/24` )
        - 网关：`172.16.1.1`
        - DNS：`172.16.1.1`
    - IPv6 网络
        - 首选 `SLAAC` 自动配置
        - IPv6 ULA 网络使用 `fdac::/64` 作为演示

### 系列章节

0. [硬件 BIOS 配置](./00.硬件BIOS配置.md)
1. [PVE 系统安装](./01.PVE系统安装.md)
2. [PVE 初始化配置](./02.PVE初始化配置.md)
3. [PVE 系统调整](./03.PVE系统调整.md)
4. [PVE 创建模板虚拟机](./04.PVE创建模板虚拟机.md)
5. [PVE 制作虚拟机模板](./05.PVE制作虚拟机模板.md)
6. [PVE 制作 DNS 服务器](./06.PVE制作DNS服务器.md)
7. [PVE 制作 TS 服务器](./07.PVE制作TS服务器.md)
8. [PVE 自动备份虚拟机](./08.PVE自动备份虚拟机.md)

### 文章说明

1. 本系列文章涉及的部分参数需要手动调整来符合切实使用需求。
2. 随着 PVE 系统的迭代更新，截图中的内容和实际页面显示可能存在差异。
3. 如需引用，请注明本文出处。
