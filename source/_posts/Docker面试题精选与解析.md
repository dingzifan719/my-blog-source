---
title: Docker面试题精选与解析
date: 2025-01-15 10:00:00
tags: [Docker, 容器化, 面试, DevOps, 微服务]
---

## 1. Docker基础概念

### 1.1 什么是Docker？

**问题**：请解释什么是Docker，以及它解决了什么问题？

**解析**：

**Docker**是一个开源的容器化平台，它允许开发者将应用程序及其依赖打包到一个轻量级、可移植的容器中，从而实现快速部署和运行。

**Docker解决的核心问题**：

```
传统部署方式的问题
│
├── 环境不一致问题
│   ├── 开发环境：Windows/Mac + Python 3.8
│   ├── 测试环境：Linux + Python 3.7
│   └── 生产环境：Linux + Python 3.6
│   └── 结果："在我的机器上能运行"
│
├── 依赖冲突问题
│   ├── 项目A需要：Redis 5.0 + Node.js 14
│   ├── 项目B需要：Redis 6.0 + Node.js 16
│   └── 同一台服务器无法同时满足
│
├── 资源利用率低
│   ├── 每个应用都需要独立的虚拟机
│   ├── 每个虚拟机都需要完整的操作系统
│   └── 内存和CPU资源浪费严重
│
└── 部署复杂
    ├── 手动配置服务器环境
    ├── 手动安装依赖
    └── 容易出错且难以回滚

Docker的解决方案
│
├── 环境一致性
│   ├── 开发、测试、生产使用相同的容器
│   ├── 容器内包含所有依赖
│   └── 确保"一次构建，到处运行"
│
├── 隔离性
│   ├── 每个容器独立运行
│   ├── 容器间互不干扰
│   └── 同一主机可运行多个不同版本的服务
│
├── 轻量级
│   ├── 容器共享主机内核
│   ├── 无需完整的操作系统
│   └── 启动速度快（秒级）
│
└── 易于管理
    ├── 版本控制和回滚
    ├── 自动化部署
    └── 与CI/CD流程集成
```

### 1.2 容器 vs 虚拟机

**问题**：Docker容器和传统虚拟机有什么区别？

**解析**：

| 特性 | Docker容器 | 虚拟机（VM） |
|------|-----------|------------|
| **架构** | 共享主机操作系统内核 | 每个VM有独立的操作系统 |
| **启动时间** | 秒级（通常1-3秒） | 分钟级（通常1-5分钟） |
| **资源占用** | 轻量级（MB级） | 重量级（GB级） |
| **性能** | 接近原生（约95-100%） | 有虚拟化开销（约70-80%） |
| **隔离性** | 进程级隔离 | 硬件级隔离 |
| **密度** | 单主机可运行数百个容器 | 单主机通常运行10-50个VM |
| **管理复杂度** | 相对简单 | 相对复杂 |
| **适用场景** | 微服务、DevOps、CI/CD | 传统应用、多操作系统需求 |

**架构对比图**：

```
虚拟机架构（Virtual Machine Architecture）
┌─────────────────────────────────────────────────────────────┐
│                         基础设施                            │
│                   (服务器/云主机/PC)                          │
└─────────────────────────────────────────────────────────────┘
                              │
┌─────────────────────────────────────────────────────────────┐
│                      虚拟化层（Hypervisor）                  │
│              (VMware, Hyper-V, KVM, Xen)                     │
└─────────────────────────────────────────────────────────────┘
          │                     │                     │
    ┌──────────┐          ┌──────────┐          ┌──────────┐
    │   VM 1   │          │   VM 2   │          │   VM 3   │
    │┌────────┐│          │┌────────┐│          │┌────────┐│
    ││  应用  ││          ││  应用  ││          ││  应用  ││
    ││ Bin/Lib││          ││ Bin/Lib││          ││ Bin/Lib││
    │├────────┤│          │├────────┤│          │├────────┤│
    ││ Guest  ││          ││ Guest  ││          ││ Guest  ││
    ││   OS   ││          ││   OS   ││          ││   OS   ││
    ││(GB级)  ││          ││(GB级)  ││          ││(GB级)  ││
    │└────────┘│          │└────────┘│          │└────────┘│
    └──────────┘          └──────────┘          └──────────┘

Docker容器架构（Container Architecture）
┌─────────────────────────────────────────────────────────────┐
│                      基础设施                               │
│                (服务器/云主机/PC)                           │
└─────────────────────────────────────────────────────────────┘
                              │
┌─────────────────────────────────────────────────────────────┐
│                   主机操作系统                              │
│           (Linux, Windows, macOS)                          │
└─────────────────────────────────────────────────────────────┘
                              │
┌─────────────────────────────────────────────────────────────┐
│              Docker引擎（Docker Engine）                    │
└─────────────────────────────────────────────────────────────┘
          │                     │                     │
    ┌──────────┐          ┌──────────┐          ┌──────────┐
    │ Container│          │ Container│          │ Container│
    │    1     │          │    2     │          │    3     │
    │┌────────┐│          │┌────────┐│          │┌────────┐│
    ││  应用  ││          ││  应用  ││          ││  应用  ││
    ││ Bin/Lib││          ││ Bin/Lib││          ││ Bin/Lib││
    ││(MB级)  ││          ││(MB级)  ││          ││(MB级)  ││
    │└────────┘│          │└────────┘│          │└────────┘│
    └──────────┘          └──────────┘          └──────────┘
                              │
                    共享主机操作系统内核
```

## 2. Docker核心组件

### 2.1 Docker架构

**问题**：Docker的架构是怎样的？各个组件的作用是什么？

**解析**：

**Docker架构图**：

```
Docker整体架构
│
├── Docker客户端（Docker Client）
│   ├── 命令行接口（docker CLI）
│   ├── REST API客户端
│   └── 与Docker Daemon通信
│
├── Docker守护进程（Docker Daemon - dockerd）
│   ├── 核心功能：
│   │   ├── 镜像管理（构建、拉取、存储）
│   │   ├── 容器管理（创建、运行、监控）
│   │   ├── 网络管理（创建、连接、隔离）
│   │   ├── 存储管理（Volume、Bind Mount）
│   │   └── 资源管理（CPU、内存、IO限制）
│   │
│   ├── 模块组成：
│   │   ├── Image Service：镜像服务
│   │   ├── Container Service：容器服务
│   │   ├── Network Service：网络服务
│   │   ├── Volume Service：存储服务
│   │   └── Execution Driver：执行驱动（runc等）
│   │
│   └── 通信接口：
│       ├── REST API（/var/run/docker.sock）
│       └── gRPC（内部组件间通信）
│
├── Containerd（容器运行时管理）
│   ├── 从Docker Daemon中分离出的核心容器运行时
│   ├── 功能：
│   │   ├── 镜像管理（拉取、推送、存储）
│   │   ├── 容器生命周期管理
│   │   └── 快照管理（Snapshot）
│   ├── 架构：
│   │   ├── Daemon：containerd服务
│   │   ├── Client：ctr命令行工具
│   │   └── API：gRPC接口
│   └── 标准化：符合OCI（Open Container Initiative）标准
│
├── Runc（容器运行时）
│   ├── 底层的容器执行工具
│   ├── 功能：
│   │   ├── 创建容器
│   │   ├── 配置namespace和cgroup
│   │   └── 执行容器内的进程
│   ├── 特点：
│   │   ├── 轻量级
│   │   ├── 符合OCI Runtime Spec
│   │   └── 被containerd调用
│   └── 命令：runc create/start/exec/delete
│
├── Docker Registry（镜像仓库）
│   ├── 功能：存储和分发Docker镜像
│   ├── 类型：
│   │   ├── 公共仓库：Docker Hub、Google Container Registry
│   │   └── 私有仓库：Harbor、自建Registry
│   ├── 镜像命名：registry/repository:tag
│   │   └── 示例：docker.io/library/nginx:latest
│   └── 操作：pull/push/search/tag
│
└── Docker生态系统组件
    ├── Docker Compose：多容器编排
    ├── Docker Swarm：容器集群管理
    ├── Docker Machine：远程Docker主机管理
    ├── Docker Desktop：桌面版Docker（Windows/Mac）
    └── 第三方工具：Kubernetes、Portainer、Rancher

详细架构流程图
═══════════════════════════════════════════════════════════

用户层
┌─────────────────────────────────────────────────────────┐
│  User                                                   │
│  └── docker build/run/ps/exec/logs/stop/rm...        │
└─────────────────────────────────────────────────────────┘
                          │
                          ▼
Docker Client层
┌─────────────────────────────────────────────────────────┐
│  Docker CLI / REST API Client                          │
│  └── 解析命令 → 构建API请求 → 发送到Docker Daemon    │
└─────────────────────────────────────────────────────────┘
                          │
                          ▼
Docker Daemon层（dockerd）
┌─────────────────────────────────────────────────────────┐
│  Docker Engine API (REST/gRPC)                         │
│  └── 接收请求 → 路由到对应服务                        │
│                                                        │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │Image Service │  │Container Svc │  │Network Svc  │  │
│  │- Build       │  │- Create      │  │- Create     │  │
│  │- Pull/Push   │  │- Start/Stop  │  │- Connect    │  │
│  │- List/Remove │  │- Exec/Attach │  │- Disconnect │  │
│  └──────────────┘  └──────────────┘  └──────────────┘  │
│                                                        │
│  ┌──────────────┐  ┌──────────────┐                   │
│  │ Volume Svc   │  │  System Svc  │                   │
│  │ - Create     │  │  - Info      │                   │
│  │ - Mount      │  │  - Events    │                   │
│  │ - Remove     │  │  - Prune     │                   │
│  └──────────────┘  └──────────────┘                   │
└─────────────────────────────────────────────────────────┘
                          │
                          ▼
Containerd层（容器运行时管理）
┌─────────────────────────────────────────────────────────┐
│  Containerd Daemon                                     │
│  ├── GRPC API                                          │
│  ├── Image Service (pull/push/manage images)           │
│  ├── Content Service (content addressing)              │
│  ├── Snapshot Service (layer management)               │
│  ├── Runtime Service (container lifecycle)           │
│  └── Task Service (process management)                 │
└─────────────────────────────────────────────────────────┘
                          │
                          ▼
Runc层（OCI运行时）
┌─────────────────────────────────────────────────────────┐
│  RunC (OCI Runtime)                                    │
│  ├── 创建容器命名空间 (Namespaces)                     │
│  ├── 配置控制组 (cgroups)                               │
│  ├── 设置文件系统隔离                                   │
│  ├── 配置网络隔离                                       │
│  └── 执行容器进程                                       │
└─────────────────────────────────────────────────────────┘
                          │
                          ▼
操作系统内核层
┌─────────────────────────────────────────────────────────┐
│  Linux Kernel                                          │
│  ├── Namespaces (PID, Network, Mount, User, IPC, UTS)   │
│  ├── cgroups (CPU, Memory, IO, Network, PIDs)          │
│  ├── Union File Systems (Overlay2, AUFS, Btrfs)        │
│  ├── Network Bridges and iptables                      │
│  └── Security Modules (SELinux, AppArmor)              │
└─────────────────────────────────────────────────────────┘
═══════════════════════════════════════════════════════════
```

## 常见面试题

### docker容器有几种状态
四种状态，运行、已停止、重新启动、已推出

### dockerfile里面最常见的指令是什么
from：置顶基础镜像
label：为镜像添加元数据标签
run：运行指定命令
cmd：启动时要运行的命令

### dockerfile中copy和add的区别

copy的src只能是本地文件

### Docker 的核心实现技术是什么？
关键点： Namespace（命名空间，实现资源隔离，如 PID、Network、Mount 等）和 Cgroups（控制组，实现资源限制，如 CPU、内存限制）。

### Docker 与传统虚拟化（VM）的区别？

关键点： 虚拟机有完整的 OS，占用资源大；容器共享宿主机内核，轻量级，启动秒级。

### 什么是 Docker 镜像与容器的关系？

关键点： 镜像（Image）是静态的模板（只读）；容器（Container）是镜像运行后的实例（动态、可读写）。

### docker的常用命令
docker pull拉取或者更新指定的镜像
docker push 将镜像推送到远程仓库
docker rm 删除容器
docker rmi 删除镜像
docker images 列出所有镜像
docker ps列出所有容器

### 容器与主机之间的数据拷贝命令
docler cp命令用于容器与主机之间的数据拷贝
主机到容器：docker cp <主机路径> <容器ID>:<容器路径>
容器到主机：docker cp <容器ID>:<容器路径> <主机路径>

### 常用命令

1.  Docker 服务管理
启动docker：systemctl start docker
停止docker：systemctl stop docker
重启docker：systemctl restart docker
查看docker状态：systemctl status docker
设置docker开启自启动：systemctl enable docker

2. 系统与信息查看
查看docker概要信息：docker info
查看docker总体帮助文档：docker --help
查看docker中的run命令帮助文档：docker run --help
查看docker版本：docker version

3. 镜像管理 (Images)
查看本地主机上的docker所有镜像：docker images
查看本地主机上的docker的所有镜像，只显示镜像id：docker images -q
去配置的镜像网站寻找某个镜像：docker search [镜像名]
从远程库拉取镜像：docker pull [镜像名]
查看镜像所占空间：docker system df
删除本地单个镜像：docker rmi [镜像ID/标签]
强制删除本地单个镜像：docker rmi -f [镜像ID/标签]
删除多个镜像：docker rmi [镜像ID1] [镜像ID2] ...

4. 容器管理 (Containers)
新建启动容器：docker run [参数] [镜像名]
列出所有正在运行的容器：docker ps
列出所有容器（包括已停止的）：docker ps -a
删除容器：docker rm [容器ID/名称]
