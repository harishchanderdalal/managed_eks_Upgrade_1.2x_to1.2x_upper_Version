# managed_eks_Upgrade_1.2x_to1.2x_upper_Version
How to upgrade aws managed eks

## Step 1 - Check you have any resource in beta version before to start upgrade
```
kubectl api-resources --verbs=list -o name | xargs -n 1 kubectl get -o name | awk -F '/' '{print $1}' | uniq | awk -F " " '{print "kubectl get "$1" -o yaml | grep v1beta1"}'
```
### If you find any fist update that not to use Beta version

## Step 2 - Update from Eks UI Page

## Step 3 - Update you Node Group / Launch Tempate New Version of AMI or version

## Step 4 - Update CNI Use the latest CNI
```
kubectl describe daemonset aws-node --namespace kube-system | grep amazon-k8s-cni: | cut -d : -f 3
kubectl apply -f https://raw.githubusercontent.com/aws/amazon-vpc-cni-k8s/v1.12.1/config/master/aws-k8s-cni.yaml
kubectl get pods -n kube-system
```

## Step 5 - Update KubeProxy :- https://docs.aws.amazon.com/eks/latest/userguide/managing-kube-proxy.html (Version Compartabilty)
```
kubectl describe daemonset kube-proxy -n kube-system | grep Image
kubectl get daemonset kube-proxy -n kube-system - o yaml > kube-proxy.yaml
kubectl edit ds kube-proxy
kubectl get daemonset kube-proxy
kubectl  get pods -n kube-system
```

## Step 6 - Update Core DNS -  https://docs.aws.amazon.com/eks/latest/userguide/managing-coredns.html
```
kubectl describe deployment coredns -n kube-system | grep Image | cut -d ":" -f 3
kubectl edit deployment coredns -n kube-system ( CHANGE IN API VERIONS )
kubectl get pods -n kube-system
kubectl get deploy coredns -n kube-system
```
