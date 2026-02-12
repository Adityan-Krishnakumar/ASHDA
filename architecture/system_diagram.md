```mermaid
graph TB
    %% Direction: Top to Bottom
    
    subgraph "RED ZONE: External Threat"
        H["Kali Linux"]
    end

    subgraph "BLUE ZONE: Corporate Infrastructure"
        
        subgraph "User & Management Tier"
            E["Employee Workstation"]
            A["Ansible Controller: Healing Engine"]
        end

        subgraph "Production Infrastructure"
            W["Web-Server: Apache2"]
            D["DB-Server: MariaDB"]
            S["Agent: ashda_pulse.sh"]
        end

        subgraph "Wazuh Security Stack (SIEM)"
            M["Wazuh Manager"]
            Dec["Custom Decoders: ashda-pulse"]
            R["Custom Rules: 100051"]
            DS["Wazuh Dashboard"]
        end
    end

    %% Access & Attack Flow
    H -- "Unauthorized Access" --> W
    E -- "Authorized Admin SSH" --> W
    E -- "Authorized Admin SSH" --> D

    %% Monitoring Flow
    W -- "Log Stream" --> M
    D -- "Log Stream" --> M
    S -- "Pulse Heartbeat" --> M
    
    %% Processing & Visualization
    M --> Dec
    Dec --> R
    R -->|Visualize| DS
    
    %% Automated Healing Loop
    R -- "Trigger Active Response" --> A
    A -- "SSH: systemctl restart" --> W
    A -- "SSH: systemctl restart" --> D

    %% Styling for Professional Look
    style H fill:#ffcccc,stroke:#ff0000,stroke-width:2px
    style BLUE ZONE fill:#f0f8ff,stroke:#0055ff,stroke-width:2px,stroke-dasharray: 5 5
    
    %% Specific Black Font Styles for Core Components
    style A fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px,color:#000000
    style M fill:#e1f5fe,stroke:#01579b,stroke-width:2px,color:#000000
    style R fill:#fff9c4,stroke:#fbc02d,stroke-width:2px,color:#000000