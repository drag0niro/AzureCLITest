[![Python package](https://github.com/microsoft/azure-devops-python-api/workflows/Python%20package/badge.svg)](https://github.com/microsoft/azure-devops-python-api/actions)
[![Build Status](https://dev.azure.com/mseng/vsts-cli/_apis/build/status/vsts-python-api?branchName=dev)](https://dev.azure.com/mseng/vsts-cli/_build/latest?definitionId=5904&branchName=dev)
[![Python](https://img.shields.io/pypi/pyversions/azure-devops.svg)](https://pypi.python.org/pypi/azure-devops)

# Azure DevOps Python API

This repository contains Python APIs for interacting with and managing Azure DevOps - please check [Microsoft/azure-devops-python-api](https://github.com/microsoft/azure-devops-python-api). 
These APIs power the Azure DevOps Extension for Azure CLI. To learn more about the Azure DevOps Extension for Azure CLI, visit the [Microsoft/azure-devops-cli-extension](https://github.com/Microsoft/azure-devops-cli-extension) repo.

## Install 
[Install the Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). You must have at least `v2.0.69`, which you can verify with `az --version` command.

1. Add the Azure DevOps Extension `az extension add --name azure-devops`

1. Run the `az login` command.

    If the CLI can open your default browser, it will do so and load a sign-in page. Otherwise, you need to open a
    browser page and follow the instructions on the command line to enter an authorization code after navigating to
    [https://aka.ms/devicelogin](https://aka.ms/devicelogin) in your browser. For more information, see the
    [Azure CLI login page](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

See the [Get started guide](https://docs.microsoft.com/azure/devops/cli/get-started?view=azure-devops) for detailed setup instructions.

Next you need to install azure-devops librabry in python:
```
pip install azure-devops
```

## Get started


To use the API, establish a connection using a [personal access token](https://docs.microsoft.com/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=vsts) and the URL to your Azure DevOps organization. Then get a client from the connection and make API calls.

```python
from azure.devops.connection import Connection
from msrest.authentication import BasicAuthentication
import pprint

# Fill in with your personal access token and org URL
personal_access_token = 'YOURPAT'
organization_url = 'https://dev.azure.com/YOURORG'

# Create a connection to the org
credentials = BasicAuthentication('', personal_access_token)
connection = Connection(base_url=organization_url, creds=credentials)

# Get a client (the "core" client provides access to projects, teams, etc)
core_client = connection.clients.get_core_client()

# Get the first page of projects
get_projects_response = core_client.get_projects()
index = 0
while get_projects_response is not None:
    for project in get_projects_response.value:
        pprint.pprint("[" + str(index) + "] " + project.name)
        index += 1
    if get_projects_response.continuation_token is not None and get_projects_response.continuation_token != "":
        # Get the next page of projects
        get_projects_response = core_client.get_projects(continuation_token=get_projects_response.continuation_token)
    else:
        # All projects have been retrieved
        get_projects_response = None
```
## Azure CLI Usage

- Checkout the CLI docs at [docs.microsoft.com - Azure DevOps CLI](https://docs.microsoft.com/azure/devops/cli/).
- Check out other examples in the [How-to guides](https://docs.microsoft.com/azure/devops/cli/?view=azure-devops#how-to-guides) section.
- You can view the various commands and its usage here - [docs.microsoft.com - Azure DevOps Extension Reference](https://docs.microsoft.com/cli/azure/ext/azure-devops/?view=azure-cli-latest)

Basic usage:
```bash
$az [group] [subgroup] [command] {parameters}
```

Adding the Azure DevOps Extension adds `devops`, `pipelines`, `artifacts`, `boards` and `repos` groups.
For usage and help content for any command, pass in the -h parameter, for example:

```bash
$ az devops -h

Group
    az devops : Manage Azure DevOps organization level operations.
        Related Groups
        az pipelines: Manage Azure Pipelines
        az boards: Manage Azure Boards
        az repos: Manage Azure Repos
        az artifacts: Manage Azure Artifacts.

Subgroups:
    admin            : Manage administration operations.
    extension        : Manage extensions.
    project          : Manage team projects.
    security         : Manage security related operations.
    service-endpoint : Manage service endpoints/service connections.
    team             : Manage teams.
    user             : Manage users.
    wiki             : Manage wikis.

Commands:
    configure        : Configure the Azure DevOps CLI or view your configuration.
    feedback         : Displays information on how to provide feedback to the Azure DevOps CLI team.
    invoke           : This command will invoke request for any DevOps area and resource. Please use
                       only json output as the response of this command is not fixed. Helpful docs -
                       https://docs.microsoft.com/en-us/rest/api/azure/devops/.
    login            : Set the credential (PAT) to use for a particular organization.
    logout           : Clear the credential for all or a particular organization.
```

## API documentation

This Python library provides a thin wrapper around the Azure DevOps REST APIs. See the [Azure DevOps REST API reference](https://docs.microsoft.com/en-us/rest/api/azure/devops/?view=azure-devops-rest-5.1) for details on calling different APIs.

## Samples

Learn how to call different APIs by viewing the samples in the [Microsoft/azure-devops-python-samples](https://github.com/Microsoft/azure-devops-python-samples) repo.

## Licenses

[MIT License](LICENSE)
# AzureCLITest
