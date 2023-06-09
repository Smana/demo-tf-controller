# Control plane


* Create a VPC (intra, private and public subnets)
* Create a management EKS cluster in a single zone
* Use SPOT instances
* Use bottlerocket AMI
* Install and configure Flux
* Write a secret that contains the cluster's specific variables that will be used with Flux. (please refer to [variables substitutions](https://fluxcd.io/flux/components/kustomize/kustomization/#post-build-variable-substitution))

## How to apply this?

1. Edit the file `backend.tf` and put your own S3 bucket name.
2. Create a file that contains your own variables. Here's an example:

`variables.tfvars`

```hcl
env          = "mgmt"
cluster_name = "controlplane-0" # Generated with petname

github_owner    = "Smana"
github_token    = <REDACTED>
repository_name = "demo-tf-controller"

tags = {
  GithubRepo = "demo-tf-controller"
  GithubOrg  = "Smana"
}
```

3. Apply with `terraform apply -var-file variables.tfvars`


<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | ~> 1.4 |
| <a name="requirement_aws"></a> [aws](#requirement\_aws) | ~> 5.0 |
| <a name="requirement_flux"></a> [flux](#requirement\_flux) | 1.0.0 |
| <a name="requirement_github"></a> [github](#requirement\_github) | ~> 5.25 |
| <a name="requirement_kubectl"></a> [kubectl](#requirement\_kubectl) | >= 1.14.0 |
| <a name="requirement_kubernetes"></a> [kubernetes](#requirement\_kubernetes) | >= 2.20 |
| <a name="requirement_tls"></a> [tls](#requirement\_tls) | 3.1.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_aws"></a> [aws](#provider\_aws) | ~> 5.0 |
| <a name="provider_flux"></a> [flux](#provider\_flux) | 1.0.0 |
| <a name="provider_github"></a> [github](#provider\_github) | ~> 5.25 |
| <a name="provider_kubernetes"></a> [kubernetes](#provider\_kubernetes) | >= 2.20 |
| <a name="provider_tls"></a> [tls](#provider\_tls) | 3.1.0 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_eks"></a> [eks](#module\_eks) | terraform-aws-modules/eks/aws | ~> 19.4 |
| <a name="module_iam_assumable_role_tfcontroller"></a> [iam\_assumable\_role\_tfcontroller](#module\_iam\_assumable\_role\_tfcontroller) | terraform-aws-modules/iam/aws//modules/iam-role-for-service-accounts-eks | 5.21.0 |
| <a name="module_vpc"></a> [vpc](#module\_vpc) | terraform-aws-modules/vpc/aws | ~> 4.0 |

## Resources

| Name | Type |
|------|------|
| [flux_bootstrap_git.this](https://registry.terraform.io/providers/fluxcd/flux/1.0.0/docs/resources/bootstrap_git) | resource |
| [github_repository_deploy_key.this](https://registry.terraform.io/providers/integrations/github/latest/docs/resources/repository_deploy_key) | resource |
| [kubernetes_config_map.flux_clusters_vars](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/config_map) | resource |
| [kubernetes_secret.flux_github_creds](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/secret) | resource |
| [tls_private_key.flux](https://registry.terraform.io/providers/hashicorp/tls/3.1.0/docs/resources/private_key) | resource |
| [aws_availability_zones.available](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/availability_zones) | data source |
| [aws_caller_identity.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/caller_identity) | data source |
| [aws_eks_cluster_auth.cluster_auth](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/eks_cluster_auth) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_cluster_name"></a> [cluster\_name](#input\_cluster\_name) | Name of the EKS cluster to be created | `string` | n/a | yes |
| <a name="input_cluster_version"></a> [cluster\_version](#input\_cluster\_version) | k8s cluster version | `string` | `"1.27"` | no |
| <a name="input_env"></a> [env](#input\_env) | The environment of the EKS cluster | `string` | n/a | yes |
| <a name="input_github_owner"></a> [github\_owner](#input\_github\_owner) | github owner | `string` | n/a | yes |
| <a name="input_github_repository"></a> [github\_repository](#input\_github\_repository) | github repository name | `string` | n/a | yes |
| <a name="input_github_token"></a> [github\_token](#input\_github\_token) | github token | `string` | n/a | yes |
| <a name="input_region"></a> [region](#input\_region) | AWS Region | `string` | `"eu-west-3"` | no |
| <a name="input_tags"></a> [tags](#input\_tags) | A map of tags to add to all resources | `map(string)` | `{}` | no |
| <a name="input_vpc_cidr"></a> [vpc\_cidr](#input\_vpc\_cidr) | The IPv4 CIDR block for the VPC | `string` | `"10.0.0.0/16"` | no |

## Outputs

No outputs.
<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
