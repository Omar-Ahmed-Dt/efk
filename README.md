# Install EFK Stack
- **Create an Amazon EKS cluster**
```bash
eksctl create cluster --name=eks-cluster \
    --region=ap-south-1 \
    --nodegroup-name=my-nodes \
    --node-type=t3.medium \
    --managed \
    --nodes=2 \
    --nodes-min=2 \
    --nodes-max=3
```
- **Create efk Namespace**
```bash
kubectl create ns efk
```
- **Deploy ElasticSearch**
```bash
cd elasticsearch-kibana
kubectl apply -f es-pvolume.yaml
kubectl apply -f es-service.yaml
kubectl apply -f es-statefulset.yaml
```
- **Deploy Kibana** : The Elastic Search and Kibana version should be the matching version
```bash
kubectl apply -f kinana-service.yaml
kubectl apply -f kibana-deployment.yaml
```
![efk](./imgs/efk.png)
---
![efk](./imgs/ui.png)
---
![efk](./imgs/query.png)
**Scaling Elasticsearch and Kibana**
```bash
kubectl get svc -n efk
cd scaling-ek-stack
```
- update `kibana.yml` with the URL
- update `config-map.yml` with the URL
![efk](./imgs/url.png)
```bash
kubectl get node
kubectl label nodes <node name> app=elasticsearch
kubectl apply -f .
```
**Deploying E-Commerce App**
```bash
cd ../../event-generator
kubectl apply -f . 
kubectl logs app-event-simulator -n efk -f
kubectl logs fluent-bit-kp9hr -n efk
```


