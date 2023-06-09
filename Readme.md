## How it works?
![alt text](https://github.com/sgutierr/APIlifecycleCICD/blob/main/docs/image1.png?raw=true)

# Installation:
Steps for installing the 3scale APILifecycle pipeline

## 1. 3scale toolbox
3scale toolbox is a CLI which allows you to set up 3scale instances. 
This is the link to the repo and documentation: https://github.com/3scale/3scale_toolbox

### Remotes set up:
Generate the 3scalerc.yaml file, which you can do by installing the 3scale toolbox in your local. 
You need to add two toolbox "remotes", one will point to the DEV and the other to the PROD 3scale instance.

For further information about 3scale TOKEN generation: https://access.redhat.com/documentation/en-us/red_hat_3scale_api_management/2.13/html/admin_portal_guide/tokens 

This is an example of the commands used to create a new remote:
   - 3scale remote add DEV https://$TOKEN@"3scale-admin-url"
   - 3scale remote add PROD https://$TOKEN@"3scale-admin-url2"

Once this file is generated, create the OpenShift secret in the namespace where you will run the pipelines:
   - **oc create secret generic 3scale-toolbox --from-file=$HOME/.3scalerc.yaml**

## 2. OpenShift pipelines

Through the operator hub, install the operator: Red Hat OpenShift Pipelines and follow these steps:
   - 1 Create a persistent volume claim (PVC) to store the git repository on task containers:
      - **oc create -f tekton/pvc.yml**
   - 2 Create the task 3scaletoolbox:
      - **oc apply -f tekton/apilifecycle-task.yml**
   - 3 Create the pipeline: 
      - **oc apply -f tekton/apilifecycle-pipeline.yml**
### Before running the pipeline first time:
   - 1- You need to know the account id of the 3scale user which test the API in each environment.To find out the id you can run:
      - **3scale account find DEV john**
      - **3scale account find PROD john**

# Running the APIlifecycle pipeline:   
## Configuration parameters:
| Name  |  Type | Description  | Example |
|---|---|---|---|
| **version** | string | represents the major version of the API | v1 |
| **api name** | string | identifies the API, this string is used to browse the git repo to locate the API spec: /apis/{api-name}/spec/{api-nane}.json  | oidc_api  |
| **api-base-url-staging** | string| the URL exposed by the staging API gateway in TEST/PRE environment | |
| **api-base-url-live** | string | the URL exposed by the production API gateway in TEST/PRE environment |  |
| **api-base-url-staging-prod** | string | URL exposed by the staging API gateway in PROD env |  | 
| **api-base-url-live-prod** | string | URL exposed by the production API gateway in PROD env |  | 
| **Workspaces --> source-code** | string | OpenShift persistent volume claim required |  | 