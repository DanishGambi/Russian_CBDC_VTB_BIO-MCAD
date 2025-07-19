# Digital Ruble System Architecture  
**Project:** Unified Transaction Processing System for Bank of Russia's Digital Ruble  
**Team:** BIO-MCAD  

## 1. Project Overview  
The Digital Ruble system provides a unified architecture for processing local and global transactions involving the Central Bank of Russia's Digital Ruble. This system integrates electronic money accounts from commercial banks while supporting three client interfaces: web application, mobile application, and NFC-based physical tokens (NFT Tokens). The architecture is designed to serve approximately 115-120 million accounts with a peak throughput capacity of 40 million transactions per hour.
*This project was developed as part of the ARCHI.TECH hacathon. The requirements for our solution can be found on the event website [here](https://architechhack.vtb.ru/track2).*

## 2. Team Composition  
| Role | Member |
|------|--------|
| **Solution Architect** | Dmitry Malyuzhantsev |
| **Data Architect** | Roman Galimov |
| **Analyst** | Timofey Tolkov |
| **Backend Developer** | Yan Pavlov |
| **Backend Developer** | Anton Dubrovin |

## 3. System Requirements  
### 3.1 Functional Requirements  
- Transaction processing between Digital Ruble accounts  
- Role-based access control:  
  - **Payer**: Initiates transactions  
  - **Payee**: Confirms/receives transactions  
  - **Commercial Bank Representative**: Manages correspondent accounts  
  - **CB Employee**: Accesses all CB services except emission  
  - **CB Administrator**: Manages Digital Ruble emission  
- Support for hybrid payment instrument (NFT Token):  
  - Physical NFC device + blockchain-registered digital identity  
  - Offline-capable payments (terminal-side connectivity only)  

### 3.2 Non-Functional Requirements  
| Metric | Target Value | 
|--------|--------------|
| **Accounts Capacity** | 120 million |
| **Daily Transactions** | 200 million | 
| **Peak Hourly Throughput** | 40 million | 
| **Transaction Latency** | < 500ms | 
| **System Availability** | 99.99% |  

## 4. Architectural Components  
### 4.1 Core Services  
#### Central Bank Services  
| Service | Technology Stack | Functionality |  
|---------|------------------|---------------|  
| **API Gateway** | gRPC, Spring Boot | Unified entry point for all system requests |  
| **Payment Service** | Java, Kafka | Transaction processing with ACID compliance |  
| **Emission Service** | PostgreSQL | Digital Ruble issuance/withdrawal management |  
| **Analytics Service** | Real-time processing | Fraud detection and financial monitoring |  

#### Commercial Bank Services  
| Service | Technology Stack | Functionality |  
|---------|------------------|---------------|  
| **Digital Exchange** | REST API | Ruble ↔ fiat/tokenized assets conversion |  
| **Data Backup Service** | Blockchain sync | Real-time replication with CB blockchain |  

### 4.2 Infrastructure Components  
| Component | Technology | Purpose |  
|-----------|------------|---------|  
| **CDN** | Apache Traffic Server | Static content delivery |  
| **Web Server** | Nginx | Client request handling |  
| **Database** | PostgreSQL (sharded) | Transactional data storage |  
| **Cache** | Redis | Session and balance caching |  
| **Messaging** | Apache Kafka | Transaction orchestration |  
| **IAM** | Keycloak | Centralized authentication |  

## 5. Deployment Architecture  
### 5.1 Node Configuration  
| Environment | Node Count | Specifications | Services Hosted |  
|-------------|------------|----------------|----------------|  
| **Central Bank** | 13-15 | 8 vCPU, 16GB RAM | Core transaction processing, Emission, Archives |  
| **Commercial Banks** | 5-6 | 8 vCPU, 16GB RAM | Client-facing services, Local backups |  

### 5.2 Network Topology  
- **Tier 1**: Client-facing applications (Web/Mobile/NFT)  
- **Tier 2**: API Gateways with WAF (Nginx + ModSecurity)  
- **Tier 3**: Core business services (Payment, Emission, Exchange)  
- **Tier 4**: Data layer (PostgreSQL, Kafka, Redis)  

## 6. Security Framework  
### 6.1 Data Protection  
- **NFT Token Security**: Hardware-backed cryptographic signatures  
- **End-to-End Encryption**: TLS 1.3 for all communications  
- **Blockchain Integrity**: Central Bank-maintained distributed ledger  

### 6.2 Access Control  
- **Authentication**: Keycloak with OAuth 2.0/OpenID Connect  
- **Authorization**: Role-based access policies  
- **Privileged Access**: Mandatory 2FA for administrative roles  

### 6.3 Threat Mitigation  
- **Web Application Firewall**: ModSecurity rules for SQLi/XSS/DDoS  
- **Real-time Monitoring**: Anomaly detection in payment flows  
- **Data Integrity**: Hash-chained transaction records  

## 7. Diagrams & Documentation  
Refer to `BIO-MCAD_ARCHI.Tech_2025_main.pptx` for:  
- UML Data Architecture Diagram  
- Transaction State Transition Graph  
- Component Interaction Sequence  
- Physical Deployment Schema  

## 8. Compliance & Regulations  
System design adheres to:  
- Bank of Russia Digital Currency Framework  
- Federal Law № 259-FZ "On Digital Financial Assets"  
- ISO 27001 Information Security Standards  

*Last Updated: 20 July 2025*  
