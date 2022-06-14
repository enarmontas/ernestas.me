---
layout: post
title: "Common Configurations with Terragrunt"
date: 2022-06-13
tags: terraform terragrunt
---

Terragrunt supports multiple included files, which is a great way to have common configurations 
with module definitions and default values included in any child configurations. 
Here is a short explanation of how it works.

## Example
Usually, when I write a Terragrunt module, I define the following blocks:

First, I define a Terraform block with a module source.

```
terraform {
  source = "git::git@github.com:example/modules.git//aws/vpc?ref=v0.10.0"
}
```

Then, I define a Terragrunt block to include the root `terragrunt.hcl` file with backend, provider,
and other configurations across all of my environments and accounts.

```
include {
  path = find_in_parent_folders()
}
```

The `find_in_parent_folders()` method traverses the directory tree beginning with the current 
`terragrunt.hcl` file and returns the absolute path to the first `terragrunt.hcl` in a parent 
folder. This block is required in any Terragrunt repository with a parent-child structure.

Finally, I can define the `inputs` block to pass all the required values to the module:

```
inputs = {
  name  = "example-vpc"
  other = "example-value"
  ...
}
```

Here is the resulting child `terragrunt.hcl`:
```
terraform {
  source = "git::git@github.com:example/modules.git//aws/vpc?ref=v0.10.0"
}

include {
  path = find_in_parent_folders()
}

inputs = {
  name  = "example-vpc"
  other = "example-value"
  ...
}
```

## Problem
Let's say I need to define 10 configurations with the same module. 
By using the same static config, I'll end up repeating the same Terraform source with locked module 
versions and most of the inputs.

In order to write less code and have more dynamic definitions, 
I can define a common configuration for the same module and then include it in the 
child `terragrunt.hcl`. Here is an example:

Let's create a file `example-module.hcl` in the `common` folder:
```
locals {
  module_url     = "git::git@github.com:example/modules.git//aws/vpc"
  module_version = "v0.10.0"
}

terraform {
  source = "${local.module_url}?ref=${local.module_version}"
}

inputs = {
  other = "example-value"
}
```

Now the child `terragrunt.hcl` configurations can include this file in order to have the same 
module definition and same inputs across all environments and accounts with the ability to 
override them in the child configurations.

## Solution

Let's add everything together in a new `terragrunt.hcl` child configuration file.

First, define the root `terragrunt.hcl` include block and give it a name:

```
include "root" {
  path = find_in_parent_folders()
}
```

Then, include the common module config:
```
include "module" {
  path = "${get_path_to_repo_root()}/common/modules/example-module.hcl"
}
```

Finally, define inputs that are unique for the specific resource:
```
inputs = {
  name = "example-vpc"
}
```

Here is the resulting child `terragrunt.hcl` configuration with a dynamically included module: 
```
include "root" {
  path = find_in_parent_folders()
}

include "module" {
  path = "${get_path_to_repo_root()}/common/modules/example-module.hcl"
}

inputs = {
  name = "example-vpc"
}
```

During the Terragrunt run, the common module configuration will be merged with 
the child `terragrunt.hcl` configuration, merging all blocks and inputs into one configuration. 
It works with Terragrunt out of the box, and it also works with the
[Terragrunt config generator](https://github.com/transcend-io/terragrunt-atlantis-config) if you 
use [Atlantis](https://github.com/runatlantis/atlantis) for your CI/CD pipelines.

Now, when you want to bump the `example-module` version, you only need to do it once in 
the `common/example-module.hcl` file and all the child configurations will pick up the change 
during subsequent Terragrunt runs.
Plus, if you want to override some common input value or module source, 
just do it in the child configuration and it will take precedence over the included one.

You can see a similar example of the same pattern in the official 
[Terragrunt live infrastructure example repo](https://github.com/gruntwork-io/terragrunt-infrastructure-live-example/tree/master/_envcommon).
