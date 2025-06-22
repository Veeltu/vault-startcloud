

13.02.2025

#### dla czego error, do czego potrzebuje tworzenie domeny/  dns

```
gcloud compute forwarding-rules create vertexaipsc \
  --global \
  --network=projects/gsk-sharedvpc-dev-f52c02/global/networks/bpc-vpc-spk-dev-cmp-001 \
  --address=projects/gsk-sharedvpc-dev-f52c02/global/addresses/bpc-ipi-pse-gptfwrvertexaipsc-glo-001 \
  --target-google-apis-bundle=all-apis
```

Created [[https://www.googleapis.com/compute/v1/projects/gsk-corp-edap-ai-devki-dev/global/forwardingRules/vertexaipsc](https://www.googleapis.com/compute/v1/projects/gsk-corp-edap-ai-devki-dev/global/forwardingRules/vertexaipsc)].  
  
WARNING: Some requests generated warnings:  
  
- [Private Service Connect for Google APIs] Failed to create DNS zone goog-psc-bpc-vpc-spk-dev-cmp--2173268272600988713 due to insufficient permissions. The DNS integration is not completed. For workaround information, see [https://cloud.google.com/vpc/docs/configure-private-service-connect-apis#troubleshooting](https://cloud.google.com/vpc/docs/configure-private-service-connect-apis#troubleshooting).

***
#### organizacja w gcp do testow resarach


odnośnie błędu przy forwarding-rule:

  
"When you create an endpoint, a Service Directory DNS zone is created. Zone creation can fail for these reasons:

- You haven't enabled the Cloud DNS API in your project.
    
- You don't have the required permissions to create a Service Directory DNS zone.
    
- A DNS zone with the same zone name exists in this VPC network.
    
- A DNS zone for p.googleapis.com already exists in this VPC network."

W linku z konsoli jest to opisane, co należy zrobić, chociaż w tym wypadku wygląda na to że chodzi o po prostu o permission.
[https://cloud.google.com/vpc/docs/configure-private-service-connect-apis#troubleshooting](https://cloud.google.com/vpc/docs/configure-private-service-connect-apis#troubleshooting).

***
https://cloud.google.com/service-directory/docs/configuring-service-directory-zone