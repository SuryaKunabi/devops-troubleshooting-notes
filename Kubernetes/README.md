
# Kubernetes Troubleshooting – Real Issues & Solutions

This document contains Kubernetes errors encountered while deploying and managing a Django application using Docker Desktop and Minikube on Windows.

## 1. Minikube – VirtualBox VT-X / AMD-V Error

**Problem:**

Minikube failed to start because hardware virtualization was unavailable or not enabled.

**Errors encountered:**
- VT-X/AMD-V virtualization errors.
- Virtual Machine Platform was disabled.

**Root Cause:**

Hardware virtualization or the required Windows virtualization feature was not enabled or available to the selected Minikube driver.

**Troubleshooting:**

1. Checked virtualization support.
2. Enabled Virtual Machine Platform in Windows Features.
3. Reviewed Minikube driver configuration.

**Commands:**

```bash
minikube start --driver=docker
minikube status
```

**Learning:** Minikube requires a properly configured virtualization environment and compatible driver.

---

## 2. Minikube – PROVIDER_DOCKER_NOT_RUNNING

**Problem:**

Minikube could not start because the Docker provider was unavailable.

**Error:**

```text
PROVIDER_DOCKER_NOT_RUNNING
```

**Root Cause:**

Docker Desktop was not running or Minikube could not communicate with the Docker daemon.

**Solution:**

1. Start Docker Desktop.
2. Wait until Docker Engine is running.
3. Verify Docker connectivity.
4. Restart Minikube.

**Commands:**

```bash
docker version
docker info
minikube start --driver=docker
minikube status
```

---

## 3. Kubernetes – localhost:8080 Connection Refused

**Problem:**

kubectl could not communicate with the Kubernetes API server.

**Error:**

```text
The connection to the server localhost:8080
was refused - did you specify the right host or port?
```

**Root Cause:**

The Kubernetes context or kubeconfig was not configured correctly, or the cluster was not running.

**Troubleshooting:**

```bash
kubectl config get-contexts
kubectl config current-context
kubectl cluster-info
minikube status
```

**Solution:**

Ensure Minikube is running and the correct context is selected.

```bash
minikube start --driver=docker
minikube update-context
kubectl config use-context minikube
kubectl get nodes
```

---

## 4. Django Application – ImagePullBackOff

**Problem:**

Django application Pods failed to start because Kubernetes could not pull the container image.

**Error:**

```text
ImagePullBackOff
ErrImagePull
```

**Environment:**

- Application: Django
- Container: Docker
- Orchestration: Kubernetes
- Cluster: Minikube

**Root Cause:**

The configured container image could not be pulled successfully.

**Troubleshooting:**

```bash
kubectl get pods
kubectl describe pod <pod-name>
kubectl get events --sort-by=.metadata.creationTimestamp
docker images
```

**Solution / Checks:**

1. Verify the image name and tag.
2. Confirm the image exists in Docker Hub or the configured registry.
3. Check registry authentication if required.
4. Verify imagePullPolicy.
5. Check Minikube image availability.

---

## 5. Kubernetes – Service Not Found

**Problem:**

Ingress configuration referenced a service that did not exist.

**Error:**

```text
service "test" not found
```

**Root Cause:**

The Ingress resource referenced the service name `test`, but the expected backend service was not available in the same namespace.

**Solution:**

Verify service names and namespaces.

```bash
kubectl get services
kubectl get ingress
kubectl describe ingress <ingress-name>
```

Update the Ingress backend service name to match the actual Kubernetes Service.

---

## 6. Kubernetes – NodePort Not Accessible

**Problem:**

Django application was deployed using NodePort, but the application was not accessible through the expected browser URL.

**Environment:**

- Windows
- Docker Desktop
- Minikube
- Kubernetes NodePort

**Troubleshooting:**

```bash
kubectl get svc
kubectl get pods -o wide
kubectl get endpoints
minikube ip
minikube service django-service --url
```

**Root Cause / Investigation:**

The Minikube Docker driver on Windows introduced networking and host-access complications.

**Learning:**

A NodePort service being created successfully does not always guarantee that its port is directly accessible from the Windows host. Verify the driver networking path and Minikube service URL.

---

## 7. Prometheus and Grafana – NodePort Access Failure

**Problem:**

Prometheus and Grafana were installed using Helm, but their NodePort URLs were not accessible through the browser.

**Environment:**

- Minikube
- Helm
- Prometheus
- Grafana
- Docker Desktop on Windows

**Commands:**

```bash
helm list
kubectl get pods
kubectl get svc
minikube ip
```

**Observed Configuration:**

Prometheus NodePort: `30139`

Minikube IP: `192.168.49.2`

**Root Cause / Investigation:**

The Docker driver networking configuration on Windows affected direct access to NodePort services.

**Learning:**

Check service endpoints, Minikube networking, and the service tunnel/access method before assuming the monitoring application is down.

---

## Key Learnings

- Understand Minikube drivers and Docker networking.
- Troubleshoot Pod lifecycle failures.
- Verify images before deploying applications.
- Understand Kubernetes Services and Ingress.
- Diagnose connectivity between Windows, Minikube, and Pods.
- Use Helm to install and manage monitoring tools.

