---
layout: doc
title: Infrastructure as Code
nav_order: 5
---

# Infrastructure as Code (IaC)

Manage infrastructure through code and version control.

## Overview

Infrastructure as Code allows you to define your infrastructure using configuration files that can be:
- Versioned in Git
- Peer reviewed
- Automatically tested
- Deployed consistently

## Tools

### Terraform
Multi-cloud IaC tool with a declarative syntax.

**Advantages**:
- Cloud-agnostic
- Large community
- Rich state management
- Module ecosystem

**Example**:
```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"
  
  tags = {
    Name = "web-server"
  }
}
```

### CloudFormation
AWS native IaC tool.

**Advantages**:
- Deep AWS integration
- Drift detection
- Change sets
- Stack policies

### Bicep
Azure's simplified IaC language.

**Advantages**:
- Cleaner syntax than ARM templates
- Strong type support
- Excellent tooling
- Full Azure integration

## Best Practices

### State Management
```hcl
# Store state remotely
terraform {
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "prod/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-locks"
  }
}
```

### Code Organization
```
.
├── environments/
│   ├── dev/
│   ├── staging/
│   └── prod/
├── modules/
│   ├── vpc/
│   ├── rds/
│   └── security/
├── variables.tf
├── outputs.tf
└── terraform.tf
```

### Validation & Testing
```bash
# Validate syntax
terraform validate

# Check formatting
terraform fmt -check

# Plan changes
terraform plan -out=tfplan

# Test with tool like terratest
go test -v
```

## Common Patterns

### Module Pattern
```hcl
module "vpc" {
  source = "./modules/vpc"
  
  cidr_block  = "10.0.0.0/16"
  environment = var.environment
}
```

### Variable Pattern
```hcl
variable "instance_count" {
  type        = number
  description = "Number of instances"
  default     = 3
}

variable "tags" {
  type        = map(string)
  description = "Common tags"
  default = {
    Environment = "prod"
    Owner       = "devops"
  }
}
```

## Workflows

### GitOps Approach
```
Developer → Git Commit → CI/CD Pipeline → Terraform Apply → Infrastructure Updated
                           ↓
                    Plan → Review → Approve
```

## Security in IaC

- Never hardcode secrets (use secret management)
- Implement RBAC for state access
- Scan for misconfigurations
- Use linters like `tflint`
- Implement least privilege in policies

## Troubleshooting

### State Lock
```bash
# If stuck with a lock
terraform force-unlock LOCK_ID
```

### Drift Detection
```bash
# Check for manual changes
terraform plan
```

## Resources

- [Terraform Documentation](https://www.terraform.io/docs)
- [CloudFormation User Guide](https://docs.aws.amazon.com/cloudformation/)
- [Bicep Documentation](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/)
