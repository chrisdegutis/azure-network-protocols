<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>

<p>
In this lab, I deployed a small Azure network environment consisting of a Windows 10 virtual machine and a Linux (Ubuntu) virtual machine inside the same Virtual Network and subnet. After connecting to the Windows VM through Remote Desktop, I used Wireshark to capture and analyze several common network protocols including ICMP, SSH, DHCP, DNS, and RDP.
</p>

<p>
I also configured and tested a Network Security Group (NSG) rule by blocking inbound ICMP traffic to the Ubuntu VM, observing the failed ping traffic, and then re-enabling ICMP to restore connectivity. This lab demonstrates how Azure networking, packet analysis, and firewall rules work together to control and monitor traffic between systems. 
</p>

<!-- <h2>Video Demonstration</h2>

 - ### [YouTube: Azure Virtual Machines, Wireshark, and Network Security Groups](https://www.youtube.com) -->

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>
<ol>
  <li>Create a Resource Group in Microsoft Azure.</li>
  <li>Deploy a Windows 10 virtual machine and allow Azure to create a Virtual Network and subnet.</li>
  <li>Deploy a Linux (Ubuntu) virtual machine in the same Resource Group and Virtual Network.</li>
  <li>Connect to the Windows VM using Remote Desktop.</li>
  <li>Install Wireshark on the Windows VM to capture network traffic.</li>
  <li>Generate and analyze ICMP traffic by pinging the Ubuntu VM and external websites.</li>
  <li>Configure a Network Security Group (NSG) to block and allow ICMP traffic and observe the results.</li>
  <li>Capture and analyze SSH traffic by connecting to the Ubuntu VM.</li>
  <li>Capture and analyze DHCP traffic by renewing the VM’s IP address.</li>
  <li>Capture and analyze DNS traffic using nslookup commands.</li>
  <li>Capture and analyze RDP traffic during a live remote session.</li>
  <li>Clean up resources by deleting the Resource Group.</li>
</ol>

<h2>Actions and Observations</h2>

<h2>Step 1: Create a Resource Group</h2>
<p>
Begin by signing in to the Azure portal and creating a new Resource Group. This Resource Group will contain all resources used throughout the lab, making them easier to organize and delete during cleanup.
</p>

<hr>

<h2>Step 2: Create a Windows 10 Virtual Machine</h2>
<p>
Create a Windows 10 virtual machine inside the Resource Group created in Step 1. During deployment, allow Azure to create a new Virtual Network and subnet for the VM.
</p>

<p>
This Windows VM will be used as the main workstation for the lab, where Remote Desktop, Wireshark, PowerShell, and SSH commands will be used to observe network traffic.
</p>

<hr>

<h2>Step 3: Create a Linux (Ubuntu) Virtual Machine</h2>
<p>
Create a Linux Ubuntu virtual machine and place it in the same Resource Group and the same Virtual Network as the Windows 10 VM.
</p>

<p>
When deploying the Ubuntu VM, use <b>Username/Password</b> as the authentication method. Keeping both virtual machines in the same Virtual Network and subnet allows direct private IP communication between them.
</p>

<hr>

<h2>Step 4: Verify Both VMs Are in the Same Virtual Network / Subnet</h2>
<p>
Confirm that both the Windows 10 VM and the Ubuntu VM are attached to the same Virtual Network and subnet. This is required so they can communicate internally over private IP addresses.
</p>

<p>
Once verified, the infrastructure for the packet analysis portion of the lab is complete.
</p>

<hr>

<h2>Step 5: Keep Both Virtual Machines Running for Part 2</h2>
<p>
Do not delete the virtual machines after deployment. They will be used in the next phase of the lab to generate and analyze live network traffic.
</p>

<hr>

<h2>Step 6: Install Microsoft Remote Desktop (Mac Only)</h2>
<p>
If working from a Mac, install Microsoft Remote Desktop so you can connect to the Windows 10 virtual machine.
</p>

<p>
This allows you to interact with the Windows VM as if you were physically using it.
</p>

<hr>

<h2>Step 7: Connect to the Windows 10 VM Using Remote Desktop</h2>
<p>
Use Remote Desktop to connect to the Windows 10 virtual machine. This system will serve as the observation point for network traffic throughout the lab.
</p>

<hr>

<h2>Step 8: Install Wireshark on the Windows 10 VM</h2>
<p>
Inside the Windows 10 virtual machine, install Wireshark. Wireshark will be used to capture and inspect packet traffic across multiple protocols.
</p>

<hr>

<h2>Step 9: Start a Packet Capture in Wireshark</h2>
<p>
Open Wireshark and begin a packet capture on the active network interface. This starts the live collection of traffic generated during the lab.
</p>

<hr>

<h2>Step 10: Filter for ICMP Traffic</h2>
<p>
Apply an ICMP filter in Wireshark so only ping-related traffic is shown. This makes it easier to observe ICMP request and reply packets without other traffic cluttering the view.
</p>

<p><b>Wireshark Filter:</b> <code>icmp</code></p>

<hr>

<h2>Step 11: Ping the Ubuntu VM by Private IP Address</h2>
<p>
Retrieve the Ubuntu VM’s private IP address from Azure and ping it from within the Windows 10 VM. Observe the ICMP echo requests and echo replies in Wireshark.
</p>

<p>
This verifies connectivity between both VMs inside the same Virtual Network and demonstrates how ICMP traffic appears in a packet capture.
</p>

<p><b>Command:</b> <code>ping &lt;Ubuntu-Private-IP&gt;</code></p>

<hr>

<h2>Step 12: Ping a Public Website</h2>
<p>
From the Windows 10 VM, open Command Prompt or PowerShell and ping a public website such as Google. Observe the traffic in Wireshark.
</p>

<p>
This demonstrates outbound ICMP traffic to an external destination and helps compare internal versus external ping behavior.
</p>

<p><b>Command:</b> <code>ping www.google.com</code></p>

<hr>

<h2>Step 13: Configure the Firewall with a Network Security Group</h2>
<p>
Start a continuous ping from the Windows 10 VM to the Ubuntu VM. Then open the Network Security Group associated with the Ubuntu VM and disable inbound ICMP traffic.
</p>

<p>
After applying the rule, return to the Windows VM and observe that the ping begins to fail and ICMP replies stop appearing in Wireshark. Then re-enable inbound ICMP traffic and confirm that the ping replies resume.
</p>

<p>
This step demonstrates how Azure NSG rules directly affect allowed and blocked network communication between systems.
</p>

<p><b>Continuous Ping Command:</b> <code>ping &lt;Ubuntu-Private-IP&gt; -t</code></p>

<hr>

<h2>Step 14: Log Back Into the Windows VM</h2>
<p>
Return to the Windows 10 VM if disconnected and prepare for the next protocol capture.
</p>

<hr>

<h2>Step 15: Start a New Wireshark Packet Capture</h2>
<p>
Start a fresh capture in Wireshark so the next protocol can be observed more clearly.
</p>

<hr>

<h2>Step 16: Filter for SSH Traffic</h2>
<p>
Apply an SSH filter in Wireshark to isolate encrypted remote shell traffic between the Windows and Ubuntu virtual machines.
</p>

<p><b>Wireshark Filter:</b> <code>ssh</code></p>

<hr>

<h2>Step 17: SSH Into the Ubuntu VM</h2>
<p>
From the Windows 10 VM, open PowerShell and initiate an SSH session into the Ubuntu VM using its private IP address.
</p>

<p>
This generates SSH traffic that can be observed in Wireshark, showing how secure remote administration traffic appears during capture.
</p>

<p><b>Command:</b> <code>ssh labuser@&lt;Ubuntu-Private-IP&gt;</code></p>

<hr>

<h2>Step 18: Run Commands in the SSH Session</h2>
<p>
After logging into the Ubuntu VM through SSH, type basic commands such as <code>pwd</code> and <code>username</code> or similar Linux commands, then watch the traffic in Wireshark.
</p>

<p>
When finished, exit the session.
</p>

<p><b>Command:</b> <code>exit</code></p>

<hr>

<h2>Step 19: Filter for DHCP Traffic</h2>
<p>
Back in Wireshark, apply a DHCP filter to isolate Dynamic Host Configuration Protocol traffic.
</p>

<p><b>Wireshark Filter:</b> <code>dhcp</code></p>

<hr>

<h2>Step 20: Renew the Windows VM IP Address</h2>
<p>
Open PowerShell as an administrator and renew the Windows VM’s IP configuration. Observe the DHCP request and response traffic in Wireshark.
</p>

<p>
This shows how a device communicates with the network when requesting renewed IP addressing information.
</p>

<p><b>Command:</b> <code>ipconfig /renew</code></p>

<hr>

<h2>Step 21: Filter for DNS Traffic</h2>
<p>
Apply a DNS filter in Wireshark to observe domain name resolution traffic.
</p>

<p><b>Wireshark Filter:</b> <code>dns</code></p>

<hr>

<h2>Step 22: Use nslookup for Domain Resolution</h2>
<p>
From the Windows 10 VM, run <code>nslookup</code> commands against domains such as Google and Disney. Observe the DNS query and response traffic in Wireshark.
</p>

<p>
This demonstrates how hostnames are translated into IP addresses by DNS.
</p>

<p><b>Commands:</b></p>
<p><code>nslookup google.com</code></p>
<p><code>nslookup disney.com</code></p>

<hr>

<h2>Step 23: Filter for RDP Traffic</h2>
<p>
Apply a display filter in Wireshark for Remote Desktop Protocol traffic on TCP port 3389.
</p>

<p><b>Wireshark Filter:</b> <code>tcp.port == 3389</code></p>

<hr>

<h2>Step 24: Observe Continuous RDP Traffic</h2>
<p>
Observe the constant stream of RDP traffic in Wireshark. Unlike protocols that only generate packets when a task is performed, RDP continuously transmits data because it is maintaining a live remote graphical session between your computer and the virtual machine.
</p>

<p>
This shows why RDP traffic appears to “spam” continuously even when only small on-screen changes are happening.
</p>

<hr>

<h2>Step 25: Close the Remote Desktop Session</h2>
<p>
Once the packet analysis is complete, close the Remote Desktop connection to the Windows 10 VM.
</p>

<hr>

<h2>Step 26: Delete the Resource Group</h2>
<p>
Delete the Resource Group created at the beginning of the lab. This removes the virtual machines, networking resources, and associated components to avoid ongoing Azure charges.
</p>

<hr>

<h2>Step 27: Verify Resource Group Deletion</h2>
<p>
Confirm that the Resource Group and all lab resources have been deleted successfully.
</p>

<hr>

<h2>Conclusion</h2>
<p>
In this lab, I created a small Azure network with a Windows 10 VM and a Linux Ubuntu VM in the same Virtual Network and used Wireshark to inspect live traffic across several important protocols including ICMP, SSH, DHCP, DNS, and RDP.
</p>

<p>
I also tested Azure Network Security Group behavior by blocking inbound ICMP traffic to the Ubuntu VM, confirming that connectivity stopped, and then re-enabling the rule to restore communication. This lab strengthened my understanding of Azure virtual networking, packet analysis, protocol behavior, and how NSG firewall rules affect network traffic in real time.
</p>
