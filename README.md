# k8s

## Commands

```bash
# Base
microk8s enable dns storage ingress helm3 dashboard
microk8s dashboard-proxy

# k8s
multipass mount k8s microk8s-vm:k8s
microk8s kubectl apply -f k8s

# Helm charts
multipass mount charts microk8s-vm:charts
microk8s helm3 install botbot-k8s-demo charts/k8s-demo

# Flux CD
microk8s helm3 repo add fluxcd https://charts.fluxcd.io
microk8s kubectl apply -f https://raw.githubusercontent.com/fluxcd/helm-operator/master/deploy/crds.yaml
microk8s kubectl create ns flux
microk8s helm3 upgrade -i flux fluxcd/flux --set git.url='git@github.com:tuanat2359media/k8s' --namespace flux
microk8s kubectl -n flux logs deployment/flux | grep identity.pub | cut -d '"' -f2
microk8s helm3 upgrade -i helm-operator fluxcd/helm-operator --set git.ssh.secretName=flux-git-deploy --set helm.versions=v3 --namespace flux
```