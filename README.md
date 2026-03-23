🔒 Remote Access IPsec VPN Implementation

 1.Objective
To configure a secure communication tunnel (IPsec VPN) using a FortiGate firewall in a virtualized environment. The primary goal is to allow an external endpoint (host computer) to securely access resources within an isolated internal network (LAN), ensuring data integrity and confidentiality.

 2.Environment
Firewall: FortiGate VM64 (v7.x or higher).

Hypervisor: VMware Workstation / ESXi.

Client: FortiClient VPN (Windows).

Interfaces:

Port1 (WAN): Bridged Mode (IP: 192.168.15.103) for tunnel termination.

Port2 (LAN): Host-Only Mode (Protected internal network).

 3.Architecture
The architecture follows the Remote Access VPN model, with the FortiGate acting as the central Gateway.

Phase 1 (IKE): Security Association (SA) establishment via UDP port 500, utilizing Pre-Shared Key (PSK) authentication.

Phase 2 (IPsec): Creation of the encapsulated data tunnel.

Encapsulation: Utilization of the ESP protocol for traffic encryption between the assigned client IP range (10.10.10.0/24) and the destination network.

 4.Data Collection
During the troubleshooting process, logs were collected via CLI (Command Line Interface) for diagnostics:

Commands Used:

diag debug application ike -1: To monitor the IKE protocol handshake.

diag debug enable: To activate debug output in the console.

Error Identification: Captured Timeout messages on FortiClient and administrative port conflict logs (TCP 443) on the FortiGate.

 5.Analysis
Technical analysis revealed three critical failure points resolved during the lab:

Port Conflict: Administrative HTTPS access (443) was colliding with the VPN listening port; management port was successfully migrated to 8443.

Routing (Split Tunneling): Implemented Split Tunneling to ensure only traffic destined for the internal network traverses the VPN, preventing the loss of the user's local internet connection.

Firewall Traversal: Adjusted the Host Operating System's firewall to permit UDP 4500 (NAT-T) and 500 traffic.

 6.Conclusion
The tunnel was successfully established, as evidenced by the "Up" status on the FortiGate dashboard and the FortiClient panel. End-to-end connectivity was validated through ICMP (ping) tests from the VPN client to the firewall's LAN interface, confirming that Firewall Policies are correctly processing traffic originating from the VPN zone.

![unnamed](https://github.com/user-attachments/assets/4f558a96-e7ee-48f9-9938-10524622d872)

