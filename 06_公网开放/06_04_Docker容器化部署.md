# Docker容器化部署

Docker是现代应用部署的核心技术。它让"一次构建，到处运行"成为可能，极大地简化了应用的部署和运维工作。本章将带你全面了解Docker，并掌握使用Docker部署应用的能力。

---

## 一、Docker是什么

### 1. 容器技术简介

在Docker出现之前，虚拟化技术主要依靠虚拟机（VM）。每个虚拟机都运行一个完整的操作系统，占用资源多、启动慢。

```
传统虚拟化架构：
┌─────────────┐  ┌─────────────┐  ┌─────────────┐
│   物理服务器  │  │   物理服务器  │  │   物理服务器  │
│ ┌───────────┐│  │ ┌───────────┐│  │ ┌───────────┐│
│ │ 虚拟机1   ││  │ │ 虚拟机2   ││  │ │ 虚拟机3   ││
│ │ OS + App  ││  │ │ OS + App  ││  │ │ OS + App  ││
│ └───────────┘│  │ └───────────┘│  │ └───────────┘│
└─────────────┘  └─────────────┘  └─────────────┘

容器化架构：
┌─────────────────────────────────────┐
│           物理服务器                  │
│  ┌────────────────────────────────┐ │
│  │         Docker引擎（内核共享）  │ │
│  │ ┌────────┐ ┌────────┐ ┌─────┐│ │
│  │ │容器1   │ │容器2   │ │容器3││ │
│  │ │App1    │ │App2    │ │App3 ││ │
│  │ └────────┘ └────────┘ └─────┘│ │
│  └────────────────────────────────┘ │
└─────────────────────────────────────┘
```

容器与虚拟机的区别：
| 特性 | 虚拟机 | 容器 |
|------|--------|------|
| 启动时间 | 几分钟 | 几秒钟 |
| 资源占用 | 高（完整OS） | 低（共享内核） |
| 隔离性 | 强 | 中等 |
| 便携性 | 较差 | 优秀 |

### 2. Docker的核心概念

**镜像（Image）**：
- 镜像是一个只读的模板，用来创建容器
- 类似于面向对象中的"类"概念
- 镜像可以分层构建，逐层叠加

**容器（Container）**：
- 容器是镜像的运行实例
- 类似于面向对象中的"对象"概念
- 容器是可写可读的
- 每个容器相互隔离

**仓库（Repository）**：
- 存储镜像的地方
- 官方仓库：Docker Hub（hub.docker.com）
- 私有仓库：企业或个人自建

### 3. 镜像与容器的关系

```
镜像（Image）─────create────────► 容器（Container）
  │                                    │
  │  只读模板                          │  运行中的实例
  │                                    │
  └─pull/push─────────────────────────┘
                    存储和分发
```

---

## 二、Docker的优势

### 1. 环境一致性

经典的"在我机器上能运行"（It works on my machine）问题：
- 开发环境：Windows + Python 3.10
- 测试环境：Ubuntu + Python 3.9
- 生产环境：CentOS + Python 3.8

使用Docker后，所有环境使用相同的镜像，消除了环境差异带来的问题。

### 2. 隔离性

每个容器独立运行，互不干扰：
- 不同的应用可以使用不同的Python版本
- 某个容器崩溃不影响其他容器
- 方便进行版本测试和回滚

### 3. 可移植性

Docker镜像可以在任何支持Docker的系统上运行：
- 从开发机到测试服务器
- 从测试服务器到生产服务器
- 从一个云服务商迁移到另一个

### 4. 资源控制

可以限制容器的资源使用：
- 内存上限
- CPU使用比例
- 网络带宽

---

## 三、Docker基本操作

### 1. 安装Docker

**Linux（Ubuntu）**：
```bash
# 安装依赖
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release

# 添加Docker官方GPG密钥
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# 安装Docker
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin

# 启动Docker
sudo systemctl start docker
sudo systemctl enable docker
```

**Windows/Mac**：
- 下载Docker Desktop：https://www.docker.com/products/docker-desktop
- 安装后启动即可

### 2. 常用命令

**镜像操作**：
```bash
# 查看镜像列表
docker images

# 拉取镜像
docker pull python:3.11

# 删除镜像
docker rmi python:3.11

# 构建镜像
docker build -t myapp:latest .
```

**容器操作**：
```bash
# 运行容器
docker run -d -p 8080:80 nginx

# 常用参数：
# -d: 后台运行
# -p: 端口映射（主机端口:容器端口）
# --name: 指定容器名称
# -v: 挂载卷
# -e: 设置环境变量

# 查看运行中的容器
docker ps

# 查看所有容器（包括已停止）
docker ps -a

# 停止容器
docker stop container_id

# 删除容器
docker rm container_id

# 查看容器日志
docker logs -f container_id

# 进入容器内部
docker exec -it container_id /bin/bash
```

### 3. 镜像加速

国内访问Docker Hub较慢，建议配置镜像加速：

**Linux**（编辑 `/etc/docker/daemon.json`）：
```json
{
  "registry-mirrors": [
    "https://docker.mirrors.ustc.edu.cn",
    "https://hub-mirror.c.163.com"
  ]
}
```

之后重启Docker：
```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

---

## 四、Docker Compose使用

### 1. Docker Compose简介

Docker Compose用于定义和运行多容器应用。通过一个YAML文件，可以一次性启动所有相关服务。

### 2. docker-compose.yml示例

```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://user:password@db:5432/mydb
      - REDIS_URL=redis://cache:6379
    depends_on:
      - db
      - cache
    volumes:
      - ./data:/app/data
    restart: always

  db:
    image: postgres:15
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: mydb
    volumes:
      - postgres_data:/var/lib/postgresql/data
    restart: always

  cache:
    image: redis:7-alpine
    restart: always

volumes:
  postgres_data:
```

### 3. 常用命令

```bash
# 启动所有服务
docker-compose up -d

# 停止所有服务
docker-compose down

# 查看服务状态
docker-compose ps

# 查看日志
docker-compose logs -f

# 重新构建并启动
docker-compose up -d --build

# 停止并删除数据卷
docker-compose down -v
```

---

## 五、Dockerfile编写

### 1. Dockerfile是什么

Dockerfile是一个文本文件，用于构建Docker镜像。它定义了从基础镜像到最终镜像的每一步操作。

### 2. Python应用Dockerfile示例

```dockerfile
# 指定基础镜像
FROM python:3.11-slim

# 设置工作目录
WORKDIR /app

# 复制依赖文件
COPY requirements.txt .

# 安装依赖
RUN pip install --no-cache-dir -r requirements.txt

# 复制应用代码
COPY . .

# 暴露端口
EXPOSE 8000

# 启动命令
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### 3. Dockerfile最佳实践

**减少镜像体积**：
- 使用轻量级基础镜像（如python:3.11-slim）
- 多阶段构建
- 清理不必要的文件

**构建速度优化**：
- 将不经常变化的内容放在前面
- 利用Docker缓存

**安全建议**：
- 不使用root用户运行
- 最小化暴露的端口

### 4. 多阶段构建示例

```dockerfile
# 构建阶段
FROM python:3.11 AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --user -r requirements.txt

# 运行阶段
FROM python:3.11-slim
WORKDIR /app
COPY --from=builder /root/.local /root/.local
COPY . .
ENV PATH=/root/.local/bin:$PATH
EXPOSE 8000
CMD ["uvicorn", "main:app", "--host", "0.0.0.0"]
```

---

## 六、本章小结

本章我们系统学习了Docker容器化技术：

1. **Docker概念**：理解镜像、容器、仓库三大核心概念
2. **Docker优势**：环境一致性、隔离性、可移植性
3. **基本操作**：掌握镜像和容器的常用命令
4. **Docker Compose**：学会编排多容器应用
5. **Dockerfile**：能够编写镜像构建文件

Docker是现代部署的核心工具，掌握它将大大简化你的部署工作。下一章，我们将把Docker知识应用到实际部署中，完成服务器部署实战。

---

## 附录：常见问题

**Q：Docker和虚拟机有什么区别？**
A：虚拟机运行完整操作系统，资源占用大；容器共享内核，轻量快速。

**Q：容器中的数据如何持久化？**
A：使用数据卷（Volume）将容器内目录挂载到主机目录。

**Q：如何更新容器中的应用？**
A：修改代码后重新构建镜像，然后重新创建容器。

**Q：容器和宿主机之间如何通信？**
A：通过端口映射（-p参数）或Docker网络（推荐多容器应用使用）。

**Q：生产环境使用Docker需要注意什么？**
A：关注容器健康检查、日志管理、资源限制、备份策略等。
