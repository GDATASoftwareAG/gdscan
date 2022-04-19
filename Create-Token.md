Since Docker images for the scan server and client are located in a private GitHub repository, you need credentials for pulling. The authentication requires some prerequisites:

1. Create a new access token with the only right "read packages". 
2. Create a string in the format $USERNAME:$PASSWORD and encode it to base64. 
3. Create a docker.config JSON file:
```json
{
    "auths": {
        "ghcr.io": {
            "auth": "$$_BASE64_ENCODED_USER_NAME_AND_PASSWORD_$$"
        }
    }
}
```
4. Encode docker.config to base64 and copy it into a values.yaml file:

```yaml
secret: 
    dockerconfigjson: $$_BASE64_ENCODED_DOCKER_CONFIG_$$
```
5. Test the `values.yaml` locally before prodiving it to a customer.
