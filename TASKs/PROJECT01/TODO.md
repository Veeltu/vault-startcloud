#### feedback 01
[x] najpierw modules
[x] default + nadpiywanie defaulta
[x] input nie tylko jako yaml yml i tf

[x] moduły prawdziwe zaimplementowac / src : terraform git ? VER important / przygotowac kilka

[] dodac obsluge forEach itd dla yaml

[] zlozone moduly / gotowce
[] po applay zapisac output co zosstalo dodane / store t apply tez - wiecej detali

[] what if dependency , kolejność
[] kiedy klient musi zaciagnac resources, co z tym zrobic, czyli kiedy juz istnieje
[] simple reference - spojnie bez cudzyslowia
[] kilka referencji na raz ???
***
##### ways of GET module
```
module "example" {
  source  = "terraform-aws-modules/vpc/aws"  # Example of a module from the Terraform Registry
  version = "v3.14.0"                        # Specify the version if needed
}

module "local_example" {
  source = "./modules/my_local_module"  # Path to a local directory containing the module
}

module "http_example" {
  source = "https://example.com/path/to/module.zip"  # URL to a zipped module
}

terraform {
  source = "git::https://github.com/username/repo.git//path/to/module?ref=branch_or_tag"
}

```

https://developer.hashicorp.com/terraform/language/modules/sources#terraform-registry

***
##### how to modules ?
- base module to call in config
```
  - module: vpc
    mymodulename: aws_vpc-02
    inputs:
      name: "my-name01"
```

module: vpc    ->     base/default vpc 
if mymodulename true    ->    mymodulename: aws_vpc_02
if inputs    ->   nadpisuje swoje inputy ( + zapisuje nowa versje jakos )

###### dla czego poprostu nie zrobic :
```
    - module: aws_vpc-01
    - module: aws_vpc-02
        inputs:
          name: "my-name01"
```

##### moduły zapisywać w yaml czy hcl ?

co to slowniki w pythonie ? dict ?

