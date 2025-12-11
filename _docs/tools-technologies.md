---
layout: doc
title: Tools & Technologies
nav_order: 3
---

# Tools & Technologies

Essential DevOps tools and platforms we recommend.

## Container Orchestration

### Docker
Containerize applications for consistency across environments.

- **Use Case**: Package applications and dependencies
- **Benefits**: Portability, isolation, lightweight
- **Learn More**: [Docker Documentation](https://docs.docker.com/)

### Kubernetes
Orchestrate containerized applications at scale.

- **Use Case**: Deploy, manage, and scale containerized apps
- **Benefits**: High availability, auto-scaling, self-healing
- **Learn More**: [Kubernetes Documentation](https://kubernetes.io/docs/)

## Infrastructure as Code

### Terraform
Define infrastructure using code.

```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

- **Benefits**: Reproducible, versionable, scalable
- **Learn More**: [Terraform Documentation](https://www.terraform.io/docs)

### Bicep
Azure's domain-specific language for IaC.

```bicep
resource storageAccount 'Microsoft.Storage/storageAccounts@2021-04-01' = {
  name: storageAccountName
  location: location
  kind: 'StorageV2'
  sku: {
    name: 'Standard_LRS'
  }
}
```

## Cloud Platforms

### AWS
Industry-leading cloud platform.

- EC2, S3, RDS, Lambda
- [AWS Documentation](https://docs.aws.amazon.com/)

### Azure
Microsoft's comprehensive cloud platform.

- App Services, Cosmos DB, Function Apps
- [Azure Documentation](https://learn.microsoft.com/azure/)

### GCP
Google's cloud platform.

- Compute Engine, Cloud Storage, Cloud Functions
- [GCP Documentation](https://cloud.google.com/docs)

## CI/CD Platforms

### GitHub Actions
Automate workflows directly in GitHub.

### GitLab CI
Native CI/CD in GitLab.

### Jenkins
Open-source automation server.

## Monitoring & Observability

### Prometheus
Metrics collection and alerting.

### ELK Stack
Elasticsearch, Logstash, Kibana for logging.

### Datadog
Comprehensive monitoring platform.

## Additional Resources

- See [Best Practices]({{ site.baseurl }}/docs/best-practices/) for usage guidelines
- Check [Getting Started]({{ site.baseurl }}/docs/getting-started/) for fundamentals
