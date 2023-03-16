# Project performance

### Requirements
1. add new nodepool
1. add taint, key:project value:performance
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
`kubectl taint nodes <your node name> key1=value1:NoExecute`
e.g.: `kubectl taint nodes k8s-playground-worker2 project=performance:NoExecute`

4. Create namespace:
`kubectl create namespace performance-tests`

5. Add label to the node
`kubectl label node <your node name> project=performance`
E.g.: `kubectl label node k8s-playground-worker2 project=performance`

6. add node affinity to the deployment first
```
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: project
                operator: In
                values:
                - performance
```

7. add deployment tolerations
```
tolerations:
      - key: "project"
        operator: "Equal"
        value: "performance"
        effect: "NoExecute"
        tolerationSeconds: 3600
```

8. apply deployment (helm chart)
Remember to use the new namespace
`helm install <deploymeny_name> <path> -n <namespace>
