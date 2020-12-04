## Setting up Mautic on Openshift

To Run:

1. **Process and apply the mariadb secret.yaml**

    Secret values are generated if not passed in: ```oc process -f ./openshift/secret.yaml | oc apply -f - -n <namespace>```

    - Example: ```oc process -f ./openshift/secret.yaml | oc apply -f - -n pltfrm-tools```

    To assign custom values to the parameters, use the -p flag. The parameters can only contain alphanumeric and underscore characters:
    ```oc process -f ./openshift/secret.yaml -p NAME=<name> -p DATABASE_USER=<database-user-name> -p DATABASE_NAME=<database-name> -p DATABASE_USER_PASSWORD=<database-user-password> -p DATABASE_ROOT_PASSWORD=<database-root-password> | oc apply -f - -n <namespace>```

    - Example: ```oc process -f ./openshift/secret.yaml -p NAME=mautic -p DATABASE_USER=mautic_db_test -p DATABASE_NAME=mautic_db -p DATABASE_USER_PASSWORD=password -p DATABASE_ROOT_PASSWORD=password2 | oc apply -f - -n pltfrm-tools```

2. **Process and apply the mautic.yaml**

    ```oc process -f ./openshift/mautic.yaml -p NAME=<app-name> -p GIT_REF=<branch-name> -p GIT_REPO=https://github.com/<git-account>/<git-repo> -p NAMESPACE=<namespace>| oc apply -f - -n <namespace>```

    - Example: ```oc process -f ./openshift/mautic.yaml -p NAME=mautic -p GIT_REF=mautic3 -p GIT_REPO=https://github.com/patricksimonian/mautic-openshift -p NAMESPACE=pltfrm-tools | oc apply -f - -n pltfrm-tools```

3. **Rollout the database and app**

    ```oc rollout latest dc/<app-name>-db -n <namespace> && oc rollout latest dc/<app-name> -n <namespace>```

    - Example: ```oc rollout latest dc/mautic-db -n pltfrm-tools && oc rollout latest dc/mautic -n pltfrm-tools```
    
## Setting up Mautic

1. Go to the Mautic Deployment route. This will lead you to the Mautic Installation - Environment Check page. 
The installer may suggest some recommendations for the configuration. Carefully review these recommendations and go to the next step.

2. On the Mautic Installation - Database Setup page, the required input should be pre-filled for you. Go to the next page.

3. On the Mautic Installation - Administrative User page, create the admin user as required.

4. On the Mautic Installation - Email Configuration page, select "Other SMTP Server" as the Mailer Transport.
To use the government server, use the following values:
- Server: apps.smtp.gov.bc.ca
- Port: 25
- Encryption: None
- Authentication mode: Login
- Username: firstname.lastname
- Password: Login Password

To use Gmail, use the following values:
- Server: smtp.gmail.com
- Port: 587
- Encryption: TLS
- Authentication mode: Login
- Username: Gmail Username
- Password: Gmail Password

Additionally, you may need to configure your security settings in Gmail to turn on "Less secure app access" at https://myaccount.google.com/security as well as turn on "Display Unlock Captcha" at https://accounts.google.com/b/0/DisplayUnlockCaptcha.

5. To allow emails to be sent out to contacts, you must change the Frequency Rule within Mautic.
    On the top right of the page, you will see a cog for the settings. Go to Configuration -> Email Settings.
    Scrolling down, you will see the "Default Frequency Rule". This number will be the maximum number of emails that can be sent to a user in the given time period. Setting this number to a reasonable value will help prevent unintentional email spamming.


