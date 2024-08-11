kubectl get node

kubectl cordon node-2.s0xxxxx.slurm.io

kubectl drain --ignore-daemonsets --delete-local-data node-2.s0xxxxx.slurm.io


зайти на ноду по
ssh node-2.s0xxxxx.slurm.io
sudo -s

конфиги храняться на ноде
/etc/kubernetes/kubelet-config.yaml
maxPods: 50

systemctl restart kubelet
systemctl status kubelet

!!! по заданию не требуется !!!
вернуться на мастер ноду
ssh master-1.s0xxxxx
sudo -s
kubectl uncordon node-2.s0xxxxx.slurm.io
kubectl describe node node-2.s0xxxxx.slurm.io | grep pods
