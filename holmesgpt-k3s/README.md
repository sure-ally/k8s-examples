

TODO: config.yaml, model.yaml are not working

brew install helm k3d kubectl


k3d cluster create k3d-rancher \
  --servers 1 \
  --agents 2 \
  -p "80:80@loadbalancer" \
  -p "443:443@loadbalancer" \
  --k3s-arg "--kube-apiserver-arg=enable-aggregator-routing=true@server:*" \
  --wait

kubectl create namespace cert-manager

helm install cert-manager jetstack/cert-manager \
    --namespace cert-manager \
    --version v1.20.2 \
    --set crds.enabled=true --wait --debug

kubectl -n cert-manager rollout status deploy/cert-manager
date
### Install the helm repos for rancher
helm repo add rancher-latest https://releases.rancher.com/server-charts/latest
helm repo update
kubectl create namespace cattle-system
helm install rancher rancher-latest/rancher \
    --namespace cattle-system \
    --version=2.14.1 \
    --set hostname=rancher.localhost \
    --set bootstrapPassword=congratsthanandayme \
    --no-hooks
kubectl -n cattle-system rollout status deploy/rancher
kubectl -n cattle-system get all,ing
date

helm search repo jetstack/cert-manager --versions

 helm search repo rancher-latest/rancher --versions

  kubectl create secret generic bootstrap-secret \
  -n cattle-system \
  --from-literal=bootstrapPassword="congratsthanandayme"

  -- wait for some time


curl -fki https://rancher.localhost
open in browser: https://rancher.localhost
  congratsthanandayme



----
Troubleshoot Any Kubernetes Workload with HolmesGPT:

General Troubleshooting like MCP Server in your VS code: (Ask and Get results)

https://medium.com/@mailprak/from-kubectl-chaos-to-clarity-troubleshoot-any-kubernetes-workload-with-holmesgpt-d2d3eb9add29


**Create a test pod** to investigate:

    kubectl apply -f https://raw.githubusercontent.com/robusta-dev/kubernetes-demos/main/pending_pods/pending_pod_node_selector.yaml


Get free token: https://aistudio.google.com/app/api-keys
Get free tier model: https://ai.google.dev/gemini-api/docs/pricing

holmes ask "what pods are failing?" --model="gemini/gemini-3.1-flash-lite"


--
