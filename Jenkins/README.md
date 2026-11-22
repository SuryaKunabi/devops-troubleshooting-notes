
# Jenkins Troubleshooting – Real Issues & Solutions

This document records CI/CD pipeline and automation troubleshooting encountered while practising Jenkins with Docker and GitHub.

## 1. Jenkins Pipeline – Docker Integration

**Problem:**

While practising Jenkins pipeline automation, Docker and Node.js environments needed to be configured for pipeline execution.

**Environment:**

- Jenkins
- Docker
- Node.js 21 Alpine image

**Pipeline Environment:**

```groovy
pipeline {
    agent {
        docker {
            image 'node:21-alpine'
        }
    }

    stages {
        stage('Build') {
            steps {
                sh 'node --version'
                sh 'npm --version'
            }
        }
    }
}
```

**Troubleshooting Checks:**

```bash
docker version
docker ps
```

Verify:
- Docker daemon availability.
- Jenkins Docker permissions.
- Docker Pipeline plugin.
- Agent access to Docker.

**Learning:** Jenkins pipelines can use Docker containers to provide consistent build environments.

---

## 2. Jenkins – Pipeline Execution Troubleshooting

**Problem:**

Pipeline execution depends on correct Jenkins agent configuration, Docker availability, and pipeline syntax.

**Troubleshooting:**

- Review Jenkins console output.
- Verify the agent is online.
- Check Docker image availability.
- Validate Jenkinsfile syntax.
- Verify required plugins.

**Useful Commands:**

```bash
sudo systemctl status jenkins
sudo journalctl -u jenkins -f
docker ps
docker images
```

**Learning:** Console output and agent logs are important for identifying pipeline failures.

---

## Key Learnings

- Understand Jenkins declarative pipelines.
- Integrate Docker with Jenkins.
- Use containerized build environments.
- Investigate pipeline execution failures.
