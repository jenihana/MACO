# Cognizant - Managed Azure Cost Optimization (MACO) - Power BI Report

## Overview

The Cognizant MACO Power BI Dashboard is a report that aims to aggregate and consolidate the information generated by several Azure services to gain quick insights on your resources to enable data driven business and financial optimization decisions. The main data sources for this Azure Infrastructure Dashboard are the **Azure Advisor REST API**, **Log Analytics API**, **Azure Security Center REST API**, **Azure Graph REST API** and several **Azure IaaS REST APIs**.

### Requirements

- The MACO Dashboard is a Power BI Template that requires to download and install the Microsoft Power BI Desktop Edition from the Microsoft Store. Below you can find the minimum requirements to run the Dashboard*
    -	Windows 10 version **14393.0** or **higher**.
    -	Internet access from the computer running Microsoft Power BI desktop.
    -   An Azure account on the desired tenant space with permissions on the subscriptions to read from the Azure Services described above.
    *Requirements up to date as of Feb 2021

Below you can find the list of providers and the actions that you will need to permit to allow to run the CCO Power BI Dashboard:
</div>

| Resource Provider Name| Permissions |
| --- | --- |
|Azure Advisor| Microsoft.Advisor/generateRecommendations/action |
|Log Analytics| Microsoft.OperationalInsights/workspaces/"workspaceName"/users
|*|*/Read|

## APIs in use
<div style="text-align: justify">
The MACO Dashboard pulls information from several APIs. You can read the public documentation if you need further information about the calls and methods available:
<br><br>
</div>

| API Name| Dashboard API Version | Last API version | Using latest version|
| --- | :---: | :---: |:---: |
| [Azure Advisor](https://docs.microsoft.com/en-us/rest/api/advisor/) | 2020-01-01|2020-01-01|:heavy_check_mark:|
| [Azure Security Center Alerts](https://msdn.microsoft.com/en/US/library/mt704034(Azure.100).aspx)  |2019-01-01 |2019-01-01|:heavy_check_mark:|
| [Azure Kubernetes Service](https://docs.microsoft.com/en-us/rest/api/aks) | 2019-08-01|2019-08-01|:heavy_check_mark:|
| [Azure Compute](https://docs.microsoft.com/en-us/rest/api/compute) | 2019-03-01|2019-03-01|:heavy_check_mark:|
| [Azure SQL Database](https://docs.microsoft.com/en-us/rest/api/sql/) | 2020-08-01-preview|2020-08-01-preview|:heavy_check_mark:|)
| [Azure Disks](https://docs.microsoft.com/en-us/rest/api/compute/disks/list) | 2019-03-01|2019-03-01|:heavy_check_mark:|
| [Azure Virtual Networks](https://docs.microsoft.com/en-us/rest/api/virtual-network) | 2019-04-01|2019-04-01|:heavy_check_mark:|
| [Azure Network Interfaces](https://docs.microsoft.com/en-us/rest/api/virtualnetwork/networkinterfaces) |2019-04-01 |2019-04-01|:heavy_check_mark:|
| [Resource Groups](https://docs.microsoft.com/en-us/rest/api/resources/resourcegroups)  |2019-05-01 |2019-05-01|:heavy_check_mark:|
| [Azure Resources](https://docs.microsoft.com/en-us/rest/api/resources/resources)  |2019-10-01 |2019-10-01|:heavy_check_mark:|
| [Azure Subscriptions](https://docs.microsoft.com/en-us/rest/api/resources/subscriptions)  |2020-01-01 |2020-01-01|:heavy_check_mark:|
| [Azure Locations](https://docs.microsoft.com/en-us/rest/api/resources/subscriptions/listlocations)  |2019-05-01 |2019-05-01|:heavy_check_mark:|
| [Azure Container Registry](https://docs.microsoft.com/en-us/rest/api/containerregistry/)  | 2017-10-01|2017-10-01|:heavy_check_mark:|
| <span style="color:#0088cc">Log Analytics Rest API </span> ([1](https://docs.microsoft.com/en-us/rest/api/loganalytics/), [2](https://dev.loganalytics.io/))  |v1 |v1|:heavy_check_mark:|

<div style="text-align: justify">

API URLs by environment:

| API Name| API URL | Environment|
| --- | :---: | :---: | 
| Management |https://management.azure.com/|Global|
| Azure AD Graph |https://graph.windows.net/|Global|
| Management |https://management.usgovcloudapi.net/|US Government|
| Azure AD Graph |https://graph.microsoft.us/|US Government|
| Management |https://management.chinacloudapi.cn/|China|
| Azure AD Graph |https://graph.chinacloudapi.cn/|China|

## Resource Providers requirements

Although some of the Resource Providers might be enabled by default, you need to make sure that at least the **Microsoft.Advisor** and the **Microsoft.Security** resource providers are registered across all the  subscriptions that you plan analyze using the Dashboard. 

Registering these 3 Resource Providers has no cost or performance penalty on the subscription:

1. Click on **Subscriptions**.
2. Click on the Subscription name you want to configure.
3. Click on **Resource Providers**.
4. Click on **Microsoft.Advisor** and **Register**.
5. Click on **Microsoft.Resourcehealth** and **Register**.
6. Click on **Microsoft.Security** and **Register**.

![resource providers](/images/resourceproviders.png)

## Azure Advisor Recommendations
Azure Advisor is a personalized cloud consultant that helps you follow best practices to optimize your Azure deployments. It analyzes your resource configuration and usage telemetry. It then recommends solutions to help improve the performance, security, and high availability of your resources while looking for opportunities to reduce your overall Azure spend.

The MACO Power BI Dashboard will directly pull data from Azure Advisor REST APIs to aggregate all the information across the Azure account subscriptions. This requires generating the recommendations before the first time we load the template else the Dashboard will be empty or will fail because it was unable to download any data.

Open the Azure Portal with your Azure Account http://portal.azure.com 

1. Click on **Advisor**.
2.	Expand the subscriptions drop-down menu.
3.	Select the subscription you want to update or generate the recommendations for the first time.
4.	Wait until the recommendations for the selected subscriptions has been loaded.
5.	Repeat these steps for each subscription you want to generate Azure Advisor recommendations.

![AdvisorRecommendations](/images/AdvisorRecommendations.png)


# Setting up the Managed Azure Cost Optimization Power BI Dashboard

## Prerequisites 

### Download MACO PBI Template

Download and open the latest MACO_PBITemplate_vX.pbit to your local machine. (The most up to date file should be hosted on the main folder of this repo.)

### Cost History Exports from Azure
In order for the initial dashboard to be stood up, two cost history reports are required from Azure Cost Management + Billing portal. Follow the steps below to aquire these reports: 
1. Navigate to Portal.Azure.com
2. Sign in with the relevant client's account
3. Search and navigate to **Cost Management + Billing**

    ![cost management + billing](/images/costmgmtsearch.jpg)

4. On the Azure Blade, navigate to **Cost analysis**

    ![azure blade](/images/blade.jpg)

5. Across the top, find and update the following filter:
    - Date(s) **Last 3 months**
    - "Group by:" = **Subscription**
    - "Granularity:" = **Monthly**
    - Type = **Table**

    ![datefilter](/images/datefilter.jpg)

    ![subscriptionfilter](/images/subfilter.jpg)
 
 6. Once the above parameters are set, click **Download**

    ![download](/images/download.jpg)
 
 7. On the subsequent menu, ensure that the type is "Excel", and click "Download Data" 
 
    ![excel](/images/excel.jpg)
 
 8. Save the file name as **"Azure Cost History_Subscription"**

Repeat steps 5-8 again with the following addendums:
<!---
1. __
2. __
3. __
4. __ --->
5. Across the top, find and update the following filter:
    - Date(s) Last 3 months
    - "Group by:" = **Service name**
    - "Granularity:" = Monthly
    - Type = Table
 
    ![servicefilter](/images/servicefilter.jpg)

<!---
6. __
7. _ --->
8. Change the file name to **"Azure Cost History_Service Name"**

You will need both files in a later step. 

## Input Parameters
From this repository, download and open the file named "MACO_PBITemplate_vX.pbit"

Upon opening the .pbit file, you will be met with the following parameters
1. TenantName 
2. AzureKind 
3. Log Analytics Workspaces
4. Subscriptions Cost History Report File Path
5. Service Cost History File Path
6. Optimization Tracking File Path

### TenantName

Input the name of the Azure Tenant in which the subscriptions exist in.

### AzureKind

Before start loading data you need to select which type of environment you're using:

- Select "Global" for Microsoft Azure commercial environments. This is the default selection.
- Select [US-Government](https://docs.microsoft.com/en-us/azure/azure-government/documentation-government-developer-guide) for Azure Us government services. Azure Government is a separate instance of the Microsoft Azure service. It addresses the security and compliance needs of United States federal agencies, state and local governments, and their solution providers.
- **Preview feature:** Select [China](https://docs.microsoft.com/en-us/azure/china/resources-developer-guide) to load data from cloud applications in Microsoft Azure operated by 21Vianet (Azure China).

![selector](/images/inputs.png)

### Log Analytics Workspaces

Next, determine the Log Analytics workspace(s) that the targeted virtual machines are connected to. The ID of the workspace is located in the overview page of the workspace in Azure and can simply be copy->pasted into the Power BI input.

If there are multiple Log Analytics workspaces with VMs then please sepearte the workspace IDs with a semi-colon ';' (no space).

EX: 59483733-898d-4b5e-8cf4-514d047b3rec;59304833-888d-4p6g-8cf4-511446b3efv

### Subscriptions Cost History Report File Path (See instructions in prerequisites)

In Azure cost management the user must download a 3 month cost history report by subscription and service name and then download it to excel. Then the user needs to copy the file path of the two exports into the Power BI inputs.

### Service Cost History File Path (See instructions in prerequisites)

## Click "Load"

****

## Modify Privacy settings

- Go to File -> Options -> Privacy and set to Always ignore privacy level settings.

![Privacy](https://user-images.githubusercontent.com/39730064/60920947-3e6d2580-a24e-11e9-9042-f799c9f6fc53.png)

## Credentials

By default, the template doesn’t have any Azure Account credentials preloaded. Hence, the first step to start showing subscriptions data is to sign-in with the right user credentials.

**IMPORTANT NOTE**: Power BI Desktop caches the credentials after the first logon. It is important to clear the credentials from Power BI desktop if you plan to switch between Azure GLobal and any other region like US Government or China. The same concept applies if you plan to switch between tenants. Otherwise, the staged credentials will be used again for the different Azure environments and the authentication or data load process will fail.

### Clean Credentials on the Data Source

In some cases, old credentials are cached by previous logins using Power BI Desktop and the dashboard might show errors or blank fields.

- Click on Data sources in **Current file/Global permissions**.
- Click on **Clear Permissions**.
- Click on **Clear All Permissions**.

![credentials1](/images/Credentials1.png) ![credentials2](/images/Credentials2.png)

### Refresh the dashboard

If the permissions and credentials are properly flushed it should ask you for credentials for each REST API and you will have to set the Privacy Levels for each of them.

- Click on **Refresh**.
  
![credentials3](/images/refresh.png)

### Credentials for <span>management.azure.com</span> REST API request:

- Click on **Organizational Account**.
- Click on **Sign in**.
- Click on **Connect**.


![credentials4](/images/Credentials4.png)

### Credentials for <span>api.loganalytics.io</span> API

- Click on **Organizational Account**.
- Click on **Sign in**.
- Click on **Connect**.

![loganalytics](/images/loganalyticsAPI.PNG)

### Enter Power BI Dataflows credentials

- Click on **Sign in**.
  
![credentials7](/images/dataflows.png)

- Sign in with your organizational account associated with the MACO PowerBI workspace.
- Press **Connect**.

![credentials8](/images/connect.png)

# Report Pages

## Cost Optimization Tracking page

In this page, you will be able to track cost savings throughout the life cycle of the engagement. Savings are categorized by resource name, type, subscription, and type of optimization that was completed. As the engagement progresses, it will also show how resource utilization as resources are rightsized.

![SavingsTracker](/images/optimizationTracking.png)

## Financial Overview page

In this page, you will be able to identify the previous month total Azure spend as well as the projected monthly and annual savings the report has generated. You can then see monthly cost breakdowns by environment, subscription, and service types. Finally you will be bale to see the top 10 Azure Advisor cost recommendations and the potential annual savings.

![overview](/images/Overview.png)

## Potential Savings Suummary page

In this page, you will be able to preview potential savings across all resource types that will be covered further on in the dashboard. You can also view an over view of Azure Advisor cost recommendations and a link to the Microsoft Documentation on the recommendation. 

![potentialSavings](/images/potentialSavings.png)

## Azure Virtual Machine Rightsizing page

In this tab, you will be able to identify the rightsizing recommendations generated by the MACO rightsizing algorithm. The recommendations are based on the past 20 days of data generated by several log analytics queries that gather metrics (Avg/Max/Burst(>80%usage)) for virtual machine cpu and memory performance. 

These recommendations take any general purpose or hyperthreaded accelerated networking requirements as well as current baseline managed disk requirements.

You can filter the information by:

- Subscription
- Resource Group
- VM Name
- Virtual Machine Efficiency Score

Data Inputs
- https://docs.microsoft.com/en-us/rest/api/compute
- http://azure.microsoft.com/api/v2/pricing/virtual-machines-base/calculator/

Data Outputs
- Virtual Machine Rightsizing Table

![azurecompute](/images/vmrightsizing.png)

## Azure Managed Disk Optimization page

In this tab, you will be able to identify the disk rightsizing recommendations generated by the MACO rightsizing algorithm. The recommendations are based on the past 20 days of data generated by several log analytics queries that gather metrics (Avg/Max/Burst(>80%usage)) for managed disk IOPS, Throughput, and Queue performance. 

These recommendations take any general purpose or hyperthreaded accelerated networking requirements as well as current baseline managed disk requirements.

You can filter the information by:

- Subscription
- Resource Group
- VM Name
- Disk Efficiency Score

Data Inputs
- https://docs.microsoft.com/en-us/rest/api/compute
- http://azure.microsoft.com/api/v2/pricing/managed-disks-base/calculator/

Data Outputs
- Managed Disk Rightsizing Table

![azurecompute](/images/diskrightsizing.png)

## Azure SQL Database Overview page

In this tab, you will be able to identify the number of Databases, Subscription/Resource Group, connected server, database size, storage replication type, and subscription option of the database. This table also features the current monthly cost of each database along with potential AHUB and Reserved SQL Capacity savings on an annual and three year basis. 

You can filter the information by:

- Subscription
- Server Name
- Database Tier

Data Inputs
- https://docs.microsoft.com/en-us/rest/api/sqlDatabases
- https://azure.microsoft.com/en-us/pricing/details/sql-database/single/

![sqlDB](/images/sqlOverview.png)

## Azure Kubernetes Service Optimization page

In this page, you will be able to identify the number of AKS Clusters, Nodes, Pods, Containers, and usage metrics and optimization recommendations for the clusters.

You can filter the information by:

- Subscription
- AKS Cluster

![aks](/images/aksrightsizing.png)

**IMPORTANT**: to receive all the information related to the Pods, Containers and Container Images a log analytics workspace configured **is required**.
</div>

## Azure Hybrid Benefit Opportunities page

In this tab, you can see any and all Virtual Machines that are eligible to use Azure Hybrid Benefit and what the corresponding savings would be. Azure Hybrid Benefit is a licensing benefit that helps you to significantly reduce the costs of running your workloads in the cloud. It works by letting you use your on-premises Software Assurance-enabled Windows Server and SQL Server licenses on Azure.

You can filter the information by:

- Tenant
- Subscription
- Resource Group
- Vm extension

Data Inputs
- https://docs.microsoft.com/en-us/rest/api/compute
- http://azure.microsoft.com/api/v2/pricing/virtual-machines-base/calculator/
- https://azure.microsoft.com/en-us/pricing/hybrid-benefit/

![azurecompute](/images/ahub.png)

## Azure Advisor Recommendations page

In this page of the report, you will be able to identify the total amount of recommendations that Azure Advisor has identified, to what resources each recommendations apply and to what subscription as well.

You can filter the information by:

- Subscription
- Resource Group
- Resource type

It will also give a high-level overview of what subscriptions require more attention and has more recommendations to snooze or implement.

If you navigate to a impacted resource you will see a quick description, potential solution and in some cases a link to a website where you can find all the steps to solve the problem.

Data Inputs:
- https://docs.microsoft.com/en-us/rest/api/advisor/

![advisor](/images/Advisor.png)

## IaaS Usage and Limits page

This tab allows to identify the usage of any Compute, Storage and Networking Azure resource and validate the limits for each region and subscription. 

You can filter the information by:

- Azure Region
- Subscription

Data Inputs
- https://docs.microsoft.com/en-us/rest/api/compute

![azure Idle](/images/UsageAndLimits.png)

## Azure Compute Overview page

In this tab, you will be able to identify the number of VMs, the Operating System, SKU, Availability Set name, location, SKU, the VNET and subnet each VM is connected, the private IP address and if the VM has any extension installed.

You can filter the information by:

- Tenant
- Subscription
- Resource Group
- Vm extension

Data Inputs
- https://docs.microsoft.com/en-us/rest/api/compute
- http://azure.microsoft.com/api/v2/pricing/virtual-machines-base/calculator/

![azurecompute](/images/Compute.png)


## IaaS Idle Resources Dashboard page

This tab is lists all the Public IPs, Network Interfaces and Disks that are disconnected, idle or unattached.

You can filter the information by:

- Subscription
- Resource Group

Data Inputs
- https://docs.microsoft.com/en-us/rest/api/compute
- https://docs.microsoft.com/en-us/rest/api/compute/disks/list
- https://docs.microsoft.com/en-us/rest/api/virtual-network
- - https://docs.microsoft.com/en-us/rest/api/virtualnetwork/networkinterfaces

![azure Idle](/images/IdleResources.png)


