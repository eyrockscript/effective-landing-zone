# AWS Landing Zone
Implementing an Effective AWS Landing Zone

```mermaid
graph TB
    subgraph "AWS Organizations"
        MA[Management Account]
        
        subgraph "Security OU"
            SEC[Security Account]
            LOG[Log Archive Account]
            AUDIT[Audit Account]
        end
        
        subgraph "Infrastructure OU"
            NET[Network Account]
            SHARED[Shared Services Account]
        end
        
        subgraph "Workloads OU"
            PRD[Production Account]
            QA[QA Account]
            DEV[Development Account]
        end
        
        %% Conexiones entre cuentas
        MA --> SEC
        MA --> LOG
        MA --> AUDIT
        MA --> NET
        MA --> SHARED
        MA --> PRD
        MA --> QA
        MA --> DEV
    end
    
    %% Componentes externos
    VPN[Enterprise VPN]
    ONPREM[On-Premises Network]
    INTERNET[Internet]
    
    %% Servicios en Network Account
    subgraph "Network Account Services"
        TGW[Transit Gateway]
        NWFW[Network Firewall]
        R53[Route53]
        SVPC[Shared VPCs]
    end
    
    %% Servicios en Security Account
    subgraph "Security Services"
        CT[Control Tower]
        GD[GuardDuty]
        SH[Security Hub]
        CONFIG[AWS Config]
        IAM[IAM Identity Center]
    end
    
    %% Conexiones de red
    ONPREM --> VPN
    VPN --> TGW
    TGW --> PRD
    TGW --> QA
    TGW --> DEV
    TGW --> NWFW
    NWFW --> INTERNET
    
    %% Servicios compartidos
    SHARED --> PRD
    SHARED --> QA
    SHARED --> DEV
    
    %% Logging y Auditoría
    PRD & QA & DEV --> LOG
    SEC --> AUDIT
    
    classDef default fill:#fff,stroke:#666666,stroke-width:2px,color:#000;
    classDef security fill:#fff3e6,stroke:#666666,stroke-width:2px,color:#000;
    classDef network fill:#e6f7ff,stroke:#666666,stroke-width:2px,color:#000;
    classDef external fill:#fff,stroke:#666666,stroke-width:2px,color:#000;
    
    class SEC,LOG,AUDIT security;
    class NET,TGW,NWFW,R53,SVPC network;
    class VPN,ONPREM,INTERNET external;
    
    linkStyle default stroke:#666666,stroke-width:1px;
    
    class SEC,LOG,AUDIT security;
    class NET,TGW,NWFW,R53,SVPC network;
    class VPN,ONPREM,INTERNET external;
