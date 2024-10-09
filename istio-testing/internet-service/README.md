# sure-aws-eks-manifests
AWS EKS Istio YAML Manifests &amp; Helm Charts

Yml file name ends with -
-p => Contains Pod
-d => Contains Deployment
-csd => Contains ClusterIp Service, Deployment
-nsd => Contains NodePort Service, Deployment
-lsd => Contains Load Balancer, Deployment
-isd => Contains Ingress, Service, Deployment

aws eks --region us-east-1 update-kubeconfig --name sure-k8s-cluster

sure-ally\sure-aws-eks-manifests\istio-testing\internet-service> kubectl apply -f .

- To test deploy:
sure-ally\sure-aws-eks-manifests\istio-testing\internet-service> kubectl -n istio-ingress get svc

NAME                 TYPE           CLUSTER-IP       EXTERNAL-IP                                                               PORT(S)                                      AGE
sure-istio-gateway   LoadBalancer   172.20.241.250   aa0d2999cbc534e2f8f1ab08e5372749-1456617634.us-east-1.elb.amazonaws.com   15021:31623/TCP,80:32303/TCP,443:30707/TCP   7m15s

curl --header "Host: app.sure-ally.com" http://aa0d2999cbc534e2f8f1ab08e5372749-1456617634.us-east-1.elb.amazonaws.com/news/TSLA

Invoke-WebRequest -Uri "http://aa0d2999cbc534e2f8f1ab08e5372749-1456617634.us-east-1.elb.amazonaws.com/news/TSLA" -Headers @{"Host"="app.sure-ally.com"}


Invoke-WebRequest -Uri "http://aa0d2999cbc534e2f8f1ab08e5372749-1456617634.us-east-1.elb.amazonaws.com/api/devices" -Headers @{"Host"="app.devopsbyexample.com"}




Trobleshooting conn
1.
PS C:\Workdesk\workspaces\sure-ally\sure-aws-eks-manifests\istio-testing\User-Service-Communication>  kubectl get svc -n istio-system
NAME     TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                                 AGE
istiod   ClusterIP   172.20.63.107   <none>        15010/TCP,15012/TCP,443/TCP,15014/TCP   58m
PS C:\Workdesk\workspaces\sure-ally\sure-aws-eks-manifests\istio-testing\User-Service-Communication> kubectl get po -n istio-system
NAME                      READY   STATUS    RESTARTS   AGE
istiod-868cc8b7d7-8fghf   1/1     Running   0          59m

2. Please note the gateway
selector:
    istio: 


