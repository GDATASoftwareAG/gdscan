# gdscan

This Helm chart deploys GD Scan, a cloud-native malware scanner.

    This project is deprecated and the lives on in [VaaS](https://github.com/GDATASoftwareAG/vaas).
    VaaS is a malware protection service for the cloud, which is available as a SaaS solution hosted by 
    G DATA CyberDefense and on-premise for self hosting.

<img src="GD Scan Server.png" alt="GDScan" style="width:50%">

The chart deploys one pod, consisting of two containers:
 * GD Scan Server that is responsible for scanning.
 * GD Scan Client that initiates scans and receive results. The client is wrapped by an HTTP API.


## Usage

1. Add token to Docker registry config.

The token has to be set in the `secret.dockerconfigjson` variable on deployment.

```yaml
# Example values.yaml
secret: 
    dockerconfigjson: $$_BASE64_ENCODED_JSON_CONTAINING_TOKEN_$$
```

Example of the dockerconfigjson

```json
{
    "auths": {
            "ghcr.io": {
                    "auth": "$$_BASE64_ENCODED_USERNAME_AND_TOKEN_$$"
            }
    }
}
```

3. Add GD Scan repository:

```
helm repo add gdscan https://gdatasoftwareag.github.io/gdscan/
```

3. Now you are able to start GD Scan:

```bash
helm install gdscan gdscan/gdscan -f values.yaml
```

Usage is described in the application's Swagger UI.

4. Updating GD Scan

To get the latest malware signatures, update the helm chart with the following command. An update is recommended every hour.

```bash
helm repo update
helm upgrade gdscan  gdscan/gdscan -f values.yaml
```

# Options

| Name                            | Description                                                                      | Value       |
| ------------------------------- | -------------------------------------------------------------------------------- | ----------- |
| `service.type`                  | service type                                                                     | `ClusterIP` |
| `service.ports.api`             | API service port                                                                 | `8080`      |
| `service.annotations`           | service annotations                                                              | `{}`        |
| `replicaCount`                  | number of pods                                                                   | `1`         |
| `autoscaling.enabled`           | enable auto scaling                                                              | `false`     |
| `autoscaling.maxReplicas`       | maximum number of replicas                                                       | `20`        |
| `autoscaling.metrics`           | custom metrics for auto scaling                                                  |             |
| `terminationGracePeriodSeconds` | max time in seconds for scans to complete                                        | `30`        |
| `debug`                         | when activated the containers produce more verbose eg for analysing scan timings | `false`     |
| `server.tempfolder.inmemory`    | activates inmemory tempfolder                                                    | `false`     |
| `client.tempfolder.inmemory`    | activates inmemory tempfolder                                                    | `false`     |

