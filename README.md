# Empowering Security with Advances Runtime Intrusion detection 


![alt text](image.jpg)

# Environment Setup

## Linux Kernel with eBPF support

eBPF is a technology that enables users to insert custom programs into the Linux kernel dynamically and access and modify various kernel features. We need a Linux kernel that supports eBPF to use eBPF-based tools for security analysis of container runtime. The earliest kernel version that supports eBPF is 4.16. However, eBPF programs may benefit from more features and stability in newer kernel versions. 

To set up a Linux kernel with eBPF support on Ubuntu 22.04 LTS, follow these steps:

1. Update the package lists and install the `linux-generic-hwe-22.04` package using the `apt` command. This package provides the latest hardware enablement kernel for Ubuntu 22.04 LTS, which supports eBPF by default.

   ```shell
   sudo apt update
   sudo apt install linux-generic-hwe-22.04
