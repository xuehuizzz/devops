# DevOps 笔记与工具集

个人 DevOps / 运维相关知识整理仓库，包含容器、数据库、监控、网络、中间件、编程语言环境等常用配置、脚本与命令速查。

> 主要用于日常运维工作中的快速查阅与实践记录。

## 目录结构

```text
.
├── containers/          # 容器相关
│   ├── docker/           # Docker 安装、常用命令、网络、Swarm、Dockerfile 示例等
│   ├── k8s/              # Kubernetes 集群搭建（本地/生产）、资源创建、运维命令
│   └── podman/           # Podman 相关笔记
├── db/                  # 数据库
│   ├── arangodb/
│   ├── duckdb/
│   ├── mongodb/
│   ├── mysql/            # 安装脚本、备份恢复、配置、Percona Toolkit 等
│   ├── postgresql/
│   └── redis/            # 配置、哨兵机制
├── elk/                 # Elasticsearch + Logstash + Kibana 部署与使用笔记
├── monitoring/          # 监控相关
├── network/             # 网络相关
├── nginx/               # Nginx 配置与实践
├── proxy/               # 代理相关
├── linux/               # Linux 系统运维笔记
├── macos/               # macOS 相关
├── python/              # Python 环境与工具
├── go/                  # Go 开发环境与工具
├── java/                # Java / SDKMAN / Maven 等
├── nodejs/
├── rust/
├── logrotate.md         # 日志轮转
├── sonarqube.md         # SonarQube
├── vault.md             # HashiCorp Vault
└── .github/workflows/   # CI（Ruff lint、CodeQL）
```

## 主要内容概览

| 分类 | 说明 |
|------|------|
| **容器** | Docker 安装与常用命令、网络模式、Swarm、Dockerfile 示例；K8s 本地/生产集群搭建、常用运维命令；Podman |
| **数据库** | MySQL（安装、备份恢复、配置、工具）、PostgreSQL、Redis（含哨兵）、MongoDB、ArangoDB、DuckDB |
| **日志与搜索** | ELK 完整 Docker Compose 部署与使用笔记 |
| **Web / 代理** | Nginx、各类代理配置 |
| **系统与运维** | Linux 常用操作、logrotate、监控相关 |
| **开发环境** | Python / Go / Java / Node.js / Rust 环境搭建与工具链笔记 |
| **其他** | SonarQube、Vault 等 |

## 使用说明

- 本仓库以**笔记 + 脚本**为主，大部分内容可直接复制使用或作为参考。
- 部分脚本（如 MySQL 备份恢复、安装脚本）需要根据实际环境修改路径、密码等配置后再执行。
- 涉及生产环境的配置（K8s、数据库、ELK 等）请务必先在测试环境验证。

## 贡献与更新

这是个人维护的知识库，会根据实际使用持续补充和修正。

如有问题或建议，欢迎提 Issue。

---

**License**: 仅供学习与参考使用。
