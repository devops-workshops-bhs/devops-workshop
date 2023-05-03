# Baptist Health DevOps Workshop for Azure, Terraform and GitHub

# Lab Instructions:

## Lab 1: Azure

  1. Create a Free Azure Account
    - https://azure.microsoft.com/en-us/free/

  2. If you do not have Azure CLI installed, run:
      ```
      $ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'; Remove-Item .\AzureCLI.msi
      ```
  3. Open a new Powershell Prompt or VS Code Terminal in Administrator

  4. Create a Service Principal (SPN) for Automation
      ```
        az login
        az account show
        az account set --subscription "<subscription_id_or_subscription_name>“
        az ad sp create-for-rbac --name <service_principal_name> --role Contributor --scopes /subscriptions/<subscription_id>
      ```
  5. This command will output 5 values:
      ```
      {
        "appId": "00000000-0000-0000-0000-000000000000",
        "displayName": "azure-cli-2017-06-05-10-41-15",
        "name": "http://azure-cli-2017-06-05-10-41-15",
        "password": "0000-0000-0000-0000-000000000000",
        "tenant": "00000000-0000-0000-0000-000000000000"
      }
      ```
  6. SAVE THE OUTPUT!
      - Copy and paste into a .txt file for now
      - We will use these value in later exercises
      - Make note of your Subscription ID as well, as you will need it later

## Lab 2: VS Code

  1. If you don't have Visual Studio (VS) Code installed, install VS Code here:
    https://code.visualstudio.com/download

### Install GIT using CHOCOLATEY
  1. If you don't have Chocolatey installed:
      - Open an Administrator Shell (CMD or PS)
      - Run the following:
      ```
      Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
      ```
  3. Run the command:
      ```
      choco install git
      ```

### Install GIT using Executable
  1. Open the following link:
    https://git-scm.com/download/win


### Configure GIT:
  1. Open VS Code
  2. Open a new terminal
  3. Link your GitHub account:
  ```
  git config --globabl user.username `<YOUR GITHUB ACCOUNT NAME HERE>`
  ```

### Intall Terraform

#### Install Terraform with Choco
1. Open an administrator terminal (VSC, CMD or PS)
2. Run:
  ```
  choco install terraform
  ```

### Install Required Extensions:
  1. In GitHub, select `Extensions` on the left-hand menu
  2. Search for and install the following extensions (Required):
      - Azure CLI Tools: This extension provides Intellisense and snippets for Azure CLI in VS Code
      - Azure Terraform : This extension supports Terraform commands and visual assistance with developing Terraform
      - GitHub Repositories : This extension provides a simple method to view and open GitHub repositores you have access to using the VS Code search bar
      - HashiCorp Terraform: This extension provides basic syntax validation and Intellisense for auto-completion
      - Remote Repositories : This extension provides additional support to the GitHub Repositories extension
      - GitLens - Git supercharged : This extension provides visual tools helpful for file history including revisions and changes
      - Terraform: This extension provides assistance with syntax, syntax validation, snippets and auto-complete
      - Teraform Azure Code Snippet: This extension provides additional assistance with syntax for resources and modules
      - Terraform Format on Save: This extension runs `terraform fmt` on save to format your terraform code upon saving

  3. Install additional helpful extensions (Optional, Recommended):



## Lab 3: Terraform

### Create your Provider File:
1. Open VS Code
2. Select `Open Folder` on the main page
3. Navigate to a folder to contain your repo
4. Right-click and create a new folder
5. Select the folder you just created and click `Select Folder`
6. Right click in the menu under your folder name that has been opened in VS Code and select `New File`
7. Give this file the name `provider.tf` or `main.tf` depending on your preferences
8. Open your .txt files from earlier when we created our Azure `SPN` in `Lab 1`. Note:
    These values map to the Terraform variables like so:
    ```
    appId is the client_id defined above.
    password is the client_secret defined above.
    tenant is the tenant_id defined above.
    ```
9. Create the base provider blocks:

    ```
    # We strongly recommend using the required_providers block to set the
    # Azure Provider source and version being used
    terraform {
      required_providers {
        azurerm = {
          source  = "hashicorp/azurerm"
          version = "=3.0.0"
        }
      }
    }

    # Configure the Microsoft Azure Provider
    provider "azurerm" {
      features {}

      client_id       = "<Insert your appId value here>"
      client_secret   = "<Insert your password value here>"
      tenant_id       = "<Insert your tenant value here">
      subscription_id = "<Insert your subscription ID here>"
    }
    ```
10. Run:
    ```
    terraform init
    ```
    Followed by
    ```
    terraform validate
    ```
11. Note:
    - The first command initializes Terraform with your provider configuration
    - The second validates your Terraform syntax.
10. At this point running either terraform plan or terraform apply should allow Terraform to run using the Service Principal to authenticate.

### Create a Resource Group named `backend-rg`

1. Open https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/resource_group to review the Terraform Documentation for Azure Resource Groups, or azurerm.resource_groups resource.
2. Review the Arguments Reference
3. Take a note of what is Required and what is Optional
4. Next, take a look at the Attributes Reference section.  This tells you what values can be exported
5. Create a new file in your VS Code directory called `resource_groups.tf`
6. Using the Example Usage and arguments reference, create a new Resource Group block
7. This should look something like this:
    ```
    resource "azurerm_resource_group" "tf_name"
      name = "backend_rg"
      location = "East US 2"
8. Once complete, save your file with CTRL+S
9. Perform the Holy Trinity, the best friend to any Terraform Developer:
    ```
    terraform init -upgrade ; terraform validate ; terraform plan
    ```
10. Review your plan for errors
11. Assuming your plan has passed, apply your plan with:
    ```
    terraform apply -auto-approve
    ```
12. Go to your Azure subscription and validate that your resource group has deployed.