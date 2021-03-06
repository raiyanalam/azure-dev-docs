---
title: Configure your local JavaScript environment for Azure development
description: How to set up a local JavaScript dev environment for working with Azure, including an editor, the Azure SDK libraries, optional tools, and the necessary credentials for library authentication.
ms.date: 09/30/2020
ms.topic: conceptual
ms.custom: devx-track-js, azure-sdk-ai-text-analytics-5.0.0
---

# Configure your local JavaScript dev environment for Azure

When creating cloud applications, developers typically prefer to test code on their local workstations before deploying that code to a cloud environment like Azure. Local development gives you the advantage of a wider variety of tools along with a familiar development environment.

This article provides setup instructions to create and validate a local dev environment that's suitable for JavaScript with Azure.

## One-time subscription creation

Azure resources are created within a subscription, which is the billing unit for using Azure. While you can create free resources (each subscription offers a free resource for most services), you should create paid-tier resources when you expect to deploy your resource to production.

|Type|Description|
|--|--|
|[Trial subscription](https://azure.microsoft.com/free/cognitive-services)|Create a _free_ trial subscription.|
|[Existing subscription](https://portal.azure.com)|If you already have a subscription, access your existing subscription in the Azure portal, the Azure CLI, or JavaScript.|
|[Across multiple subscriptions](/azure/governance/management-groups/create-management-group-javascript)|If you need to manage multiple subscriptions, learn how to create a management group with JavaScript.|

## One-time installation

To develop using an Azure resource with JavaScript on your local workstation, you need the following installed once:

|Name/Installer|Description|
|--|--|
|[Node.js](https://www.npmjs.com/)|Install latest long-term support (LTS) runtime environment for local workstation development. |
| NPM (installed with modern versions of Node.js) or [Yarn](https://yarnpkg.com/)|Package manager to install Azure SDK libraries.|
|[Visual Studio Code](https://code.visualstudio.com/)| Visual Studio Code will give you a great JavaScript integration and coding experience but it is not required. You can use any code editor. For this document, if you are using a different editor, check for integration with Azure or use the Azure CLI.|
|[Azure CLI](/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)|You can use the Azure CLI to recreate and manage Azure resources from a command line, terminal, or bash shell.|

> [!CAUTION]
> If you plan to use an Azure resource as the runtime environment for your code, such as an Azure web app or an Azure Container Instance, you should verify your local Node.js development environment matches the Azure resource runtime you plan to use.

### Optional local installations

The following common local workstation installations are optional to help with your local development tasks.

|Name/Installer|Description|
|--|--|
| [git](https://git-scm.com/downloads) | Command-line tools for source control. You can use a different source control tool if you prefer. |

## One-time configuration of service principal

Each Azure service has an authentication mechanism. This can include keys and endpoints, connection strings, or other mechanisms. To conform to best practices, create resources and authenticate to resources using a [service principal](node-sdk-azure-authenticate-principal.md). A service principal allows you to concretely define the access scope to the immediate development need.

Steps to **create service principal**: 

1. Sign in to Azure with your individual user account.
1. Create a _named_ service principal with specific scope. Because most quickstarts ask you to create an Azure resource, the service principal needs to have the ability to create resources.
1. Log off Azure with your individual user account.

Steps to **use service principal**:

1. Authenticate to Azure programmatically with the service principal with a certificate, environment variables, or a `.json` file. 
1. Create Azure resources with service principal and use the service.

Learn [how to create a service principal](node-sdk-azure-authenticate-principal.md). Remember to save the response from the creation step. You will need the response's `appId`, `tenant`, and `password` values to use the service principal.

[Create Azure resources with your service principal](/cli/azure/create-an-azure-service-principal-azure-cli#create-a-resource-using-service-principal).

## Steps for each new development project setup

The [Azure SDK libraries](azure-sdk-library-package-index.md) are provided individually for each service. You install each library based on the Azure service you need to use.

Each new project using Azure should:
- Create Azure resources and save associated keys or configuration to a [secure location]().
- Install Azure SDK libraries from NPM or Yarn. 
- Use Service Principal to authenticate to Azure SDKs, then use configuration information to access specific services.

## Securing configuration information

You have several options to store configuration information:
- [Dotenv](https://www.npmjs.com/package/dotenv) is a popular npm package to read environment variables from a `.env` file. Make sure to add the `.env` file to the `.gitignore` file so the `.env` file is not checked into to source control.
- Azure [Key Vault](https://docs.microsoft.com/azure/key-vault/) to create and maintain keys that access and encrypt your cloud resources, apps, and solutions

### Create environment variables for the Azure libraries

To use the Azure settings needed by the Azure SDK libraries to access the Azure cloud, set the most common values to environment variables. The following commands set the environment variables to the local workstation. Another common mechanism is to use the `DOTENV` NPM package to create a `.env` file for these settings. If you plan to use a `.env`, make sure to not check in the file to source control. Add the `.env` file to git's `.ignore` file is the standard way to ensure those settings are checked into source control.

In the following examples, the client ID is the service principal ID and service principal secret.

# [bash](#tab/bash)

```bash
AZURE_SUBSCRIPTION_ID="aa11bb33-cc77-dd88-ee99-0918273645aa"
AZURE_TENANT_ID="00112233-7777-8888-9999-aabbccddeeff"
AZURE_CLIENT_ID="12345678-1111-2222-3333-1234567890ab"
AZURE_CLIENT_SECRET="abcdef00-4444-5555-6666-1234567890ab"
```

# [cmd](#tab/cmd)

```cmd
set AZURE_SUBSCRIPTION_ID="aa11bb33-cc77-dd88-ee99-0918273645aa"
set AZURE_TENANT_ID=00112233-7777-8888-9999-aabbccddeeff
set AZURE_CLIENT_ID=12345678-1111-2222-3333-1234567890ab
set AZURE_CLIENT_SECRET=abcdef00-4444-5555-6666-1234567890ab
```

---

Replace the values shown in these commands with those of your specific service principal.

## Install npm packages

For every project, we recommend that you always create a separate folder, and its own `package.json` file using the following steps:

1. Open a terminal, command prompt, or bash shell and create a new folder to the project. Then move into that new folder.

    ```console
    mkdir MY-NEW-PROJECT && cd MY-NEW-PROJECT
    ```

1. Initialize the package file:

    ```console
    npm init -y
    ```

    This command runs the NPM command to create the package.json file and initialize the minimum properties. As you install Azure SDK library packages with NPM or Yarn, the package.json file captures that installation information.

1. Install the Azure SDK libraries you need for the quickstart. The following command is an example.

    ```console
    npm install @azure/ai-text-analytics@5.0.0
    ```

## Use source control

We recommend that you get into the habit of creating a source control repository whenever you start a project. If you have Git installed, run the following command:

```bash
git init
```

From there, you can run commands like `git add` and `git commit` to commit changes. By regularly committing changes, you create a commit history with which you can revert to any previous state.

To make an online backup of your project, we also recommend uploading your repository to [GitHub](https://github.com) or [Azure DevOps](/azure/devops/user-guide/code-with-git?view=azure-devops). If you've initialized a local repository first, use `git remote add` to attach the local repository to GitHub or Azure DevOps.

Documentation for git is found on [git-scm.com/docs](https://git-scm.com/docs) and all around the Internet.

Visual Studio Code includes a number of built-in git features. For more information, see [Using Version Control in VS Code](https://code.visualstudio.com/docs/editor/versioncontrol).

You can also use any other source control tool of your choice; Git is simply one of the most widely used and supported.

## Next steps

* [Create and use a service principal](node-sdk-azure-authenticate-principal.md)
* [Authenticate with the Azure modules for Node.js](node-sdk-azure-authenticate.md)
* [Deploy a static website to Azure from Visual Studio Code](tutorial-vscode-static-website-node-01.md)