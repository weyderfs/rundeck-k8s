# Chart Name: app-chart

## Introduction

This Helm chart deploys a generic application using a Kubernetes Deployment and Service. It provides a basic template that can be customized for various stateless applications.

## Prerequisites

*   Kubernetes cluster (version 1.19+ recommended)
*   Helm 3 installed

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
helm install my-release ./app-chart
```

To install the chart with a custom values file:

```bash
helm install my-release ./app-chart -f myvalues.yaml
```

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
helm uninstall my-release
```

## Configuration

The following table lists the configurable parameters of the `app-chart` chart and their default values.

| Parameter             | Description                                     | Default          |
|-----------------------|-------------------------------------------------|------------------|
| `replicaCount`        | Number of replicas for the Deployment           | `1`              |
| `image.repository`    | Container image repository                      | `nginx`          |
| `image.tag`           | Container image tag (overrides chart's appVersion) | `""` (uses chart appVersion) |
| `image.pullPolicy`    | Container image pull policy                     | `IfNotPresent`   |
| `nameOverride`        | String to override the chart name               | `""`             |
| `fullnameOverride`    | String to override the fully qualified app name   | `""`             |
| `serviceAccount.create` | Specifies whether a service account should be created | `true`           |
| `serviceAccount.name` | The name of the service account to use          | `""` (generated) |
| `podAnnotations`      | Annotations to add to the pod                   | `{}`             |
| `podSecurityContext`  | Pod security context                            | `{}`             |
| `securityContext`     | Container security context                      | `{}`             |
| `service.type`        | Type of Kubernetes service to create            | `ClusterIP`      |
| `service.port`        | Port for the Kubernetes service                 | `80`             |
| `ingress.enabled`     | Enable ingress controller resource              | `false`          |
| `ingress.annotations` | Annotations for the ingress resource            | `{}`             |
| `ingress.hosts`       | Hostname(s) for the ingress                     | `chart-example.local` |
| `ingress.tls`         | TLS configuration for the ingress               | `[]`             |
| `resources`           | CPU/memory resource requests and limits         | `{}`             |
| `autoscaling.enabled` | Enable Horizontal Pod Autoscaler                | `false`          |
| `autoscaling.minReplicas` | Minimum number of replicas for HPA          | `1`              |
| `autoscaling.maxReplicas` | Maximum number of replicas for HPA          | `100`            |
| `autoscaling.targetCPUUtilizationPercentage` | Target CPU utilization for HPA | `80`             |
| `nodeSelector`        | Node labels for pod assignment                  | `{}`             |
| `tolerations`         | Node taints to tolerate                         | `[]`             |
| `affinity`            | Node/pod affinities                             | `{}`             |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install` or `helm upgrade`. For example:

```bash
helm install my-release ./app-chart --set replicaCount=3 --set image.repository=my-custom-app
```

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example:

```bash
helm install my-release ./app-chart -f myvalues.yaml
```

All values in the `app-chart/values.yaml` file can be configured. This README highlights the most common ones. Refer to the `values.yaml` file for a full list of configurable options.
