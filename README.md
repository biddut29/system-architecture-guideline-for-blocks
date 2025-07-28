# System Architecture Guideline
### Table of Contents
1. [What is System Architecture?](#what-is-system-architecture)
2. [Key Components of the Architecture](#key-components-of-the-architecture)
3. [System Design & Flow](#system-design--flow)
4. [Software Components](#software-components)
5. [CI/CD Flow](#cicd-flow)
6. [Communication Between Microservices](#communication-between-microservices)
7. [Security & Observability](#security--observability)
8. [Scalability & Performance](#scalability--performance)
9. [Notification & Event Flow](#notification--event-flow)
10. [Architecture Highlights](#architecture-highlights)
11. [Implementation Roadmap](#implementation-roadmap)

---

## What is System Architecture?

System architecture is a blueprint that defines the structure, interaction, and deployment of software components, databases, and infrastructure. It ensures the system is scalable, maintainable, secure, and resilient, enabling smooth operation across distributed environments.

### Key Design Principles:

**Modular Microservices Design**: Independent services for better maintainability and scaling.

**Separation of Responsibilities**:
- **Web Services (API Layer)**: Handle synchronous client requests
- **Host Services (Worker Layer)**: Process background tasks, heavy reports, and schedulers
- **Multi-Tenant Architecture**: Isolated tenant data with dedicated database routing

**Event-Driven Architecture**: RabbitMQ ensures asynchronous communication between services.

**Real-Time Notification Layer**: WebSockets and Firebase Cloud Messaging (FCM) for instant updates.

**Observability**: Integrated monitoring and centralized logging across all services.

---

## Key Components of the Architecture

### 2.1 Overview of the System

**Code Management**: Centralized Git repository with GitFlow branching strategy.

**Environments**: Independent setups for Development, Staging, and Production with GitHub branching strategy.

**Infrastructure Components**:
- **Azure Kubernetes Services (AKS)**: Container orchestration platform with nodes and pods
- **Azure Container Registry**: Secure container image storage and management
- **Azure Web Application Firewall (WAF)**: Advanced threat protection
- **Application Gateway**: Load balancing and traffic management
- **Virtual Networks (VNET)**: Network segmentation and security

**Data Stores**:
- **MongoDB Cluster**: Primary NoSQL database with encrypted data storage and multi-tenant isolation
- **Redis**: High-speed volatile in-memory caching
- **Azure Blob Storage**: Encrypted storage for data and media content with public/private buckets
- **Container Hard Drives**: Encrypted persistent storage

**Messaging & Communication**:
- **RabbitMQ**: Asynchronous messaging and event-driven communication
- **Service Bus VNET**: Secure messaging infrastructure

**Security & Compliance**:
- **Key Vaults**: Hardware-encrypted secrets management
- **Network Security Groups (NSG)**: Network-level security controls
- **VPN Access**: Secure internal user access with strong authentication
- **Bastion Server**: Secure administrative access

**Containerization & Orchestration**: Containerized services deployed using Azure Kubernetes Service (AKS) with secure container registry.

### Azure Blob Storage Implementation

**Storage Strategy**:
- **Public Buckets**: Store publicly accessible content (e.g., website banners, static assets) with direct URL access
- **Private Buckets**: Store confidential content requiring authentication and pre-signed URLs for access

**Environment Separation**:
- **Development**: `ecap-falcon/dev`
- **Staging**: `ecap-falcon/stage` 
- **Production**: `ecap-falcon/prod`

**Use Cases**:
- **Public Content**: Website images, banners, and static assets accessible via direct URLs
- **Private Content**: Client-uploaded media requiring secure access through pre-signed URLs

### Multi-Tenant Architecture

**Tenant Isolation Strategy**:
- **Dedicated Database Routing**: Each tenant has isolated database access through Tenant Service
- **Microservices Entry Point**: Centralized microservice handles tenant-specific requests
- **Data Segregation**: Complete data isolation between tenants for security and compliance

**Architecture Flow**:
- **Tenant Requests**: Multiple tenants access shared microservices infrastructure
- **Service Routing**: Microservices route requests to Tenant Service for database selection
- **Database Isolation**: Tenant Service directs requests to tenant-specific MongoDB instances
- **Data Security**: Complete isolation ensures no cross-tenant data access

**Benefits**:
- **Security**: Complete data isolation between tenants
- **Scalability**: Independent scaling of tenant databases
- **Compliance**: Meets regulatory requirements for data segregation
- **Performance**: Optimized database access for each tenant

### High-Level Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              CLIENT LAYER                                   │
├─────────────────────────────────────────────────────────────────────────────┤
│  External Users │ Internal Users │ VPN Access │ TLS Secure Connection     │
├─────────────────────────────────────────────────────────────────────────────┤
│                              SECURITY & ENTRY LAYER                        │
├─────────────────────────────────────────────────────────────────────────────┤
│  Azure WAF │ Public IP │ Application Gateway │ VNET Inbound Security      │
├─────────────────────────────────────────────────────────────────────────────┤
│                            MICROSERVICES LAYER                             │
├─────────────────────────────────────────────────────────────────────────────┤
│  Azure Kubernetes Services │ Nodes │ Pods │ Ingress │ TLS Communication   │
├─────────────────────────────────────────────────────────────────────────────┤
│                            SUPPORTING SERVICES LAYER                       │
├─────────────────────────────────────────────────────────────────────────────┤
│  MongoDB Cluster │ RabbitMQ │ Redis │ Blob Storage │ Container Storage   │
├─────────────────────────────────────────────────────────────────────────────┤
│                            INFRASTRUCTURE LAYER                            │
├─────────────────────────────────────────────────────────────────────────────┤
│  Azure Container Registry │ Prometheus │ Grafana │ Wazuh │ Key Vaults      │
└─────────────────────────────────────────────────────────────────────────────┘
```

### CI/CD Pipeline Flow

```
Code Development → Version Control (GitHub) → Automated Testing → 
Container Build → Image Registry → Orchestration Platform → 
Production Deployment → Monitoring & Maintenance
```

---

## System Design & Flow

### 2.2 System Design & Flow

**Client Layer**
- **External Users**: Access via TLS-secured connections through Public IP
- **Internal Users**: Secure VPN access with strong authentication
- **TLS Encryption**: End-to-end encryption for all communications

**Security & Entry Layer**
- **Azure Web Application Firewall (WAF)**: Advanced threat protection and security
- **Public IP**: Secure external access point
- **Application Gateway**: Load balancing and traffic management
- **VNET Inbound Security**: Network-level security controls

**Microservices Layer**
- **Azure Kubernetes Services (AKS)**: Container orchestration with nodes and pods
- **Ingress**: Traffic routing and management
- **TLS Communication**: Secure inter-service communication
- **Microservices**: Distributed application components

**Supporting Services Layer**
- **MongoDB Cluster**: Primary database with encrypted data storage and multi-tenant isolation
- **RabbitMQ**: Asynchronous messaging and event-driven communication
- **Redis**: High-speed volatile in-memory caching
- **Azure Blob Storage**: Encrypted storage for data and media content with public/private buckets
- **Container Storage**: Encrypted persistent storage

**Infrastructure & Security**
- **Azure Container Registry**: Secure container image management
- **Prometheus & Grafana**: Comprehensive monitoring and visualization
- **Wazuh**: Security monitoring and compliance
- **Key Vaults**: Hardware-encrypted secrets management
- **Network Security Groups (NSG)**: Network-level security controls

---

## Software Components

### 2.3 Software Components

**Frontend**: Modern web applications and mobile apps.

**Backend**: Microservices-based architecture with Azure Kubernetes Services.

**Messaging**: RabbitMQ for event-driven communication.

**Notification Layer**: Real-time messaging and push notifications.

**Databases**: MongoDB Cluster with multi-tenant isolation and Redis caching.

**Storage**: Azure Blob Storage with public/private buckets for data and media content storage.

**Monitoring & Logging**: Prometheus, Grafana, and Wazuh for comprehensive monitoring.

---

## CI/CD Flow

### 2.4 CI/CD Flow

**Development Process**:
- Developers push code to GitHub version control system
- GitHub Actions automatically trigger the CI/CD pipeline

**Build & Test Phase**:
- Automated build process compiles and packages the application
- Comprehensive testing including unit tests, integration tests, and quality checks
- SonarQube performs static code analysis for quality assurance

**Containerization & Deployment**:
- Docker images are created and pushed to Azure Container Registry
- Azure Kubernetes Service pulls the latest images and deploys to production
- Automated rollback mechanisms ensure service stability

**Monitoring & Maintenance**:
- Continuous monitoring of deployed applications
- Performance tracking and health checks
- Automated maintenance and updates

---

## Communication Between Microservices

### 2.5 Communication Between Microservices

**Direct API Calls**: For real-time, synchronous operations.

**RabbitMQ Event Bus**: For asynchronous tasks like report processing or background workflows.

**Hybrid Approach**: Combines direct APIs and events for flexible, reliable communication.

---

## Security & Observability

### 2.6 Security & Observability

**Security Layers**:
- **Azure Web Application Firewall (WAF)**: Advanced threat protection and security
- **TLS Encryption**: End-to-end encryption for all communications
- **Authentication**: Secure token-based authentication for stateless sessions
- **Single Sign-On**: Integration with enterprise identity providers
- **Access Control**: Role-Based Access Control (RBAC) at all levels

**Monitoring & Compliance**:
- **Prometheus & Grafana**: Comprehensive metrics collection and visualization
- **Wazuh Security & Compliance**: Advanced security monitoring and threat detection
- **Centralized Logging**: Comprehensive log aggregation and analysis
- **Real-time Monitoring**: Continuous system health and performance tracking
- **Automated Audits**: Regular security assessments and compliance checks
- **GDPR Compliance**: Full compliance with General Data Protection Regulation requirements

---

## Scalability & Performance

### 2.7 Scalability & Performance

**Horizontal Scaling**: Auto-scaling of services based on load.

**Caching**: Multi-level caching reduces database reads and improves response time.

**Message Queues**: Prevent bottlenecks by queuing background tasks.

---

## Security Architecture

### Multi-Layer Security

#### Network Security:
Network Security Groups (NSG) control traffic flow between services, ensuring secure communication and preventing unauthorized access.

#### Identity & Access Management:
Enterprise identity management with role-based access control for developers, operators, and administrators.

#### Secrets Management:
Azure Key Vaults provide centralized secrets management for secure storage and access to sensitive information.

### Data Security

#### Encryption:
- **At Rest**: AES-256 encryption for all data stores
- **In Transit**: TLS 1.3 for all communications
- **Application Level**: Field-level encryption for sensitive data

#### Data Classification:
Data is classified into public, internal, confidential, and restricted categories based on sensitivity and business requirements.

#### Compliance:
- **GDPR**: Data protection and privacy
- **SOC 2**: Security controls and monitoring
- **PCI DSS**: Payment card security
- **HIPAA**: Healthcare data protection

---

## Notification & Event Flow

### 3.1 Event Workflow

User performs an action (e.g., submits a request).

Web Service updates the database and publishes an event to the message queue.

Background Service or other microservices consume this event asynchronously.

Results are processed, and responses are sent back to the client (if needed).

### 3.2 Notification Workflow

Notification Service listens to RabbitMQ for events like "Task Completed."

For active users, updates are pushed through real-time communication.

For offline/mobile users, push notifications are sent.

---

## Architecture Highlights

### 4.1 Key Features

**Multi-Layer Security**: Azure WAF, TLS encryption, and Wazuh security monitoring.

**Comprehensive Service Architecture**: Microservices with Azure Kubernetes Services and multi-tenant database isolation.

**Event-Driven Communication**: Asynchronous messaging through RabbitMQ for scalable workflows.

**Real-Time Capabilities**: Instant updates and notifications for users.

**Cloud-Native Deployment**: Containerized services with Azure Kubernetes orchestration.

**Automated CI/CD Pipeline**: GitHub Actions, SonarQube quality checks, and automated deployments.

**Multi-Environment Support**: Development, Staging, and Production environments with GitHub branching strategy.

