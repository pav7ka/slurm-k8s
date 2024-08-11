helm repo add longhorn https://charts.longhorn.io

helm upgrade --install -f ./values.yaml longhorn longhorn/longhorn --namespace longhorn --create-namespace --set ingress.host=longhorn.s0xxxxx.edu.slurm.io 
