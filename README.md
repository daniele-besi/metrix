# metrics-server

## Installazione metrics-server

```bash
git clone  https://gitlab.aruba.it/DevOps/k8s/metrics-server
cd metrics-server
kubectl apply --filename deploy/1.8+/
```

Validate
```bash
kubectl get deploy,pods -n kube-system --selector='k8s-app=metrics-server'
```

[sample output]
```bash
NAME                                   DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.extensions/metrics-server   1         1         1            1           28m

NAME                                  READY     STATUS    RESTARTS   AGE
pod/metrics-server-6fbfb84cdd-74jww   1/1       Running   0          28m
```

## Fixing issues with Metrics deployment

To apply a patch to metrics server
```bash
wget -c https://gist.githubusercontent.com/initcron/1a2bd25353e1faa22a0ad41ad1c01b62/raw/008e23f9fbf4d7e2cf79df1dd008de2f1db62a10/k8s-metrics-server.patch.yaml
kubectl patch deploy metrics-server -p "$(cat k8s-metrics-server.patch.yaml)" -n kube-system
```

Now validate with
```bash
kubectl top node 
kubectl top pod 
```

# Horizonntal Pod Autoscaling

per scalare vanno inseriti i controlli hpa sui servizi da tenere sotto autoscale
```bash
kubectl autoscale deployment <deployment-name> --cpu-percent=50 --min=1 --max=10

kubectl autoscale statefulsets <statefulsets-name> --cpu-percent=50 --min=1 --max=10
```

per vedere le regole e lo stato
```bash
kubectl get hpa
```