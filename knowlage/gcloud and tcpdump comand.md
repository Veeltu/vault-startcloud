gcloud compute backend-services describe bluebackendservice --global --project=test-mtls-442109

### describe map
gcloud compute url-maps describe myurlmap --project=test-mtls-442109

### validacja
#export
gcloud compute url-maps export myurlmap --destination=myurlmap.yaml --global
#validate
gcloud compute url-maps validate --source=myurlmap.yaml --load-balancing-scheme=EXTERNAL_MANAGED --global

terraform apply -var="deployment_strategy=blue"
terraform apply -var="deployment_strategy=green"

***
### get IP for load balancer
gcloud compute addresses describe lb-ipv4-1 \
   --format="get(address)" \
   --global

curl -I -H "Host: one.com" http://34.54.133.26
***


curl -i http://34.117.151.46

sudo tcpdump -i eth0 host 34.98.124.99

tcpdump -s 0 -A dst port 4080 gives E..e..@.?.$bb...b....:......w........Q.....G..1.b..HEAD /dapper_serving/AdMonkey?ping=1 HTTP/1.0


gcloud compute firewall-rules list --project=test-mtls-442109
gcloud compute forwarding-rules list --global --project=test-mtls-442109

gcloud compute url-maps validate --project=test-mtls-442109

gcloud compute url-maps describe --project=test-mtls-442109


gcloud compute backend-services get-health greenbackendservice --project=test-mtls-442109

gcloud compute backend-services describe greenbackendservice --project=test-mtls-442109

gcloud compute instance-groups describe app-instance-group --zone=europe-west4-a --project=test-mtls-442109




curl -I -H "Host: yourdomain.com" https://34.160.205.54:443
curl -I -H "Host: yourdomain.com" https://34.36.170.125
ping 34.160.205.54:443

https://34.160.205.54:443
34.36.170.125 loadbalancer
34.160.205.54:443 proxy

Apigee
