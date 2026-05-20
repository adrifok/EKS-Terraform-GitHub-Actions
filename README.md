# EKS-Terraform-GitHub-Actions

Terraform modules to provision a production-ready EKS cluster on AWS, with a GitHub Actions workflow to run plan, apply, or destroy on demand.

What gets deployed

VPC with public and private subnets across multiple availability zones, NAT gateway, EKS cluster with mixed node groups — on-demand and spot instances. IAM roles for both the cluster and the node group. State stored in S3 backend.

Module structure

- `module/vpc.tf` — VPC, public/private subnets, IGW, NAT gateway, route tables
- `module/eks.tf` — EKS cluster, managed node groups (on-demand + spot)
- `module/iam.tf` — IAM roles and policies for cluster and node group
- `module/gather.tf` — data sources (AMI, availability zones)

GitHub Actions workflow

Triggered manually via `workflow_dispatch`. Accepts a `.tfvars` file path and an action (`plan`, `apply`, or `destroy`). Pipeline stages: checkout, Terraform setup, init, format, validate, and the selected action.

Variables

- `env` — environment name, used as resource prefix
- `cluster-name` — EKS cluster name
- `aws-region` — target AWS region
- `vpc-cidr-block` — VPC CIDR block
- `ondemand_instance_types` — on-demand node instance types (default: `t3a.medium`)
- `spot_instance_types` — spot node instance types
- `desired/min/max_capacity_on_demand` — on-demand node group scaling config
- `desired/min/max_capacity_spot` — spot node group scaling config
- `cluster-version` — Kubernetes version
- `endpoint-private-access` / `endpoint-public-access` — API server endpoint access flags
