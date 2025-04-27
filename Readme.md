# Weblate Automated Deployment

In this repository, dockerized deployment of weblate/weblate is shown using AWS ECS Fargate. Blue Green deployments are configured automatically using AWS CodeDeploy. This code consists of four modules which is written in terraforms.
1. VPC for virtual private cloud
2. Database 
3. Containerized service Elastic container service
4. code_deploy

# Modules

All modules along with their details are discussed below.
Creates an AWS VPC with public and private subnets. All the networking and management takes place in this module.
An RDS database with PostgreSQL engine is configured in this module. It is to be connected to the Weblate cluster for using the database. 
An ECS Fargate cluster is configured in this module. Fargate cluster deploy a Weblate app in one container and a Redis caching in the other, which is acheived by defining a task definition. A load balancer with listener and target groups is also configured which sits on the top of the an ECS service.
Deployment tasks are achieved using AWS CodeDeploy. This module creates an application and a deployment group along with the relevant permissions.

# Usage
To deploy this setup on your AWS environment.
1. Configure your **AWS CLI** along with the **credentials** using:										            `aws configure`
2. Install **terraform** in your local system.
3. Configure backend if required using *backend.tf* file
4. Initialize the modules using: 																				`terraform init`
5. Create your parameter file/ Edit the *parameters-dev.tfvars* file to configure the environment.
6. Create a new workspace with a unique name/ or use:																`terraform workspace new dev`
7. Apply using the given configuration.																				`terraform apply --var-file=.\parameters-dev.tfvars` and type *yes* when prompted.