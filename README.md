# gdscan

This Helm chart deploys GD Scan, a cloud-native malware scanner.

<img src="GD Scan Server.png" alt="GDScan" style="width:50%">

The chart deploys one pod, consisting of two containers:
 * GD Scan Server that is responsible for scanning. It contains two scan engines, which are Bitdefender and G DATA.
 * GD Scan Client that initiates scans and receive results. The client is wrapped by an HTTP API.


## Usage

1. Contact G DATA to get an access token (free trail possible): [Contact us](mailto:oem@gdata.de)

The token has to be set in the `secret.dockerconfigjson` variable on deployment.

```yaml
# Example values.yaml
secret: 
    dockerconfigjson: $$_BASE64_ENCODED_TOKEN_$$
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

## Pricing

For pricing details please [contact us](mailto:oem@gdata.de). A free trail is possible.


# Options

| Name                               | Description                                                                                                                      | Value                    |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------ |
| `service.type`                     | service type                                                                                                          | `ClusterIP`              |
| `service.ports.api`                | API service port                                                                                                      | `8080`                   |
| `service.annotations`              | service annotations                                                                                              | `{}`                     |
| `replicaCount`              | number of pods                                                                                              | `1`                     |
