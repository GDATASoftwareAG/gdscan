# gdscan

This Helm chart deploys GD Scan, an HTTP-based virus scanner. 

<img src="GD Scan Server.png" alt="GDScan" style="width:50%">

The chart deploys one pod, consisting of two containers:
 * GD Scan Server that is responsible for scanning. It contains two scan engines, which are Bitdefender and G DATA.
 * GD Scan Client that initiates scans and receive results. The client is wrapped by an HTTP API.


## Usage

Since Docker images for the scan server and client are located in a private GitHub repository, you need username and password for pulling. The authentication requires some prerequisites:

1. Create a string in the format $USERNAME:$PASSWORD and encode it to base64. 

2. Create a docker.config JSON file:
```
{
    "auths": {
        "ghcr.io": {
            "auth": "$$_BASE64_ENCODED_USER_NAME_AND_PASSWORD_$$"
        }
    }
}
```
3. Encode docker.config to base64 and copy it into a values.yaml file:

```
secret: 
    dockerconfigjson: $$_BASE64_ENCODED_DOCKER_CONFIG_$$
```

4. Add GD Scan repository:

```
helm repo add gdscan https://gdatasoftwareag.github.io/gdscan/
```

5. Now you are able to start GD Scan:

```
helm install gdscan gdscan/gdscan -f values.yaml
```

For further information about usage, please checkout the application's Swagger UI.

## Credentials

If you are interested and want to use the GD Scan by yourself, please [contact us](mailto:oem@gdata.de).
