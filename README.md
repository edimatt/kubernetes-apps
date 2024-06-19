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

Verify the secret is correctly created:

```
$ kubectl get secrets -n oracle
NAME                     TYPE                             DATA   AGE
oracle-registry-secret   kubernetes.io/dockerconfigjson   1      41h
```

Using the secret, pull the oracle enterprise image from oracle registry.

```
kubectl create -f oracle-deployment.yml
```

Wait for the database to be created. Inspect kubernetes logs on the pod to check completion.
Once completed, refer to the oracle image documentation for instruction to set sys password.
Add a NodePort service to expose the listener:

```
kubectl create -f oracle-service.yml
```

Access the database using *sqlcl*

```
sql sys/system@192.168.1.91:31521/ORCLCDB as sysdba


SQLcl: Release 24.1 Production on Wed Jun 19 09:52:18 2024

Copyright (c) 1982, 2024, Oracle.  All rights reserved.

Connected to:
Oracle Database 21c Enterprise Edition Release 21.0.0.0.0 - Production
Version 21.3.0.0.0

SQL>
```
