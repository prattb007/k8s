<img width="1077" height="593" alt="image" src="https://github.com/user-attachments/assets/ec9b1565-bdcc-4a1a-8551-74d461f12a68" />



Master Node ===  Control Plane 

Worker Node  ===  Data Plane 
-----------------------------------

	POD = container(s) 

-------------
Master Node (Control Plane)
-----------

		1) API Server 
				*  Communicate with Data Plane ( Worker Node )
				*  Centralized Management 
				*  RBAC ( Role Based Access Control)
						- Ekam		(3)
						- Harman	(4)
						- Ravi		(5)
						- Prateek 	(8)
					
					Authentication:  Who can login into my cluster ?
					Authorization:   What the person can do  ?
					
		2) 	Controller Manager 	/ Kube Controller 
				
				* Maintain desire state of application inside the POD 
					
				* Cross  check all the pod that have at least one container ?
				
				* All  controlling will be done by API-Server
				
		
				
		3) Kube Scheduler / Scheduler 
			
			* Decide the best fit NODE ( WORKER NODE [ Data Plane ] ) for deploy our application. 
			
			* We can install / setup (Deploy) our application on specific hardware.
			
		4) etcd 	cluster / etcd 
			
			* it is distributed database which stores the complete information about the :  
						CLUSTER INFORMATION 
						
			* It stores data in shape of KEY and VALUE pair ( JSON / YAML )
			
--------------------------------------------------------

Data Plane ( Worker Node )
----------------------------
	
			Kubelet :   It is a service which  runs on worker node  +  master node 
							
								( kubectl  ---->  CLI tool)
								
			Kube-Proxy :  Kube-proxy a critical component which responsible for 
			
									Network Proxy  
										+
									Load Balanacing 
									
-----------------------------------------------------------

		kubeadm  (Master Node)   -->   API-Server, Controller, Scheduler, etcd
		kubelet  ( master  +  worker )  -->  Agent for worker node and communicate with API Server 
		kubectl  (master +  worker)  -->  Command for deploy YAML , service, cluster check 
		
---------------------------------------------------------------
<img width="678" height="466" alt="image" src="https://github.com/user-attachments/assets/4888a637-6370-40f6-b22d-f5aa8c78e6fd" />


Namespace Linux : it is a feature of linux kernel that partition kernel resource such that one set of process sees one set of resources while another set of process sees a different set of resources. 


Namespace in k8s : In k8s namespace provide a mechanism for isolating group of resources within a single cluster. 

   Namespace based scoping is applicable only for namespace objects (Deployment, services)  and not for cluster wide objects  (Storageclass, Nodes, pvc) 
   
   
 ------------------------------------------


		Master Node   (1)
		
		Worker Node   (2)
		
---------------------------------------------


<img width="949" height="473" alt="image" src="https://github.com/user-attachments/assets/34f78640-8ded-4567-998b-221d2c946d08" />

