# ESCARDE: Empowering Security of Containers with Advanced Runtime Detection 


![Image](https://i.ibb.co/tq4jfBZ/Untitled-design.png)  
This Documentation provides the required tools installation guide for environment setup of:

Container Runtime Threat Detection Solution PFE 2023 

Constantine 2 Universtiy X Octodet

Project made By:

- Soltane Mohcene
- Amoura Mohamed Elhadi 

Second Degree Software Engineering Students at University of Constantine 2, NTIC Faculty, TLSI Departement.

# Environment Setup

To conduct a security analysis of container runtime using eBPF-based tools, we must first set up our environment and install the necessary tools on the Linux Ubuntu environment.

<details>

<summary>Ubuntu</summary>


Ubuntu is a popular Linux distribution that is based on Debian and composed mostly of free and open-source software. It offers three editions: Desktop, Server, and Core for Internet of things devices and robots. Ubuntu is also a Nguni Bantu term that expresses the philosophy of “humanity” or “I am because we are”.

To Download and Install Ubuntu 22.04 LTS, refer to this: [Download Ubuntu Desktop](https://ubuntu.com/download/desktop) 

</details>

<details>

<summary>Linux Kernel with eBPF support</summary>



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
</details>

<details>

<summary>Docker Engine Installation</summary>



[Docker Engine Documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/install-elasticsearch.html)

Docker Engine is the software that runs and manages containers on a host machine. To use eBPF-based tools for security analysis of container runtime, we need to install and configure Docker Engine on our Linux system. Follow these steps to install and configure Docker Engine:

Install using the apt repository
Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

- Set up the repository
1. Update the `apt` package index and install packages to allow `apt` to use a repository over HTTPS:

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

</details>

<details>

<summary>Kubernetes Installation</summary>



[Kind Documentation](https://kind.sigs.k8s.io/docs/user/quick-start/)

Kubernetes is an open-source platform that orchestrates and scales containerized applications across multiple nodes. To use eBPF-based tools for security analysis of container runtime, we need to install and configure Kubernetes on our Linux system. In this case, we are using KIND, which is a tool that runs a local Kubernetes cluster using Docker containers as nodes. To install and configure Kubernetes with KIND, we need to follow these steps:
1. Kind install command:
   ```shell
   # For AMD64 / x86_64
   [ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
   chmod +x ./kind
   sudo mv ./kind /usr/local/bin/kind
   ```
2. Install kubectl binary using these commands:
   ```shell
   curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
   sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
   kubectl version --client

</details>

<details>

<summary>Cilium Installation</summary>

[Cilium Documentation](https://docs.cilium.io/en/v1.13/gettingstarted/k8s-install-default/)

`Cilium` is an open source project that uses eBPF to provide secure and observable connectivity for cloud native applications running on Kubernetes. `Cilium` can enforce network policies, monitor network flows, and perform service discovery and load balancing at the kernel level. To use eBPF-based tools for security analysis of container runtime, we need to install and configure Cilium on our machine:
   
      
      CILIUM_CLI_VERSION=$(curl -s https://raw.githubusercontent.com/cilium/cilium-cli/master/stable.txt)
      CLI_ARCH=amd64
      if [ "$(uname -m)" = "aarch64" ]; then CLI_ARCH=arm64; fi
      curl -L --fail --remote-name-all https://github.com/cilium/cilium-cli/releases/download/${CILIUM_CLI_VERSION}/cilium-linux-${CLI_ARCH}.tar.gz{,.sha256sum}
      sha256sum --check cilium-linux-${CLI_ARCH}.tar.gz.sha256sum
      sudo tar xzvfC cilium-linux-${CLI_ARCH}.tar.gz /usr/local/bin
      rm cilium-linux-${CLI_ARCH}.tar.gz{,.sha256sum}
</details>

<details>

<summary>Tetragon Installation</summary>

Tetragon is an open-source project that uses eBPF to perform security observability and
enforcement for a container runtime. Tetragon can filter and observe events and apply policies in real time without sending events to an agent running outside the kernel.
Tetragon can address numerous security and observability use cases such as syscall tracing, policy auditing, threat detection, forensics, and compliance. To install and configure
Tetragon on our Linux system, we need to follow these steps:

1. To install Tetragon, run the following commands:
    ```shell
   helm repo add cilium https://helm.cilium.io
   helm repo update
2. A second way is to pretty print the events using the `tetra CLI`. The tool also allows filtering by process, pod, and other fields.
   ```shell
   GOOS=$(go env GOOS)
   GOARCH=$(go env GOARCH)
   curl -L --remote-name-all             
   https://github.com/cilium/tetragon/releases/latest/download/tetra-${GOOS}-${GOARCH}.tar.gz{,.sha256sum}
   sha256sum --check tetra-${GOOS}-${GOARCH}.tar.gz.sha256sum
   sudo tar -C /usr/local/bin -xzvf tetra-${GOOS}-${GOARCH}.tar.gz
   rm tetra-${GOOS}-${GOARCH}.tar.gz{,.sha256sum}

</details>

<details>

<summary>Elasticsearch and Kibana Installation</summary>


Elasticsearch is a distributed engine for search and analytics of various data types. It
works with Logstash and Beats to collect, aggregate, and enrich data before storing it in
Elasticsearch. Kibana enables interactive exploration, visualization, and sharing of data
insights and management and monitoring of the Elastic Stack. Elasticsearch performs the
indexing, searching, and analyzing of data with near real-time efficiency. It can handle
any data type, such as structured or unstructured text, numerical data, or geospatial data.
It can also do complex aggregations to discover trends and patterns in data. And as data
and query volume grow, Elasticsearch can scale up smoothly to accommodate it. we can
install Elasticsearch following the next steps: 
1. Download and install the public signing key:
   ```shell
   wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor 
   -o /usr/share/keyrings/elasticsearch-keyring.gpg

2. The `apt-transport-https` package on Debian may need to be installed before proceeding:
   ```shell
   sudo apt-get install apt-transport-https

3. Saving the repository definition to `/etc/apt/sources.list.d/elastic-8.x.list`:
   ```shell
   echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] 
   https://artifacts.elastic.co/packages/8.x/apt       
   stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list

4. The Elasticsearch Debian package can be installed with:
   ```shell
   sudo apt -get update && sudo apt -get install elasticsearch

Kibana is the gateway to the Elastic Stack. With Kibana, the following can be done:

- Explore, observe, and secure the data. Whether discovering documents, analyzing logs, or finding security vulnerabilities, Kibana provides access to these features and more.
- Analyze the data. Hidden insights can be uncovered, visualized in charts, gauges, maps, graphs, and more, and combined in a dashboard.
- Manage, monitor, and secure the Elastic Stack. The data can be managed, the health of the Elastic Stack cluster can be monitored, and access to different features can be controlled.

To install Kibana, you can use the following command:

```shell
sudo apt-get update && sudo apt-get install kibana
```
</details><details>

<summary>Additional Tools</summary>

- `Helm v3`

```shell
sudo snap install helm --classic
```

- `Go Language`

```shell
sudo snap install go --classic
```

</details>


# Kubernetes GOAT Deployment

[Kubernetes Goat](https://github.com/madhuakula/kubernetes-goat). 


The Kubernetes Goat is designed to be an intentionally vulnerable cluster environment to learn and practice Kubernetes security


Also, Refer to https://madhuakula.com/kubernetes-goat for the guide

<details>

<summary>Cluster Creation</summary>

first of all, we create a KIND cluster with cilium CNI:

```shell
kind create cluster --config=kind-config.yaml
```
</details>

<details>

<summary>Cilium Installation</summary>

Install Cilium:
```shell
cilium install
```

</details>


<details>

<summary>Kubernetes Goat Deployment</summary>

To set up the Kubernetes Goat resources in your cluster, run the following commands:
```shell
git clone https://github.com/madhuakula/kubernetes-goat.git
cd kubernetes-goat
chmod +x setup-kubernetes-goat.sh
bash setup-kubernetes-goat.sh
```
Ensure the pods are running before running the access script
```shell
kubectl get pods
```
Access Kubernetes Goat by exposing the resources to the local system (port-forward) by the following command:
```shell
bash access-kubernetes-goat.sh
```
</details>

# Data Collection

<details>

<summary>Tetragon</summary>

Rolling out Tetragon
```shell
helm install tetragon cilium/tetragon -n kube-system
kubectl rollout status -n kube-system ds/tetragon -w
```
To begin with, we need to activate the feature that allows us to monitor the modifications of capability and namespace through the configmap. This can be done by changing
the values of `enable-process-cred` and `enable-process-ns` from false to true, running the
following command will open the configmap in a terminal editor:
```shell
kubectl edit cm -n kube -system tetragon -config
# change "enable-process-cred" from "false" to "true"
# change "enable-process-ns" from "false" to "true"
# then hit :wq
```

Enable File Access tracingPolicy:
```shell
kubectl apply -f https://raw.githubusercontent.com/cilium/tetragon/main/examples/tracingpolicy/sys_write_follow_fd_prefix.yaml
```
Enable Network Observability TracingPolicy:
```shell
kubectl apply -f https://raw.githubusercontent.com/cilium/tetragon/main/examples/tracingpolicy/tcp-connect.yaml
```


</details>


<details>

<summary>Tetragon Logs path</summary>

To locate the stored logs from Tetragon, we need to access the cluster files. For that, we
have to find the Docker container that hosts this cluster.

This command will let us see the existing containers in Docker by the Container ID
and Container Name:
```shell
docker ps
```
```shell
#  If the container is not running, we need to start it before using the docker exec command. Use the docker start command followed by the container ID.
docker start <container_id>
```
We access the cluster node and explore the Tetragon pod for the logs files.
```shell
# this will let us access the cluster node
docker exec -it kind-control-plane /bin/bash
# this allow us to get on the pods directory 
cd var/log/pods
# this will show directories and files under pods directory 
ls
```
To get the full path to the log files, use the `pwd` command.
```shell
cd export-stdout
#then
pwd
```
</details>

<details>

<summary>Elastic-Agent and Fleet server configuration</summary>

1. Get on Kibana: `menu > fleet > add fleet server` and follow the steps



2. Copy the installation script after configuring the fleet ip address same as Elasticsearch.

![Image](https://i.imgur.com/6mwtt4h.png)

3. Get on the Cluster Host:

```shell
docker exec -it kind-control-plane /bin/bash
```

4. Paste the script to install Elastic agent.

![Image](https://i.imgur.com/oKUqXH0.png)

6. Wait until the Fleet is connected to Elastic agent and recognised.

![Image](https://i.imgur.com/MN7w4A2.png)

</details>


<details>

<summary>Configuring Custom Logs Integration</summary>

Install custopm logs integration for the fleet server policies.
and use the following path `/var/log/pods/kube-system_tetragon-*/export-stdout/*.log`

![image](https://i.imgur.com/dzYNBXc.png)

</details>


# Data Normalisation
To normalize the collected data, we need to ingest a pipeline `log-tetra-default` that contains many processors, each processor having a role in transforming, enriching, or filtering the data.
| **Processor**        | **Description**           |
| ------------- | ------------- |
| Dissect      | Splits a field into multiple fields using a delimiter. |
| JSON     | Parses a JSON string and adds it as a new object field. |
| Rename | Renames an existing field to a new name. |
| Split  | Splits a field into an array using a separator character. |
| Set | Sets the value of a field to a predefined value. |
| Script | Executes a script to modify documents before indexing. |
| Remove | Removes one or more fields from the document. |
<details>

<summary> Ingest Pipeline </summary>

To add the pipeline, you need to go to `Menu > Dev Tools > Console` in Kibana and paste the content of `log-tetra-default.json`:

![Image](https://i.imgur.com/SqwUGY8.png)


</details>

<details>
<summary> Apply Pipeline in Custom Logs Integration </summary>
   
Add the pipeline in the Processor field on the Advanced Options section.

![Image](https://i.imgur.com/IyVr9Xf.png)

</details>

# Threat Detection

This is how we performed the container escape attack. With the threat detection rule we created, we were able to trigger an alert in Kibana and Email notification.

https://github.com/aminaeldj/Octodet-/assets/121443476/030e4e78-003a-44db-b17f-cff7d66596e0
