# TEE DAO Private Key Sharding System

## System Overview

The TEE DAO Private Key Sharding System is a distributed key management platform based on threshold cryptography, implementing secure key generation, storage, and signing functions using FROST (Flexible Round-Optimized Schnorr Threshold) and ECDSA GG20 protocols. The system ensures that no single node can obtain the complete private key by distributing private key shards across multiple TEE (Trusted Execution Environment) nodes, providing extremely high security.

## Core Features

### 🔐 Distributed Key Generation (DKG)
- **No Single Point of Private Key Exposure**: Private keys are never generated or stored in a single location
- **Threshold Cryptography**: Requires cooperation of at least t nodes to perform signing operations
- **Multi-Protocol Support**: Supports both FROST (Schnorr) and ECDSA GG20 protocols
- **Multi-Curve Support**: Supports elliptic curves such as ED25519, SECP256K1, SECP256R1, P256

### 🏗️ Distributed Architecture
- **Raft Consensus**: Uses Raft algorithm for distributed decision-making and leader election
- **TEE Environment**: All critical operations performed in trusted execution environment
- **Secure Communication**: Inter-node communication secured with mutual TLS authentication
- **Fault-Tolerant Design**: Supports recovery from node failures and network partitions

### 📊 Performance Monitoring and Automation
- **Real-time Performance Monitoring**: Tracks task completion rates, execution times, failure patterns
- **Automatic Leader Switching**: Automatically replaces inefficient leaders based on performance metrics
- **Load Balancing**: Random participant selection improves load distribution
- **Debouncing Mechanism**: Prevents frequent leader switching

### 🔄 Key Lifecycle Management
- **Key Resharing**: Regularly updates key shards without changing public keys
- **Automatic Rotation**: Time-based automatic key resharing (default 24 hours)

## System Architecture

### Core Components

#### 1. Node Management (Node)
- **Node Discovery**: Automatic discovery and management of nodes in the network
- **Lifecycle Management**: Handles node startup, operation, and shutdown
- **Role Management**: Distinguishes between TEE nodes, mesh nodes, and application nodes

#### 2. Task Manager (TaskManager)
- **Task Coordination**: Unified management of DKG, signing, and resharing tasks
- **Performance Monitoring**: Real-time tracking of task execution statistics and leader performance
- **Cache Management**: Maintains independent caches for different task types
- **Concurrency Control**: Task concurrency limits based on semaphores

#### 3. Key Manager (KeyManager)
- **Persistent Storage**: Uses MongoDB to store key information and metadata
- **Memory Caching**: LRU cache provides fast access
- **Serialization**: Secure serialization of complex cryptographic objects
- **Version Control**: Supports key version management and rotation

#### 4. Communication Layer (Communicator)
- **gRPC Services**: Efficient remote procedure calls
- **TLS Security**: Mutual TLS authentication ensures communication security
- **Connection Pooling**: Persistent connections with automatic reconnection mechanisms
- **Message Routing**: Intelligent message routing and load balancing

#### 5. Consensus Layer (Raft)
- **Leader Election**: Automatic selection of coordinating nodes
- **State Synchronization**: Maintains distributed system state consistency
- **Fault Recovery**: Handles node failures and network partitions

### Supported Cryptographic Protocols

#### FROST Protocol (Schnorr Signatures)
```
Round 1: Commitment Generation
├── Each participant generates polynomial commitments
├── Broadcast public values to all participants
└── Point-to-point send secret shards

Round 2: Shard Verification and Key Computation
├── Verify correctness of received shards against commitments
├── Compute verification key shards
├── Broadcast verification data
└── Generate final group public key
```

#### ECDSA GG20 Protocol
```
Round 1: Initial Setup
├── Generate Paillier key pairs and commitments
└── Broadcast public components

Round 2: Shard Distribution
├── Generate and verify secret shards
├── Send encrypted shards via point-to-point
└── Broadcast verification data

Rounds 3-4: Zero-Knowledge Proofs
├── Generate PSF (Paillier Schnorr Factorization) proofs
├── Verify all received proofs
└── Complete DKG and compute verification keys
```

## Security Model

### Threat Model Protection
- **Malicious Minority**: System remains secure as long as fewer than t nodes are compromised
- **Network Attacks**: All communications encrypted using mutual TLS
- **Internal Threats**: No single node can reconstruct the private key
- **Availability**: Byzantine fault tolerance achieved through threshold protocols

### Access Control
- **Certificate-Based Authentication**: Uses X.509 certificates for node identity verification
- **Role-Based Permissions**: Different node types have different permissions
- **API Security**: gRPC services use TLS termination
- **Audit Logging**: Complete security event tracking

## License

This project is proprietary and confidential to TEENet Technology (Hong Kong) Limited.