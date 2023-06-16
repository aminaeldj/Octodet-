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

2. Optionally, install the linux-tools-generic-hwe-22.04 package using the apt command. This package provides tools for working with eBPF programs.
   
     ```shell
      sudo apt install linux-tools-generic-hwe-22.04
3. Reboot the system and verify that the new kernel is running with eBPF support. You can use the following commands:
   
    ```shell
    uname -r
    bpftool feature
    
 to Check the kernel version and the eBPF features available on your system.

## Docker Engine Installation

Docker Engine is the software that runs and manages containers on a host machine. To use eBPF-based tools for security analysis of container runtime, we need to install and configure Docker Engine on our Linux system. Follow these steps to install and configure Docker Engine:

Install using the apt repository
Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

- Set up the repository
1. Update the 'apt' package index and install packages to allow 'apt' to use a repository over HTTPS:

   ```shell
   sudo apt-get update
   sudo apt-get install ca-certificates curl gnupg

2. Add Docker’s official GPG key:
   ```shell
   sudo install -m 0755 -d /etc/apt/keyrings
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
   sudo chmod a+r /etc/apt/keyrings/docker.gpg
3. Use the following command to set up the repository:
   ```shell
    echo \
    "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] 
    https://download.docker.com/linux/ubuntu \
    "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
- Install Docker Engine
1. Update the 'apt' package index:
   ```shell
   sudo apt-get update
2. Install Docker Engine, containerd, and Docker Compose, this should install the latest version of docker engine for ubuntu, run: 
   ```shell
   sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

3. Verify that the Docker Engine installation is successful by running the 'hello-world' image (Optional)
   ```shell
   sudo docker run hello-world

## Kubernetes Installation
Kubernetes is an open-source platform that orchestrates and scales containerized appli-
cations across multiple nodes. To use eBPF-based tools for security analysis of container
runtime, we need to install and configure Kubernetes on our Linux system. In this case,
we are using KIND, which is a tool that runs a local Kubernetes cluster using Docker
containers as nodes. To install and configure Kubernetes with KIND, we need to follow
these steps:
1. Kind install command:
   ```shell
   curl -Lo ./ kind
   https :// kind.sigs.k8s.io/dl/v0 .18.0/ kind -linux - amd64
   chmod +x ./ kind
   sudo mv ./ kind /usr/ local /bin/kind
2. Install kubectl binary using these commands:
   ```shell
   curl -LO " https :// dl.k8s.io/ release /$(curl -L -s
   https :// dl.k8s.io/ release / stable .txt)/bin/ linux /
   amd64 / kubectl "
   sudo install -o root -g root -m 0755 kubectl
   /usr/local /bin/ kubectl
## Cilium Installation:
Cilium is an open source project that uses eBPF to provide secure and observable connectivity for cloud native applications running on Kubernetes. Cilium can enforce network policies, monitor network flows, and perform service discovery and load balancing at the kernel level. To use eBPF-based tools for security analysis of container runtime, we need to install and configure Cilium on our machine:
   
      
      CILIUM_CLI_VERSION=$(curl -s https://raw.githubusercontent.com/cilium/cilium-cli/master/stable.txt)
      CLI_ARCH=amd64
      if [ "$(uname -m)" = "aarch64" ]; then CLI_ARCH=arm64; fi
      curl -L --fail --remote-name-all https://github.com/cilium/cilium-            cli/releases/download/${CILIUM_CLI_VERSION}/cilium-      linux-${CLI_ARCH}.tar.gz{,.sha256sum}
      sha256sum --check cilium-linux-${CLI_ARCH}.tar.gz.sha256sum
      sudo tar xzvfC cilium-linux-${CLI_ARCH}.tar.gz /usr/local/bin
      rm cilium-linux-${CLI_ARCH}.tar.gz{,.sha256sum}


[Cilium Documentation](https://docs.cilium.io/en/v1.13/gettingstarted/k8s-install-default/)

## Tetragon Installation
Tetragon is an open-source project that uses eBPF to perform security observability and
enforcement for a container runtime. Tetragon can filter and observe events and ap-
ply policies in real time without sending events to an agent running outside the kernel.
Tetragon can address numerous security and observability use cases such as syscall trac-
ing, policy auditing, threat detection, forensics, and compliance. To install and configure
Tetragon on our Linux system, we need to follow these steps:

1. To install Tetragon, run the following commands:
    ```shell
   helm repo add cilium https :// helm. cilium .io
   helm repo update
   helm install tetragon cilium / tetragon -n kube - system
2. A second way is to pretty print the events using the 'tetra CLI'. The tool also allows filtering by process, pod, and other fields.
   ```shell
   GOOS=$(go env GOOS)
   GOARCH=$(go env GOARCH)
   curl -L --remote-name-all             https://github.com/cilium/tetragon/releases/latest/download/tetra-${GOOS}-${GOARCH}.tar.gz{,.sha256sum}
   sha256sum --check tetra-${GOOS}-${GOARCH}.tar.gz.sha256sum
   sudo tar -C /usr/local/bin -xzvf tetra-${GOOS}-${GOARCH}.tar.gz
   rm tetra-${GOOS}-${GOARCH}.tar.gz{,.sha256sum}

## Elasticsearch and Kibana Installation




