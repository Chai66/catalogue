#!groovy
@Library('rs-shared-library') _

// responsibility to pass what type of application and component to this pipeline deicssion

def configMap = [
    application: "nodejsVM",
    component: "catalogue"
] //It is calling from rs-shared-library repo using pipelineDecission
if( ! env.BRANCH_NAME.equalsIgnoreCase('main')){ 
    pipelineDecission.decidePipeline(configMap)
}
else{
    echo "This is PRODUCTION, deal with CR process"
}