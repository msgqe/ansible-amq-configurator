Role Name
=========

amqcfg configuration role

Requirements
------------

Make sure your ansible host machine has pip installed (need for virtualenv)
[amqcfg](https://bitbucket.org/msgqe/amqcfg/)

ansible-amq-broker (not a real requirement, but it is closely used after this role)


Role Variables: Basic Variables
--------------


| Name              | Default Value       | Description          |
|-------------------|---------------------|----------------------|
| `amq_configurator_workspace_dir` | `.` |  Environment variable taken from `WORKSPACE`. |
| `amq_configurator_profile` |  empty | Environment variable `AMQCFG_PROFILE` to define which amqcfg profile to use. |
| `amq_configurator_master_file` | `${WORKSPACE}/deployment_configuration.yaml` | Master configuration yaml file passed to test suite. |
| `amq_configurator_broker_file` | `${WORKSPACE}/<hostname>/profile_data.yaml` | Broker specific yaml configuration file. |
| `amq_configurator_broker_config_dir` | `${WORKSPACE}/<hostname>` | Output directory for amqcfg profile generation. |
| `amq_configurator_broker_db_file` | `${WORKSPACE}/db_tune.yaml` | Optional JDBC tuning file for amqcfg profile generation. |
| `amq_configurator_deploy_mode` | false | Optional environemnt `DEPLOY_MODE` variable to define whether to reuse existing profile or create anew if it does not exist already. |

Role Variables: Reused from `ansible-amq-broker`
--------------

| Name              | Default Value       | Description          |
|-------------------|---------------------|----------------------|
| `amq_broker_local_facts_dir` | `inventory dir` | Environment variable where facts are generated `AMQ_BROKER_FACTS_DIR`. |
| `amq_broker_default_facts_file` | `${AMQ_BROKER_FACTS_DIR}/amq_broker_facts_<hostname>.yaml` | Specific broker facts. |
| `amq_broker_node_info_file` | `${AMQ_BROKER_FACTS_DIR}/node_information_<hostname>.yaml` | Specific node information facts for given broker. |
| `amq_broker_home_instance` | figured out from facts | Broker instance directory. |

These variables are meant to be passed from `ansible-amq-broker` role in the future.
Playbook should call `ansible-amq-broker` role for deployment of Artemis broker and followed by this role.


Role Variables: Internal variables
--------------

| Name              | Default Value       | Description          |
|-------------------|---------------------|----------------------|
| `amq_configurator_venv` | `${WORKSPACE}/venv` | Virtual env to be created and used by amqcfg, yq tools.  |
| `amq_configurator_amqcfg_cmd` | `${WORKSPACE}/venv/bin/amqcfg` | Executable path to amqcfg bin in venv. |
| `amq_configurator_yq_cmd` | `${WORKSPACE}/venv/bin/yq` | Executable path to yq bin in venv. |
| `amq_configurator_copy_only` | False | Whether to copy only existing profile or generate new one. This is based on existence of profile dir and deploy_mode. |

Usage
------
```yaml
- name: Configure brokers using amqcfg
  hosts: all
  gather_facts: True
#  vars_files:
#    - group_vars/amq_broker_defaults.yml
  roles:
     - ansible-amq-configurator
```
