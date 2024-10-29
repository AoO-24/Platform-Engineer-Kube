## Code



### On-Prem

https://github.com/AdminTurnedDevOps/Kubernetes-Quickstart-Environments/tree/main/Bare-Metal/kubeadm-cilium


### AKS

https://github.com/AdminTurnedDevOps/Kubernetes-Quickstart-Environments/tree/main/azure/aks



### EKS

https://github.com/AdminTurnedDevOps/Kubernetes-Quickstart-Environments/tree/main/aws/eks




## CICD

### Authentication For Azure
```
export subscriptionId=220284d2-6a19-4781-87f8-5c564ec4fec9
export resourceGroup=devrelasaservice


az ad sp create-for-rbac --name "githubactions1" --role contributor \
                            --scopes /subscriptions/$subscriptionId/resourceGroups/$resourceGroup \
                            --sdk-auth
  ```


### Clouds

EKS Create: https://github.com/AdminTurnedDevOps/kubernetes-real-world-course/blob/main/.github/workflows/terraform.yml

AKS Create: https://github.com/AdminTurnedDevOps/kubernetes-real-world-course/blob/main/.github/workflows/terraformaks.yml

EKS Destroy: https://github.com/AdminTurnedDevOps/kubernetes-real-world-course/blob/main/.github/workflows/destroy.yml

AKS Destroy: https://github.com/AdminTurnedDevOps/kubernetes-real-world-course/blob/main/.github/workflows/aksDestroy.yml