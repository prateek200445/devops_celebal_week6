Ques 1 : **Deploy Replica Set and Replication Controller, and
deployment. Also learn the advantages and disadvantages of each.**

SOLN:

FIRST WE WILL DEPLOY THESE USING BASIC KUBERNETES LOCAL CLUSTER AND TRY
TO CONFIGURE THE

1\> REPLICATION CONTROLLER

2\> REPLICA SET

3\> DEPLOYMENT

**Steps to Deploy ReplicationController, ReplicaSet & Deployment Using
*****kubeadm***** Kubernetes Cluster :**

### **Prerequisites**

Ensure:

- You have **2 Linux VMs** (Ubuntu preferred) --- 1 master + 1 worker
- *kubeadm*, *kubelet*, *kubectl*, *docker/containerd* are installed

SO LETS GO WITH THE INSTALLATIONS :

COMMANDS I HAVE WRITTEN DOWN HERE FOR THE FULL INSTALLATIONS WE HAVE
DONE IT IN ASSIGNMENT 5 FROM THE SRATCH

### Kubernetes & Containerd Setup (All-in-One Script) {#kubernetes-containerd-setup-all-in-one-script}

*sudo apt update && sudo apt install -y apt-transport-https curl
ca-certificates gnupg lsb-release && curl -fsSL
https://packages.cloud.google.com/apt/doc/apt-key.gpg \| sudo gpg
\--dearmor -o /usr/share/keyrings/kubernetes-archive-keyring.gpg && echo
\"deb \[signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg\]
https://apt.kubernetes.io/ kubernetes-xenial main\" \| sudo tee
/etc/apt/sources.list.d/kubernetes.list && sudo apt update && sudo apt
install -y kubelet kubeadm kubectl containerd && sudo apt-mark hold
kubelet kubeadm kubectl*

### 🔧 After that, configure containerd (required): {#after-that-configure-containerd-required}

*sudo mkdir -p /etc/containerd && containerd config default \| sudo tee
/etc/containerd/config.toml \> /dev/null && sudo systemctl restart
containerd && sudo systemctl enable containerd*

### **Local Kubernetes Setup (kubeadm)**

#### 1️ **Initial Setup on Master Node** {#initial-setup-on-master-node}

**sudo kubeadm init --pod-network-cidr=192.168.0.0/16**

#### **2️ Configure kubectl for Master** {#configure-kubectl-for-master}

*mkdir -p \$HOME/.kube*

*sudo cp /etc/kubernetes/admin.conf \$HOME/.kube/config*

*sudo chown \$(id -u):\$(id -g) \$HOME/.kube/config*

****3 **Install Pod Network (e.g., Calico or Flannel) **

kubectlapply-f
<https://raw.githubusercontent.com/projectcalico/calico/v3.26.1/manifests/calico.yaml>

![](Pictures/100000000000078000000438712216A0.png){width="17cm"
height="9.562cm"}

Join Worker Node to Cluster

sudo kubeadm join \<master-ip\>:6443 \--token \<token\>
\--discovery-token-ca-cert-hash sha256:\<hash\>

kubectl get nodes

![](Pictures/10000001000002E3000001FACA5C8E82.png){width="15.48cm"
height="10.599cm"}kubectl get pods -A

![](Pictures/10000001000003C4000001FD5B5788E2.png){width="17cm"
height="8.975cm"}

master@master:\~\$ kubectl apply -f replicationcontroller.yaml
\--validate=false

replicationcontroller/nginx-rc created

![](Pictures/10000001000002F6000001B61DFE6736.png){width="17cm"
height="9.823cm"}

![](Pictures/10000001000004000000011932AEDF0A.png){width="17cm"
height="4.664cm"}

### **Deploy Your Application**

#### 1. ReplicationController {#replicationcontroller}

*kubectl apply -f replicationcontroller.yaml*

#### 2. ReplicaSet {#replicaset}

*kubectl apply -f replicaset.yaml*

#### 3. Deployment {#deployment}

*kubectl apply -f deployment.yaml*

#### 4. Service {#service}

*kubectl apply -f service.yaml*

## Step-by-Step: Create & Deploy *ReplicaSet* {#step-by-step-create-deploy-replicaset}

### 📝 1. Create a file called *replicaset.yaml* {#create-a-file-called-replicaset.yaml}

In your terminal:

*nano replicaset.yaml*

Paste the following content:

yaml

CopyEdit

*apiVersion: apps/v1*

*kind: ReplicaSet*

*metadata:*

* name: nginx-rs*

* labels:*

* app: nginx*

*spec:*

* replicas: 2*

* selector:*

* matchLabels:*

* app: nginx*

* template:*

* metadata:*

* labels:*

* app: nginx*

* spec:*

* containers:*

*  - name: nginx*

* image: nginx:latest*

* ports:*

*  - containerPort: 80*

Save and exit:

- Press *Ctrl + O*, then *Enter* to save
- Press *Ctrl + X* to exit

### 

### 

### 2. Apply the ReplicaSet {#apply-the-replicaset}

*kubectl apply -f replicaset.yaml*

###  3. Confirm it's Running {#confirm-its-running}

*kubectl get rs*

*kubectl get pods -l app=nginx*

This should show 2 pods created by the ReplicaSet.

###  4. Bonus (Optional): Delete one pod an {#bonus-optional-delete-one-pod-an}

![](Pictures/1000000000000780000004388BF1D462.png){width="13.843cm"
height="7.786cm"}

Lets configure the same using cloud native development :

Using AKS ( Azure Kubernetes services ):

FIRST CREATE A AKS CLUSTER USING AZURE PORTAL AND THE CONFIGURATIONS AS
GIVEN BELOW :

THEN

****

****1. Connect to AKS Cluster****

command used = az aks get-credentials \--resource-group
\<your-resource-group\> \--name \<your-cluster-name\>

kubectl get nodes \# Confirm connection

![](Pictures/100000010000034A000000F01879FFAA.png){width="17cm"
height="4.844cm"}

**2. Package Your App into a Docker Image**

THAT I ALREADY HAD YOU CAN ALSO ACCESS THE IMAGE AT THIS

**3. Write 3 Separate YAML Files**

**a. ReplicationController YAML**

@\"

apiVersion: v1

kind: ReplicationController

metadata:

name: myapp-rc

spec:

replicas: 2

selector:

app: myapp

template:

metadata:

labels:

app: myapp

spec:

containers:

 - name: myapp-container

image: prateek2004/my-frontend

ports:

 - containerPort: 80

\"@ \| Out-File -Encoding UTF8 -FilePath .\replicationcontroller.yaml

![](Pictures/10000001000002580000014992E5F4F1.png){width="15.877cm"
height="8.707cm"}

B. ReplicaSet YAML

@\"

apiVersion: apps/v1

kind: ReplicaSet

metadata:

name: myapp-rs

spec:

replicas: 2

selector:

matchLabels:

app: myapp

template:

metadata:

labels:

app: myapp

spec:

containers:

 - name: myapp-container

image: prateek2004/my-frontend

ports:

 - containerPort: 80

\"@ \| Out-File -Encoding UTF8 -FilePath .\replicaset.yaml

![](Pictures/100000010000037100000156A1F17F0B.png){width="17cm"
height="6.599cm"}

C. Deployment YAML

@\"

apiVersion: apps/v1

kind: Deployment

metadata:

name: myapp-deploy

spec:

replicas: 2

selector:

matchLabels:

app: myapp

template:

metadata:

labels:

app: myapp

spec:

containers:

 - name: myapp-container

image: prateek2004/my-frontend

ports:

 - containerPort: 80

\"@ \| Out-File -Encoding UTF8 -FilePath .\deployment.yaml

![](Pictures/10000001000002CC0000015859EA1C5D.png){width="17cm"
height="8.167cm"}

NOW LETS ALSO CREATE A SERVICE CONFIGURATION FOR THE SAME DEPLOYMENT TO
VISIBLE A PUBLIC IP :

#### Service YAML

*@\"*

*apiVersion: v1*

*kind: Service*

*metadata:*

* name: myapp-service*

*spec:*

* selector:*

* app: myapp*

* type: LoadBalancer*

* ports:*

*  - port: 80*

* targetPort: 80*

*\"@ \| Out-File -Encoding UTF8 -FilePath .\service.yaml*

![](Pictures/100000010000027D000000D188C539CC.png){width="16.856cm"
height="5.53cm"}

Apply Files to AKS :

command used :

kubectl apply -f .\replicationcontroller.yaml

kubectl apply -f .\replicaset.yaml

kubectl apply -f .\deployment.yaml

kubectl apply -f .\service.yaml

![](Pictures/1000000100000270000000995E79CE4D.png){width="16.512cm"
height="4.048cm"}

## Next Steps: Verify the Setup

### 1. ✅ Check All Resources {#check-all-resources}

kubectl get all

**EVERYTHING UPTO THE POINT** :

![](Pictures/10000001000003BE000001C3252D0497.png){width="17cm"
height="8.003cm"}

YOU CAN GET THE SERVICE USING :

kubectl get svc myapp-service

![](Pictures/10000001000002B1000000637A70A79C.png){width="17cm"
height="2.441cm"}**URL:** http://**4.213.203.76** it can be accessed on
the web

![](Pictures/1000000000000780000004388BF1D462.png){width="13.843cm"
height="7.786cm"}

## Recap of What We've Done:

|                           |                              |                                                           |
|---------------------------|------------------------------|-----------------------------------------------------------|
| **ReplicationController** | ✅ Created                   | Manages legacy pods (less commonly used now)              |
| **ReplicaSet**            | ✅ Created                   | Modern controller ensuring fixed number of identical pods |
| **Deployment**            | ✅ Created                   | Manages rollout, updates, rollback over ReplicaSets       |
| **Service**               | ✅ LoadBalancer              | Exposes app to the internet                               |
| **Docker Image**          | ✅ *prateek2004/my-frontend* | custom personal web app                                   |

Advantages and Disadvantages

|                           |                                                                                                                     |                                                                      |
|---------------------------|---------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------|
| **ReplicationController** | \- Ensures desired pod count- Was Kubernetes\' first controller                                                     | \- **Deprecated** for most use-cases- No rolling updates or rollback |
| **ReplicaSet**            | \- Ensures desired number of pods- Supports label-based selection                                                   | \- Doesn't support **rollbacks or declarative updates** on its own   |
| **Deployment**            | \- Built on top of ReplicaSet- Supports **rolling updates**, rollback, pause/resume- Most widely used in real-world | \- Slightly more complex YAML structure                              |

To check the **use case and behavior** of each controller ---
**ReplicationController**, **ReplicaSet**, and **Deployment (manager)**
--- here's how you can **observe, test, and differentiate** them
practically

1\. **Check Which Pod Was Created by Which Controller**

kubectl get pods --show-labels

kubectl describe pod \<pod-name\>

![](Pictures/1000000100000373000001D0DDEAED4A.png){width="17cm"
height="8.932cm"}HENCE In the description, check the *****Controlled
By***** section:

- If it says **ReplicationController**, the pod was managed by it.
- If it says **ReplicaSet**, same logic.
- For Deployment, it will show Deployment → ReplicaSet → Pods.

SO HERE ITS BY REPLICATION CONTROLLER

2\. **LETS** **Simulate a Pod Failure**

Manually delete a pod and observe:

*kubectl delete pod \<pod-name\>*

Now check:

kubectl get pods

You'll see that the controller **automatically recreates** the deleted
pod.  
This demonstrates the **self-healing** use case.

myapp-rc-pdnb9 1/1 Running 0 18s

REGENERATED 18SECS BEFORE SHOWING SELF HEAL PROPERTY

3\. **Check Ownership and Hierarchy**

kubectl get rs

kubectl describe rs \<replicaset-name\>

You'll see:

- If a ReplicaSet is **created by a Deployment**, it says so in
  \"Controlled By\".
- If it's **standalone**, it has no owner --- useful when comparing RS
  vs Deployment.

![](Pictures/10000001000002E0000001B177C50082.png){width="17cm"
height="10.001cm"}

4\. **Try a Rolling Update (Deployment Only)**

kubectl set image deployment/myapp-deploy myapp-container=nginx

![](Pictures/10000001000002FF0000005D645EF81A.png){width="17cm"
height="2.06cm"}

NOW CHECK:

kubectl rollout status deployment/myapp-deploy

![](Pictures/100000010000024C0000002DAD35C4F5.png){width="15.559cm"
height="1.191cm"}  
![](Pictures/10000001000002DD000000C0996ADBF2.png){width="17cm"
height="4.452cm"}

Visit the *EXTERNAL-IP* --- you'll now see the **NGINX welcome page**

![](Pictures/1000000000000780000004387F9B75B5.png){width="17cm"
height="9.562cm"}

ROLLED AGAIN TO ORIGINAL:

![](Pictures/1000000000000780000004389F1A4C45.png){width="17cm"
height="9.562cm"}

In this task, I deployed a **ReplicationController**, a **ReplicaSet**,
and a **Deployment** in Azure Kubernetes Service (AKS) to manage my
custom web application hosted on Docker Hub
(*prateek2004/my-frontend*).  
Each of these ensures high availability by maintaining multiple replicas
(2 pods) of the application.  
I used YAML files to define and apply all three controllers, and exposed
them via a **LoadBalancer Service** for external access.  
I also performed a **rolling update** on the Deployment to test seamless
version changes (from my image to *nginx* and back).  
To verify, I checked pod health, ReplicaSets, rollout history, and
container images using various *kubectl* commands.  
In addition, I learned the ownership difference between RC, RS, and
Deployment, and how Deployment provides better lifecycle and update
management

### ****

### ****Extra****S ****: **** {#extras}

- Switched from self-hosted kubeadm to Azure AKS due to reliability.
- Created Docker image of a personal project and hosted on Docker Hub.
- Understood and compared the behaviors of ReplicaSet vs
  ReplicationController vs Deployment.
- Used *kubectl get*, *set image*, *rollout status*, and *describe* for
  deeper insights.
- Created a LoadBalancer-type service to make the app publicly
  accessible.
