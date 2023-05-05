# Baptist Health DevOps Workshop for Azure, Terraform and GitHub

# Day 1 Lab: Azure and VS Code

## Lab 1: Azure

  1. Create a Free Azure Account with a personal email
    - https://azure.microsoft.com/en-us/free/

  2. If you do not have Azure CLI installed, run:
      ```
      $ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'; Remove-Item .\AzureCLI.msi
      ```
  2.  If you have issues installing with that command, you can install Azure CLI here using the MSI: https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-windows?tabs=azure-cli

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

      {
  "appId": "f0fee029-6f84-45ff-9453-76fc636fdde1",
  "displayName": "spn-tf-jbald01",
  "password": "9Op8Q~fLRkfJOeJqBhPUXW.nMrzRzBCs972S7ab8",
  "tenant": "a198e3af-4618-44df-8241-0f411c95e41c"
}

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
  3. Link your BHS GitHub account:
  ```
  git config --globabl user.username `<YOUR GITHUB ACCOUNT NAME HERE>`
  ```

### Install Terraform

#### Install Terraform with Choco
1. Open an administrator terminal (VSC, CMD or PS)
2. Run:
  ```
  choco install terraform
  ```

#### Install Terraform with Executable
1. Open the following link: https://developer.hashicorp.com/terraform/downloads
2. Download the terraform ZIP file from Terraform site.
3. Extract the .exe from the ZIP file to a folder eg C:\Terraform copy this path location like C:\terraform\
4. Add the folder location to your PATH variable,
5. eg: Control Panel -> System -> System settings -> Environment Variables
6. In System Variables, select Path > edit > new > Enter the location of the Terraform .exe, eg C:\Terraform then click OK
7. Open a new CMD/PowerShell and the Terraform command should work


### Install Required Extensions:
  1. In VS Code, select `Extensions` on the left-hand menu
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

