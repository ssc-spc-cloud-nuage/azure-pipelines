# terragrunt-multi-envs

## Overview

The Azure pipeline defined in this folder is designed to automatically deploy Terraform code in a secure and controlled manner, with dual approval gates for the `apply` stage. It uses Terragrunt, a thin wrapper for Terraform that provides extra tools for working with multiple Terraform modules, to manage and deploy infrastructure as code (IaC).

The pipeline is structured into multiple stages and jobs that handle the initialization, planning, and applying of Terraform configurations across different layers and environments. The stages are designed to run sequentially, with the `apply` stage requiring manual approval in the Azure DevOps environment. Once the `apply` operation is successful, the changes can be merged into the main branch.

The process involves:

1. **Initialization**: Setting up the environment for Terraform and Terragrunt, detecting changes in the repository specific to the environment and layer configurations.
2. **Planning**: Executing the `terragrunt plan` command to create an execution plan for Terraform.
3. **Applying**: After manual approval, the `terragrunt apply` is executed to apply the changes described by the plan.

The pipeline is triggered either manually or by a pull request into the main branch  and uses a date-based naming convention for the run (`name: $(Date:yyyyMMdd).$(Rev:r)`).

## Usage

To use this pipeline, you should:

1. Adapt the example YAML configuration to match your environment specifics, including the service connection, agent pool, and layers you want to track for changes.
2. Commit the YAML files to your Azure DevOps repository in the appropriate directory structure.
3. Configure the necessary service connections in Azure DevOps to allow the pipeline to authenticate with your Azure subscription.
4. Set up the required environment in Azure DevOps for the `apply` stage approval.
5. Manually trigger the pipeline run in Azure DevOps when you want to deploy changes to your infrastructure.
6. Review the Terraform plan in the Azure DevOps pipeline run and approve the `apply` stage to deploy the changes.
7. Once the `apply` is successful and you have verified the infrastructure changes, merge the changes into the main branch to keep your IaC definitions up to date.

Make sure to thoroughly review and test your pipeline configuration in a non-production environment before using it to manage production infrastructure to ensure it behaves as expected.