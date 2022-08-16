# CE-HELPER
Minimal DevOps automation helper for IBM Code Engine

## Pre-reqs
* Location of *ce-helper* file should be added to $PATH variable
* Project must have package.json file with version in it. Minimal example `{"version":"0.1"}`
* Project must have any of these 3 environment files *.ce.prod.env* , *.ce.stage.env* , *.ce.test.env*
* Environment values file must be properly filled (check *.ce.example.env* in this repo for details)
* Node must be installed on the system (check with `node -v`)
* Docker is installed on machine (required for local builds only)
* IBM Cloud CLI (with IBM Container Registry plugin, IBM Code Engine plugin) installed
* Active IBM Cloud account with provisioned IBM Container Registry (with created Namespace) and IBM Code Engine (with created Project)

### Optional login with IAM key
If instead of SSO login you want to use IBM Cloud API key, do the following
1. Create IAM API Key
```
ibmcloud login --sso
ibmcloud iam api-key-create ce-helper -d devops --file ~/.ibmcloud_key
```
2. Set in environment file (*.ce.prod.env* , *.ce.stage.env* , *.ce.test.env* )
```
CLOUD_LOGIN_TYPE=file
CLOUD_LOGIN_IAM_KEY_FILE=~/.ibmcloud_key
```

### Optional usage with Travis CI

* Configure Travis CI to work with your project code repository
* Set environment variable **ibmcloud_key** to the value of IBM Cloud IAM key

Copy file *.travis.remote.example.yml* into your project for remote build (dockerfile, buildpacks).
Don't forget to rename travis file to *.travis.yml*


## Usage
```
ce-helper [OPTION] [ENVIRONMENT] [ACTION]
```

Application accepts up to 3 arguments, one of each type. 2nd (ENVIRONMENT) and 3rd argument (ACTION) is optional and has the values *prod* and *update* by default

**OPTION:**
```
-h, --help  Display this help message and exit.
-l, --login  Login to IBM Cloud and IBM Container Registry
-b, --build  Build docker image, tag it and push to IBM Container Registry
-d, --deploy  Deploy to Kubernetes instance
-a, --all  Build and Deploy procedures
-al, --all-with-login  Login, Build and Deploy procedures
```

**ENVIRONMENT:** *Optional*
```
-p, --prod Use production environment (Default)
-s, --stage Use staging environment
-t, --test Use test environment
```

**ACTION:** *Optional*
```
-c, --create Use to create new application on IBM Code Engine
-u, --update Use to update existing application on IBM Code Engine (Default)
```

### Examples:
```
ce-helper -a -s -u
```
*Deploy all (without login) on stage by updating existing application*

```
ce-helper -al -p -c
```
*Deploy all with login on production by creating a new application*

```
ce-helper -l
```
*Login to IBM Cloud and select Code Engine project*
