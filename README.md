# servicemesh-examples

![topo](/imgs/topo.png?raw=true "linkerd")


## LINKERD
#### install linkerd
```
curl -sL https://run.linkerd.io/install | sh
export PATH=$PATH:$HOME/.linkerd2/bin
linkerd install | kubectl apply -f -
```
#### deploy microservices
```
git clone https://github.com/mehrdadrad/servicemesh-examples.git
cd servicemesh-examples/k8s

kubectl apply -f backend.yaml
kubectl apply -f middleware.yaml
kubectl apply -f frontend.yaml
```
#### linkerd injection
```
kubectl get deploy/frontend-deployment -o yaml | linkerd inject - \ | kubectl apply -f -
kubectl get deploy/middleware-deployment -o yaml | linkerd inject - \ | kubectl apply -f -
kubectl get deploy/backend-deployment -o yaml | linkerd inject - \ | kubectl apply -f -
```
#### pods status
```
kubectl get pods
NAME                                     READY   STATUS    RESTARTS   AGE
backend-deployment-7c65c5cdbf-6gs9z      2/2     Running   0          3h19m
backend-deployment-7c65c5cdbf-m2ckt      2/2     Running   0          3h19m
frontend-deployment-697fb6566-prpd7      2/2     Running   0          4m5s
frontend-deployment-697fb6566-z78sx      2/2     Running   0          4m12s
middleware-deployment-7599d78474-d4ftv   2/2     Running   0          4m24s
middleware-deployment-7599d78474-lxrjm   2/2     Running   0          4m11s
```
#### load test
```
kubectl get svc frontend-external
NAME                TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)          AGE
frontend-external   LoadBalancer   10.107.208.163   192.168.55.102   8082:32415/TCP   16h

while true; do curl 192.168.55.102:8082/time; sleep 2; done
```

#### linkerd cmd stat deployment
```
linkerd stat deploy
NAME                    MESHED   SUCCESS      RPS   LATENCY_P50   LATENCY_P95   LATENCY_P99   TCP_CONN
backend-deployment         2/2   100.00%   0.5rps          75ms          98ms         100ms          6
frontend-deployment        2/2   100.00%   0.5rps          75ms          98ms         100ms          2
middleware-deployment      2/2   100.00%   0.5rps          75ms          98ms         100ms          6
```

#### linkerd and grafana dashboards screenshots

```
linkerd dashboard &
```

![linkerd](/imgs/Linkerd.png?raw=true "linkerd")
![linkerd](/imgs/Linkerd_Control_Plane.png?raw=true "linkerd")
![linkerd](/imgs/Linkerd_Backend_Grafana.png?raw=true "linkerd")
![linkerd](/imgs/Linkerd_Middleware_Grafana.png?raw=true "linkerd")
