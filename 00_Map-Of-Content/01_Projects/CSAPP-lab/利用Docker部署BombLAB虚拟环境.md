---
tags:
  - Tools/DevOps/Docker
  - Knowledge/computer-science/CSAPP
created: 2025-11-07
author:
  - ln1
status: In Progress
---

# 利用 Docker 部署 CSAPP BombLab 虚拟环境

## 为什么需要 Docker？

在 Apple M4 Pro (ARM64 架构) 上，BombLab 的 x86-64 二进制文件无法直接运行。Docker 提供了一个轻量级的解决方案：

- **架构模拟**：通过 QEMU，Docker 可以在 ARM 芯片上运行 x86-64 容器
- **环境隔离**：不会污染你的主机系统
- **可复现性**：一次配置，随处运行
- **微服务思维**：每个实验环境都是独立的"服务"

## Docker 核心概念速览

### 1. 镜像 (Image) vs 容器 (Container)
```
镜像 = 类 (Class)          → 只读模板，包含运行环境
容器 = 对象 (Instance)     → 镜像的运行实例，可读写
```

### 2. Dockerfile
定义如何构建镜像的"配方"文件，类似于安装脚本。

### 3. 微服务视角
- 传统方式：一个大型虚拟机运行所有服务
- Docker 方式：每个服务（如 BombLab）运行在独立容器中
- 优势：轻量、快速启动、易于管理

---

## 实战：部署 BombLab 环境

### 步骤 1：安装 Docker Desktop

如果还没安装，从官网下载 Mac 版本：
```bash
# 或使用 Homebrew 安装
brew install --cask docker
```

启动 Docker Desktop 后，确认安装成功：
```bash
docker --version
docker info
```

### 步骤 2：创建项目目录

```bash
mkdir -p ~/csapp-bomblab
cd ~/csapp-bomblab
```

### 步骤 3：创建 Dockerfile

创建一个 `Dockerfile` 文件，定义 BombLab 运行环境：

```dockerfile
# 使用 Ubuntu 20.04 x86-64 基础镜像
FROM --platform=linux/amd64 ubuntu:20.04

# 避免交互式提示
ENV DEBIAN_FRONTEND=noninteractive

# 安装必要的工具
RUN apt-get update && apt-get install -y \
    gcc \
    gdb \
    make \
    vim \
    objdump \
    binutils \
    file \
    && rm -rf /var/lib/apt/lists/*

# 创建工作目录
WORKDIR /bomblab

# 设置默认命令
CMD ["/bin/bash"]
```

### 步骤 4：构建镜像

```bash
# 构建镜像，命名为 bomblab
docker build --platform linux/amd64 -t bomblab .
```

这个过程会：
1. 下载 Ubuntu 20.04 x86-64 镜像
2. 安装 GCC、GDB 等工具
3. 创建工作目录

### 步骤 5：下载 BombLab 文件

从 CMU 官网或你的课程网站下载 `bomb.tar`，解压到当前目录：
```bash
# 假设你已经下载了 bomb.tar
tar -xvf bomb.tar
# 现在应该有一个 bomb 目录
```

### 步骤 6：运行容器

```bash
# 运行容器，挂载当前目录到容器的 /bomblab
docker run --platform linux/amd64 -it \
  -v "$(pwd)":/bomblab \
  --name bomblab-container \
  bomblab
```

参数说明：
- `--platform linux/amd64`：强制使用 x86-64 架构
- `-it`：交互式终端
- `-v "$(pwd)":/bomblab`：将当前目录挂载到容器（文件共享）
- `--name bomblab-container`：给容器命名
- `bomblab`：使用的镜像名称

### 步骤 7：在容器内运行 BombLab

现在你已经进入容器的 bash 环境：

```bash
# 查看文件
ls -la

# 确认架构
uname -m  # 应该显示 x86_64

# 运行 bomb
./bomb

# 使用 GDB 调试
gdb bomb
```

### 步骤 8：常用 Docker 命令

```bash
# 退出容器（但不停止）
Ctrl + P, Ctrl + Q

# 重新进入运行中的容器
docker attach bomblab-container

# 停止容器
docker stop bomblab-container

# 启动已停止的容器
docker start bomblab-container

# 删除容器
docker rm bomblab-container

# 查看所有容器
docker ps -a

# 查看所有镜像
docker images
```

---

## 微服务思维的体现

### 传统虚拟机 vs Docker

| 特性 | 虚拟机 | Docker 容器 |
|------|--------|-------------|
| 启动时间 | 分钟级 | 秒级 |
| 资源占用 | GB 级内存 | MB 级内存 |
| 隔离级别 | 完全隔离（独立 OS） | 进程级隔离 |
| 适用场景 | 运行不同操作系统 | 运行不同应用环境 |

### BombLab 作为"微服务"

在这个场景中：
- **服务定义**：BombLab 实验环境
- **依赖管理**：Dockerfile 明确列出所有依赖（GCC、GDB 等）
- **环境一致性**：无论在谁的 M4 Pro 上运行，环境完全相同
- **可扩展性**：如果需要运行多个 bomb 实例，只需启动多个容器

---

## 进阶技巧

### 1. 使用 Docker Compose（可选）

如果你有多个实验需要管理，可以创建 `docker-compose.yml`：

```yaml
version: '3.8'
services:
  bomblab:
    build:
      context: .
      platforms:
        - linux/amd64
    image: bomblab
    container_name: bomblab-container
    volumes:
      - ./:/bomblab
    stdin_open: true
    tty: true
    command: /bin/bash
```

然后使用：
```bash
docker-compose up -d    # 启动
docker-compose exec bomblab bash  # 进入容器
docker-compose down     # 停止并删除
```

### 2. 保存你的工作

容器内的修改（除了挂载目录）在删除容器后会丢失。重要文件应该：
- 保存在挂载的 `/bomblab` 目录（对应主机的当前目录）
- 或使用 `docker commit` 保存容器状态为新镜像

### 3. 性能优化

M4 Pro 运行 x86-64 容器会有性能损失（QEMU 模拟）。如果需要更好的性能：
- 考虑使用云端 x86-64 服务器
- 或使用 ARM 原生工具链重新编译 bomb（如果有源码）

---

## 故障排除

### 问题 1：容器启动慢
- 原因：QEMU 模拟 x86-64 需要时间
- 解决：首次启动较慢，后续会快很多

### 问题 2：找不到 bomb 文件
- 检查挂载路径：`docker run` 时的 `-v` 参数是否正确
- 确认文件在主机的当前目录

### 问题 3：权限问题
- 容器内默认是 root 用户
- 如果需要匹配主机用户权限，添加：
  ```bash
  docker run --user $(id -u):$(id -g) ...
  ```

---

## 总结

通过这个实验，你学到了：

1. **Docker 基础**：镜像、容器、Dockerfile 的概念
2. **跨架构运行**：在 ARM 芯片上运行 x86-64 程序
3. **微服务思维**：将实验环境作为独立服务管理
4. **实用技能**：容器化开发环境的搭建

这种方法不仅适用于 BombLab，也适用于其他需要特定环境的实验（如 AttackLab、MallocLab 等）。

---

## 参考资源

- [Docker 官方文档](https://docs.docker.com/)
- [CSAPP 官方网站](http://csapp.cs.cmu.edu/)
- [Docker Hub - Ubuntu](https://hub.docker.com/_/ubuntu)

