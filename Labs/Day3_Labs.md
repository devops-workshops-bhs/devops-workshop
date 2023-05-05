# Day 3 Labs: Terraform and GitHub
- These labs match the demos performed on Day 2 and build upon what was demo'ed

## Lab 3: Terraform CLI / OSS

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

      client_id       = "9a709a52-7f63-46fa-9cb3-4a24b8d012ef"
      client_secret   = "< INSERT_YOUR_CLIENT_SECRET_HERE >"
      tenant_id       = "a198e3af-4618-44df-8241-0f411c95e41c"
      subscription_id = "2276ba88-e0b2-4c57-a2b6-c21bf9c971a2"
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
13. Update the provider version from "=3.0.0" to `"~>3.0"` and run `terraform init -upgrade`
14. Take note of the updated Terraform version that is now displayed in your terminal.


### Plan Outputs and Initial Resource Group Deployment

1. Open https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/resource_group to review the Terraform Documentation for Azure Resource Groups, or azurerm.resource_groups resource.
2. Review the Arguments Reference
3. Take a note of what is Required and what is Optional
4. Next, take a look at the Attributes Reference section.  This tells you what values can be exported
5. Create a new file in your VS Code directory called `resource_groups.tf`
6. Using the Example Usage and arguments reference, create a new Resource Group block
7. This should look something like this:
    ```
    resource "azurerm_resource_group" "tf_name"
      name = "rg-<name prefix>-eus2"              # REPLACE <name prefix> with the first letter of your first name and full last name, i.e. jbaldridge
      location = "East US 2"                      # Use East US 2, BHS's primary region
    ```
8. Once complete, save your file with CTRL+S
9. Perform the Holy Trinity, the best friend to any Terraform Developer:
    ```
    terraform init -upgrade ; terraform validate ; terraform plan
    ```
10. Review your plan for errors
11. Now, perform another Terraform plan, but output the results with:
    ```
    terraform plan > plan.txt
    ```
12. Once complete, open the file and review the human-readable plan
13. Now, run another terraform plan to produce a machine-readable Binary plan file:
    ```
    terraform plan -out="plan.tfplan"
    ```
14. Open the file and review the binary output.  While this is not human-readable, this is a machine-readable output file that can be used for Terraform Plan
14. Assuming your plan has passed, apply your plan with:
    ```
    terraform apply "plan.tfplan"
    ```
15. Go to your Azure subscription, go to Resource Groups, and validate that your resource group has deployed.
16. Once you've validated the Resource Group has been created, go back to VS Code and destroy the resource with
    ```
    terraform destroy -auto-approve
    ```


## Lab 4: GitHub

### Create a personal GitHub account
  - https://github.com
  - Alternatively, you may use your BHS GitHub account

### Creaet an Organization
  1. In the upper-right corner of any page, click the `+` drop-down, then click `New organization`.
  2. Select `Join for free`
  3. Create an Organization account name
  4. Enter your Contact email
  5. Choose whether the organization belongs to your `Personal account` (i.e. your GitHub account) or `A business or institution` (the BHS Enterprise)
  6. Verify your account
  7. Accept the Terms of Service
  8. Select next
  9. Select Complete setup

### Create a Repo

  1. In the upper-right corner of any page, use the  drop-down menu, and select New repository.
  2. Type a short, memorable name for your repository. For example, "hello-world".
  3. Optionally, add a description of your repository. For example, "My first repository on GitHub."
  4. Choose a repository visibility. For more information, see "About repositories."
  5. Select Initialize this repository with a README.
  6. Click Create repository.
  7. Once created, click the green `Code` button within your Repository
  8. Select and copy the HTTPS URL. You will use this in the next lab

### Clone your new repo to your VS Code
1. Login to GitHub in VS Code
    - At the bottom left-hand corner of VS Code, click on the icon for user settings (this looks like a person in a circule)
    - Select "Sign In"
    - Login using your GitHub account
2. Once logged in, open the Command Palette with `View` > `Comand Palette`
3. Type `Git Clone` and paste the Repository URL that you copied from your Repository
4. When prompted, open the Repository in a new window


### Push Code to GitHub Repo
1. Move or copy your provider file and resource group file from the original directory into your clone repo's directory
2. Push your code to your GitHub repo:
  ```
  git add . ; git commit -m "Some message describing your commit" ; git push
  ```
3. This command adds the files to commit, creates a commit message, and pushes the code to your GitHub repository
4. Go to your GitHub repository and refresh the page
5. Take note that you will now see a message that you have made a commit, as well as your commit message
6. You should also see your new files that have been pushed to the repository.

## Lab 5: Terraform Cloud

## Exercise	Create a personal Terraform Cloud Org	20 min

### Create a TFC Account and Organization
1. Go to https://app.terraform.io/public/signup/account
2. Enter a username, email and password, agree to te Terms of Use and Privacy Policy, and select `Create Account`
3. Go to your email address, find the `Confirmation` email and confirm your email address
4. Once confirmed, select `Organizations` on the left-hand menu
5. Enter a name for your organization
6. Select `Create organization`
7. You will now be prompted to create a new Terraform Cloud Workspace

### Create a TFC Workspace
1. Choose your workflow: select `Version control workflow`
2. Connect to VCS (version control system)
3. Select `GitHub`
4. Select the drop-down in the menu to view your GitHub organizations
5. Select your organization that you previously created
6. Select your repository that you previusly created (you may need to select `Add another organization` and follow the prompts to add your repo)
7. `Configure settings`:
  - Add a name for your workspace (for our purposes, use the auto-generated name of the GitHub repo)
  - Add an optional description for your workspace
8. Select `Create workspace`
9. Select `Go to workspace overview` to view your new Terraform Cloud Workspace
10. Congrats! You have created your first Terraform Cloud organization, configured your VCS Settings, and created your first TFC Workspace!

### Configure TFC Workspace Settings:
1. Review General Settings:
    - Execution Mode
    - Apply Method
    - Terraform Version
    - Terraform Working Directory
    - Remote state sharing
    - User interface
2. On the left-hand settings menu, click through Locking, Notifications, Run Triggers, SSH Key and finally Version Control
3. In `Version Control`, review the Version Control settings for:
    - VCS source
    - Workspace Settings:
        - Terraform Working Directory (This should be blank; this makes the Working Directory your repo's root directory)
        - Apply Method (leave as Manual Apply)
        - VCS Triggers (leave the default, Always trigger runs, or chose Only trigger runs when files in specified paths change)
        - VCS Branch (leave as `default branch`; this will use your main branch)
        - Pull Requests: uncheck Automatic speculative plans
    - Select `Update VCS Settings` to save your settings
    - Review `Destruction and Deletion` settings and take note of the options, but do `NOT` select any of these settings:
        - Destroy Infrastructure: Allow destroy plans
        - Queue destroy plan
        - Delete from Terraform Cloud

### TFC Authentication
  -  Setup SPN / AZURE Variables: https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/guides/service_principal_client_secret
  1. Navigate back to your Workspace Overview
  2. Select `Variables` on the left-hand menu
  3. Select `+ Add Variable` to create a new variable
  4. Take note of the options for Variables:
      - Select variable category:
          - Terraform variable: These are variables that will be used for your actual Terraform Configuration
          - Environment variable: These are variables for your environment configuration, i.e. authentication to Azure
      - Key: this is the name of your variable
      - Value: this is the value of your variable
      - Variable Description: This is a description of the variable
      - Other Options:
          - HCL: This identifies the variable as a variable for HashiCorp Configuration Language
          - Sensitive: Use this to identify sensitive values, i.e. secrets, keys, etc
  5. Now, create four required Environment variables using the values for your SPN as you configured them in your local provider.tf file:

      | KEY | VALUE | CATEGORY | OPTION |
      |-----|-------|----------|--------|
      | ARM_CLIENT_ID | client_id value | env | n/a |
      | ARM_CLIENT_SECRET | client_secret value | env | Sensitive |
      | ARM_TENANT_ID | tenant_id value | env | n/a |
      | ARM_SUBSCRIPTION_ID | subscription_id value | env | n/a |

  6. Update your `provider.tf` file and REMOVE the ARM variables from your AzureRM Provider block
  7. Perform `git add . ; git commit -m "Remove provider vars" ; git push`

### Perform Terraform Commands in Terraform Cloud:
1. Return to your TFC Workspace Overview
2. Select the `Actions` drop-down on the top right-hand corner
3. Select `Start new run`
4. Enter a descriptin under `Reason for starting run`
5. Select `Plan only` to perform a speculative run
6. Allow the run to perform and then review the plan
7. You should now see that Terraform has planned to deploy your resource group previously created in CLI
8. Perform another run, but this time select `Plan and apply (standard)`
9. You should now see your `Plan` has completed.
10. Review the plan and once ready, select `Confirm & Apply` to apply your configuration and deploy the resource group
11. Optionally, add a comment before selecting `Confirm Plan`
12. Allow the Apply to finish, and you should now see your resource group has been deployed.
13. Congrats, you have performed your first `Terraform Apply` with Terraform Cloud!

## Using Variables
1. Create a file named `variables.tf`
2. create a variable block:
  ```
  variable "name_prefix" {
    description = "Prefix used for naming conventions"
    type = string
    default = ""
  }
  ```
3. Next, create a .TFVARS file called `terraform.tfvars`
4. Define your variable value:
  ```
  name_prefix = "jbaldridge"
  ```
    - Your variable should be named `name_prefix`
    - The value should be your first name's first letter and your last name, as seen above
5. Now modify your resource group block to use this prefix. Change:
  ```
  name = "rg-jbaldridge-eus2"
  ```
  and make this be like the following:
  ```
  name = "rg-${var.name_prefix}-eus2"
  ```
6. Perform the trinity to commit and push your code
7. Run a Terraform Plan in your TFC Workspace
8. You should now see no changes, as this should match the current resource group configuration