
Assumptions: 
   - Two application plans per API with name: sandbox and production
   - One API backend creation - From the Admin console is possible to add more backends.

Parameters:
   - Developer email: admin+test@3scale.apps.ocp4.quitala.eu (DEV,PROD)
   - Service ID
   - Remote (DEV,PROD)
   - staging and production URLs (DEV,PROD)
   - URL Backend


toolbox.secretName is the name of the Kubernetes Secret containing the 3scale_toolbox configuration file
toolbox.openshiftProject is the OpenShift project in which Kubernetes Jobs will be created


Preparation:
3scale remote add 3scale-tenant https://$TOKEN@$TENANT.3scale.net/
oc create secret generic 3scale-toolbox --from-file=$HOME/.3scalerc.yaml


Install Operator: Red Hat OpenShift Pipelines

PIPELINE

** Clone repository (API spec,application plans, policies)
** DEV environment
**** Import API spec: importOpenAPI() 
**** Application plans creation: importing a sandbox app plan 
**** Generate API-KEY
**** Application creation
**** TEST API: runIntegrationTests
**** Promote to TEST APIcast

** Promotion to PROD
**** Import API product
**** TEST API: runIntegrationTests

Export application plan configuration:

3scale application-plan export -k hetzner oidc_api sandbox -f sandbox_app_plan.json
3scale application-plan export -k hetzner oidc_api production -f production_app_plan.json

Create an application:
    export ACCOUNT_ID=$(3scale account find hetzner admin+test@3scale.apps.ocp4.quitala.eu |  grep id | awk '{ print $3 }')
 for anonymous policy we set the user-key now:  
    3scale application create --user-key 802e2ccd4 --description sandbox_test_client hetzner $ACCOUNT_ID oidc_api sandbox sandbox_test_client
 for general case we use a randmon string:
    export USERKEY=$(echo $RANDOM | md5sum | head -c 28; echo;)
    3scale application create --user-key $USERKEY --description sandbox_test_client hetzner $ACCOUNT_ID oidc_api sandbox sandbox_test_client


Export a product:

3scale product export -f oidc_product.yaml hetzner oidc_api



3scale account find hetzner admin+test@3scale.apps.ocp4.quitala.eu 




Example of Application apply :
3scale application apply --account 3 --user-key 802e2ccd4008b006e7272aef941750da --description sandbox_test_client --name  sandbox_test_client --plan sandbox --service oidc_api hetzner
Example of Application delete :
3scale application delete hetzner 211


   image-registry.openshift-image-registry.svc:5000/appdev-apimanager/toolbox-rhel82
registry.redhat.io/3scale-amp2/toolbox-rhel8:3scale2.13.0
https://github.com/sgutierr/APIlifecycleCICD.git


export KEY=$(3scale application show prod 227 | grep 227 | awk '{ print $9 }')

curl "https://oidc-api-test-apicast-staging.apps.ocp4.quitala.eu:443/common/security/v3/oauth20/authorize" -H 'api-key:"'"$KEY"'"'

curl "https://oidc-api-test-apicast-staging.apps.ocp4.quitala.eu:443/common/security/v3/oauth20/authorize" -H 'api-key:b2da1981a3e4cb52304e0cdaedad55d2'


curl "https://oidc-api-test-apicast-staging.apps.ocp4.quitala.eu:443/common/security/v3/oauth20/authorize$" -H api-key:| more user_key 

curl "https://oidc-api-prod-apicast-staging.apps.ocp4.quitala.eu:443/common/security/v3/oauth20/authorize -H 'api-key:'$KEY

curl -k $ENDPOINT -H'user_key:'$user_key


oc delete pod --field-selector=status.phase==Failed
oc delete pod --field-selector=status.phase==Succeeded