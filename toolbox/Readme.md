
There are different ways to install 3scale toolbox as a container in an OpenShift container, if the purpose of this is running an API lifecycle pipeline you can follow these steps:

1. Install 3scale toolbox in your local for define more comfortably the 3scale remotes. This is the syntax:
     3scale remote add 3scale-tenant https://$TOKEN@$TENANT.3scale.net/
   Toolbox creates this file: $HOME/.3scalerc.yaml   
2. Login in you OCP cluster and create a secret with the remotes definitions: 
     oc create secret generic 3scale-toolbox --from-file=$HOME/.3scalerc.yaml
3. Toolbox deployment in OpenShift:
     oc create -f toolbox_deployment.yaml
4. Status of this job:
    oc describe jobs/toolbox
5. The container created is completed, get the pod name and run this command to startup:
    oc debug pod/toolbox-rkf5s
