# Intro

## Create and build the Dockerfile

Next, we'll create the Dockerfile.  

1. Open a browser and navigate to the Agent pools tab for your Azure Pipelines organization or TFS server:  
Azure Pipelines: `https://dev.azure.com/{your_organization}/_settings/agentpools`  
Azure DevOps Server 2019: `https://dev.azure.com/{your_collection}/_settings/agentpools`  
TFS 2018: `https://{your_server}/DefaultCollection/_admin/_AgentPool`  
TFS 2017: `https://{your_server}/tfs/DefaultCollection/_admin/_AgentPool`  
TFS 2015: `http://{your_server}:8080/tfs/_admin/_AgentPool`  
That didn't work: [Get the correct URL](https://docs.microsoft.com/en-us/azure/devops/server/admin/websitesettings)

2. Download the agent

3. Copy the agent to the folder where dockerfile is located

4. Modify `dockerfile`
   Modify the line what is `COPY ./{your_agent_file} ./agent/` 

5. Modify `start.sh`
   Modify the line what is `AZP_AGENTPACKAGE_FILE=$(echo '{your_agent_file}')`

6. Run the following command within that directory:

    ```shell
    docker build -t vsts-agent:latest .
    ```

    This command builds the Dockerfile in the current directory.

    The final image is tagged `vsts-agent:latest`.
    You can easily run it in a container as `vsts-agent`, since the `latest` tag is the default if no tag is specified.

## Start the image

Now that you have created an image, you can spin up a container.

1. Open a terminal
2. Run the container. This will install the latest version of the agent, configure it, and run the agent targeting the `Default` pool of a specified Azure DevOps or Azure DevOps Server instance of your choice:

    ```shell
    docker run -e AZP_URL=<Azure DevOps instance> -e AZP_TOKEN=<PAT token> -e AZP_AGENT_NAME=yourdockeragent vsts-agent:latest
    ```

You can optionally control the pool and agent work directory using additional [environment variables](#environment-variables).

## Environment variables

| Environment variable | Description                                                 |
|----------------------|-------------------------------------------------------------|
| AZP_URL              | The URL of the Azure DevOps or Azure DevOps Server instance |
| AZP_TOKEN            | Personal Access Token (PAT) granting access to `AZP_URL`    |
| AZP_AGENT_NAME       | Agent name (default value: the container hostname)          |
| AZP_POOL             | Agent pool name (default value: `Default`)                  |
| AZP_WORK             | Work directory (default value: `_work`)                     |

## Reference

* [Azure Pipelines agents](https://docs.microsoft.com/zh-cn/azure/devops/pipelines/agents/agents?view=azure-devops)
* [Running a self-hosted agent in Docker](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/docker?view=azure-devops#linux)