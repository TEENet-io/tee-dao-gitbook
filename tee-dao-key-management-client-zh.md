# TEE DAO 密钥管理客户端

TEE DAO 密钥管理客户端是一个多语言客户端库，用于 TEE DAO 密钥管理操作，通过 TEE（可信执行环境）节点和 AppID 服务集成提供简化的安全消息签名访问。

## 🚀 支持的语言

- **Go**
- **TypeScript**

## 特性

- **安全消息签名**：使用分布式密码学密钥签名消息
- **AppID 服务集成**：使用 AppID 获取公钥和签名消息
- **多协议支持**：支持 ECDSA 和 Schnorr 签名协议
- **多曲线支持**：支持 ED25519、SECP256K1、SECP256R1 曲线
- **TLS 安全**：使用双向 TLS 认证与 TEE 节点安全通信
- **简单 API**：易于使用的客户端接口，自动配置
- **多语言支持**：Go 和 TypeScript 实现，具有相同的 API

## Go 实现

### 安装

```bash
go get github.com/TEENet-io/tee-dao-key-management-client/go
```

### 快速开始

```go
package main

import (
    "fmt"
    "log"
    
    client "github.com/TEENet-io/tee-dao-key-management-client/go"
)

func main() {
    // 使用配置服务器地址创建客户端
    client := client.NewClient("localhost:50052")
    defer client.Close()

    // 初始化客户端（获取配置 + 建立 TLS 连接）
    if err := client.Init(); err != nil {
        log.Fatalf("初始化失败: %v", err)
    }

    fmt.Printf("客户端已连接，节点 ID: %d\n", client.GetNodeID())

    // 示例 1：通过 app ID 获取公钥
    appID := "xxxxxx"
    publicKey, protocol, curve, err := client.GetPublicKeyByAppID(appID)
    if err != nil {
        log.Printf("通过 app ID 获取公钥失败: %v", err)
    } else {
        fmt.Printf("App ID %s 的公钥:\n", appID)
        fmt.Printf("  - 协议: %s\n", protocol)
        fmt.Printf("  - 曲线: %s\n", curve)
        fmt.Printf("  - 公钥: %s\n", publicKey)
    }

    // 示例 2：使用 app ID 签名消息
    message := []byte("来自 AppID 服务的问候!")
    signature, err := client.SignWithAppID(message, appID)
    if err != nil {
        log.Printf("使用 app ID 签名失败: %v", err)
    } else {
        fmt.Printf("使用 app ID 签名成功!\n")
        fmt.Printf("消息: %s\n", string(message))
        fmt.Printf("签名: %x\n", signature)
    }

    // 示例 3：使用显式协议和曲线的传统签名
    publicKeyBytes := []byte("来自外部 DKG 服务的示例公钥") // 来自外部 DKG 服务
    message2 := []byte("你好，TEE DAO!")
    
    signature2, err := client.Sign(message2, publicKeyBytes, 1, 1) // ECDSA, ED25519
    if err != nil {
        log.Fatalf("签名失败: %v", err)
    }
    fmt.Printf("传统签名成功!\n")
    fmt.Printf("消息: %s\n", string(message2))
    fmt.Printf("签名: %x\n", signature2)
}
```

### 运行 Go 示例

```bash
cd go
go run example/main.go
```

## TypeScript 实现

### 安装

```bash
cd typescript
npm install
```

### 快速开始

```typescript
import { Client } from './src/client';

async function main() {
  // 使用配置服务器地址创建客户端
  const client = new Client('localhost:50052');

  try {
    // 初始化客户端（获取配置 + 建立 TLS 连接）
    await client.init();
    console.log(`客户端已连接，节点 ID: ${client.getNodeId()}`);

    // 示例 1：通过 app ID 获取公钥
    const appID = 'xxxxxx';
    try {
      const { publickey, protocol, curve } = await client.getPublicKeyByAppID(appID);
      console.log(`App ID ${appID} 的公钥:`);
      console.log(`  - 协议: ${protocol}`);
      console.log(`  - 曲线: ${curve}`);
      console.log(`  - 公钥: ${publickey}`);
    } catch (error) {
      console.error(`通过 app ID 获取公钥失败: ${error}`);
    }

    // 示例 2：使用 app ID 签名消息
    const message = new TextEncoder().encode('来自 AppID 服务的问候!');
    try {
      const signature = await client.signWithAppID(message, appID);
      console.log('使用 app ID 签名成功!');
      console.log(`消息: ${new TextDecoder().decode(message)}`);
      console.log(`签名: ${Buffer.from(signature).toString('hex')}`);
    } catch (error) {
      console.error(`使用 app ID 签名失败: ${error}`);
    }

    // 示例 3：使用显式协议和曲线的传统签名
    const publicKey = new TextEncoder().encode('来自外部 DKG 服务的示例公钥'); // 来自外部 DKG 服务
    const message2 = new TextEncoder().encode('你好，TEE DAO!');
    
    const signature2 = await client.sign(message2, publicKey, 1, 1); // ECDSA, ED25519
    console.log('传统签名成功!');
    console.log(`消息: ${new TextDecoder().decode(message2)}`);
    console.log(`签名: ${Buffer.from(signature2).toString('hex')}`);

  } catch (error) {
    console.error('错误:', error);
  } finally {
    await client.close();
  }
}

main();
```

### 运行 TypeScript 示例

```bash
cd typescript
npm run example  # 构建并运行，无警告
# 或
npm run build && node dist/example.js
```

## API 参考

Go 和 TypeScript 实现都提供相同的功能：

### 客户端创建和初始化

**Go:**
```go
client := client.NewClient("localhost:50052")
err := client.Init()
```

**TypeScript:**
```typescript
const client = new Client('localhost:50052');
await client.init();
```

### AppID 服务方法

**通过 AppID 获取公钥:**
```go
publicKey, protocol, curve, err := client.GetPublicKeyByAppID(appID)
```

**TypeScript:**
```typescript
const { publickey, protocol, curve } = await client.getPublicKeyByAppID(appId)
```

**使用 AppID 签名:**
```go
signature, err := client.SignWithAppID(message, appID)
```

**TypeScript:**
```typescript
const signature = await client.signWithAppID(message, appId)
```

### 传统消息签名

**Go:**
```go
signature, err := client.Sign(message, publicKey, protocol, curve)
```

**TypeScript:**
```typescript
const signature = await client.sign(message, publicKey, protocol, curve)
```

#### 协议常量

**Go:**
- `constants.ProtocolECDSA` (1)
- `constants.ProtocolSchnorr` (2)

**TypeScript:**
- `Protocol.ECDSA` (1)
- `Protocol.SCHNORR` (2)

#### 曲线常量

**Go:**
- `constants.CurveED25519` (1)
- `constants.CurveSECP256K1` (2)
- `constants.CurveSECP256R1` (3)

**TypeScript:**
- `Curve.ED25519` (1)
- `Curve.SECP256K1` (2)
- `Curve.SECP256R1` (3)

### 实用方法

#### 获取节点 ID
**Go:** `nodeID := client.GetNodeID()`  
**TypeScript:** `const nodeId = client.getNodeId()`

#### 设置超时
**Go:** `client.SetTimeout(30 * time.Second)`  
**TypeScript:** `client.setTimeout(30000)`

#### 关闭连接
**Go:** `client.Close()`  
**TypeScript:** `await client.close()`

## 架构

两个实现都由三个主要组件组成：

- **配置客户端**：处理与配置服务器的通信以检索节点配置
- **任务客户端**：管理与 TEE 节点的安全通信以进行密码学操作
- **AppID 客户端**：管理与 AppID 服务的通信以进行用户管理操作

客户端工作流程：
1. 使用配置服务器地址初始化客户端
2. 获取节点配置（证书、密钥、目标地址）
3. 建立到 TEE 节点的安全 TLS 连接
4. 建立到 AppID 服务的安全 TLS 连接
5. 使用指定协议和曲线或使用 AppID 执行签名操作

## Protocol Buffers

客户端使用 gRPC 和 Protocol Buffers 进行通信：
- `proto/node_management/`：用于配置的节点管理服务
- `proto/key_management/user_task.proto`：用于签名操作的用户任务定义
- `proto/appid/appid_service.proto`：用于用户管理的 AppID 服务定义

## 要求

### Go
- Go 1.24.2 或更高版本
- gRPC 和 Protocol Buffers 支持

### TypeScript
- Node.js 18.0.0 或更高版本
- npm 或 yarn

## 项目结构

```
├── go/                     # Go 实现
│   ├── client.go          # 主客户端
│   ├── pkg/               # 核心包
│   │   ├── config/        # 配置客户端
│   │   ├── constants/     # 协议和曲线常量
│   │   ├── task/          # 任务客户端（签名）
│   │   └── usermgmt/      # 用户管理客户端
│   ├── example/           # Go 示例
│   └── proto/             # 生成的 Go protobuf 文件
├── typescript/            # TypeScript 实现
│   ├── src/               # TypeScript 源代码
│   │   ├── client.ts      # 主客户端
│   │   ├── config-client.ts # 配置客户端
│   │   ├── task-client.ts # 任务客户端（签名）
│   │   ├── appid-client.ts # AppID 客户端
│   │   ├── types.ts       # TypeScript 类型和常量
│   │   └── example.ts     # TypeScript 示例
│   ├── proto/             # Protobuf 定义
│   └── dist/              # 编译后的 JavaScript
```

## 示例

- **Go**: 参见 [go/example/main.go](go/example/main.go)
- **TypeScript**: 参见 [typescript/src/example.ts](typescript/src/example.ts)

## 安全说明

- 为所有通信启用了 TLS 双向认证
- 保持主机名验证
- 通过 .gitignore 排除证书和密钥文件
- 无硬编码凭据或秘密

## 许可证

本项目是 TEENet 生态系统的一部分，用于安全的分布式密钥管理。