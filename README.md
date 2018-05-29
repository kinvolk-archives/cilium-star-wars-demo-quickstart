# Cilium Star Wars Demo Quickstart on Kubernetes 1.9

Start a new minikube cluster:

```
minikube start \
  --network-plugin=cni --bootstrapper=localkube --memory=4096 \
  --extra-config=apiserver.Authorization.Mode=RBAC --kubernetes-version v1.9.4
```

Allow kube-dns to work with RBAC enabled:

```
kubectl create \
  clusterrolebinding kube-system-default-binding-cluster-admin \
  --clusterrole=cluster-admin --serviceaccount=kube-system:default
```

Make sure all pods are running:

```
kubectl get pods --all-namespaces
```

Install etcd for cilium

```
kubectl create -f \
  https://raw.githubusercontent.com/cilium/cilium/doc-1.0/examples/kubernetes/addons/etcd/standalone-etcd.yaml
```

Install cilium:

```
kubectl create -f \
  https://raw.githubusercontent.com/cilium/cilium/doc-1.0/examples/kubernetes/1.9/cilium.yaml
```

Again, make sure all pods are running:

```
kubectl get pods --all-namespaces
```

Deploy the demo app:

```
kubectl create -f \
  https://raw.githubusercontent.com/cilium/cilium/doc-1.0/examples/minikube/http-sw-app.yaml
```

Note how both `empire` and `alliance` ships can land on the deathstar:

```
kubectl exec $(kubectl get pod -l class=tiefighter -o jsonpath='{.items[0].metadata.name}') \
  -- curl -s -XPOST deathstar.default.svc.cluster.local/v1/request-landing
```

```
kubectl exec $(kubectl get pod -l class=xwing -o jsonpath='{.items[0].metadata.name}') \
  -- curl -s -XPOST deathstar.default.svc.cluster.local/v1/request-landing
```

Deploy demo layer 3/4 policy:

```
kubectl create -f https://raw.githubusercontent.com/cilium/cilium/doc-1.0/examples/minikube/sw_l3_l4_policy.yaml
```

Try to land both ships again. The landing request for `xwing` should timeout now.

But the tiefighter is still able to make the deathstar explode!

```
kubectl exec $(kubectl get pod -l class=tiefighter -o jsonpath='{.items[0].metadata.name}') \
  -- curl -s -XPUT deathstar.default.svc.cluster.local/v1/exhaust-port
```

Deploy demo layer 7 policy:

```
kubectl apply -f \
  https://raw.githubusercontent.com/cilium/cilium/doc-1.0/examples/minikube/sw_l3_l4_l7_policy.yaml
```

And try to make the deathstar explode again.

## Source

http://docs.cilium.io/en/doc-1.0/gettingstarted/minikube/
