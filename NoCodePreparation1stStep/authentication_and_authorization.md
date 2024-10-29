## Standard RBAC

```
kubectl create sa podcreator
```

```
kubectl get sa podcreator
```

```
kubectl apply -f - <<EOF
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: podcreator
rules:
- apiGroups: [""]
  resources: ["pods", "deployments"]
  verbs: ["get", "update", "list", "create"]
EOF
```

```
kubectl apply -f - <<EOF
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: write-pod-default
  namespace: default
subjects:
- kind: ServiceAccount
  name: podcreator
  apiGroup: ""
  namespace: default
roleRef:
  kind: Role
  name: writer
  apiGroup: rbac.authorization.k8s.io
EOF
```


## Azure Active Directory

```
az ad user create --display-name miketheuser --password 'Password12!@' --user-principal-name miketheuser@mlevan1992outlook.onmicrosoft.com
```

```
az role definition list \
	--query "[?contains(roleName, 'Azure Kubernetes Service RBAC')].{roleName:roleName,description:description}"
```

```
az role assignment create \
    --assignee "miketheuser@mlevan1992outlook.onmicrosoft.com" \
    --role "Azure Kubernetes Service RBAC Reader" \
    --scope $(az aks show \
        --resource-group devrelasaservice \
        --name aksenvironment01 \
        --query id -o tsv)
```

## Keycloak

```
kubectl create -f https://raw.githubusercontent.com/keycloak/keycloak-quickstarts/latest/kubernetes/keycloak.yaml
```

```
kubectl get svc
```

default username and password are both "admin"

Take a look at the docs here: https://www.keycloak.org/getting-started/getting-started-kube