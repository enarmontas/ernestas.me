---
layout: post
title: "Detect Terragrunt Infrastructure Drift With Driftctl"
date: 2021-11-17
tags: terraform terragrunt driftctl
comments: true
summary: |
  Driftctl is a tool that helps detect infrastructure drift by comparing Terraform state with live
  resources. Here is how to use it with Terragrunt.
---

[Driftctl](https://github.com/cloudskiff/driftctl) is a tool that helps detect infrastructure
drift by comparing Terraform state with live resources.
It returns a list of resources not covered by infrastructure as code (IaC) codebase.

Since [Terragrunt](https://terragrunt.gruntwork.io/) is using standard Terraform state files,
driftctl works with Terragrunt by using the glob pattern passed to the `--from` option.

However, their [official guide](https://driftctl.com/how-to-use-driftctl-with-terragrunt/)
did not match my needs as I am using different folder structure with region names in the path,
similar to Terragrunt
[recommended structure](https://github.com/gruntwork-io/terragrunt-infrastructure-live-example).

In this article I will document my workflow to detect Terragrunt infrastructure drift using
driftctl.

### Terragrunt Structure
My Terragrunt structure uses the following format:
```
account                 <- production, testing
 └ project              <- project-1, project-2
    └ region            <- global, eu-west-1
       └ module         <- s3-bucket, iam-user
          └ resource    <- example-bucket-1, example-user-1
```

It translates to something like this:
```
production
    project-1
        eu-west-1
            s3-bucket
                example-bucket-1
                    terragrunt.hcl
                example-bucket-2
                    terragrunt.hcl
                example-bucket-3
                    terragrunt.hcl
        global
            iam-assumable-role
                example-role-1
                    terragrunt.hcl
            iam-policy
                example-policy-1
                    terragrunt.hcl
            iam-user
                example-user-1
                    terragrunt.hcl
    project-2
        eu-west-1
            sns-topic
                example-sns-topic-1
                    terragrunt.hcl
        global
            iam-group-with-policies
                example-group-with-policies-1
                    terragrunt.hcl
            iam-policy
                example-policy-1
                    terragrunt.hcl
            iam-user
                example-user-1
                    terragrunt.hcl
                example-user-2
                    terragrunt.hcl
testing
    project-3
        eu-west-1
            s3-bucket
                example-bucket-1
                    terragrunt.hcl
        global
            iam-assumable-role
                example-role-1
                    terragrunt.hcl
            iam-policy
                example-policy-1
                    terragrunt.hcl
            iam-user
                example-user-1
                    terragrunt.hcl
```

### Pull Terragrunt State
During runtime driftctl is not running any of the Terragrunt commands,
so I would need to pass additional parameters to driftctl for authentication to my backend.

Since I am using [Consul backend](/storing-terraform-state-in-consul) and my configuration is
defined in the Terragrunt code, I will run `terragrunt state pull` on each of my modules.
It will automatically generate Terraform backend config and pull the state to a local folder.

Then, I will pass my local state files using glob pattern to `driftctl scan` command.

Here is the process.

Go to the target Terragrunt account:
```
cd <terragrunt>/<account>
```

Find all account modules by searching for `terragrunt.hcl` file:
```
MODULES=$(find . -name "terragrunt.hcl" -exec dirname {} \; | grep -v ".terragrunt-cache" | sed 's/\.\///g' | grep '/')
```

Pull the Terraform state for each Terragrunt module to a separate `states` folder,
while maintaining the same module structure:
```
echo $MODULES | \
  while read module; do \
    echo "Pulling state for $module"; \
    mkdir -p states/$module; \
    terragrunt state pull --terragrunt-working-dir $module > states/$module/tg.tfstate; \
  done
```

Check if the state files were actually saved:
```
ls states/**/tg.tfstate
```
It should return a similar list:
```
states/project-1/eu-west-1/s3-bucket/example-bucket-1/tg.tfstate
states/project-1/eu-west-1/s3-bucket/example-bucket-2/tg.tfstate
states/project-1/eu-west-1/s3-bucket/example-bucket-3/tg.tfstate
...
```

Now that we have our Terragrunt state files locally,
we can go to the next step and actually detect the infrastructure drift.

### Detect Infrastructure Drift
driftctl scans all live resources in the same account that AWS is authenticated to.

If the AWS CLI region is set to `eu-west-2`, but the state files have resources from `eu-west-1`
region, it will show that resources are found in the state, but not in the AWS account.
Also, driftctl will always scan global resources like IAM users or Route53 zones.

In order to match my current AWS region and global resources,
I always use two `tfstate://` glob patterns:
one for the current region and one for global resources.

Here is my resulting driftctl command.
Let's run it to detect the infrastructure drift in the `eu-west-1` region.
```
export AWS_REGION=eu-west-1
driftctl scan --from tfstate://states/**/$AWS_REGION/**/tg.tfstate,tfstate://states/**/global/**/tg.tfstate
```

Here are the scan results:

```
Found resources not covered by IaC:
  aws_api_gateway_account:
    - api-gateway-account
  aws_iam_access_key:
    - AKIA123456789EXAMPLE
        User: example-user-2
  aws_iam_role:
    - OrganizationAccountAccessRole
  aws_iam_role_policy:
    - OrganizationAccountAccessRole:AdministratorAccess
  aws_network_acl_rule:
    - nacl-1234567891
        CIDR: 0.0.0.0/0, Egress: true, Network: acl-1aa2b333, Protocol: All, Rule number: 100
    - nacl-1234567892
        CIDR: 0.0.0.0/0, Egress: false, Network: acl-1aa2b444, Protocol: All, Rule number: 100
Found changed resources:
  From tfstate://states/project-1/global/iam-assumable-role/example-role-1/tg.tfstate
    - example-role-1 (aws_iam_role.this):
        - name_prefix: "" => <nil>
        - permissions_boundary: "" => <nil>
  From tfstate://states/project-2/global/iam-policy/example-policy-1/tg.tfstate
    - example-policy-1 (aws_iam_policy.this):
        - name_prefix: "" => <nil>
Found 60 resource(s)
 - 90% coverage
 - 54 resource(s) managed by terraform
     - 2/54 resource(s) out of sync with Terraform state
 - 6 resource(s) not managed by Terraform
 - 0 resource(s) found in a Terraform state but missing on the cloud provider
```

It gives a great overview of how my live cloud resources match the Terragrunt state.

### Conclusion
Driftctl is an amazing tool that helps detect infrastructure drift.
It can work with Terragrunt even with custom workflows and structure.
I will use this tool for catching all unmanaged resources across all of my cloud accounts.
