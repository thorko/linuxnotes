[link](https://github.com/kubernetes-sigs/federation-v2/blob/master/docs/userguide.md#binaries)

```
cd /opt/go/src/github.com/
systemctl start docker
git clone https://github.com/kubernetes-sigs/federation-v2.git
cd federation-v2
./scripts/download-binaries.sh
make
export PATH=$PATH:/root/federation-v2/bin
# setup kubectl.conf with contexts of each cluster
# install federation to one of the clusters with helm [https://github.com/kubernetes-sigs/federation-v2/blob/master/charts/federation-v2/README.md]
cat << EOF | kubectl apply -f -
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
EOF
helm init --service-account tiller --upgrade -c
```
install with helm
```
helm repo add kubefed-charts https://raw.githubusercontent.com/kubernetes-sigs/kubefed/master/charts
helm search kubefed
helm install kubefed-charts/kubefed --name kubefed --version=0.1.0-rc6 --namespace kube-federation-system
```

for each cluster you running kubernetes create a cluster in federation
make sure you set the kubeconfig context to the cluster which runs the federation
```

kubefedctl join ams1 --cluster-context ams1 --host-cluster-context ams1 --v=2
kubefedctl join fra1 --cluster-context fra1 --host-cluster-context ams1 --v=2

kubectl -n kube-federation-system get kubefedclusters.core.kubefed.io
# make sure all clusters are ready

```

# enable api groups on federation
```
# the core type will be always enabled
kubefedctl enable <your type>

kubectl --context=ams1 api-resources
```

# create namespace and federate it
```
kubectl create namespace federatednamespace
kubefedctl federate namspace federatednamespace
# federation a namespace without --contents will only enable federation types on namespace and doesn't distribute all contents to other clusters

# federate deployment to all clusters
kubefedctl -n federatednamespace federate deployments.apps nginx
```


## review
```
kubectl --context fra1 -n federatednamespace describe pod nginx-
```

## delete federated namespaces

```bash
kubefedctl orphaning-deletion enable <federated type> <name>
# check status
kubefedctl orphaning-deletion status <federated type> <name>
# delete namespace
kubectl delete federatednamespaces logging
```

