# Remote Desktop Troubleshooting Lab

**Scenario:** A Windows 11 domain client was unable to connect to a Windows Server using Remote Desktop. I reproduced the connection failure, tested network connectivity and TCP port 3389, identified a disabled Windows Firewall rule as the root cause, corrected the issue, and verified that Remote Desktop access was restored.

## 1. Reproduce the RDP Connection Failure

I attempted to connect to the Windows Server at `10.1.10.2` using Remote Desktop. The connection failed, confirming the issue and providing a starting point for troubleshooting.


<img width="1356" height="1160" alt="01-rdp-connection-failure png" src="https://github.com/user-attachments/assets/0c4349b1-dd81-4a07-9945-95dd6ff21d10" />


## 2. Test RDP Port 3389

I verified that the server was reachable and used `Test-NetConnection` to test TCP port `3389`, which is used by Remote Desktop. The test returned `TcpTestSucceeded: False`, showing that the server was reachable but RDP traffic was not getting through.


<img width="1056" height="976" alt="02-rdp-port-3389-failure png" src="https://github.com/user-attachments/assets/5e81bcbd-14a8-43b8-868a-a9fda4c995e4" />


## 3. Identify the Root Cause

I inspected the Windows Firewall inbound rules on the server and discovered that the **Remote Desktop - User Mode (TCP-In)** rule was disabled. This prevented inbound RDP traffic from reaching the server.


<img width="940" height="947" alt="03-rdp-firewall-root-cause-annotated (1)" src="https://github.com/user-attachments/assets/47d2ff47-c60e-4281-b73d-90152bc6305d" />


## 4. Apply the Fix and Verify Access

I enabled the Remote Desktop TCP inbound firewall rule and tested TCP port `3389` again. After confirming that the port was reachable, I successfully connected to the Windows Server using Remote Desktop.


<img width="1191" height="896" alt="04-rdp-access-restored png" src="https://github.com/user-attachments/assets/b51d18bb-dd9f-4f77-9929-b6dcd1f9b761" />


**Tools used:** Remote Desktop Connection • PowerShell • `Test-NetConnection` • `ping` • Windows Defender Firewall with Advanced Security • Windows Server • Windows 11

**Result:** Successfully diagnosed an RDP connection failure, identified a disabled Windows Firewall rule as the root cause, restored TCP 3389 connectivity, and verified successful Remote Desktop access.

## What I Learned

I learned how to troubleshoot Remote Desktop problems by separating general network connectivity from service-specific connectivity. A server being reachable does not necessarily mean that RDP is accessible.

This lab also reinforced the importance of gathering evidence before changing settings. Testing TCP port `3389` helped narrow the problem before I investigated the firewall and identified the root cause.

**Reproduce → Test → Identify Root Cause → Fix → Verify**
