
<img width="1193" height="348" alt="image" src="https://github.com/user-attachments/assets/b0d7430d-fe08-49a0-8829-08ac0a670ef0" />

Config 
- master,e2-custom-2-6144,us-east1-d,debian-12

- worker-1,e2-custom-2-6144,us-east1-b,debian-12

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
