---
title: "Deploy Grafana"
date: 2018-10-14T20:54:13-04:00
weight: 15
draft: true
---

#### Download Grafana and update configuration

```
curl -o grafana-values.yaml https://raw.githubusercontent.com/helm/charts/master/stable/grafana/values.yaml
```

Search for **# storageClassName: default**, uncomment and change the value to **"prometheus"**
Search for **# adminPassword: strongpassword**, uncomment and change the password to **"EKS!sAWSome"** or something similar.

Search for **datasources: {}** and uncomment entire block, update prometheus to the endpoint referred earlier by helm response. The configuration will look similar to below

```
datasources:
 datasources.yaml:
   apiVersion: 1
   datasources:
   - name: Prometheus
     type: prometheus
     url: http://prometheus-server.prometheus.svc.cluster.local
     access: proxy
     isDefault: true
```

Now let's expose Grafana dashboard using AWS ELB service. Search for **service:**, and update the value of **type: ClusterIP** to **type: LoadBalancer**

#### Deploy grafana

```
helm install -f grafana-values.yaml stable/grafana --name grafana --namespace grafana
```
Run the command to check if Grafana is running properly
```
kubectl get all -n grafana
```
You should see similar results. They should all be Ready and Available

```
NAME                          READY     STATUS    RESTARTS   AGE
pod/grafana-b9697f8b5-t9w4j   1/1       Running   0          2m

NAME              TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
service/grafana   ClusterIP   10.100.49.172   <none>        80/TCP    2m

NAME                      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/grafana   1         1         1            1           2m

NAME                                DESIRED   CURRENT   READY     AGE
replicaset.apps/grafana-b9697f8b5   1         1         1         2m
```

You can get Grafana ELB URL using this command. Copy & Paste the value into browser to access Grafana web UI

```
kubectl get svc -n grafana grafana -o jsonpath='{.status.loadBalancer.ingress[0].hostname'}
```
