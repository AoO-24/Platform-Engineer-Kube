## Upgrades

On-prem and EKS updates

### On-Prem


```
# Find the latest 1.28 version in the list.
# It should look like 1.28.x-*, where x is the latest patch.

apt update
apt-cache madison kubeadm
```

```
apt-mark unhold kubeadm && \
apt-get update && apt-get install -y kubeadm='1.28.x-*' && \
apt-mark hold kubeadm
```

```
kubeadm upgrade plan
```

```
sudo kubeadm upgrade apply v1.28.x
```

```
sudo kubeadm upgrade node
```

```
apt-mark unhold kubelet kubectl && \
apt-get update && apt-get install -y kubelet='1.28.x-*' kubectl='1.28.x-*' && \
apt-mark hold kubelet kubectl
```

```
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```

```
kubectl uncordon <node-to-uncordon>
```


### EKS

https://docs.aws.amazon.com/eks/latest/userguide/update-cluster.html


## Monitoring and Observability

When implementing monitoring and observability, you have two primary options:
- Homegrown
- Enterprise

Homegrown is comprised of open-source tools or a monitoring/observability tool that you build in-house.

Enterprise is a paid tool. Think Datadog or AppDynamics

### Open-Source

- Grafana
- Prometheus
- Loki
- Tempo

#### Grafana and Prometheus

```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

```
helm install kube-prometheus prometheus-community/kube-prometheus-stack
```

Grafana Creds
- Username: admin
- Password: admin

(password may also be: prom-operator)

#### Install Loki

```
helm repo add grafana https://grafana.github.io/helm-charts
```

Configure a `values.yaml` file
```
loki:
  commonConfig:
    replication_factor: 1
  storage:
    type: 'filesystem'
singleBinary:
  replicas: 1
```

```
helm install -n monitoring --values loki-values.yaml loki grafana/loki
```
#### Install Tempo

```
helm repo add grafana https://grafana.github.io/helm-charts
```

```
helm install -n monitoring tempo grafana/tempo
```


### Enterprise

- Datadog

```
helm repo add datadog https://helm.datadoghq.com
```

```
helm repo update
```

```
CLUSTER_NAME=
API_KEY=
```

```
helm install datadog -n datadog \
--set datadog.site='datadoghq.com' \
--set datadog.clusterName=$CLUSTER_NAME \
--set datadog.clusterAgent.replicas='2' \
--set datadog.clusterAgent.createPodDisruptionBudget='true' \
--set datadog.kubeStateMetricsEnabled=true \
--set datadog.kubeStateMetricsCore.enabled=true \
--set datadog.logs.enabled=true \
--set datadog.logs.containerCollectAll=true \
--set datadog.apiKey=$API_KEY \
--set datadog.processAgent.enabled=true \
--set targetSystem='linux' \
datadog/datadog --create-namespace
```