
<img width="1193" height="348" alt="image" src="https://github.com/user-attachments/assets/b0d7430d-fe08-49a0-8829-08ac0a670ef0" />

Config 
- master,e2-custom-2-6144,us-east1-d,debian-12

- worker-1,e2-custom-2-6144,us-east1-b,debian-12

--------------------------------------------------------------------------------------------------------

Swap memory must be disabled 
	
		swapon -s 
		
		swapoff -a
		
	SELinux  should disabled or permissive. 

			vim /etc/sysconfig/selinux

			SELINUX=disabled
			
Note:  IN Linux there is only one service which is SELinux require restart 


#On Debian 12 (bookworm)

You don’t need to disable SELinux (because it isn’t active).

You only need to make sure AppArmor doesn’t block Kubernetes pods. Normally kubelet & containerd work fine with AppArmor. 

<img width="528" height="62" alt="image" src="https://github.com/user-attachments/assets/6b363aaf-0e6d-42a7-b4fe-d78bf5576118" />


---------------------------------------------------------------------------------------------------------------------------------
- Ports Needs to be open 

  2379/tcp: Kubernetes etcd server client API (on master nodes in multi-master deployments)

  2380/tcp: Kubernetes etcd server client API (on master nodes in multi-master deployments)

  6443/tcp: Kubernetes API server (master nodes)

  8090/tcp: Platform Agent (master and worker nodes)

  8091/tcp: Platform API Server (operator node)

  8472/udp: Flannel overlay network, VxLAN backend (master and worker nodes)   [CNI]

  10250/tcp: Kubernetes kubelet API server (master and worker nodes)

  10251/tcp: Kubernetes kube-scheduler (on master nodes in multi-master deployments)

  10252/tcp: Kubernetes kube-controller-manager (on master nodes in multi-master deployments)

  10255/tcp: Kubernetes kubelet API server for read-only access with no authentication (master and worker nodes)

 <img width="798" height="366" alt="image" src="https://github.com/user-attachments/assets/9061e8f2-9f91-48e8-804a-b42f7a6c82b4" />


 -------------------------------------------------------------------------------------------------------- 

1.2 set hostname and note down IP address  (private IP only)				[ All VM ]

			hostnamectl set-hostname master
			hostnamectl set-hostname worker-1
			hostnamectl set-hostname worker-2

vi /etc/hosts 
	
10.0.0.5	master
10.0.0.8	worker-1
10.0.0.9	worker-2

<img width="685" height="641" alt="image" src="https://github.com/user-attachments/assets/5da256b5-fb3a-433f-ad93-f15224df9a29" />


--------------------------------------------------------------------------------------------------------------------

1.3  Public and private generate for communication between master and wn1   (ONLY ON MASTER NODE)

			Goto master node and generate public and private key :

				ssh-keygen  -t  rsa

<img width="654" height="441" alt="image" src="https://github.com/user-attachments/assets/fe0756f2-bea5-4f6d-9004-2b6341d97fa3" />



		now copy your public key and share with all worker node.

		 cat  /root/.ssh/id_rsa.pub

<img width="837" height="590" alt="image" src="https://github.com/user-attachments/assets/f3c3d778-9448-4737-bf36-fa8643e4c879" />


	1.4  Now login to worker1 and 2  machine and open following file with user root
	
	
		vi  /root/.ssh/authorized_keys
 		 Now paste here your master node public key
	
<img width="887" height="486" alt="image" src="https://github.com/user-attachments/assets/a8b2610f-17b4-49e5-bafc-abbd7a82412a" />

		
			
			save and exit 

   -----------------------------------------------------------------------------------------

   - Expected Errors (may be) on Worker node 

<img width="764" height="94" alt="image" src="https://github.com/user-attachments/assets/378c7b05-e71b-4e90-bad9-d0e5b9e98db1" />

Set the correct permissions 

chmod 700 /root/.ssh

chmod 600 /root/.ssh/authorized_keys

chown -R root:root /root/.ssh

Cat  /etc/ssh/sshd_config

check the belwo permoissions 


<img width="698" height="259" alt="image" src="https://github.com/user-attachments/assets/341b4702-24b5-48de-8bcf-bdf1350b9a0c" />

Set the permissions to YES 


<img width="786" height="324" alt="image" src="https://github.com/user-attachments/assets/a7d61fde-d79c-4658-b354-fd7be7bebc6a" />


Test the connection from Master node 


<img width="870" height="221" alt="image" src="https://github.com/user-attachments/assets/b23b982c-e842-45b4-9874-8abd3751d4d8" />

-------------------------------------------------------------------------------------------------------------------------------------

Step 2 Add the kernal argument for Ipv4 and Ipv6 traffic (on worker as well as Master)

	vi   /etc/sysctl.d/k8s.conf		
		
net.bridge.bridge-nf-call-ip6tables = 1

net.bridge.bridge-nf-call-iptables = 1


<img width="617" height="75" alt="image" src="https://github.com/user-attachments/assets/7b1d7aba-7e30-4913-8c84-bd2f1307342d" />

Reload config with **sysctl --system**

---------------------------------------------------------------------------------------------------------------

Step -3 Docker install

Docker installation and start the service before k8s installation [ All  VM ]


- For RHEL/CentOS/Rocky/Alma Linux system
  
   yum install docker -y 
   
   systemctl start docker && systemctl enable docker

 - For Debian

    apt update -y
   
	apt install -y docker.io

	systemctl start docker

	systemctl enable docker


-----------------------------------------------------------------------------------------------------------------
- #for centOs or RHEL
  
Step-4 Create apt-get (Ubuntu ) |  Centos /RHEL (DNF / yum) repo for download k8s package 	[ All  VM ]


Step-4 Create apt-get (Ubuntu ) |  Centos /RHEL (DNF / yum) repo for download k8s package 	[ All  VM ]

cd  /etc/yum.repos.d/

vim k8s.repo

[kubernetes]
name=Kubernetes
baseurl=https://pkgs.k8s.io/core:/stable:/v1.33/rpm/
enabled=1
gpgcheck=1
gpgkey=https://pkgs.k8s.io/core:/stable:/v1.33/rpm/repodata/repomd.xml.key
exclude=kubelet kubeadm kubectl cri-tools kubernetes-cni


4.1 
		yum clean all
		yum makecache

- #For Debian 

# Update system
apt update -y
apt install -y apt-transport-https ca-certificates curl gpg

# Create keyrings directory if not exists
mkdir -p /etc/apt/keyrings

# Download Kubernetes repo GPG key
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key \
  | gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

# Add Kubernetes repo
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] \
https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /" \
| tee /etc/apt/sources.list.d/kubernetes.list

4.1
apt update -y

-----------------------------------------------------------------------------------------------------------

Step-5  Pull / download packages from k8s repo ( ALL VM )


		yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes

- #For Debian

apt install -y kubelet kubeadm kubectl

apt-mark hold kubelet kubeadm kubectl


--------------------------------------------------------------------------------------------------------------

Step-6 Start kubelet (Agent)  service       		[ALL VM]

	
	systemctl start kubelet ; systemctl enable kubelet ; systemctl status kubelet

on Master 

<img width="882" height="227" alt="image" src="https://github.com/user-attachments/assets/b3f99b6f-34a2-4e98-8e72-7d3cdd1d1f98" />

On Worker 

<img width="895" height="377" alt="image" src="https://github.com/user-attachments/assets/e82aa78c-67cf-4955-9385-1e4e221e349f" />


-----------------------------------------------------------------------------------------------------------------------

Step 7 initialize kube administartive agent on master node only 

kubeadm init 


<img width="893" height="601" alt="image" src="https://github.com/user-attachments/assets/df07b432-f192-4733-9784-bfa1b59b99db" />


	mkdir -p $HOME/.kube
  	sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  	sudo chown $(id -u):$(id -g) $HOME/.kube/config


<img width="919" height="256" alt="image" src="https://github.com/user-attachments/assets/e60f02d6-7d04-42e1-aca2-0a6ba0882aec" />


kubeadm join 10.0.0.5:6443 --token 6slvus.736cfzv5bcskf28b \
        --discovery-token-ca-cert-hash sha256:71990b049728de2aa19ee25003e7275a5fab585d166c8894852fb0d9161fe037


----------------------------------------------------------------------------------------------

Network setup for Cluster on Master Network 

			calico	  Layer 3 (routing)
			flunel    (Overlay)
			weavenet   (Overlay) (We will choose this )

<img width="765" height="379" alt="image" src="https://github.com/user-attachments/assets/93bda293-797a-4404-858f-c6b5bc164151" />

kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml

<img width="905" height="173" alt="image" src="https://github.com/user-attachments/assets/30845584-5a3c-428b-b866-5dcf6297125d" />


----------------------------------------------------------------------------------------------------------------------------------

Follow Kubeadm join steps on worker nodes 

<img width="902" height="457" alt="image" src="https://github.com/user-attachments/assets/e1182584-358f-4424-89d0-4e8b8de2c25f" />

<img width="892" height="483" alt="image" src="https://github.com/user-attachments/assets/78b3b3eb-ae57-4a3a-a39b-0b0e0d9b7e60" />

<img width="905" height="139" alt="image" src="https://github.com/user-attachments/assets/775cce76-c6f8-4901-9e03-07bd49d68cf7" />





















      
