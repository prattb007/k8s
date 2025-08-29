
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

  resource "google_compute_firewall" "k8s_ports" {
  name    = "allow-k8s-ports"
  network =   google_compute_network.test-vpc.name

  allow {
    protocol = "tcp"
    ports    = ["2379", "2380", "6443", "8090", "8091", "10250", "10251", "10252", "10255"]
  }

  allow {
    protocol = "udp"
    ports    = ["8472"]
  }

  source_ranges = ["0.0.0.0/0"]

}

 -------------------------------------------------------------------------------------------------------- 
