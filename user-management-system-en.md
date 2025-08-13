# User Management System

## System Overview

The User Management System is a modern web platform that integrates user authentication, project management, application deployment, and TEE key signing functionality. The system adopts passwordless authentication, supporting email verification codes and Google OAuth2 login, providing users with secure and convenient distributed application deployment and key management services.

## Core Features

### 🔐 Multi-Factor Authentication
- **Passwordless Login**: Secure login using 6-digit email verification codes
- **Google OAuth2**: One-click Google account login
- **Account Merging**: Support for binding email verification and Google authentication to the same account
- **Session Management**: JWT-based stateless authentication with 24-hour default validity

### 📊 Project Management
- **Project Creation**: Assign unique 32-bit App ID for each project
- **Application Deployment**: Support for ZIP, TAR, JAR, WAR format file uploads
- **Service Configuration**: Internal/public access configuration and port mapping
- **Status Monitoring**: Real-time tracking of container deployment status and health

### 🔑 TEE Key Management
- **Distributed Key Generation**: Integration with TEE DAO private key sharding system
- **Multi-Protocol Support**: ECDSA and Schnorr signature protocols
- **Multi-Curve Support**: ED25519, SECP256K1, SECP256R1
- **Project Association**: Assign dedicated signing keys to projects

### 🚀 Application Deployment
- **Containerized Deployment**: Use Docker containers to run user applications
- **Multi-Language Support**: Automatic detection and building of Go, Node.js, Python applications
- **Service Exposure**: Support for internal services and public web services
- **Health Monitoring**: Real-time monitoring of application running status

## Business Workflows

### User Registration/Login Workflow
```
User Access → Select Login Method → Email Verification Code/Google OAuth → 
Account Creation/Merging → JWT Token → Enter Main Interface
```

### Project Deployment Workflow
```
Create Project → Assign App ID → Select Key → Upload Application File → 
Configure Service Type → Push to Deployment Client → Docker Build → 
Container Startup → Status Feedback → Service Access
```

### Key Signing Workflow
```
Application Request Signing → App ID Lookup Public Key → TEE Client Manager → 
Distributed Signing Protocol → Multi-Node Collaboration → Signature Aggregation → Return Signature Result
```

## Security Features

### Authentication Security
- **Passwordless Design**: Eliminates password-related security vulnerabilities
- **Verification Code Limits**: Prevents brute force attacks and spam
- **OAuth2 Security**: State parameter validation prevents CSRF
- **Session Management**: Secure JWT token generation and validation

### Input Validation
- **Parameterized Queries**: Use GORM ORM to prevent SQL injection
- **Input Sanitization**: Server-side validation and sanitization of all inputs
- **File Validation**: Upload file type and size restrictions
- **XSS Protection**: Content security policy and output encoding

### Communication Security
- **TLS Encryption**: All gRPC communications use TLS encryption
- **Certificate Verification**: Mutual TLS authentication ensures identity verification
- **TEE Integration**: Key operations performed in trusted execution environment
- **Container Isolation**: Deployed applications run in isolated Docker containers

### Audit and Monitoring
- **Operation Logs**: Detailed user operation and system event logs
- **Error Tracking**: Complete stack traces for exceptions and errors
- **Performance Monitoring**: API response time and system resource usage monitoring
- **Security Events**: Real-time alerts for login failures and suspicious activities