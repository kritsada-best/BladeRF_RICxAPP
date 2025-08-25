# BladeRF_RICxAPP
This project explains how to install, configure, and run all required software to build a Private 5G Network on Linux (Ubuntu/Debian).
The setup uses BladeRF as the base station, srsRAN as the 5G Core, and optionally RIC + xApp for intelligent RAN control.
Once completed, the private network allows real UEs (such as smartphones or modems) to connect and access the Internet.
##
### Flow Explanation
- **UE --> RAN** : The UE connects to the network through the radio node (gNB/eNB).  
- **RAN --> RIC** : The RAN sends performance metrics to the RIC.  
- **RIC --> xApp** : The xApp analyzes and controls resources via the RIC.  
- **RAN --> Core Network** : The RAN forwards user traffic to the Core (AMF, SMF, UPF).  
- **Core Network --> Internet** : User traffic exits to the external Internet.  
- **xApp --> RIC --> RAN** : Feedback loop for resource optimization and control.  


## Network Flow

```mermaid
flowchart LR
    UE[" UE (User Equipment)"] --> RAN["RAN (Radio Access Network)"]
    RAN --> RIC["RIC (RAN Intelligent Controller)"]
    RIC --> xAPP["xApp (Control / Analytics)"]
    RAN --> Core["Core Network (AMF / UPF)"]
    Core --> Internet["Internet"]

    %% optional feedback loop
    xAPP -- Control / Policy --> RIC
    RIC -- Optimization --> RAN

