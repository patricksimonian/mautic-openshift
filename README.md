## Mautic Openshift

To Run:

1. Process and apply the mariadb secret.yaml

    Secret values are generated if not passed in: ```oc process -f ./openshift/secret.yaml | oc apply -f - -n <namespace>```

    Example: ```oc process -f ./openshift/secret.yaml | oc apply -f - -n pltfrm-tools```

    To assign custom values to the parameters, use the -p flag. The parameters can only contain alphanumeric and underscore characters:
    ```oc process -f ./openshift/secret.yaml -p NAME=<name> -p DATABASE_USER=<database-user-name> -p DATABASE_NAME=<database-name> -p DATABASE_USER_PASSWORD=<database-user-password> -p DATABASE_ROOT_PASSWORD=<database-root-password> | oc apply -f - -n <namespace>```

    Example: ```oc4 process -f ./openshift/secret.yaml -p NAME=mautic -p DATABASE_USER=mautic_db_test -p DATABASE_NAME=mautic_db -p DATABASE_USER_PASSWORD=password -p DATABASE_ROOT_PASSWORD=password2 | oc4 apply -f - -n pltfrm-tools```

2. Process and apply the mautic.yaml

    ```oc process -f ./openshift/mautic.yaml -p NAME=<app-name> -p GIT_REF=<branch-name> -p GIT_REPO=https://github.com/<git-account>/<git-repo> -p | oc apply -f - -n <namespace>```

    Example: ```oc4 process -f ./openshift/mautic.yaml -p NAME=mautic -p GIT_REF=mautic3 -p GIT_REPO=https://github.com/patricksimonian/mautic-openshift | oc4 apply -f - -n pltfrm-tools```

3. Rollout the database and app

    ```oc rollout latest dc/<app-name>-db -n <namespace> && oc rollout latest dc/<app-name> -n <namespace>```

    Example: ```oc4 rollout latest dc/mautic-db -n pltfrm-tools && oc4 rollout latest dc/mautic -n pltfrm-tools```
    
4. Go to the Mautic deployment and start the Mautic Installation. Change the ```Database Port``` to 3306

