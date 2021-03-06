---
title: VS Code Snippets
description: A collection of VS Code Snippets for working with Azure ML.
---

We have compiled a collection of useful templates in the form of
[VS code snippets](https://code.visualstudio.com/docs/editor/userdefinedsnippets).

![VS Code Snippets](vs-code-snippets-demo.gif)

To add these snippets to your VS Code: `ctrl+shift+p` > Type 'Configure user
snippets' > Select `python.json`. All of these snippets are available here:
[python.json](https://github.com/Azure/azureml-snippets/blob/main/snippets/python.json)

### Imports Group: Basic

Description: Import collection of basic Azure ML classes

Prefix: `import-basic`

```python
from azureml.core import Workspace              # connect to workspace
from azureml.core import Experiment             # connect/create experiments
from azureml.core import ComputeTarget          # connect to compute
from azureml.core import Environment            # manage e.g. Python environments
from azureml.core import Datastore, Dataset     # work with data
```
### Import Workspace

Description: Import Workspace class

Prefix: `import-workspace`

```python
from azureml.core import Workspace
```
### Import Compute Target

Description: Import ComputeTarget class

Prefix: `import-compute-target`

```python
from azureml.core import ComputeTarget
```
### Import Environment

Description: Import Environment class

Prefix: `import-environment`

```python
from azureml.core import Environment
```
### Import ScriptRunConfig

Description: Import ScriptRunConfig class

Prefixes: `import-script-run-config`, `import-src`

```python
from azureml.core import ScriptRunConfig
```
### Import Dataset

Description: Import Dataset class

Prefix: `import-dataset`

```python
from azureml.core import Dataset
```
### Import Datastore

Description: Import Datastore class

Prefix: `import-datastore`

```python
from azureml.core import Datastore
```
### Import Run

Description: Import Run class

Prefix: `import-run`

```python
from azureml.core import Run
```
### Import Conda Dependencies

Description: Import CondaDependencies class

Prefix: `import-conda-dependencies`

```python
from azureml.core.conda_dependencies import CondaDependencies
```
### Get Workspace From Config

Description: Get Azure ML Workspace from config

Prefixes: `get-workspace-config`, `ws-config`

```python
from azureml.core import Workspace
ws = Workspace.from_config()
```
### Get Workspace

Description: Get Azure ML Workspace

Prefixes: `get-workspace`, `get-ws`

```python
from azureml.core import Workspace
ws = Workspace.get(
    name='${1:name}',
    subscription_id='${2:subscription_id}',
    resource_group='${3:resource_group}',
)
```
### Get Compute

Description: Get Azure ML Compute Target

Prefix: `get-compute`

```python
from azureml.core import ComputeTarget
target = ComputeTarget(${2:ws}, '${1:<compute_target_name>}')
```
### Get Compute with SSH

Description: Get Azure ML Compute Target with SSH

Prefix: `get-compute-ssh`

```python
from azureml.core.compute import AmlCompute
from azureml.core.compute_target import ComputeTargetException

ssh_public_key = 'public-key-here'
compute_config = AmlCompute.provisioning_configuration(
    vm_size='$2',
    min_nodes=$3,
    max_nodes=$4,
    admin_username='$5',
    admin_user_ssh_key=ssh_public_key,
    vm_priority='${6|lowpriority,dedicated|}',
    remote_login_port_public_access='Enabled',
    )

cluster = ComputeTarget.create(
    workspace=${7:workspace_name},
    name='${8:target_name}',
    compute_config=compute_config,
)
```
### Get Environment

Description: Get Azure ML Environment

Prefix: `get-environment`

```python
from azureml.core import Environment
${2:env} = Environment('${1:<env-name>}')
```
### Get Environment From Pip

Description: Create environment from pip requirements.txt

Prefixes: `get-environment-pip`, `env-pip`

```python
from azureml.core import Environment
env = Environment.from_pip_requirements(
    name='${1:env_name}',
    file_path='${2:requirements.txt}',
)
```
### Get Environment From Conda

Description: Create environment from Conda env.yml file

Prefixes: `get-environment-conda`, `env-conda`

```python
from azureml.core import Environment
env = Environment.from_conda_specification(
    name='${1:env_name}',
    file_path='${2:env.yml}',
)
```
### Get Environment From SDK

Description: Create environment using CondaDependencies class

Prefixes: `get-environment-sdk`, `env-sdk`

```python
from azureml.core import Environment
from azureml.core.conda_dependencies import CondaDependencies
env = Environment('${1:my-env}')

conda = CondaDependencies()

# add channels
conda.add_channel('$2')

# add conda packages
conda.add_conda_package('$3')

# add pip packages
conda.add_pip_package('$4')

# add conda dependencies to environment
env.python.conda_dependencies = conda
```
### Get Environment From Custom image

Description: Create environment using Custom image

Prefixes: `get-environment-custom-image`, `env-image`

```python
from azureml.core import Environment
env = Environment('${1:my-env}')

env.docker.enabled = True

# base image for DockerHub
env.docker.base_image = '${2}'

# if you are using base image from a Dockerfile
# env.docker.base_image = None
# env.docker.base_dockerfile = './Dockerfile'

# The user_managed_dependencies flag to True will use your custom image's built-in Python environment. 
env.python.user_managed_dependencies = True

```
### Workspace Compute Targets

Description: Get compute target from workspace

Prefix: `ws-compute-target`

```python
target = ws.compute_targets['${1:target-name}']
```
### Workspace Environments

Description: Get environment from workspace

Prefix: `ws-environment`

```python
env = ws.environments['${1:env-name}']
```
### Workspace Datastores

Description: Get datastore from workspace

Prefix: `ws-datastore`

```python
datastore = ws.datastores['${1:datastore-name}']
```
### Workspace Datasets

Description: Get dataset from workspace

Prefix: `ws-dataset`

```python
dataset = ws.datasets['${1:dataset-name}']
```
### Workspace Experiment

Description: Get (existing) experiment from workspace

Prefix: `ws-experiment`

```python
exp = ws.experiments['${1:experiment-name}']
```
### Workspace Models

Description: Get model from workspace

Prefix: `ws-model`

```python
model = ws.models['${1:model-name}']
```
### Script Run Config

Description: Set up ScriptRunConfig including compute target, environment and experiment

Prefixes: `script-run-config`, `src`

```python
from azureml.core import Workspace, Experiment, ScriptRunConfig

# get workspace
ws = Workspace.from_config()

# get compute target
target = ws.compute_targets['${1:target-name}']

# get registered environment
env = ws.environments['${2:env-name}']

# get/create experiment
exp = Experiment(ws, '${3:experiment_name}')

# set up script run configuration
config = ScriptRunConfig(
    source_directory='${4:.}',
    script='${5:script.py}',
    compute_target=target,
    environment=env,
    arguments=[${6:'--meaning', 42}],
)

# submit script to AML
run = exp.submit(config)
print(run.get_portal_url()) # link to ml.azure.com
run.wait_for_completion(show_output=True)
```
### Script Run Config with Command

Description: Set up ScriptRunConfig using command argument

Prefixes: `script-run-config-command`, `command-src`, `src-command`

```python
from azureml.core import Workspace, Experiment, ScriptRunConfig

# get workspace
ws = Workspace.from_config()

# get compute target
target = ws.compute_targets['${1:target-name}']

# get registered environment
env = ws.environments['${2:env-name}']

# get/create experiment
exp = Experiment(ws, '${3:experiment_name}')

# create command
command = 'python ${4:script.py} ${5:--argument value}'.split()

# set up script run configuration
config = ScriptRunConfig(
    source_directory='${6:.}',
    command=command,
    compute_target=target,
    environment=env,
)

# submit script to AML
run = exp.submit(config)
print(run.get_portal_url()) # link to ml.azure.com
run.wait_for_completion(show_output=True)
```
### Script Run Config with Distributed Config

Description: Set up ScriptRunConfig for distributed training.

Prefixes: `script-run-config-distributed`, `distributed-src`, `src-distributed`

```python
from azureml.core import Workspace, ScriptRunConfig, Environment, Experiment
from azureml.core.runconfig import MpiConfiguration

# get workspace
ws = Workspace.from_config()

# get compute target
target = ws.compute_targets['${1:target-name}']

# get curated environment
curated_env_name = '${2:AzureML-PyTorch-1.6-GPU}'
env = Environment.get(workspace=ws, name=curated_env_name)

# get/create experiment
exp = Experiment(ws, '${3:experiment_name}')

# distributed job configuration
distributed_job_config = MpiConfiguration(process_count_per_node=4, node_count=2)

# set up script run configuration
config = ScriptRunConfig(
    source_directory='${4:.}',
    script='${5:script.py}',
    compute_target=target,
    environment=env,
    distributed_job_config=distributed_job_config,
)

# submit script to AML
run = exp.submit(config)
print(run.get_portal_url()) # link to ml.azure.com
run.wait_for_completion(show_output=True)
```
### Run Details Widget

Description: Represents a Jupyter notebook widget used to view the progress of model training.

Prefix: `run-details-widget`

```python
from azureml.core import Workspace,Experiment,Run
from azureml.widgets import RunDetails

# get workspace
ws = Workspace.from_config()

# get/create experiment
exp = Experiment(ws, '${1:experiment_name}')

# get run
run = Run(exp,'${2:run_id}')

# submit script to AML
RunDetails(run).show()
```
### Consume Dataset

Description: Download Azure ML dataset to current working directory

Prefix: `consume-dataset`

```python
#azureml-core of version 1.0.72 or higher is required
from azureml.core import Workspace, Dataset

# get/create experiment
ws = Workspace.from_config()

# get dataset
dataset = Dataset.get_by_name(ws, name='${1:dataset_name}')
dataset.download(target_path='.', overwrite=False)
```
### Create Tabular Dataset

Description: Create Azure ML tabular dataset.

Prefix: `create-tabular-dataset`

```python
from azureml.core import Workspace, Datastore, Dataset

datastore_name = '${1:datastore_name}'

# get workspace
ws = Workspace.from_config()

# retrieve an existing datastore in the workspace by name
datastore = Datastore.get(ws, datastore_name)

# create a TabularDataset from 1 file paths in datastore
datastore_paths = [(datastore, ${2:file_path})]

custom_ds = Dataset.Tabular.from_delimited_files(path=datastore_paths)
```
### Create File Dataset

Description: Create Azure ML file dataset.

Prefix: `create-file-dataset`

```python
# create a FileDataset pointing to files in 'animals' folder and its subfolders recursively
from azureml.core import Workspace, Datastore, Dataset

datastore_name = '${1:datastore_name}'

# get workspace
ws = Workspace.from_config()

# retrieve an existing datastore in the workspace by name
datastore = Datastore.get(ws, datastore_name)

# create a FileDataset pointing to files in your folder and its subfolders recursively, you can also use public web urls paths
datastore_paths = [(datastore, ${2:file_path})]

custom_ds = Dataset.File.from_files(path=datastore_paths)
```
