# Step by Step Guide to Deploy SAP Systems via Azure DevOps

# Prerequesites for SAP DevOps Deployments
* [Azure Subscription](https://portal.azure.com/) 
* [Azure DevOps](http://dev.azure.com/) and [Github](http://github.com/) account
* SAP S-User for the [Software Downloads](https://launchpad.support.sap.com/)

# Preparations
* Create a Project in Azure DevOps
* Import this repository into the Azure Devops repo: https://github.com/mimergel/sap-hana-config.git
* Create a service principle in Azure CLI
`az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<Your subscription ID>" --name="<a name>"` <br />
Note down all details, especially the Secret in a password store
* Create a service connection to your Azure Subscription (Type: Azure Resource Manager)
    Service principle (manual)

## Create a Variable group in the Library 
* Required variables:
    ARM_SUBSCRIPTION_ID
    ARM_CLIENT_ID
    ARM_CLIENT_SECRET
    ARM_TENANT_ID
    AZURECONNECTIONNAME
    optional:   skipComponentGovernanceDetection    true

## Create the Piplines
    Where is your code? -> Azure Repos Git
    Select a repository -> <Name of the Repo>
    Configure your Pipeline -> Existing Azure Pipeline YAML File 
        -> /DevOpsPipelines/01-prepare-region.yaml
        -> Continue
    Review your pipeline YAML -> Save (hidden on the right side under the run button drop down)
        Edit -> More Actions -> Triggers
        Override the YAML continuous integration trigger from here -> Disable continuous integration
        Variables Tab -> Variable Groups -> Link variable group "SAP-deployment-variables"
        YAML Tab -> Name -> Set the name to e.g. "01-prepare-region"
        SAVE -> SAVE

    Repeat this for all Pipeline YAML Files

## Run the Pipelines
    01-prepare-region
    02-sap-workload-zone
    03-sap-system-deployment
    04-sap-binaries
    05-DB-and-SAP-install

