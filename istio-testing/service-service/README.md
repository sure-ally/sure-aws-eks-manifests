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

sure-ally\sure-aws-eks-manifests\istio-testing\service-service> kubectl apply -f .\stock-deployment.yml
sure-ally\sure-aws-eks-manifests\istio-testing\service-service> kubectl apply -f .\news-deployment-service.yml

- To test deploy:
sure-ally\sure-aws-eks-manifests\misc> kubectl -n backend exec -it client -- sh
~ $ curl http://10.0.12.135:5100/news/TSLA


- To test deploy after istio vs and dr:
sure-ally\sure-aws-eks-manifests\misc> kubectl -n backend exec -it client -- sh
~ $ curl http://news-api-d-l.news-ns:5000/news/TSLA

curl: (6) Could not resolve host: news-api-d-l.news-ns

Debugging: 
Check: 
curl http://10.0.12.135:5100/news/TSLA
curl http://10.0.1.33:5100/news/TSLA

mostly the problem is with labels

After fixing try again:
while true; do curl http://news-api-d-l.news-ns:5000/news/TSLA && echo "" && sleep 1; done
