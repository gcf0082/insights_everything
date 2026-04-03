# MiniStack 洞察报告

## 基本信息

- **洞察链接**：https://ministack.org/
- **项目名称**：MiniStack — Free Open-Source Local AWS Emulator
- **更新时间**：2026年4月1日

## 项目概述

MiniStack 是一个免费、开源的本地 AWS 云服务模拟器，旨在作为 LocalStack 的替代方案。由于 LocalStack 将核心服务迁移至付费计划，MiniStack 作为其免费替代品出现，提供 33 个 AWS 服务的本地模拟，且完全免费、无需账户、无需许可证密钥、无遥测数据。

## 核心特性

### 主要优势

- **33 个 AWS 服务**：涵盖 S3、SQS、SNSDynamoDB、Lambda、IAM、STS、Secrets Manager、CloudWatch Logs、SSM、EventBridge、Kinesis、CloudWatch Metrics、SES、Step Functions、API Gateway v1/v2、RDS、ElastiCache、ECS、Glue、Athena、Firehose、Route53、Cognito、EC2、EMR、EBS、EFS、ALB/ELBv2、ACM、SES v2、WAF v2
- **快速启动**：约 2 秒启动时间
- **低资源占用**：空闲时仅约 30MB 内存
- **轻量级 Docker 镜像**：150MB
- **真实基础设施**：RDS 运行真实 Postgres/MySQL 容器，ElastiCache 运行真实 Redis 容器，ECS 启动真实 Docker 容器
- **MIT 许可证**：永久免费，无功能限制

### 与 LocalStack 对比

| 特性 | LocalStack 免费版 | LocalStack 专业版 | MiniStack |
|------|-------------------|-------------------|-----------|
| 核心服务（S3、SQS 等） | 现已付费 | ✅ | ✅ 免费 |
| Lambda、IAM、SSM、EventBridge | 付费 | ✅ | ✅ 免费 |
| RDS（真实数据库容器） | ❌ | ✅ | ✅ 真实 Postgres/MySQL |
| ElastiCache（真实 Redis） | ❌ | ✅ | ✅ 真实 Redis |
| ECS（真实 Docker） | ❌ | ✅ | ✅ 真实 Docker |
| Athena（真实 SQL） | ❌ | ✅ | ✅ DuckDB（可选） |
| 启动时间 | ~15-30秒 | ~15-30秒 | ~2秒 |
| 空闲内存 | ~500MB | ~500MB | ~30MB |
| Docker 镜像 | ~1GB | ~1GB | 150MB |
| 许可证 | BSL（受限） | 专有 | MIT |
| 价格 | 现已付费 | $35+/月 | 免费 |

### 真实基础设施

MiniStack 与其他模拟器的关键区别在于其使用真实的基础设施而非模拟：

- **RDS**：通过 CreateDBInstance 启动真实的 Postgres 或 MySQL Docker 容器，可通过 psycopg2 等标准库连接
- **ElastiCache**：通过 CreateCacheCluster 启动真实的 Redis 容器，支持 redis-py 库的各种操作
- **ECS**：RunTask 通过 Docker Socket 拉取并启动真实的 Docker 容器
- **Athena**：通过可选的 DuckDB 执行真实 SQL 查询 S3 数据

## 技术细节

### 支持的协议

- REST/XML
- JSON+Query
- Query/XML
- REST/JSON
- CBOR

### 启动命令

```bash
docker run -p 4566:4566 nahuelnucera/ministack
```

### AWS CLI 使用示例

```bash
# 创建 S3 存储桶
aws --endpoint-url=http://localhost:4566 s3 mb s3://my-bucket

# 创建 RDS 数据库实例
aws --endpoint-url=http://localhost:4566 rds create-db-instance \
    --db-instance-identifier mydb \
    --engine postgres \
    --master-username admin \
    --master-user-password secret

# 创建 ElastiCache Redis 集群
aws --endpoint-url=http://localhost:4566 elasticache \
    create-cache-cluster \
    --cache-cluster-id my-redis \
    --engine redis
```

## 适用场景

- 本地开发和测试
- CI/CD 持续集成
- AWS 相关工具测试（boto3、AWS CLI、Terraform、CDK、Pulumi）
- 无需支付 LocalStack 专业版费用的团队

## 总结

MiniStack 是一个功能强大的 LocalStack 替代方案，提供 33 个 AWS 服务的本地模拟，具有快速启动、低资源占用、真实基础设施等优点。对于需要在本地进行 AWS 开发测试的开发者来说，是一个值得考虑的免费开源选择。
