
# 🛡️ Cybersecurity Testing Lab Setup — VirtualBox & Kali Linux

This project documents the setup and configuration of a virtualized cybersecurity testing lab using Oracle VirtualBox and Kali Linux.

Completed as part of **Week 1 — Project Module 1** of my Cybersecurity Internship, this task focuses on establishing a dedicated lab environment with custom network configuration, static IP addressing, internet connectivity, shared folders, and virtual machine snapshot management.

The lab provides a foundation for future cybersecurity practical exercises and authorized security testing in a controlled environment.

---

## 📌 Project Overview

The objective of this project was to establish a functional cybersecurity lab using Oracle VirtualBox as the virtualization platform and Kali Linux as the primary testing machine.

### Key Configurations

- Virtualization Platform: Oracle VirtualBox
- Operating System: Kali Linux
- Network Type: Custom NAT Network
- Network Subnet: `10.0.0.0/24`
- Kali Linux IP Address: `10.0.0.2/24`
- Default Gateway: `10.0.0.1`
- Internet Connectivity: Enabled
- Shared Clipboard: Bidirectional
- Drag and Drop: Bidirectional
- Shared Folder: `/downloads`
- Virtual Machine Snapshot: Configured

---

## 📐 Network Architecture

### Network Specifications

| Parameter | Configuration |
|---|---|
| Virtualization Platform | Oracle VirtualBox |
| Network Type | NAT Network |
| Network Name | `NatNetwork` |
| Network Prefix | `10.0.0.0/24` |
| Subnet Mask | `255.255.255.0` |
| Gateway Address | `10.0.0.1` |
| DHCP | Enabled |
| Kali Linux IP Address | `10.0.0.2` |
| DNS Server | `8.8.8.8` |
| Internet Connectivity | Enabled |

### Network Topology

```text
                 Host Machine
                Windows 10/11
                      |
                      |
              Oracle VirtualBox
                      |
                      |
                NAT Network
                 NatNetwork
               10.0.0.0/24
                      |
               Gateway: 10.0.0.1
                      |
                +-----+-----+
                |           |
                |           |
           Kali Linux     Internet
              VM
          10.0.0.2/24
                |
                |
        Future Target VMs
          (Optional Phase)
```

The Kali Linux virtual machine is connected to a custom NAT Network, providing internet access and a dedicated virtual networking environment for future lab machines.

---

## 🛠️ System Requirements

### Recommended Host Specifications

| Component | Recommended Specification |
|---|---|
| RAM | 8 GB minimum; 16 GB recommended |
| Storage | 256 GB SSD or higher |
| Processor | Intel Core i3/i5 or AMD equivalent |
| Virtualization | Intel VT-x / AMD-V enabled |
| Host Operating System | Windows 10 / Windows 11 |

### Software & Tools

- [Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [7-Zip](https://www.7-zip.org/)
- [Kali Linux](https://www.kali.org/get-kali/)

---

## 🚀 Implementation & Configuration

### Phase 1: VirtualBox NAT Network Configuration

A custom NAT Network was created in Oracle VirtualBox.

**Configuration Steps:**

1. Open Oracle VirtualBox Manager.
2. Navigate to the Network settings.
3. Select the NAT Networks section.
4. Create a new NAT Network.
5. Configure the following parameters:

| Setting | Value |
|---|---|
| Name | `NatNetwork` |
| IPv4 Prefix | `10.0.0.0/24` |
| Enable DHCP | Yes |

6. Apply and save the configuration.

### Phase 2: Kali Linux Virtual Machine Setup

The Kali Linux virtual machine was imported into VirtualBox and configured as the primary cybersecurity testing machine.

#### 1. Network Adapter Configuration

Navigate to:

`Kali Linux VM → Settings → Network → Adapter 1`

| Setting | Value |
|---|---|
| Enable Network Adapter | Enabled |
| Attached To | NAT Network |
| Network Name | `NatNetwork` |
| Promiscuous Mode | Allow All |
| Virtual Cable Connected | Enabled |

#### 2. Virtual Machine Integration

The following settings were configured to improve interaction between the host and guest operating systems.

**General → Advanced**

| Setting | Value |
|---|---|
| Shared Clipboard | Bidirectional |
| Drag and Drop | Bidirectional |

#### 3. Shared Folder Configuration

A shared folder was configured between the host machine and Kali Linux.

| Parameter | Value |
|---|---|
| Host Folder | Windows Downloads Folder |
| Guest Folder | `/downloads` |
| Auto-Mount | Enabled |

This configuration enables convenient file transfer between the host operating system and the Kali Linux virtual machine.

### Phase 3: Static IP Address Configuration

The Kali Linux virtual machine was assigned a static IPv4 address.

**Configuration Steps:**

1. Start the Kali Linux virtual machine.
2. Open the network connection settings.
3. Edit the `Wired connection 1` profile.
4. Navigate to IPv4 Settings.
5. Set the addressing method to Manual.
6. Apply the following configuration:

| Parameter | Value |
|---|---|
| Method | Manual |
| IP Address | `10.0.0.2` |
| Netmask | `255.255.255.0` |
| Prefix Length | `/24` |
| Gateway | `10.0.0.1` |
| DNS Server | `8.8.8.8` |

7. Save the configuration and reconnect the network interface.

### Phase 4: Network & Internet Connectivity Verification

The network configuration was verified using basic connectivity tests.

#### 1. Verify the IP Address

```bash
ip addr show
```

This command displays the configured IP addresses and network interfaces.

#### 2. Verify Gateway Connectivity

```bash
ping -c 4 10.0.0.1
```

This test checks connectivity between Kali Linux and the NAT Network gateway.

#### 3. Verify Internet Connectivity

```bash
ping -c 4 google.com
```

This test checks external connectivity and DNS resolution.

#### 4. Verify Web Access

Firefox was used to confirm internet browsing functionality from within Kali Linux.

### Phase 5: Virtual Machine Snapshot

A snapshot was created to preserve the initial configured state of the Kali Linux virtual machine.

| Parameter | Value |
|---|---|
| Snapshot Name | `Snapshot 1` |
| Description | Base State: Configured IP Address `10.0.0.2/24` and verified internet connectivity |

The snapshot provides a recovery point for future cybersecurity practical exercises.

---

## 📸 Project Screenshots

Screenshots documenting the lab setup and configuration are maintained separately in the repository.

The `screenshots/` directory contains visual evidence of the completed implementation.

### Evidence Included

- VirtualBox NAT Network configuration
- Kali Linux network adapter settings
- Static IP address configuration
- Internet connectivity verification
- Shared folder configuration
- Virtual machine snapshot

These screenshots provide supporting evidence of the lab configuration and completed task.

---

## 🔧 Troubleshooting

### Kali Linux Internet Connectivity Issues

If Kali Linux experiences network connectivity problems, the following troubleshooting steps can be performed.

#### 1. Verify VirtualBox Network Settings

- Confirm that the NAT Network has been created.
- Verify that Kali Linux is connected to `NatNetwork`.
- Ensure that the virtual cable is connected.
- Check that the configured IP address is correct.

#### 2. Check for Duplicate IP Addresses

Ensure that no other virtual machine on the same network is using:

```text
10.0.0.2
```

#### 3. Apply NetworkManager Configuration Fix

For applicable VirtualBox 7.x and newer Kali Linux configurations, execute:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0

sudo nmcli connection down "Wired connection 1"

sudo nmcli connection up "Wired connection 1"
```

Restart the virtual machine and verify connectivity again.

---

## 📚 Learning Outcomes

Through this task, I gained practical experience in:

- Virtual machine deployment using Oracle VirtualBox.
- Configuring custom NAT Networks.
- Understanding IPv4 addressing and subnetting.
- Assigning static IP addresses in Kali Linux.
- Configuring network gateways and DNS servers.
- Verifying network and internet connectivity.
- Configuring shared folders and VM integration features.
- Managing virtual machine snapshots.
- Troubleshooting basic network connectivity issues.

---

## 🔮 Future Enhancements

The following tasks are planned for future lab phases:

- Deploy additional Windows virtual machines.
- Configure target machines within the `10.0.0.0/24` subnet.
- Perform inter-VM connectivity and ping tests.
- Practice network traffic analysis using Wireshark.
- Perform authorized security testing against intentionally vulnerable lab machines.
- Expand the lab environment for practical cybersecurity exercises.

---

## ✅ Project Status

**Week 1 — Project Module 1: Lab Setup**

The base Kali Linux testing environment has been configured with a static IP address, custom VirtualBox NAT Network, internet connectivity, shared folder integration, and snapshot management.

The initial lab environment is ready for future cybersecurity practical exercises.

**Completed:** Kali Linux lab setup and network configuration.

**Pending / Future Work:** Deployment of additional target virtual machines and inter-VM connectivity testing.

---

## ⚠️ Disclaimer

This lab environment is intended for educational purposes and authorized cybersecurity testing only.

All security testing activities should be conducted within systems and networks for which explicit permission has been granted.

---

**Project:** Cybersecurity Testing Lab Environment Setup  
**Platform:** Oracle VirtualBox  
**Operating System:** Kali Linux  
**Network:** `10.0.0.0/24`  
**Task:** Week 1 — Project Module 1
