# redisinsight

![Version: 0.1.0](https://img.shields.io/badge/Version-0.1.0-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 0.1.0](https://img.shields.io/badge/AppVersion-0.1.0-informational?style=flat-square)

A Helm chart for Redis Insight.

**Homepage:** <https://developer.redis.com/>

## Source Code

* <https://developer.redis.com/>

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` | Configuration for affinity |
| autoscaling | object | `{"enabled":false,"maxReplicas":100,"minReplicas":1,"targetCPUUtilizationPercentage":80,"targetMemoryUtilizationPercentage":80}` | Configuration for Autoscaling |
| fullnameOverride | string | `""` | Overrides the fullname |
| image | object | `{"pullPolicy":"IfNotPresent","repository":"redislabs/redisinsight","tag":"latest"}` | Image to use for deploying |
| image.tag | string | `"latest"` | Overrides the image tag whose default is the chart appVersion. |
| imagePullSecrets | list | `[]` | Secret for container registry |
| nameOverride | string | `""` | Overrides the name |
| namespace | object | `{"name":"default"}` | Configure default namespace |
| nodeSelector | object | `{}` | Configuration for nodeSelector |
| podAnnotations | object | `{}` | Additional annotations will be added to the pods of this component as well as to your Deployments or StatefulSets used to create the pods. |
| podSecurityContext | object | `{}` | Pod SecurityContext settings |
| replicaCount | int | `1` | Configure the replicas for the pods |
| resources | object | `{"limits":{"cpu":"500m","memory":"2048M"},"requests":{"cpu":"20m","memory":"512Mi"}}` | Configuration for resources limits (CPU/MEM requests and limits) |
| securityContext | object | `{}` | SecurityContext settings |
| service | object | `{"port":80,"type":"ClusterIP"}` | Service Type which are use to expose service |
| serviceAccount | object | `{"annotations":{},"create":true,"name":""}` | Configuration for service account |
| tolerations | list | `[]` | Configuration for tolerations |

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.9.1](https://github.com/norwoodj/helm-docs/releases/v1.9.1)
