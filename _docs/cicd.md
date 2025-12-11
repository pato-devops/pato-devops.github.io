---
layout: doc
title: CI/CD Pipelines
nav_order: 6
---

# CI/CD Pipelines

Automate building, testing, and deploying your applications.

## What is CI/CD?

### Continuous Integration (CI)
Automatically build and test code changes as they're committed.

### Continuous Deployment (CD)
Automatically deploy tested code to production.

### Continuous Delivery (CD)
Prepare code for production deployment with manual approval.

## Pipeline Stages

```
Source → Build → Test → Security → Deploy → Monitor
```

### Source
- Code commit triggers pipeline
- Webhook from Git service

### Build
```yaml
- Checkout code
- Compile/build
- Run unit tests
- Generate artifacts
- Build container image
```

### Test
```yaml
- Integration tests
- End-to-end tests
- Performance tests
- Smoke tests
```

### Security
```yaml
- SAST (Static Application Security Testing)
- Dependency scanning
- Container scanning
- Secret scanning
```

### Deploy
```yaml
- Create infrastructure (IaC)
- Deploy application
- Run smoke tests
- Monitor deployment
```

### Monitor
```yaml
- Health checks
- Performance metrics
- Error tracking
- User feedback
```

## GitHub Actions Example

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run tests
      run: npm test
    
    - name: Build
      run: npm run build
    
    - name: Security scan
      run: npm audit
    
    - name: Deploy
      if: github.ref == 'refs/heads/main'
      run: npm run deploy

  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Run integration tests
      run: |
        docker-compose up -d
        npm run test:integration
        docker-compose down
```

## GitLab CI Example

```yaml
stages:
  - build
  - test
  - deploy

variables:
  REGISTRY: registry.gitlab.com
  IMAGE: $REGISTRY/$CI_PROJECT_PATH

build:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t $IMAGE:$CI_COMMIT_SHA .
    - docker push $IMAGE:$CI_COMMIT_SHA

test:
  stage: test
  image: $REGISTRY/$CI_PROJECT_PATH:$CI_COMMIT_SHA
  script:
    - npm test
    - npm run lint

deploy:
  stage: deploy
  image: bitnami/kubectl:latest
  script:
    - kubectl set image deployment/app app=$IMAGE:$CI_COMMIT_SHA
  only:
    - main
```

## Best Practices

### Pipeline Design
- **Keep it fast**: Optimize for speed
- **Fail fast**: Stop on first error
- **Parallel execution**: Run independent steps concurrently
- **Artifacts**: Cache dependencies
- **Notifications**: Alert on failures

### Testing Strategy
```
Unit Tests (Fast) → Integration Tests → E2E Tests (Slow)
```

- Unit tests: Frequent, fast, isolated
- Integration tests: Medium frequency
- E2E tests: Less frequent, slower, full system

### Deployment Safety
- **Staging environment**: Test before production
- **Blue-green deployments**: Zero downtime
- **Canary releases**: Gradual rollout
- **Automatic rollback**: On health check failure

### Monitoring & Observability
```yaml
deploy:
  after_script:
    - |
      kubectl rollout status deployment/app
      if [ $? -ne 0 ]; then
        kubectl rollout undo deployment/app
      fi
```

## Advanced Patterns

### Matrix Builds
Test against multiple versions:

```yaml
strategy:
  matrix:
    node-version: [16, 18, 20]
    os: [ubuntu-latest, windows-latest]

runs-on: ${{ matrix.os }}

steps:
  - uses: actions/setup-node@v3
    with:
      node-version: ${{ matrix.node-version }}
```

### Conditional Deployments
```yaml
deploy:
  if: |
    github.event_name == 'push' &&
    github.ref == 'refs/heads/main'
  script:
    - ./deploy.sh
```

## Troubleshooting

### Pipeline Failures
1. Check logs for error details
2. Reproduce locally
3. Use debug mode
4. Check resource quotas
5. Verify permissions

### Common Issues

**Secret not accessible**:
```bash
# Verify secret exists
gh secret list

# Update secret
gh secret set MY_SECRET
```

**Timeout errors**:
- Increase timeout limits
- Optimize performance
- Add caching

**Flaky tests**:
- Review test isolation
- Check for race conditions
- Add retry logic
- Increase timeouts

## Tools

- **GitHub Actions**: Native to GitHub
- **GitLab CI**: Native to GitLab
- **Jenkins**: Open-source automation
- **CircleCI**: Cloud-native CI/CD
- **Travis CI**: GitHub-focused

## Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [GitLab CI Documentation](https://docs.gitlab.com/ee/ci/)
- [Jenkins Documentation](https://www.jenkins.io/doc/)
