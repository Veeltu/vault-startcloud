
Atmos : 
	https://atmos.tools/
	
some project with using simillar concept
	https://www.youtube.com/watch?v=HMHTQD3S1hI
	
terraform stack
	https://www.hashicorp.com/blog/terraform-stacks-explained
			- not for our use case, we want to create yaml file as main source to share it with many tools/
			- 
licenses Apache2 for atmos 
	https://fossa.com/blog/open-source-licenses-101-apache-license-2-0/

cloud posses # Reference Architecture
	https://docs.cloudposse.com/

***
#### atmos - ## Organize Your Project Directory


```
├── atmos.yaml
├── components/
│   └── terraform/
│       └── weather/
│           ├── README.md
│           ├── main.tf
│           ├── outputs.tf
│           ├── variables.tf
│           └── versions.tf
│
│   # Centralized stacks configuration
└── stacks/
    ├── catalog/
    │   └── station.yaml
    └── deploy/
        ├── dev.yaml
        ├── prod.yaml
        └── staging.yaml
```

```
atmos terraform plan -s <stack-name>

atmos terraform apply station -s dev

atmos terraform apply station -s staging

atmos terraform apply station -s prod
```

```
# .../stacks/catalog/station.yaml 
  
components:  
	terraform:  
	station:  
		metadata:  
			component: weather  
		vars:  
			location: Los Angeles  
			lang: en  
			format: ''  
			options: '0'  
			units: m
```

```
# .../stacks/deploy/dev.yaml
  
vars:  
	stage: dev  
  
import:  
	- catalog/station  
  
components:  
	terraform:  
		station:  
			vars:  
				location: Stockholm  
				lang: se

```