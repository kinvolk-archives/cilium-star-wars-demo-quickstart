# Cilium Star Wars Demo Quickstart

.. for the impatient copy-paster :)

```
#### Start new minikube cluster
minikube start --network-plugin=cni --bootstrapper=localkube --memory=4096 --extra-config=apiserver.Authorization.Mode=RBAC --kubernetes-version v1.9.4

#### Allow kube-dns to work with RBAC enabled
kubectl create clusterrolebinding kube-system-default-binding-cluster-admin --clusterrole=cluster-admin --serviceaccount=kube-system:default

#### Make sure all pods are running
kubectl get pods --all-namespaces

#### Install etcd for cilium
kubectl create -f https://raw.githubusercontent.com/cilium/cilium/doc-1.0/examples/kubernetes/addons/etcd/standalone-etcd.yaml

#### Install cilium
kubectl create -f https://raw.githubusercontent.com/cilium/cilium/doc-1.0/examples/kubernetes/1.9/cilium.yaml

#### Make sure all pods are running
kubectl get pods --all-namespaces

#### Deploy the demo app
kubectl create -f https://raw.githubusercontent.com/cilium/cilium/doc-1.0/examples/minikube/http-sw-app.yaml

#### Deploy demo layer 3/4 policy
kubectl create -f https://raw.githubusercontent.com/cilium/cilium/doc-1.0/examples/minikube/sw_l3_l4_policy.yaml

#### Deploy demo layer 7 policy
kubectl apply -f https://raw.githubusercontent.com/cilium/cilium/doc-1.0/examples/minikube/sw_l3_l4_l7_policy.yaml
```

## Source

http://docs.cilium.io/en/doc-1.0/gettingstarted/minikube/
