
## 1. Sprawdzenie dostępnych przestrzeni nazw

`gcloud service-directory namespaces list --location=europe-west4`

## 2. Sprawdzenie dostępnych usług w przestrzeni nazw

`gcloud service-directory services list --namespace=my-namespace-name01 --location=europe-west4`

## 3. Tworzenie usługi

`gcloud service-directory services create my-service-name01 --namespace=my-namespace-name01 --location=europe-west4`

## 4. Dodanie punktu końcowego

`gcloud service-directory endpoints create my-endpoint01 --service=my-service-name01 --namespace=my-namespace-name01 --location=europe-west4 --address=10.0.0.1 --port=8080`

## 5. Rozwiązywanie usługi

`gcloud service-directory services resolve my-service-name01 --namespace=my-namespace-name01 --location=europe-west4`

```bash
pawelandrys@cloudshell:~ (test-service-directory)$ gcloud service-directory namespaces list --location=europe-west4
---
name: projects/test-service-directory/locations/europe-west4/namespaces/my-namespace-name01
uid: aa561c2fc5734849800554ddcdc5d059

pawelandrys@cloudshell:~ (test-service-directory)$ gcloud service-directory services list --namespace=my-namespace-name01 --location=europe-west4
---
name: projects/test-service-directory/locations/europe-west4/namespaces/my-namespace-name01/services/my-service-name01
uid: 77e30c1c41b44307986250e3f6524e52

pawelandrys@cloudshell:~ (test-service-directory)$ gcloud service-directory endpoints create my-endpoint01 --service=my-service-name01 --namespace=my-namespace-name01 --location=europe-west4 --address=10.0.0.1 --port=8080
---
ERROR: (gcloud.service-directory.endpoints.create) ALREADY_EXISTS: Resource 'projects/test-service-directory/locations/europe-west4/namespaces/my-namespace-name01/services/my-service-name01/my-endpoint01' already exists.

pawelandrys@cloudshell:~ (test-service-directory)$ gcloud service-directory services resolve my-service-name01 --namespace=my-namespace-name01 --location=europe-west4
service:
  endpoints:
  - address: 34.13.159.164
    name: projects/test-service-directory/locations/europe-west4/namespaces/my-namespace-name01/services/my-service-name01/endpoints/my-endpoint01
    port: 8080
    uid: 2b06f5266aa24df3ba773db16f082be1
  name: projects/test-service-directory/locations/europe-west4/namespaces/my-namespace-name01/services/my-service-name01
  uid: 77e30c1c41b44307986250e3f6524e52

```

https://cloud.google.com/sdk/gcloud/reference/service-directory



# gcloud outputs

##### pawelandrys@cloudshell:~ (test-service-directory)$ gcloud service-directory namespaces list --location=europe-west4
---
name: projects/test-service-directory/locations/europe-west4/namespaces/my-namespace-name01
uid: aa561c2fc5734849800554ddcdc5d059
##### pawelandrys@cloudshell:~ (test-service-directory)$ gcloud service-directory services list --namespace=my-namespace-name01 --location=europe-west4
---
name: projects/test-service-directory/locations/europe-west4/namespaces/my-namespace-name01/services/my-service-name01
uid: 77e30c1c41b44307986250e3f6524e52
pawelandrys@cloudshell:~ (test-service-directory)$ gcloud service-directory endpoints create my-endpoint01 --service=my-service-name01 --namespace=my-namespace-name01 --location=europe-west4 --address=10.0.0.1 --port=8080
ERROR: (gcloud.service-directory.endpoints.create) ALREADY_EXISTS: Resource 'projects/test-service-directory/locations/europe-west4/namespaces/my-namespace-name01/services/my-service-name01/my-endpoint01' already exists.

##### pawelandrys@cloudshell:~ (test-service-directory)$ gcloud service-directory services resolve my-service-name01 --namespace=my-namespace-name01 --location=europe-west4
service:
  endpoints:
  - address: 34.13.159.164
    name: projects/test-service-directory/locations/europe-west4/namespaces/my-namespace-name01/services/my-service-name01/endpoints/my-endpoint01
    port: 8080
    uid: 2b06f5266aa24df3ba773db16f082be1
  name: projects/test-service-directory/locations/europe-west4/namespaces/my-namespace-name01/services/my-service-name01
  uid: 77e30c1c41b44307986250e3f6524e52

***

##### gcloud service-directory services resolve main-server --namespace=my-node-server --location=europe-west4
pawelandrys@cloudshell:~ (test-service-directory)

$ gcloud service-directory services resolve main-server --namespace=my-node-server --location=europe-west4
service:
  endpoints:
  - address: 34.91.59.4
    name: projects/test-service-directory/locations/europe-west4/namespaces/my-node-server/services/main-server/endpoints/my-endpoint-main-server
    port: 80
    uid: 298a6b0c3a364c1792e66b636bae01c4
  name: projects/test-service-directory/locations/europe-west4/namespaces/my-node-server/services/main-server
  uid: ab10431a29e34d5abfe206d18aa2524d
  
***
