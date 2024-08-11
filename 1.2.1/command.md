ssh s0ххххх@sbox.slurm.io

pass

ssh master-1.s0xxxxx

sudo -s

cd /srv 

git clone https://github.com/kubernetes-sigs/kubespray

cd /srv/kubespray

pip3 install -r requirements.txt

cd /srv/kubespray/inventory

cp -r sample s0xxxxx

cd s0xxxxx

ip a -> 172.xx.xxx

#inventory.ini

[all]
master-1.s0xxxxx.slurm.io ansible_host=172.xx.xxx.2 ip=172.xx.xxx.2
master-2.s0xxxxx.slurm.io ansible_host=172.xx.xxx.3 ip=172.xx.xxx.3
master-3.s0xxxxx.slurm.io ansible_host=172.xx.xxx.4 ip=172.xx.xxx.4
ingress-1.s0xxxxx.slurm.io ansible_host=172.xx.xxx.5 ip=172.xx.xxx.5
node-1.s0xxxxx.slurm.io ansible_host=172.xx.xxx.6 ip=172.xx.xxx.6
node-2.s0xxxxx.slurm.io ansible_host=172.xx.xxx.7 ip=172.xx.xxx.7

[kube_control_plane]
master-1.s0xxxxx.slurm.io
master-2.s0xxxxx.slurm.io
master-3.s0xxxxx.slurm.io

[etcd]
master-1.s0xxxxx.slurm.io
master-2.s0xxxxx.slurm.io
master-3.s0xxxxx.slurm.io

[kube_node]
ingress-1.s0xxxxx.slurm.io
node-1.s0xxxxx.slurm.io
node-2.s0xxxxx.slurm.io

#[calico_rr]

[kube_ingress]
ingress-1.s0xxxxx.slurm.io

[k8s_cluster:children]
kube_node
kube_control_plane
#calico_rr

#group_vars/all/containerd.yml

containerd_registries_mirrors:
  - prefix: docker.io
    mirrors:
      - host: "http://172.20.100.52:5000"
        capabilities: ["pull", "resolve"]
        skip_verify: true
      - host: "https://mirror.gcr.io"
        capabilities: ["pull", "resolve"]
        skip_verify: false
      - host: "https://registry-1.docker.io"
        capabilities: ["pull", "resolve"]
        skip_verify: false

#group_vars/all/etcd.yml

etcd_deployment_type: kubeadm

#group_vars/k8s_cluster/k8s-cluster.yml

kube_version: v1.30.0
container_manager: containerd

kube_network_plugin: calico
kube_service_addresses: 10.233.0.0/18
kube_pods_subnet: 10.233.64.0/18
kube_proxy_mode: ipvs
cluster_name: s0xxxxx.local

kubeconfig_localhost: true
kubectl_localhost: true

event_ttl_duration: "2h0m0s"
auto_renew_certificates: true

#group_vars/k8s_cluster/addons.yml

metrics_server_enabled: true
ingress_nginx_enabled: true
ingress_nginx_host_network: true
ingress_nginx_configmap:
  server-tokens: "False"
  proxy-body-size: "2048M"
  proxy-buffer-size: "16k"
  worker-shutdown-timeout: "180"
ingress_nginx_nodeselector:
  node-role.kubernetes.io/ingress: ""
ingress_nginx_tolerations:
  - key: "node-role.kubernetes.io/ingress"
    operator: "Exists"

#group_vars/kube_ingress.yml

---
node_labels:
  node-role.kubernetes.io/ingress: ""
node_taints:
  - "node-role.kubernetes.io/ingress=:NoSchedule"

выполняем команду
cd /srv/kubespray

ansible-playbook -u s0xxxxx -i inventory/s0xxxxx/inventory.ini cluster.yml -b --diff
