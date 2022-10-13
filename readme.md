# Setting up environment

### Installing pipenv using pip

```
pip install pipenv
```

### Initializing pipenv

```
pipenv install
```

### Activating virtual environment

```
pipenv shell
```

### Installing databricks-cli into pipfile

Inside your shell (in virtual environment), write the following command:

```
pipenv install databricks-cli
```

### Installing Azure-cli

To install Azure-cli in your machine, you have two method:

-   Using Package manager

In this example, we will be using `chocolatey`[https://chocolatey.org/install] package manager.

> `choco install azure-cli -y`

-   using MSI installer

Go to this Link: [https://aka.ms/installazurecliwindows]

# Logging into your Azure account with azure CLI tool

### Login with az command

```
az login
```

This will open your browser, login from the browser.

# Creating Resource Group from CLI

```
az group create -l <location> -n <name>
```

You can choose location from the given list:

-   centralus
-   eastasia
-   southeastasia
-   eastus
-   eastus2
-   westus
-   westus2
-   northcentralus
-   southcentralus
-   westcentralus
-   northeurope
-   westeurope
-   japaneast
-   japanwest
-   brazilsouth
-   australiasoutheast
-   australiaeast
-   westindia
-   southindia
-   centralindia
-   canadacentral
-   canadaeastuksouth
-   ukwest
-   koreacentral
-   koreasouth
-   francecentral
-   southafricanorth
-   uaenorth
-   australiacentral
-   switzerlandnorth
-   germanywestcentral
-   norwayeast
-   jioindiawest
-   westus3
-   qatarcentral
-   swedencentral
-   australiacentral2

## Creating Databricks workspace for given resource group

```
az databricks workspace create --resource-group <group_name> --name <workspace_name> --location <location> --sku <tier>
```

Currently we have the following 3 tiers:

-   standard
-   premium
-   trial

> Make sure to copy Workspace url from the json response you get in your terminal after creating databricks workspace [1]

```json
  "workspaceId": "6446554103509083",
  "workspaceUrl": "adb-6446554103509083.3.azuredatabricks.net"
}
```

## Create Cluster using databricks

Before creating a cluster inside a given databricks workspace, we first need to **authorize** the databricks command line tool.

#### Authorizing databricks-cli using token

```
databricks configure --token
```

It will ask for workspace URL, which you have copied from [1]
Make sure to add `https://` before the url.

#### Getting the token

After providing Workspace URL, it will ask for a token.
To get your token, login to https://portal.azure.com and _go to_ databricks worspaces. Choose and **launch** your workspace.

After you land to the Databricks workspace page, on the top right corner; click your username and a drop-down menu will pop up. Click on **user settings**.

-   Click on Generate Token
-   Add any text if it promps
-   Copy the token
-   Paste it to the terminal where it asks for token

and voila! you have successfully configured and authorised your databricks-cli.

#### Writing your cluster information to a JSON file

You can use the template found in this project namely `create-cluster.json` and modify it according to your needs

```json
{
  "cluster_name": "<cluster_name>",
  "spark_version": "<spark_version>",
  "node_type_id": "node_type",
  "num_workers": <int:number_of_workers>,
  "spark_conf": {
    "spark.databricks.cluster.profile": "<profile>",
    "spark.speculation": true,
    "spark.master": "local[*, 4]"
  },
  "custom_tags": {
    "ResourceClass": "<resource_class>"
  }
}
```

You can find spark version here: `./available-spark-runtime.json`
You can find node type here: `./nodetype-list.json`

###### Running the databricks cli tool to create cluster

```
databricks clusters create --json-file <location_of_json_file>
```

It should greet you with your cluster id in json format similar to this:

```json
{
    "cluster_id": "1012-183419-isyhjgxk"
}
```

Which indicates your cluster has been created.
