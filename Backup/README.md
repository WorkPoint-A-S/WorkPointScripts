# WorkPoint Backup & Recovery Scripts

## Overview
These code sections accompany the document [WorkPoint Backup & Recovery - Technical Procedures](https://support.workpoint.dk/hc/en-us/articles/11427496217746-Backup-and-recovery-for-WorkPoint-365) by providing scripting templates that can be adapted to recover data in a WorkPoint solution.

## Important Notes
- These scripts do not backup WorkPoint or SPO content. That remains the responsibility of the Backup-as-a-Service (BaaS) provider. Refer to the technical documentation (link above) for details on how to ensure the BaaS provider is configured correctly to support _all_ recovery workflows.
- The steps provided are only to be used after content is recovered from a BaaS provider and, in specific scenarios (highlighted in the technical documents) where additional script or API manipulation is required in order to ensure the system properties used by the WorkPoint system are updated in the event that recovery manipulates system or item identifiers.
- The partner is expected to adjust runtime parameters, including authentication, data sources, and recovery targets, to match the specific implementation. 
- The scripts reflect a _basic, synthetic_ WorkPoint configuration with a single ``parent > child`` module hierarchy, the **partner** will need to adapt the scripts to handle the specific modules and hierarchies used in a client solution.

## Script Usage
- Study each section of the script to understand the recovery process thoroughly.
- **Not all recovery steps in the workflow require code/script**. Refer to the document first and revert to the code as required.
- Adapt the script by interpreting and modifying the provided utility functions, variables, and configuration settings according to your organization's WorkPoint365 setup and backup provider.
- Execute specific portions of the script as needed for your data recovery scenarios.


## Technical Setup

### Runtime Environment Setup
The scripts included are written in PowerShell 7, using the [PnP PowerShell Module](https://pnp.github.io/powershell/). 

### Code Structure
For ease of use and understanding, the scripts are provided in a single Jupyter Notebook which allows each script and process to be presented (and executed) in distinct sections with Markdown documentation to explain runtime parameters and context.

 For details on setting up your VSCode environment to run both Jupyter Notebooks and the PowerShell module refer to the following 
 - [Jupyter Notebooks in VS Code](https://code.visualstudio.com/docs/datascience/jupyter-notebooks)
 - [Jupyter PowerShell Module](https://github.com/vors/jupyter-powershell)

 ### Authentication & Permissions
 For reference, most operations use the PnP PowerShell module to inspect and manipulate SharePoint site, list or item properties. The user will require Site Collection Administrator privileges on the WorkPoint solution and entity sites in order to inspect and apply changes to site and item properties.

 ### WorkPoint API

 Several recovery operations use the WorkPoint API to invoke WorkPoint services for Site/Entity Relationships, Invoking 
 which requires an App Registration in the Solution Tenant which has `ApplicationAccess` to the WorkPoint API in the SharePoint solution tenant. 

 Refer to the operation: `Get-WPAPIAccessToken` in the attached [script notebook](wp-recover.ipynb), here the Application Registration uses the Application Registration details obtained from environment variables to authenticate with the WorkPoint API and obtain an ``AccessToken``. 
 
 The following environment variables are used in the example scripts to facilitate authentication for invoking WorkPoint APIs directly from code. 

 ```
  environment variables
  WP_APPREG_TENANT_ID = <GUID>:The GUID of the Solution Tenant
  WP_APPREG_CLIENT_ID = <GUID>:The Application (client) ID of the App Registration (obtained from Azure Portal > Entra > App Reg > Overview)
  WP_APPREG_CLIENT_SECRET = <STRING>: The Application Registration Secret (generated in Azure Portal > Entra > App Reg > Certificates & Secrets)
```

> Note: The use of environment variables is provided as an example only; the partner can adapt how these are stored and retrieved at runtime to suit their implementation. 


 ## Disclaimer
This script is intended for educational and informational purposes. While it outlines the recovery process, it should be customized and tested in a controlled environment before use in a production setting. Careful consideration of authentication, security, and data integrity is paramount during the recovery process.