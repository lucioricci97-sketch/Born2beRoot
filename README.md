*This project has been created as part of the 42 curriculum by luricci.*

## Description
**Born2beRoot** is an introductory project to the world of virtualization and system administration. The goal is to create a secure, highly configured server using a Virtual Machine (VM).

This project requires setting up a **Debian** operating system with strict rules regarding storage (LVM), security (Password policies, Firewall), and privilege management (Sudo). It also involves automating system monitoring tasks using Bash scripts and Cron jobs.

## Instructions
Since the Virtual Machine file (`.vdi`) is too large for the Git repository, this repository contains the **digital signature** of the machine to verify its integrity.

### How to Verify the Signature
To validate that the VM has not been modified since submission, follow these steps during the evaluation:

1. Locate the `born2beroot.vdi` file on the local machine.
2. Run the checksum command corresponding to your OS:
   * **Linux/Mac:** `shasum born2beroot.vdi`
   * **Windows:** `Get-FileHash .\born2beroot.vdi -Algorithm SHA1`
3. Compare the output with the hash provided in `signature.txt`.

### Execution
Once the signature is verified:
1. Open **VirtualBox**.
2. Start the VM named `Born2beRoot`.
3. Log in using the credentials set up during the defense.
4. The monitoring script will automatically broadcast system info every 10 minutes.

## Resources
* [Debian Documentation](https://www.debian.org/doc/)
* [UFW Manual](https://manpages.debian.org/unstable/ufw/ufw.8.en.html)
* [LVM Guide (Arch Wiki)](https://wiki.archlinux.org/title/LVM)
* **YouTube Tutorials:** Used for visual guides on partitioning and VirtualBox configuration.

### AI Usage
Artificial Intelligence (Gemini) was used as a learning assistant and troubleshooting partner during this project.

* **Tasks:**
    * **Concept Explanation:** Clarifying the differences between partitions and LVM, and explaining the specific differences between AppArmor and SELinux.
    * **Scripting:** Assisted in writing and debugging the Bash monitoring script to correctly extract system information (CPU load, disk usage, etc.) using `awk` and `grep`.
    * **Troubleshooting:** Helping resolve specific errors regarding SSH connection permission denied and Sudo configuration.
    * **Documentation:** Used AI to generate the structure and content of this `README.md` file to ensure it complies with the subject requirements.
    * **Guidance:** Verified the order of operations for generating the VM signature to ensure project compliance.

## Project Description

### Operating System Choice
For this project, I chose **Debian (Stable)**.
* **Why Debian?** It is renowned for its stability, security, and vast community support. It uses the `apt` package manager and `.deb` format, which are widely documented. It is a community-driven project (free software), making it ideal for educational purposes.

### Design Choices
* **Partitioning:** Used **LVM (Logical Volume Manager)** to allow for dynamic resizing of partitions and flexible storage management without reformatting.
* **Security Policies:**
    * **SSH:** Configured on port **4242** (non-standard) to reduce scanner attacks; Root login disabled.
    * **Firewall:** **UFW** enables only port 4242.
    * **Password Policy:** strict rules (min 10 chars, uppercase, lowercase, digits) enforced via `libpam-pwquality`.
* **User Management:** Root access is restricted. A dedicated user is assigned to groups `sudo` and `user42`. Sudo usage is strictly logged in `/var/log/sudo/`.

### Comparisons

#### 1. Debian vs Rocky Linux
* **Debian:** Uses `apt` and `dpkg`. Known for extreme stability ("Stable" branch) and being the base for Ubuntu. It is purely community-driven.
* **Rocky Linux:** Uses `dnf` and `rpm`. It is a downstream, binary-compatible build of RHEL (Red Hat Enterprise Linux). It replaces CentOS and is geared more toward enterprise server environments that require Red Hat compatibility.

#### 2. AppArmor vs SELinux
* **AppArmor (Used in Debian):** Uses a **Path-based** approach. It restricts programs based on the executable's path. It is generally considered easier to configure and learn for beginners.
* **SELinux (Used in Rocky/RHEL):** Uses a **Label-based** approach (inodes). It assigns security context labels to every file and process. It is more granular and powerful but has a steeper learning curve and is harder to troubleshoot.

#### 3. UFW vs Firewalld
* **UFW (Uncomplicated Firewall):** A simplified interface for `iptables`. It is designed to be easy to use with simple commands (e.g., `ufw allow 4242`). Standard on Debian/Ubuntu.
* **Firewalld:** Uses "Zones" and services to manage traffic. It is more dynamic (changes apply instantly without reloading connections) and is the standard for Red Hat/Rocky Linux.

#### 4. VirtualBox vs UTM
* **VirtualBox:** A Type-2 hypervisor developed by Oracle. It is widely used on x86/x64 hardware (Intel/AMD) across Windows, Linux, and Mac. It is mature and feature-rich for standard virtualization.
* **UTM:** A virtualization software for macOS (specifically optimized for Apple Silicon M1/M2/M3 chips) that uses QEMU under the hood. It allows running x86 operating systems on ARM hardware via emulation, which VirtualBox does not support efficiently on Apple Silicon.

