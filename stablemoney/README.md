## Getting Started

## Introduction

Observability helps understand system behavior via logs, metrics, and traces. It's key for reliability and debugging in microservices. This guide covers setting up an observability stack in Kubernetes using modern tools.


## Core Components of Observability

- **Metrics**:  Quantitative data like CPU usage, memory, latency, and request rates to monitor system performance.

- **Logging**: Event records capturing errors and info messages for context, debugging, and auditing.

- **Traces**: End-to-end request flow across services, revealing latency, bottlenecks, and dependencies.

## Observability Stack (011y) – Prerequisites

![image](https://github.com/user-attachments/assets/4c70e593-bd6b-4973-aa78-c2458fc32be0)

![image](https://github.com/user-attachments/assets/1aa4c283-432e-468c-8c74-810adecdba48)

## Tools Used in Observability Setup

- **VictoriaMetrics**: High-performance time-series database for storing and querying metrics.

- **Loki**: Scalable log aggregation system for collecting and storing logs efficiently.

- **Alertmanager**: Manages alerts from metrics or logs and notifies relevant teams of system issues.

- **Tempo**: Distributed tracing system for visualizing request flows across microservices.

## Deployment Method

The observability stack is deployed using Helm charts, providing a standardized, repeatable, and scalable approach for managing all components efficiently within the Kubernetes environment.


## Step-by-Step Installation

1 . **VictoriaMetrics Setup**

Clone the Repository and navigate to the VictoriaMetrics Directory

```
git clone https://github.com/ot-client/stablemoney/blob/o11y/README.md

cd /home/opstree/stablemoney/o11y_stack/staging/o11y_stack/victoriametrics/
```

## Execute Makefile commands one by one with brief explanations.

- Silently downloads and saves the vm-0.0.3.tgz Helm chart from GitHub.
```
- curl https://github.com/OT-CONTAINER-KIT/helm-charts/releases/download/vm-0.0.3/vm-0.0.3.tgz -O -J -L -s
```
- Generates manifests from the vm Helm chart using values.yaml.
```
- helm template --name-template=vm vm -n monitoring -f values.yaml
```
- Applies VictoriaMetrics CRDs to the Kubernetes cluster.

```
- kubectl apply -f https://raw.githubusercontent.com/VictoriaMetrics/helm-charts/refs/tags/victoria-metrics-k8s-stack-0.25.5/charts/victoria-metrics-k8s-stack/charts/crds/crds/crd.yaml
```

- Performs a dry-run to create the monitoring namespace and applies it to the cluster.
```
- kubectl create namespace monitoring --dry-run=client -o yaml | kubectl apply -f -
```
- Applie the  Helm chart manifests using values.yaml.

```
- helm template --name-template=vm vm -n monitoring -f values.yaml | kubectl apply -f -
```
## This image represents a sample Makefile.

![image](https://github.com/user-attachments/assets/a4f8fd81-0428-4058-a0e3-d60b0a6c9e94)

## Verification
- Switch to the Monitoring Namespace
```
kubectl config set-context --current --namespace=monitoring
```
- Check Resources: 
```
kubectl get all -n monitoring 
```

This ensures the successful creation of all required resources in the monitoring namespace.

## Expected output:
![image](https://github.com/user-attachments/assets/a3707274-54e9-4ac9-88c5-b41a08a41768)

![image](https://github.com/user-attachments/assets/7a0e7a96-05cb-4825-8850-d762a2eeaab4)

2 .  **Logging Setup (Loki)**

Navigate to the Logging Folder

**Open the Makefile and Execute Commands One by One**

```
- curl https://github.com/OT-CONTAINER-KIT/helm-charts/releases/download/loki-1.0.1/loki-1.0.1.tgz -O -J -L
- helm template --name-template=logging loki/ -n logging -f values.yaml
- kubectl create namespace logging --dry-run=client -o yaml | kubectl apply -f -
- helm template --name-template=logging loki/ -n logging -f values.yaml | kubectl apply -f -
```

**This image represents a sample Makefile**

![image](https://github.com/user-attachments/assets/9956e9ce-7a76-4a2a-bf33-55a5e06d8092)

## Verification:

```
kubectl config set-context --current --namespace=logging 
```

- Check Resources: 

```
kubectl get all -n logging
```

## Expected Output

![image](https://github.com/user-attachments/assets/d08637fe-73bc-41ae-97e2-a52cf03d6ebb)


3 . **OpenTelemetry (OTel) Setup**

Navigate to OpenTelemetry Folder

**Open the Makefile and Execute Commands**

![image](https://github.com/user-attachments/assets/92afe816-f7b0-4481-8228-de4197926879)

## Verification:

```
kubectl config set-context --current --namespace=observability
```

```
kubectl get all -n observability
```

## Expected Output

![image](https://github.com/user-attachments/assets/65b4d157-d612-4061-89b4-0f4151660368)


4 . **Tempo Setup**

Navigate to Tempo Folder

**Open the Makefile and Execute Commands One by One**

```
- curl https://github.com/OT-CONTAINER-KIT/helm-charts/releases/download/otel-operator-1.0.0/otel-operator-1.0.0.tgz -O -J -L

- helm template --name-template=otel otel-operator/ -n observability -f values.yaml

- kubectl create namespace observability --dry-run=client -o yaml | kubectl apply -f -

- helm template --name-template=otel otel-operator/ -n observability -f values.yaml | kubectl apply -f -
```

**This image represents a sample Makefile**

![image](https://github.com/user-attachments/assets/9542f344-5c8d-4bb0-b5a9-1183c7576704)


## Verification:

```
kubectl config set-context --current --namespace=observability
```

```
kubectl get all -n observability
```
## Expected Output

![image](https://github.com/user-attachments/assets/a860713c-92c7-4d58-94d4-7dcd972e41f9)




