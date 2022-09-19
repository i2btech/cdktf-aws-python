# CDKTF-AWS

A CDK for Terraform application in Python that runs a nginx container on AWS ECS using a EC2 instance.

Some context:
- Developed in Ubuntu 20.04 LTS
- Stack `just_modules` create resources using public modules found in [registry.terraform.io](https://registry.terraform.io/)
- Stack `just_provider` create resources using the provider classes, without external modules.

## Software prerequisites

```bash
$ python --version
Python 3.8.10

$ pipenv --version
pipenv, version 2022.8.13

$ npm --version
8.15.0

$ aws --version
aws-cli/2.7.22 Python/3.9.11 Linux/5.15.0-46-generic exe/x86_64.ubuntu.20 prompt/off

$ terraform --version
Terraform v1.2.9
on linux_amd64

$ cdktf --version
0.12.2
```

## Install prerequisites

Install Pipenv by running:

```bash
sudo pip install pipenv
```

[Install cdktf-cli](https://learn.hashicorp.com/tutorials/terraform/cdktf-install?in=terraform/cdktf)

```bash
npm install --global cdktf-cli@0.12.2
```

## Usage

Create/update AWS credencials in `~/.aws/credentials`

If youn are using [tfenv](https://github.com/tfutils/tfenv), set version of Terraform to use

```bash
tfenv use v1.2.9
```

If you are using [nvm](https://github.com/nvm-sh/nvm), set version of node to use

```bash
nvm use v16.17.0
```

Next commands need to be run from working copy of project

Install python dependencies
```bash
pipenv sync
```

Generate CDK constructs for Terraform providers and modules used in the project.

***this command take several minutes to finish***
```bash
cdktf get
```

***TODO***: use [prebuilt provider](https://www.terraform.io/cdktf/concepts/providers#install-pre-built-providers) to short development time

You can now edit the python files if you want to modify any code.

Run cdktf-cli commands (similar to plan and apply in terraform)

```bash
cdktf diff [just_modules|just_provider]
cdktf deploy [just_modules|just_provider]
```

After deploy the `just_modules` stack, you need to enable manually the port 80 on the security group associated with the ec2 instance and get the EIP assigned to the instance to access nginx via browser.

After deploy the `just_provider` stack, you can copy the EIP from the output to access nginx via browser.

Delete all AWS resources

```bash
cdktf destroy [just_modules|just_provider]
```

Compile and generate Terraform configuration for debugging

```bash
cdktf synth
```

The above command will create a folder called `cdktf.out` that contains all Terraform JSON configuration that was generated.

## Links

- https://www.terraform.io/cdktf
- https://github.com/hashicorp/terraform-cdk/tree/43ed33370510c31cd62b5f0b07a812197e89d252/examples/python
- https://registry.terraform.io/modules/terraform-aws-modules/vpc/aws
- https://registry.terraform.io/modules/infrablocks/ecs-cluster/aws
- https://registry.terraform.io/modules/lazzurs/ecs-service/aws
- https://github.com/celeguim/cdktf-aws-python/tree/ac02109830b11512f54ee1a6d40a54598ac38ca8


## Tips

- To find the import that is needed, search the name of the Terraform resource in the imports directory
