## Mautic Openshift

To Run:

1. Process and apply the mariadb secret.yaml

    ```oc process -f ./openshift/secret.yaml | oc apply -f - -n <namespace>```
 This command will create arbitrary values    
 parameters:
- name: NAME
  displayName: name
  description: the value used to group components by a common name
  value: mautic
- name: DATABASE_USER
  description: the database root user
  displayName: Database Root Username
  from: '[a-zA-Z0-9]{8}'
  generate: expression
- name: DATABASE_NAME
  description: the database name
  displayName: Database Name
  from: '[a-zA-Z0-9]{8}'
  generate: expression
- name: DATABASE_USER_PASSWORD
  description: The database user password
  displayName: Database User Password
  from: '[a-zA-Z0-9]{8}'
  generate: expression
- name: DATABASE_ROOT_PASSWORD
  description: The database root password
  displayName: Database Root User Password
  from: '[a-zA-Z0-9]{8}'
  generate: expression

    Example: ```oc4 process -f ./openshift/secret.yaml | oc4 apply -f - -n pltfrm-tools```

2. Process and apply the mautic.yaml

    ```oc process -f ./openshift/mautic.yaml -p NAME=<app-name> -p GIT_REF=<branch-name> -p GIT_REPO=https://github.com/<git-account>/<git-repo> -p | oc apply -f - -n <namespace>```

    Example: ```oc process -f ./openshift/mautic.yaml -p NAME=mautic -p GIT_REF=mautic3 -p GIT_REPO=https://github.com/patricksimonian/mautic-openshift | oc apply -f - -n pltfrm-tools```

3. Rollout the database and app

    ```oc rollout latest dc/<database-name> -n <namespace> && oc rollout latest dc/<app-name> -n <namespace>```

    Example: ```oc rollout latest dc/mautic-db -n pltfrm-tools && oc rollout latest dc/mautic -n pltfrm-tools```
    
4. Go to the Mautic deployment and start the Mautic Installation. Change the ```Database Port``` to 3306

