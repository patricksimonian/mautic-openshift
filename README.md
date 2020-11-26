## Mautic Openshift

To Run:

1. Process and apply the mariadb secret.yaml

    ```oc process -f ./openshift/secret.yaml | oc apply -f - -n <namespace>```

    Example: ```oc4 process -f ./openshift/secret.yaml | oc4 apply -f - -n pltfrm-tools```

2. Process and apply the mautic.yaml

    ```oc process -f ./openshift/mautic.yaml -p NAME=<app-name> -p GIT_REF=<branch-name> -p GIT_REPO=https://github.com/<git-account>/<git-repo> -p MARIA_DB_SERVICE_NAME=<database-name> | oc apply -f - -n <namespace>```

    Example: ```oc4 process -f ./openshift/mautic.yaml -p NAME=mautic -p GIT_REF=mautic3 -p GIT_REPO=https://github.com/patricksimonian/mautic-openshift -p MARIA_DB_SERVICE_NAME=mautic-db | oc4 apply -f - -n pltfrm-tools```

3. Rollout the database and app

    ```oc rollout latest dc/<database-name> -n <namespace> && oc rollout latest dc/<app-name> -n <namespace>```

    Example: ```oc4 rollout latest dc/mautic-db -n pltfrm-tools && oc4 rollout latest dc/mautic -n pltfrm-tools```
    
4. Go to the Mautic deployment and start the Mautic Installation. Change the ```Database Port``` to 3306

