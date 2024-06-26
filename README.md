---
name: Creative Writing Assistant
description: Working with Agents using Promptflow (Python Implementation) 
languages:
- python
- typescript
- bicep
- azdeveloper
products:
- azure-openai
- bing-search
- azure-ai-search
- azure
page_type: sample
urlFragment: agent-openai-python-prompty
---

# Creative Writing Assistant: Working with Agents using Promptflow (Python Implementation) 

This sample demonstrates how to create and work with AI agents. The app takes a topic and instruction input and then calls a research agent, writer agent, and editor agent. 

# Creative Writing Assistant: Working with Agents using Promptflow (Python Implementation) 

In this sample we will be using the creative writting assistant to find the latest camping trends and activities in winter. The `research agent` will recieve some context we provide and an instruction to find information on what we are looking for. It will use this information to create queries, which it will pass to the [Bing Search API](https://www.microsoft.com/en-us/bing/apis/bing-web-search-api) to search the web for relevant information to return. The `product agent` will also recieve the context we provided and will use [Azure AI Search](https://azure.microsoft.com/en-gb/products/ai-services/ai-search) to search through camping product information in a vector store and return the products that are semantically similar to the context. 

The research and products returned will be sent to the `writing agent`, along with the context and instructions we provided. The writer then uses all of this information to create an article. This article is passed to an `editor agent` that analyzes the article, provides feedback for writer and decides whether to accept or reject the article. If the article is rejected the feedback is sent to the researcher and writer agents and a new article is created that incoperates the feedback. In this sample the editor can only reject the article twice. The edited article is then returned to the user. 

  This sample uses the **[Azure OpenAI](https://learn.microsoft.com/en-us/azure/ai-services/openai/)** to access the LLM that will drive the agents. For this sample we recommend using either GPT-35-turbo or GPT-4 with versions 1106 or 0125. You can find the regions where these versions are available [here](). It also leverages **[Prompty and Prompt Flow](https://microsoft.github.io/promptflow/how-to-guides/develop-a-prompty/index.html)** to create, manage and evaluate the prompts into the code. Prompty is a 'markdown-like' file with a `.prompty` extension for developing prompt templates. 

By the end of deploying this template you should be able to:

 1. Describe what Prompty and Prompt Flow provide
 2. Understand Agentic workflows for building LLM Apps
 3. Build, run, evaluate, and deploy, an AI Agent App to Azure.
 
## Features

This project template provides the following features:

* An `Agents` folder with all the agents mentioned in the project description. Each agent subfolder is made up of a `.prompty` and `.py` file. 
* An `orchestrator.py` file where the agent workflow is defined. 
* `requirements.txt` file with all the python packages needed to run this example.
* An `app.py` file that enables you to run this application as a Flask app. 
* A `.env.sample` file to let you know which provisioned resources you will need to run this app. 

## Security

Each template has either [Managed Identity](https://learn.microsoft.com/en-us/entra/identity/managed-identities-azure-resources/overview) or Key Vault built in to eliminate the need for developers to manage these credentials. Applications can use managed identities to obtain Microsoft Entra tokens without having to manage any credentials. 

Additionally, we have added a [GitHub Action tool](https://github.com/microsoft/security-devops-action) that scans the infrastructure-as-code files and generates a report containing any detected issues. 

To ensure best practices in your repo we recommend anyone creating solutions based on our templates ensure that the [Github secret scanning](https://docs.github.com/en/code-security/secret-scanning/about-secret-scanning) setting is enabled in your repos.

### Architecture Diagram
![Architecture Digram]()


### Demo Video 
(Embed demo video here)

## Getting Started

### Prerequisites

- **Azure Subscription** - [Signup for a free account.](https://azure.microsoft.com/free/)
- **Visual Studio Code** - [Download it for free.](https://code.visualstudio.com/download)
- **GitHub Account** - [Signup for a free account.](https://github.com/signup)
- **Access to Azure Open AI Services** - [Learn about getting access.](https://learn.microsoft.com/legal/cognitive-services/openai/limited-access)
- **Ability to provision Azure AI Search (Paid)** - Required for Semantic Ranker
- **Terrafrom** - Install terraform to run deployments
- **Docker Desktop** - Install Docker Desktop to run deployments
- **Recommended Deployment Region** - East US 2 is the recommened region for this deployment. Not all models and services are available for each region. Learn more [here](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models#model-summary-table-and-region-availability).


## Step 1: Development Environment

The repository is instrumented with a `devcontainer.json` configuration that can provide you with a _pre-built_ environment that can be launched locally, or in the cloud. You can also elect to do a _manual_ environment setup locally, if desired. Here are the three options in increasing order of complexity and effort on your part. **Pick one!**

 1. **Pre-built environment, in cloud** with GitHub Codespaces
 2. **Pre-built environment, on device** with Docker Desktop
 3. **Manual setup environment, on device** with Anaconda or venv

The first approach is _recommended_ for minimal user effort in startup and maintenance. The third approach will require you to manually update or maintain your local environment, to reflect any future updates to the repo.

To setup the development environment you can leverage either GitHub Codespaces, a local Python environment (using Anaconda or venv), or a VS Code Dev Container environment (using Docker).

### Step 1.1: Pre-Built Environment, in cloud (GitHub Codespaces)

**This is the recommended option.**
 - Fork the repo into your personal profile.
 - In your fork, click the green `Code` button on the repository
 - Select the `Codespaces` tab and click `Create codespace...` You can also click this button:
[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/Azure-Samples/agent-openai-python-prompty)

This should open a new browser tab with a Codespaces container setup process running. On completion, this will launch a Visual Studio Code editor in the browser, with all relevant dependencies already installed in the running development container beneath. **Congratulations! Your cloud dev environment is ready!**

- Once you've launched Codespaces you can proceed to [step 2]().

### Step 1.2: Pre-Built Environment, on device (Docker Desktop)

This option uses the same `devcontainer.json` configuration, but launches the development container in your local device using Docker Desktop. To use this approach, you need to have the following tools pre-installed in your local device:
 - Visual Studio Code (with Dev Containers Extension)
 - Docker Desktop (community or free version is fine)

**Make sure your Docker Desktop daemon is running on your local device.** Then,
 - Fork this repo to your personal profile
 - Clone that fork to your local device
 - Open the cloned repo using Visual Studio Code

If your Dev Containers extension is installed correctly, you will be prompted to "re-open the project in a container" - just confirm to launch the container locally. Alternatively, you may need to trigger this step manually. See the [Dev Containers Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) for more information.

Once your project launches in the local Docker desktop container, you should see the Visual Studio Code editor reflect that connection in the status bar (blue icon, bottom left). **Congratulations! Your local dev environment is ready!**

- Once you've launched your docker container environment you can proceed to [step 2]().

### Step 1.3: Manual Setup Environment, on device (Anaconda or venv)

#### Local Requirements
In order to run this sample locally you will need to: 

If all of the above are correctly installed you can set up your local developer environment as follows. 

1. First, fork the repo, and then clone the code sample locally: 

   ``` bash
   git clone https://github.com/Azure-Samples/agent-openai-python-prompty.git
   ```

2. Open the repo in VS Code and navgate to the src directory

   ```bash
   cd 
   code .
   cd src
   ```

3. Install the [Prompt Flow Extension](https://marketplace.visualstudio.com/items?itemName=prompt-flow.prompt-flow) in VS Code
      - Open the VS Code Extensions tab
      - Search for "Prompt Flow"
      - Install the extension

4. Install the [Azure CLI](https://learn.microsoft.com/cli/azure/install-azure-cli) for your device OS

5. Cd into the src/api folder

```
cd src/api
```
6. Create a new local Python environment using **either** [anaconda](https://www.anaconda.com/products/individual) **or** [venv](https://docs.python.org/3/library/venv.html) for a managed environment.

    a. **Option 1**: Using anaconda

        ```bash
        conda create -n agent-openai-python-prompty python=3.11
        conda activate agent-openai-python-prompty
        pip install -r requirements.txt
        ```

    b. **Option 2:** Using venv

        ```bash
        python3 -m venv .venv
        source .venv/bin/activate
        pip install -r requirements.txt
        ```

## 2. Create Azure resources

We setup our development ennvironment in the previous step. In this step, we'll **provision Azure resources** for our project, ready to use for developing our LLM Application.


### 2.1 Authenticate with Azure

Start by connecting your Visual Studio Code environment to your Azure account:

1. Open the terminal in VS Code and use command `az login`. 
2. Complete the authentication flow. 

**If you are running within a dev container, use these instructions to login instead:**
 1. Open the terminal in VS Code and use command `az login --use-device-code`
 2. The console message will give you an alphanumeric code
 3. Navigate to _https://microsoft.com/devicelogin_ in a new tab
 4. Enter the code from step 2 and complete the flow.

In either case, verify that the console shows a message indicating a successful authentication. **Congratulations! Your VS Code session is now connected to your Azure subscription!**

### 2.2 Provision with Azure Developer CLI

For this project, we need to provision multiple Azure resources in a specific order. **Before**, we achieved this by running the `provision.sh` script. **Now**, we'll use the [Azure Developer CLI](https://learn.microsoft.com/azure/developer/azure-developer-cli/overview) (or `azd`) instead, and follow the steps below.
Visit the [azd reference](https://learn.microsoft.com/azure/developer/azure-developer-cli/reference) for more details on tool syntax, commands and options.

#### 2.2.1 Install `azd`
- If you setup your development environment manually, follow [these instructions](https://learn.microsoft.com/azure/developer/azure-developer-cli/install-azd?tabs=winget-windows%2Cbrew-mac%2Cscript-linux&pivots=os-windows) to install `azd` for your local device OS.
- If you used a pre-built dev container environment (e.g., GitHub Codespaces or Docker Desktop) the tool is pre-installed for you.
- Verify that the tool is installed by typing ```azd version``` in a terminal.

#### 2.2.2 Authenticate with Azure
- Start the authentication flow from a terminal:
    ```bash
    azd auth login
    ```
- This should activate a Device Code authentication flow as shown below. Just follow the instructions and complete the auth flow till you get the `Logged in on Azure` message indicating success.
    ```bash
    Start by copying the next code: <code-here>
    Then press enter and continue to log in from your browser...
    ```

#### 2.2.3 Provision and Deploy 

- Run this unified command to provision all resources. This will take a non-trivial amount of time to complete.
    ```bash
    azd up
    ```
- On completion, it automatically invokes a`postprovision.sh` script that will attempt to log you into Azure. You may see something like this. Just follow the provided instructions to complete the authentication flow.
    ```bash
    No Azure user signed in. Please login.
    ```
- Once logged in, the script will do the following for you:
    - Download `config.json` to the local device
    - Populate `.env` with required environment variables
    - Populate your data (in Azure AI Search, Azure CosmosDB)
    - Create relevant Connections (for prompt flow)
    - Upload your prompt flow to Azure (for deployment)

That's it! You should now be ready to continue the process as before. Note that this is a new process so there may be some issues to iron out. Start by completing the verification steps below and taking any troubleshooting actions identified.

#### 2.2.4 Verify Provisioning

The script should **set up a dedicated resource group** with the following resources:

 - **Azure AI services** resource
 - **Azure Machine Learning workspace** (Azure AI Project) resource
 - **Search service** (Azure AI Search) resource
 - **Bing Search** (Bing Search) resource

The script will set up an **Azure AI Studio** project with the following model deployments created by default, in a relevant region that supports them. _Your Azure subscription must be [enabled for Azure OpenAI access](https://learn.microsoft.com/azure/ai-services/openai/overview#how-do-i-get-access-to-azure-openai)_.
 - gpt-3.5-turbo
 - text-embeddings-ada-002
 - gpt-4

The Azure AI Search resource will have **Semantic Ranker** enabled for this project, which requires the use of a paid tier of that service. It may also be created in a different region, based on availability of that feature.

### 2.3 Verify `config.json` setup

The script should automatically create a `config.json` in your root directory, with the relevant Azure subscription, resource group, and AI workspace properties defined. _These will be made use of by the Azure AI SDK for relevant API interactions with the Azure AI platform later_.

If the config.json file is not created, simply download it from your Azure portal by visiting the _Azure AI project_ resource created, and looking at its Overview page.

### 2.4 Verify `.env` setup

The default sample has an `.env.sample` file that shows the relevant environment variables that need to be configured in this project. The script should create a `.env` file that has these same variables _but populated with the right values_ for your Azure resources.

If the file is not created, simply copy over `.env.sample` to `.env` - then populate those values manually from the respective Azure resource pages using the Azure Portal (for Azure CosmosDB and Azure AI Search) and the Azure AI Studio (for the Azure OpenAI values)

## 3. Run the app locally

Change to api/agents folder:
```
cd src/api
```

To run just the orchestrator logic:
```
python -m api.agents.orchestrator
```

To run the flask webserver:
```
flask --debug --app api.app:app run --port 5000
```
```
http://127.0.0.1:5000/get_article?context=Write an article about camping in alaska&instruction=find specifics about what type of gear they would need and explain in detail
```

In a new terminal
```
cd src/web
```

First install node packages:
```
npm install
```

Then run the web app with a local dev web server:
```
npm run dev
```

Then run evaluation
```
cd evaluate
python evaluate.py
```
## 4. Evaluating prompt flow results

Now, we need to understand how well our prompt flow performs using defined metrics like **groundedness**, **coherence** etc. To evaluate the prompt flow, we need to be able to compare it to what we see as "good results" in order to understand how well it aligns with our expectations. 

We may be able to evaluate the flow manually (e.g., using Azure AI Studio) but for now, we'll evaluate this by running the prompt flow using **gpt-4** and comparing our performance to the results obtained there. To do this, follow the instructions and steps in the notebook `evaluate-chat-prompt-flow.ipynb` under the `eval` folder.

## 5. Deployment with SDK

At this point, we've built, run, and evaluated, the prompt flow **locally** in our Visual Studio Code environment. We are now ready to deploy the prompt flow to a hosted endpoint on Azure, allowing others to use that endpoint to send _user questions_ and receive relevant responses.

This process consists of the following steps:
 1. We push the prompt flow to Azure (effectively uploading flow assets to Azure AI Studio)
 2. We activate an automatic runtime and run the uploaded flow once, to verify it works.
 3. We deploy the flow, triggering a series of actions that results in a hosted endpoint.
 4. We can now use built-in tests on Azure AI Studio to validate the endpoint works as desired.

Just follow the instructions and steps in the notebook `push_and_deploy_pf.ipynb` under the `deployment` folder. Once this is done, the deployment endpoint and key can be used in any third-party application to _integrate_ with the deployed flow for real user experiences.


## 6. Deploy with GitHub Actions

### 6.1. Create Connection to Azure in GitHub
- Login to [Azure Shell](https://shell.azure.com/)
- Follow the instructions to [create a service principal here](hhttps://github.com/microsoft/llmops-promptflow-template/blob/main/docs/github_workflows_how_to_setup.md#create-azure-service-principal)
- Follow the [instructions in steps 1 - 8  here](https://github.com/microsoft/llmops-promptflow-template/blob/main/docs/github_workflows_how_to_setup.md#steps) to add create and add the user-assigned managed identity to the subscription and workspace.

- Assign `Data Science Role` and the `Azure Machine Learning Workspace Connection Secrets Reader` to the service principal. Complete this step in the portal under the IAM.
- Setup authentication with Github [here](https://github.com/microsoft/llmops-promptflow-template/blob/main/docs/github_workflows_how_to_setup.md#set-up-authentication-with-azure-and-github)

```bash
{
  "clientId": <GUID>,
  "clientSecret": <GUID>,
  "subscriptionId": <GUID>,
  "tenantId": <GUID>
}
```
- Add `SUBSCRIPTION` (this is the subscription) , `GROUP` (this is the resource group name), `WORKSPACE` (this is the project name), and `KEY_VAULT_NAME` to GitHub.

### 6.2. Create a custom environment for endpoint
- Follow the instructions to create a custom env with the packages needed [here](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-manage-environments-in-studio?view=azureml-api-2#create-an-environment)
  - Select the `upload existing docker` option 
  - Upload from the folder `runtime\docker`

- Update the deployment.yml image to the newly created environemnt. You can find the name under `Azure container registry` in the environment details page.

<br/>

## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
