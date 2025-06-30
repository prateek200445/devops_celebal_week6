# devops_celebal_week6
# Kubernetes Assignment - AKS, Controllers, Services, Scaling & More
This repository documents the solutions I implemented as part of a Kubernetes assignment. Each task has been solved practically using YAML files, Azure Kubernetes Service (AKS), kubectl commands, and Docker. The assignment includes key Kubernetes components such as ReplicaSets, Deployments, Services, Probes, Scaling, and Storage.

🔧  ****Q1: Deploy Replica Set, Replication Controller, and Deployment**
Objective: Understand differences and implement all three Kubernetes controllers.**

Solutions Covered:

Local setup using kubeadm

AKS cluster deployment

Docker image deployment

Rolling updates, self-healing test, hierarchy validation

YAML configs for RC, RS, Deployment, Service

📄 Solution PDF: ques1.pdf
📚 Reference: YouTube Video

🌐 ****Q2: Kubernetes Service Types (ClusterIP, NodePort, LoadBalancer)**
Objective: Demonstrate the use of all 3 service types.**
Highlights:

Created LoadBalancer service on AKS

Configured ClusterIP and NodePort services

Explained flow of traffic and cloud usage

📄 Solution PDF: ques2.pdf
📚 Reference: YouTube Video

💾   Q3:** PersistentVolume (PV) & PersistentVolumeClaim (PVC)**
Objective: Implement persistent storage for applications
Highlights:

Created PV & PVC

Mounted volumes to pods

Verified persistent data storage with shell commands

📄 Solution PDF: ques3 and 7.pdf
📚 Reference: YouTube Video

☁️ **Q4: Managing Kubernetes with AKS**
Objective: Learn scaling, upgrading AKS clusters.
Highlights:

Enabled cluster autoscaler

Upgraded AKS version

Used CLI commands for real-world cloud scaling

📄 Solution PDF: ques4.pdf
📚 Reference: Azure Docs

❤️‍🔥  **Q5: Configure Liveness and Readiness Probes****
Objective: Improve app health monitoring in AKS.
Highlights:

Configured probes in deployment YAML

Simulated failure and recovery

Explained real-life importance in prod systems

📄 Solution PDF: ques 5 and 8.pdf
📚 Reference: YouTube Video

🧭 **Q6: Configure Taints and Tolerations**
Objective: Restrict workloads to specific nodes.
Highlights:

Tainted specific node

Added toleration in pod YAML

Explained scenario with dedicated GPU workloads

📄 Solution PDF: ques6.pdf
📚 Reference: YouTube Video

🗄️ **Q7: Mount PVC to Deployment**
Objective: Use PV/PVC in deployed applications
Highlights:

Modified existing deployment

Mounted persistent volume

Tested using in-container file creation

📄 Solution PDF: ques3 and 7.pdf
📚 Reference: Azure Docs

⚙️ **Q8: Health Probe Simulation**
Objective: Understand and simulate Kubernetes’ self-healing.
Highlights:

Deliberate misconfiguration of liveness probe

Observed pod restart behavior

Demonstrated resiliency

📄 Solution PDF: ques 5 and 8.pdf
📚 Reference: YouTube Video

📈 **Q9: Configure Horizontal Pod Autoscaler (HPA)**
Objective: Scale pods automatically based on CPU usage.
Highlights:

Installed metrics-server

Configured HPA using kubectl

Simulated CPU load to observe autoscaling

📄 Solution PDF: ques_9.pdf
📚 Reference: YouTube Video

In this repo there is a complete conceptual and practical undestanding of the AKS services also knwn as azure kubernetes services

Public Access to Deployed App
Public IP (via LoadBalancer):
http://4.213.203.76
↳ This IP was provisioned automatically by Azure AKS using the LoadBalancer service type.

Docker Image Used
docker pull prateek2004/my-frontend

