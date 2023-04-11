# Guideline

PIPELINE

** Clone repository (API spec,application plans, policies)
** DEV environment
**** Import API spec: importOpenAPI() 
**** Application plans creation: importing a sandbox app plan 
**** Generate API-KEY
**** Application creation
**** Import policies configuration
**** TEST API: runIntegrationTests
**** Promote to TEST APIcast

** Promotion to PROD
**** Import API spec: importOpenAPI() 
**** Application plans creation: importing a sandbox app plan 
**** Generate API-KEY
**** Application creation
**** Import policies configuration
**** TEST API: runIntegrationTests
**** Promote to TEST APIcast

## Assumptions: 
   - Two application plans per API with name: sandbox and production
   - One API backend creation - From the Admin console is possible to add more backends.

## Parameters:
   - Developer email: admin+test@3scale.apps.ocp4.quitala.eu (DEV,PROD)
   - Service ID
   - Remote (DEV,PROD)
   - staging and production URLs (DEV,PROD)
   - URL Backend
   - Toolbox.secretName is the name of the Kubernetes Secret containing the 3scale_toolbox configuration file (by default "3scale-toolbox")


## Implementation characteristics

    - **Key generation**: pipeline creates a random key for testing: *KEY= echo $RANDOM | md5sum | head -c 28*
    - **Testing APIs**: smoke test calls the "health" endpoint (it must be in the spec with GET method) and the API-key generated, this is the format of the curl command: 
           *curl -k $api-base-url-staging/health -H'user_key:'$user_key* 
# Roadmap 
    - Use predefined dev 3scale accounts: 3scale account find hetzner admin+test@3scale.apps.ocp4.quitala.eu 
    - Explore the option export/import a product: 3scale product export -f oidc_product.yaml hetzner oidc_api

# Tooling

For deleting "Completed" and "Error" pods created after pipeline-runs or tasks runs use these commands in pipeline namespace:

*oc delete pod --field-selector=status.phase==Failed*
*oc delete pod --field-selector=status.phase==Succeeded*

For v2

oidc-api-test-apicast-v2-staging.apps.ocp4.quitala.eu
oidc-api-test-apicast-v2-production.apps.ocp4.quitala.eu
oidc-api-prod-apicast-v2-staging.apps.ocp4.quitala.eu
oidc-api-prod-apicast-v2-production.apps.ocp4.quitala.eu
