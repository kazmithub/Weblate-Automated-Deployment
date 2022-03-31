# Weblate Automated Deployment

In this repository, dockerized deployment of weblate/weblate is shown using AWS ECS Fargate. Blue Green deployments are configured automatically using AWS CodeDeploy. This is a terraform code consisting of 4 modules, namely
1. networking
2. rds
3. ecs
4. code_deploy

# Task

Your task is to to use ECS/EKS to prepare an autoscaling deployment of Weblate https://github.com/WeblateOrg/weblate
The system needs proper message qeueing based on Celery and caching based on Redis in addition to access to database that should be provisioned using RDS Postgres and proper config management too.
We should be able to upgrade the versions when needed and downgrade in a reliable manner in case of a botched update.

# Modules

All modules along with their details are discussed below.

## networking

Creates an AWS VPC with public and private subnets. All the networking and management takes place in this module.

## rds

An RDS database with PostgreSQL engine is configured in this module. It is to be connected to the Weblate cluster for using the database. 

## ecs

An ECS Fargate cluster is configured in this module. Fargate cluster deploy a Weblate app in one container and a Redis caching in the other, which is acheived by defining a task definition. A load balancer with listener and target groups is also configured which sits on the top of the an ECS service.

## code_deploy

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