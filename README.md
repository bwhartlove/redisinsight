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


## Enabling TLS
It is possible to deploy Nginux with Redisinsight to work as a reverse proxy.  The reverse proxy will allow for TLS to be enabled or to implement basic-auth.  To enable TLS you must have a secret with a key.  You can create a key and certificate and have it signed by a trusted authority and load it in using below command:

```	
kubectl create secret tls redis-insight-tls --key="tls.key" --cert="tls.crt"
```

It is also possible to utilize cert-manager.  The below yaml will create a self signed CA and sign a certificate.  Make sure you update the external.domain to the external domain of your Kubernetes cluster.  There are other ways to do this (Lets ecrypt as an example) but that is outside the scope of this readme.

```
apiVersion: cert-manager.io/v1
kind: Issuer
metadata:
  name: redis-insight-issuer
spec:
  selfSigned: {}
---
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: redis-insight
spec:
  commonName: redis-insight.external.domain
  isCA: false
  privateKey:
    algorithm: RSA
    encoding: PKCS1
    size: 2048
  usages:
    - server auth
  secretName: redis-insight
  dnsNames:
    # Ingress domain
    - redis-insight.external.domain
    # Internal domain
    - redis-insight
  issuerRef:
    name: redis-insight-issuer
```

To enable the nginx proxy 

Set the externName to the fqdn of you external environment:

```
externalName: redisinsight.external.domain
```

Configure the Proxy to be enabled and refer to the tls secret you created earlier

```
# -- Configure to deploy with Proxy -- This will enable TLS and authentication
proxy:
  enabled: true
  ## Needs to be different then the port redisinsight is listening on (port 8001)
  nginxPort: 9000
  tls:
    tlsEnabled: true
    tlsSecret: redis-insight-tls
```

## Configure authentication
If you want to enable authentication to the console you can use a basic auth file.  You must have the proxy enabled for this to work.  To do this you need to generate a hash.  The easiest way to do this is run the command:

```
openssl passwd -6
```

Then configure auth to be enabled and put the password hash you created in the password.  The password included is "password"

```
  auth:
    enabled: false
    user: redisinsight
    ###  To create this has you can use either command:
    ###     openssl passwd -6
    ###     htpasswd -5 -n username  (only take the password portion after the first :
    password: $6$odj8aDdQK14QXYtc$U3NPBgc/4ffABDpC4BrbEonUW1VtMEFvYUsynv/4QcaUhYbS2uqUywCoRsoxY.zKacYmeHBI5.cr.RfyK2tge1
```