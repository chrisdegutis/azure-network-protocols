<p align="center">
  <img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

# Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines

In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups.

## Environments and Technologies Used

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

## Operating Systems Used

- Windows 10 (21H2)
- Ubuntu Server 22.04

## High-Level Steps

1. Create Virtual Machines in Azure.
2. Observe ICMP traffic between Virtual Machines using Wireshark.
3. Configure a Firewall (Network Security Group) and analyze its impact on network traffic.
4. Observe various protocol traffic (SSH, DHCP, DNS, RDP) using Wireshark.

---

## Part 1: Create Virtual Machines

1. Log in to [Azure Portal](https://portal.azure.com/).
2. **Create a Resource Group**:
   - Navigate to "Resource Groups" and click "Create."
   - Provide a name for your Resource Group and select a region.
   - Click "Review + Create," then "Create."
  
<p>
<img src="https://i.imgur.com/nkfDzdG.png" height="80%" width="80%" alt="Step 1 Lab 2"/>
</p>

3. **Create a Windows 10 Virtual Machine**:
   - Navigate to "Virtual Machines" and click "Create."
   - Select the Resource Group you just created.
   - Configure the Virtual Machine:
     - OS: Windows 10
     - Create username and password
     - Head to the Networking section, then create a new virtual network titled "Lab2-vnet"
   - Complete the setup and deploy the VM.
  
<p>
<img src="https://i.imgur.com/ln5shD0.png" height="80%" width="80%" alt="Step 1 Lab 2"/>
</p>

<p>
<img src="https://i.imgur.com/dBrcujc.png" height="80%" width="80%" alt="Step 1 Lab 2"/>
</p>

<p>
<img src="https://i.imgur.com/MCv4Q1g.png" height="80%" width="80%" alt="Step 1 Lab 2"/>
</p>

<p>
<img src="https://i.imgur.com/iU9lo7m.png" height="80%" width="80%" alt="Step 1 Lab 2"/>
</p>

4. **Create a Linux (Ubuntu) Virtual Machine**:
   - Navigate to "Virtual Machines" and click "Create."
   - Select the same Resource Group and Virtual Network used for the Windows 10 VM.
   - Configure the Virtual Machine:
     - OS: Ubuntu Server 22.04
     - Authentication: Username/Password.
   - Ensure both VMs are in the same Virtual Network and Subnet as the Windows 10 VM.
   - Complete the setup and deploy the VM.

<p>
<img src="https://i.imgur.com/uWwnh01.png" height="80%" width="80%" alt="Step 1 Lab 2"/>
</p>

<p>
<img src="https://i.imgur.com/6iHplkL.png" height="80%" width="80%" alt="Step 1 Lab 2"/>
</p>

<p>
<img src="https://i.imgur.com/K2kS6ye.png" height="80%" width="80%" alt="Step 1 Lab 2"/>
</p>

<p>
<img src="https://i.imgur.com/HhOHkjo.png" height="80%" width="80%" alt="Step 1 Lab 2"/>
</p>

---

## Part 2: Observe ICMP Traffic

1. Use [Microsoft Remote Desktop](https://apps.microsoft.com/store) to connect to your Windows 10 Virtual Machine (if on Mac, install the client first).
2. **Install Wireshark** on the Windows 10 VM:
   - Download and install Wireshark from [https://www.wireshark.org/](https://www.wireshark.org/).
3. Open Wireshark and start a packet capture.
4. Filter for ICMP traffic in Wireshark.
5. Retrieve the private IP address of the Ubuntu VM and attempt to ping it from the Windows 10 VM:
   - Open Command Prompt or PowerShell and run: `ping <Ubuntu VM Private IP>`.
   - Observe the ping requests and replies in Wireshark.
6. From the Windows 10 VM, ping a public website (e.g., `www.google.com`) and observe the ICMP traffic in Wireshark.

<p>
<img src="https://i.imgur.com/iN88a8x.png" height="80%" width="80%" alt="Step 2 Lab 2"/>
</p>

<p>
<img src="https://i.imgur.com/jcsAXgl.png" height="80%" width="80%" alt="Step 2 Lab 2"/>
</p>

---

## Part 3: Configure a Firewall (Network Security Group)

### Observe ICMP Traffic with Firewall Changes

1. Initiate a continuous ping from your Windows 10 VM to the Ubuntu VM:
   - Command: `ping <Ubuntu VM Private IP> -t`.
2. Open the Network Security Group associated with the Ubuntu VM.
3. Disable inbound ICMP traffic in the Network Security Group.
4. Observe the ICMP traffic in Wireshark and the command line Ping activity (should stop).
5. Re-enable ICMP traffic in the Network Security Group.
6. Observe the ICMP traffic in Wireshark and the command line Ping activity (should resume).
7. Stop the ping activity.

<p>
<img src="https://i.imgur.com/X9VBE8M.png" height="80%" width="80%" alt="Step 1 Lab 2"/>
</p>

<p>
<img src="https://i.imgur.com/xOvIUta.png" height="80%" width="80%" alt="Step 1 Lab 2"/>
</p>

<p>
<img src="https://i.imgur.com/p8MGqMZ.png" height="80%" width="80%" alt="Step 1 Lab 2"/>
</p>

<p>
<img src="https://i.imgur.com/37YhB1B.png" height="80%" width="80%" alt="Step 1 Lab 2"/>
</p>

<p>
<img src="https://i.imgur.com/7O8lApB.png" height="80%" width="80%" alt="Step 1 Lab 2"/>
</p>

### Observe SSH Traffic

1. In Wireshark, start a new packet capture and filter for SSH traffic.
2. From the Windows 10 VM, SSH into the Ubuntu VM:
   - Command: `ssh <username>@<Ubuntu VM Private IP>`.
   - Enter the password when prompted.
3. Type commands within the SSH session and observe the SSH traffic in Wireshark.
4. Exit the SSH session: `exit`.

<p>
<img src="https://i.imgur.com/4UxjL6i.png" height="80%" width="80%" alt="Step 1 Lab 2"/>
</p>

### Observe DHCP Traffic

1. In Wireshark, filter for DHCP traffic.
2. From the Windows 10 VM, issue a new IP address:
   - Open PowerShell as admin and run: `ipconfig /renew`.
3. Observe the DHCP traffic in Wireshark.

<p>
<img src="https://i.imgur.com/b11FUBi.png" height="80%" width="80%" alt="Step 1 Lab 2"/>
</p>

### Observe DNS Traffic

1. In Wireshark, filter for DNS traffic.
2. From the Windows 10 VM, use `nslookup` to find IP addresses for websites:
   - Example: `nslookup google.com`, `nslookup disney.com`.
3. Observe the DNS traffic in Wireshark.

<p>
<img src="https://i.imgur.com/gZnp0cn.png" height="80%" width="80%" alt="Step 1 Lab 2"/>
</p>

### Observe RDP Traffic

1. In Wireshark, filter for RDP traffic:
   - Use the filter: `tcp.port == 3389`.
2. Observe the continuous RDP traffic between the Windows 10 VM and your local machine.

<p>
<img src="https://i.imgur.com/AblB0s9.png" height="80%" width="80%" alt="Step 1 Lab 2"/>
</p>

---
