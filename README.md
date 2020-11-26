## Mautic Openshift

To Run:

1. Process and apply the mariadb secret.yaml

    ```oc process -f ./openshift/secret.yaml | oc apply -f - -n <namespace>```

2. Process and apply the mautic.yaml

    ```oc process -f ./openshift/mautic.yaml GIT_REPO=https://github.com/<git-account>/<git-repo> | oc apply -f - -n <namespace>```

3. Rollout the database and app

    ```oc rollout latest dc/mautic-db -n <namespace> && oc rollout latest dc/mautic -n <namespace>```
    
4. Go to the Mautic deployment and start the Mautic Installation. Change the ```Database Port``` to 3306

