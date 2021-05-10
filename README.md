# Rundeck K8s Deployment

**A Rundeck Deployment in Kubernetes + LDAP or Active Directory to autentication.**

![Rundeck K8s](https://pbs.twimg.com/media/DWwhoieX4AAnfmx.jpg)


## Rundeck configs
In my sample, I used to **Kubernetes Sercrets** to protect my data. **I strongly advise you to do the same**. To understand how to handle _secrets_ in Kubernetes look [here](https://kubernetes.io/docs/tasks/configmap-secret/managing-secret-using-config-file/#create-the-config-file). I've created the _secret_ file`rundeck-secretes` and encode the values using _base64_

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

#### Variables in used
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
---------------------------
:bowtie: **Author**: Weyder

:computer: SRE | DevOps Culture | AWS

:round_pushpin: **LinkedIn**: [@weyderfs](https://www.linkedin.com/in/weyderfs)

:email: **Email**: weyderfs@gmail.com

:coffe: Você pode me ajudar me comprando um [café](https://www.buymeacoffee.com/weyderfs).