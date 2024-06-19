Create a config.json file containing:

```
{
  "auths": {
    "container-registry.oracle.com": {
      "auth": "base64-encoded-username-and-token"
    }
  }
}
```

Where base64-encoded-username-and-token is:

```
echo -n 'username:token' | base64
```

Replace username and token with your oracle account credentials.
Create the secret:

```
kubectl create secret generic oracle-registry-secret --from-file=.dockerconfigjson=./config.json --type=kubernetes.io/dockerconfigjson
```
