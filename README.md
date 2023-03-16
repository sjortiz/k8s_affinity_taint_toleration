# Test

### Requirements
1. add new nodegrouo
1. add taint, key:AppName value:ts_performance_test
1. add affinity and toleration to the deployment


### Commands

1. Add node pool
```
az aks nodepool add \
      --resource-group myResourceGroup \
      --cluster-name myAKSCluster \
      --node-vm-size Standard_D8ds_v3 \
      --name torch-server-test \
      --node-count 1
```
2. List nodes
`kubectl get nodes`
3. Add the taint
e.g.: `kubectl taint nodes <your node name> key1=value1:NoExecute`
`kubectl taint nodes k8s-playground-worker2 AppName=ts_performance_test:NoExecute`
4. add node affinity first

5. add deployment tolerations

6. apply deployment (helm chart)