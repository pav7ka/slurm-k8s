https://helm.sh/ru/docs/intro/install/
https://github.com/helm/helm/releases

wget -c https://get.helm.sh/helm-v3.15.3-linux-amd64.tar.gz

/usr/local/bin/helm


helm search hub grafana --output yaml

helm repo add grafana https://grafana.github.io/helm-charts

helm show values grafana/grafana > values.yaml
#включить ingress
ingress:
  enabled: true
  ingressClassName: nginx
  hosts:
    - grafana.s0xxxxx.edu.slurm.io

persistence:
#  type: pvc
#  enabled: true
  inMemory:
    enabled: true
    sizeLimit: 300Mi

grafana.s0xxxxx.edu.slurm.io

helm install grafana grafana/grafana -f values.yaml -n monitoring

kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo

admin
