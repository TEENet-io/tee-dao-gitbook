# TEE DAO 系统文档

<div align="right">
  <a href="README.md" style="display: inline-block; padding: 8px 16px; background-color: #007bff; color: white; text-decoration: none; border-radius: 4px; font-weight: bold;">
    🇺🇸 English
  </a>
</div>

欢迎使用 TEE DAO 分布式密钥管理和应用部署平台的文档。

## 文档结构

本文档包含四个主要部分：

### 📖 使用指南

#### 1. [系统使用指南](system-usage-guide-zh.md)
- **分步说明**：从用户登录到应用部署的完整流程
- **截图演示**：10 个详细的界面操作截图
- **功能介绍**：各个功能模块的具体使用方法
- **最佳实践**：安全使用建议和故障排除方法

### 📚 系统组件文档

#### 2. [TEE DAO 私钥分片系统](tee-dao-key-management-system-zh.md)
- **核心功能**：分布式密钥生成、阈值签名、密钥重分享
- **技术特性**：FROST 和 ECDSA GG20 协议、Raft 共识、TEE 环境
- **架构设计**：节点管理、任务协调、性能监控、自动化运维
- **安全模型**：阈值密码学、双向 TLS、拜占庭容错

#### 3. [TEE DAO 密钥管理客户端](tee-dao-key-management-client-zh.md)
- **多语言支持**：Go 和 TypeScript 实现
- **API 接口**：统一的签名和验证接口
- **安全通信**：双向 TLS 认证、证书管理
- **开发工具**：签名工具、示例代码、CI/CD 集成

#### 4. [用户管理系统](user-management-system-zh.md)
- **用户认证**：无密码登录、Google OAuth2、账户合并
- **项目管理**：应用部署、容器化执行、服务配置
- **TEE 集成**：分布式密钥调用、签名服务集成

## 核心特性

### 🔐 安全性
- **分布式密钥**：私钥分片存储，无单点故障
- **阈值签名**：需要多节点协作完成签名
- **TEE 环境**：可信执行环境保护关键操作
- **TLS 加密**：端到端通信加密

### 🚀 高可用性
- **Raft 共识**：分布式决策和故障恢复
- **自动重连**：网络故障自动恢复
- **负载均衡**：智能任务分发
- **性能监控**：实时性能监控和自动优化

### 🛠️ 易用性
- **Web 界面**：直观的图形用户界面
- **多语言 SDK**：Go 和 TypeScript 客户端
- **完整示例**：丰富的示例代码和工具
- **详细文档**：完整的使用说明和 API 文档

### 📊 可扩展性
- **微服务架构**：组件解耦，独立扩展
- **容器化部署**：Docker 容器化支持
- **多协议支持**：ECDSA 和 Schnorr 协议
- **多曲线支持**：多种椭圆曲线选择

## 快速开始
### 使用客户端 SDK
```go
// Go 示例
import client "github.com/TEENet-io/tee-dao-key-management-client/go"

client := client.NewClient("localhost:50052")
client.Init()
signature, err := client.SignWithAppID(message, appID)
```

```typescript
// TypeScript 示例
import { Client } from './src/client';

const client = new Client('localhost:50052');
await client.init();
const signature = await client.signWithAppId(message, appId);
```

## 图片资源

`pic/` 目录包含系统使用的截图资源：

| 文件名 | 描述 | 用途 |
|--------|------|------|
| `01-login-page.png` | 登录页面 | 展示邮箱验证码和 Google OAuth 登录 |
| `02-dashboard.png` | 系统仪表板 | 显示项目统计和节点监控 |
| `03-project-management.png` | 项目管理 | 项目列表和管理功能 |
| `04-create-project.png` | 创建项目 | 新建项目表单界面 |
| `05-deploy-project.png` | 部署项目 | 应用部署配置界面 |
| `06-projects-list.png` | 项目列表 | 部署后的项目状态 |
| `07-signature-tool.png` | 签名工具 | Web 签名工具界面 |
| `08-signature-verification.png` | 签名验证 | 签名和验证结果显示 |
| `09-public-key-management.png` | 公钥管理 | 公钥列表和管理功能 |
| `10-generate-key.png` | 生成密钥 | 密钥生成配置界面 |

## 技术支持

### 开发资源
- **源代码**：查看各组件的源代码实现
- **API 文档**：详细的接口说明和参数定义
- **示例代码**：完整的使用示例和最佳实践
- **测试套件**：全面的单元测试和集成测试

### 社区支持
- **GitHub 仓库**：提交问题和功能请求
- **开发者论坛**：技术讨论和经验分享
- **技术博客**：深度技术文章和案例研究
- **在线会议**：定期技术分享和答疑会议

### 商业支持
- **技术咨询**：专业的架构设计和实施指导
- **定制开发**：基于需求的定制化开发服务
- **运维支持**：7×24 系统运维和监控服务
- **培训服务**：技术培训和认证服务

## 版本信息

- **当前版本**：v1.0.0
- **发布日期**：2025-08-04
- **更新频率**：定期更新和维护
- **兼容性**：向后兼容性保证

## 许可证

本项目归 TEENet Technology (Hong Kong) Limited 所有。
- **私钥分片系统**：专有许可证
- **用户管理系统**：MIT 许可证
- **客户端 SDK**：TEENet 生态系统许可证

---

*感谢您使用 TEE DAO 系统！如果您有任何问题或建议，请联系我们的技术支持团队。*