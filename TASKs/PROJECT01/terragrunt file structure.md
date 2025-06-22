
```
infrastructure/
├── terragrunt.hcl              # Main Terragrunt configuration file
├── modules/
│   ├── vpc/
│   │   ├── main.tf              # VPC module definition
│   │   ├── variables.tf         # Variables for the VPC module
│   │   └── outputs.tf           # Outputs for the VPC module
│   ├── iam/
│   │   ├── main.tf              # IAM module definition
│   │   ├── variables.tf         # Variables for the IAM module
│   │   └── outputs.tf           # Outputs for the IAM module
│   ├── api/
│   │   ├── main.tf              # API module definition
│   │   ├── variables.tf         # Variables for the API module
│   │   └── outputs.tf           # Outputs for the API module
│   └── ipam/
│       ├── main.tf              # IPAM (IP Address Management) module definition
│       ├── variables.tf         # Variables for the IPAM module
│       └── outputs.tf           # Outputs for the IPAM module
├── environments/
│   ├── dev/
│   │   ├── terragrunt.hcl       # Terragrunt config
│   ├── staging/
│   │   ├── terragrunt.hcl       # Terragrunt config
│   └── prod/
│       ├── terragrunt.hcl       # Terragrunt config
└── inputs/
    └── parameters.json           # JSON file with input parameters

```


```
my-multicloud-project/
├── terragrunt.hcl                  # Główny plik konfiguracyjny Terragrunt
├── aws/                            # Katalog dla zasobów AWS
│   ├── terragrunt.hcl              # Plik konfiguracyjny dla AWS
│   ├── vpc/                        # Katalog dla VPC w AWS
│   │   └── terragrunt.hcl          # Plik konfiguracyjny dla VPC
│   └── ec2/                        # Katalog dla instancji EC2 w AWS
│       └── terragrunt.hcl          # Plik konfiguracyjny dla EC2
└── azure/                          # Katalog dla zasobów Azure
    ├── terragrunt.hcl              # Plik konfiguracyjny dla Azure
    ├── resource_group/             # Katalog dla grupy zasobów w Azure
    │   └── terragrunt.hcl          # Plik konfiguracyjny dla grupy zasobów
    └── vm/                         # Katalog dla maszyn wirtualnych w Azure
        └── terragrunt.hcl          # Plik konfiguracyjny dla VM



```

```
infrastructure/
├── terragrunt.hcl              # Main Terragrunt configuration file
├── modules/
│   ├── aws/
│   │   ├── vpc/
│   │   │   ├── main.tf          # VPC module definition for AWS
│   │   │   ├── variables.tf     # Variables for the AWS VPC module
│   │   │   └── outputs.tf       # Outputs for the AWS VPC module
│   │   └── iam/
│   │       ├── main.tf          # IAM module definition for AWS
│   │       ├── variables.tf     # Variables for the AWS IAM module
│   │       └── outputs.tf       # Outputs for the AWS IAM module
│   └── gcp/
│       ├── vpc/
│       │   ├── main.tf          # VPC module definition for GCP
│       │   ├── variables.tf     # Variables for the GCP VPC module
│       │   └── outputs.tf       # Outputs for the GCP VPC module
│       └── iam/
│           ├── main.tf          # IAM module definition for GCP
│           ├── variables.tf     # Variables for the GCP IAM module
│           └── outputs.tf       # Outputs for the GCP IAM module
├── environments/
│   ├── dev/
│   │   ├── terragrunt.hcl       # Terragrunt config for dev environment
│   ├── staging/
│   │   ├── terragrunt.hcl       # Terragrunt config for staging environment
│   └── prod/
│       ├── terragrunt.hcl       # Terragrunt config for prod environment
└── inputs/
    └── parameters.json           # JSON file with input parameters


```