Link: https://logz.io/blog/deploying-the-elk-stack-on-kubernetes-with-helm/

1. Create storageclass elk
2. Get values from link
curl -O https://raw.githubusercontent.com/elastic/Helm-charts/master/elasticsearch/examples/minikube/values.yaml
3. Deploy elasticsearch by helm
Helm install elasticsearch elastic/elasticsearch -f ./values.yaml
4. Deploy kibana by helm
Helm install kibana elastic/kibana 
5. Deploy metricbeat by helm
Helm install metricbeat elastic/metricbeat 
6. Deploy fluentd by helm
helm install fluentd stable/fluentd-elasticsearch