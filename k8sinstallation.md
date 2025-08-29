
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
