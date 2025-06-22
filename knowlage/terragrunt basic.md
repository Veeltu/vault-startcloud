
1. find_in_parrent_folders()
This function is used within Terragrunt to locate the nearest parent directory that contains a `terragrunt.hcl` file. This is particularly useful in complex project structures where you want to keep your configurations DRY by inheriting common configurations from a parent directory.

1. incluede block
The `include` block in Terragrunt allows a child configuration to inherit all or part of its configuration from a specified parent. This is crucial for avoiding duplication across environments like development, staging, and production, where much of the configuration is shared.

```
# in child terragrunt.hcl  
include {  
path = find_in_parent_folders()  
}
```

1. dependency block
The `dependency` block manages dependencies between Terraform modules handled by Terragrunt. This ensures that dependent resources are created in the correct order, a common requirement in cloud infrastructure where certain resources must exist before others can be provisioned.

#### How it works:

Using `dependency`, Terragrunt can automatically detect when one module must wait for another to complete before starting. This reduces errors and manual tracking of dependencies in complex deployments.

```
# in app/terragrunt.hcl  
dependency "vpc" {  
config_path = "../vpc"  
}  
  
inputs = {  
subnet_ids = dependency.vpc.outputs.subnet_ids  
}
```

In this example, the application configuration waits to retrieve subnet IDs from the VPC module, ensuring the network is ready before deploying the application.


