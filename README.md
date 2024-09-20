# sure-aws-eks-manifests
AWS EKS YAML Manifests &amp; Helm Charts

Yml file name ends with -
-p => Contains Pod
-d => Contains Deployment
-csd => Contains ClusterIp Service, Deployment
-nsd => Contains NodePort Service, Deployment
-lsd => Contains Load Balancer, Deployment
-isd => Contains Ingress, Service, Deployment

aws eks --region us-east-1 update-kubeconfig --name sure-k8s-cluster

sure-ally\sure-aws-eks-manifests\misc> kubectl apply -f .\aws-cli.yml
- To test pod:
sure-ally\sure-aws-eks-manifests\misc> kubectl apply -f ..\stock-services\stockAPI-p.yml
sure-ally\sure-aws-eks-manifests\misc> kubectl exec -it aws-cli -- bash
bash-4.2# curl http://10.0.27.138:5000/stock/NVDA
{"Global Quote":{"01. symbol":"NVDA","02. open":"117.3500","03. high":"119.6600","04. low":"117.2500","05. price":"117.8700","06. volume":"286038878","07. latest trading day":"2024-09-19","08. previous close":"113.3700","09. change":"4.5000","10. change percent":"3.9693%"}}

- To test deploy:
sure-ally\sure-aws-eks-manifests\misc> kubectl apply -f ..\stock-services\stockAPI-d.yml
sure-ally\sure-aws-eks-manifests\misc> kubectl get po -o wide
NAME                           READY   STATUS    RESTARTS   AGE    IP            NODE                          NOMINATED NODE   READINESS GATES
aws-cli                        1/1     Running   0          168m   10.0.10.217   ip-10-0-14-180.ec2.internal   <none>           <none>
stock-api-d-74dc5577b5-29v2h   1/1     Running   0          5s     10.0.27.138   ip-10-0-14-180.ec2.internal   <none>           <none>
sure-ally\sure-aws-eks-manifests\misc> kubectl exec -it aws-cli -- bash
bash-4.2# curl http://10.0.27.138:5000/stock/V
{"Global Quote":{"01. symbol":"V","02. open":"291.0900","03. high":"291.4775","04. low":"282.8700","05. price":"285.2400","06. volume":"10288063","07. latest trading day":"2024-09-19","08. previous close":"288.4800","09. change":"-3.2400","10. change percent":"-1.1231%"}}

- To test clusterIP service:
sure-ally\sure-aws-eks-manifests\misc> kubectl apply -f ..\stock-services\stockAPI-csd.yml
kubectl get po -o wide
NAME                           READY   STATUS    RESTARTS   AGE     IP            NODE                          NOMINATED NODE   READINESS GATES
stock-api-d-59578df579-shwph   1/1     Running   0          3m17s   10.0.27.138   ip-10-0-14-180.ec2.internal   <none>           <none>
kubectl exec -it aws-cli -- bash
bash-4.2# curl http://10.0.27.138:5000/stock/FDX
{"Global Quote":{"01. symbol":"FDX","02. open":"304.1300","03. high":"308.0000","04. low":"297.8700","05. price":"300.3900","06. volume":"4220491","07. latest trading day":"2024-09-19","08. previous close":"298.1700","09. change":"2.2200","10. change percent":"0.7445%"}}
exit
kubectl get svc
NAME          TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
kubernetes    ClusterIP   172.20.0.1      <none>        443/TCP    4h40m
stock-api-s   ClusterIP   172.20.77.244   <none>        5500/TCP   23m

kubectl port-forward -n default service/stock-api-s 5400:5500
Forwarding from 127.0.0.1:3000 -> 80
Forwarding from [::1]:3000 -> 80

PS C:\> curl http://localhost:5400/stock/FDX

- To test NodePort Service:
Same as above

- To test LoadBalacer service:
sure-ally\sure-aws-eks-manifests\misc> kubectl apply -f ..\stock-services\stockAPI-lsd.yml
kubectl get deploy
kubectl get po -o wide
NAME                           READY   STATUS    RESTARTS   AGE     IP            NODE                          NOMINATED NODE   READINESS GATES
aws-cli                        1/1     Running   0          4h29m   10.0.10.217   ip-10-0-14-180.ec2.internal   <none>           <none>
stock-api-d-788795fbd8-9flxb   1/1     Running   0          22s     10.0.4.230    ip-10-0-14-180.ec2.internal   <none>           <none>
kubectl get svc       
NAME             TYPE           CLUSTER-IP      EXTERNAL-IP                                                              PORT(S)          AGE
kubernetes       ClusterIP      172.20.0.1      <none>                                                                   443/TCP          5h13m
stock-api-lb-s   LoadBalancer   172.20.141.51   aec21790e960d4249ae68ed184b87a99-488623080.us-east-1.elb.amazonaws.com   5500:30909/TCP   30s

In browser: 
http://aec21790e960d4249ae68ed184b87a99-488623080.us-east-1.elb.amazonaws.com:5500/stock/NKE

