# Rundeck K8s Deployment

**A Rundeck Deployment in Kubernetes + LDAP or Active Directory to autentication.**

![Rundeck K8s](https://pbs.twimg.com/media/DWwhoieX4AAnfmx.jpg)


## Rundeck configs
This deployment uses parameterized Kubernetes manifest files. Key values like namespace (`{{NAMESPACE}}`) and domain (`{{DOMAIN}}`) are placeholders. Refer to the "Placeholder Replacement" section for instructions on how to substitute these with your specific values before deployment.

The main deployment configuration is in `rundeck.yaml`. This file has been enhanced to include:
*   **Resource Requests and Limits:** CPU and memory requests and limits are defined for the Rundeck container to ensure predictable performance and resource allocation within your Kubernetes cluster.
*   **Liveness and Readiness Probes:** These probes are configured to help Kubernetes manage the Rundeck pod's lifecycle, improving reliability by automatically restarting unhealthy containers and ensuring traffic is only routed to ready pods.

In my sample, I used to **Kubernetes Sercrets** to protect my data. **I strongly advise you to do the same**. To understand how to handle _secrets_ in Kubernetes look [here](https://kubernetes.io/docs/tasks/configmap-secret/managing-secret-using-config-file/#create-the-config-file). I've created the _secret_ file `rundeck-secretes` and encode the values using _base64_

Example to enconde:
```
$ echo -n 'myvalue' | openssl ba64
bXl2YWx1ZQ==
```
To decode:
```
echo -n 'bXl2YWx1ZQ==' | base64 --decode
```

#### Rundeck Role
The file `rundeck-admin-role.yaml` is used to create a policy that _allow_ your Active Directory users use the Rundeck features, in this file gave the **admin** permitions to users. To manage policy you can check [here](https://docs.rundeck.com/docs/administration/security/authorization.html#access-control-policy). In my deployment this policy is provide via **secret**.

#### Variables in use:
```
RUNDECK_DATABASE_DRIVER
RUNDECK_DATABASE_PASSWORD
RUNDECK_DATABASE_URL
RUNDECK_DATABASE_USERNAME
RUNDECK_GRAILS_URL
RUNDECK_JAAS_MODULES_0
RUNDECK_LOGGING_AUDIT_ENABLED
RUNDECK_SERVER_FORWARDED
```

## LDAP or Acive Directory configs

It's necessary creat at AD user to _bind_ the authentication validation of the user. And you need set your Active Direcotry florest to vars, follow a example bellow:

```
RUNDECK_JAAS_LDAP_BINDDN=rundeckuser@mycompany.foo
RUNDECK_JAAS_LDAP_BINDPASSWORD=somepassword
RUNDECK_JAAS_LDAP_FLAG=sufficient
RUNDECK_JAAS_LDAP_PROVIDERURL=ldap://0.0.0.0:389
RUNDECK_JAAS_LDAP_ROLEBASEDN=OU=RundeckRoles,OU=Users,OU=MYCOMPANY,DC=mycompany,DC=foo
RUNDECK_JAAS_LDAP_ROLEMEMBERATTRIBUTE=member
RUNDECK_JAAS_LDAP_ROLEOBJECTCLASS=group
RUNDECK_JAAS_LDAP_USERBASEDN=OU=Users,OU=MYCOMPANY,DC=foo,DC=mycompany
RUNDECK_JAAS_LDAP_USERIDATTRIBUTE=sAMAccountName
RUNDECK_JAAS_LDAP_USERRDNATTRIBUTE=sAMAccountName
RUNDECK_JAAS_MODULES_0=JettyCombinedLdapLoginModule
```

## Placeholder Replacement

Before deploying Rundeck, you need to replace the placeholders in the YAML files with your specific values. The following placeholders are used:

*   `{{NAMESPACE}}`: Replace with the Kubernetes namespace where you want to deploy Rundeck.
*   `{{DOMAIN}}`: Replace with the domain name for accessing Rundeck.

You can replace these placeholders using `sed` or by manually editing the files.

**Example using `sed`:**

```bash
export NAMESPACE_VALUE="your-namespace"
export DOMAIN_VALUE="your.domain.com"
sed -i "s/{{NAMESPACE}}/$NAMESPACE_VALUE/g" ./*.yaml
sed -i "s/{{DOMAIN}}/$DOMAIN_VALUE/g" rundeck.yaml
```

## Secrets Management

The current method for managing secrets involves manually creating Kubernetes Secrets using `kubectl create secret`. This typically involves base64 encoding the secret values and applying them to the cluster. While functional, this approach may not be suitable for all environments, especially production, due to the manual steps and the fact that secrets are stored in plain text (though base64 encoded) in manifests or version control if not handled carefully.

For more robust secret management in production, consider using tools like:
*   [HashiCorp Vault](https://www.vaultproject.io/docs): Vault provides secure storage, access control, and dynamic secrets generation. It can integrate with Kubernetes to inject secrets directly into pods.
*   [Sealed Secrets](https://github.com/bitnami-labs/sealed-secrets): Another approach for GitOps workflows is Sealed Secrets. This allows you to encrypt secrets so they can be safely stored in Git repositories. A controller in the cluster then decrypts them for use.

---------------------------
:bowtie: **Author**: Weyder

:computer: SRE | DevOps Culture | AWS

:round_pushpin: **LinkedIn**: [@weyderfs](https://www.linkedin.com/in/weyderfs)

:email: **Email**: weyderfs@gmail.com

:coffee: You can support me with a [coffee](https://www.buymeacoffee.com/weyderfs).