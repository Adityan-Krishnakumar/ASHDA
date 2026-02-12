```mermaid
graph TB
    subgraph "External Threat"
        H[Kali Linux: Hydra/Brute Force]
    end

    subgraph "User & Management Tier"
        E[Employee Workstation: Admin Access]
        A[Ansible Controller: Healing Engine]
    end

    subgraph "Production Infrastructure"
        W[Web-Server: Apache2]
        D[DB-Server: MariaDB]
        S[Agent: ashda_pulse.sh]
    end

    subgraph "Wazuh Security Stack (SIEM)"
        M[Wazuh Manager]
        Dec[Custom Decoders: ashda-pulse]
        R[Custom Rules: 100051]
        DS[Wazuh Dashboard]
    end

    %% Access & Attack Flow
    H -- "Unauthorized Access" --> W
    E -- "SSH Admin Management" --> W
    E -- "SSH Admin Management" --> D

    %% Monitoring Flow
    W -- "local_decoder.xml" --> M
    D -- "local_rules.xml" --> M
    S -- "JSON/Syslog Heartbeat" --> M
    
    %% Processing & Visualization
    M --> Dec
    Dec --> R
    R -->|Visualize| DS
    
    %% Automated Healing Loop
    R -- "Trigger Active Response" --> A
    A -- "SSH: systemctl restart" --> W
    A -- "SSH: systemctl restart" --> D