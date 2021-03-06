# Ansible Centreon Configurator

Table of Contents

- [Overview](#overview)
- [Compatible Centreon versions](#compatible-centreon-versions)
- [Requirements](#requirements)
  - [Operating systems](#operating-systems)
- [Installation](#installation)
- [Role Variables](#role-variables)
- [Examples](#examples)
  - [Scenarios](#scenarios)
  - [Simple Example](#simple-example)
  - [Advanced Example](#advanced-example)
- [Screencasts](#screencasts)
- [License](LICENSE)

## Overview

![wrapper diagram](docs/images/wrapper_diagram.png)

The `wrapper` is built to intermediate the apply of objects configured in a single template and can be applied remotely through a set of predefined instructions and using the Centreon webapi tools for configuration in the environment.

The idea behind this `wrapper` is to have a tool that can apply the Centreon configuration objects to remote monitoring points in the network structure, even if there are access restrictions or limitations to apply settings from a certain point.

Because we can upload the `wrapper` to any other host and start the trigger on this host, it can still do remote access to apply the settings. So too, how can the host itself be applied.

The configuration objects are translated into a single json file, which is sent along with the wrapper. This file is dynamic and is assembled from Ansible.

## Compatible Centreon versions

Following versions of Centreon are successfully tested with this role:

- 18.10.0

## Requirements

### Operating systems

This role will work on the following operating systems:

- Red Hat / Centos
- Debian
- Ubuntu
- Windows (coming soon)

## Installation

### Using GIT

```bash
cd ~/work-dir
git clone https://github.com/centreon/centreon-iac-ansible-configurator.git
cd centreon-iac-ansible-configurator
```

### Using Ansible Galaxy

Coming soon

## Role Variables

- `centreon_webapi_host`: Host or ip to connect in WebAPI
- `centreon_webapi_port`: TCP port to connect in WebAPI
- `centreon_admin_password`: Password used by Administrator user
- `configuration`: Is a dictionary variable with values to use for template of objects. You must use the same names defined in the Centreon [WebAPI documentation](https://documentation.centreon.com/docs/centreon/en/latest/api/clapi/objects/index.html)

Sample values for the variable `configuration`:

```yaml
        cg:
        - {
          'name': 'testers',
          'alias': 'Testers contacts',
          'state': 'enabled'
          }
        contact:
        - {
          'name': 'Test contact One',
          'alias': 'test01',
          'email': 'test01@test.centreon.com',
          'password': 'easypass',
          'admin': '0',
          'gui': '1',
          'language': 'en_US',
          'auth_type': 'local',
          'state': 'enabled' # enabled, disabled, absent
          }
        host:
        - {
          'name': 'test01',
          'alias': 'Server teste 01',
          'address': '127.0.0.1',
          'template': 'generic-active-host-custom',
          'instance': 'central',
          'hostgroup': '',
          'state': 'enabled' # enabled, disabled, absent
        }
```

## Examples

### Scenarios

You can create playbooks to add a set of objects directly to Centreon, or you can create the objects into separate playbooks for each host you add using Ansible.

For example, to create a host with a webserver service with Nginx, you can have a playbook as defined [here: deploy-nginx.yml](examples/deploy-nginx.yml)

### Simple example

```yaml
---

- name: Add monitor to Centreon
  hosts: centreon-web
  remote_user: root

  roles:
  - role: "roles/configurator"
    centreon_admin_password: "centreon"
    centreon_webapi_host: "http://localhost"
    centreon_webapi_port: "80"
    configuration:
        cg:
        - {
          'name': 'testers',
          'alias': 'Testers contacts',
          'state': 'enabled'
          }
        contact:
        - {
          'name': 'Test contact One',
          'alias': 'test01',
          'email': 'test01@test.centreon.com',
          'password': 'easypass',
          'admin': '0',
          'gui': '1',
          'language': 'en_US',
          'auth_type': 'local',
          'state': 'enabled' # enabled, disabled, absent
          }
        host:
        - {
          'name': 'test01',
          'alias': 'Server teste 01',
          'address': '127.0.0.1',
          'template': 'generic-active-host-custom',
          'instance': 'central',
          'hostgroup': '',
          'state': 'enabled' # enabled, disabled, absent
        }
```

### Advanced example

![wrapper diagram](docs/images/wrapper_with_proxy.png)

We may have a situation where Ansible may not have access directly to Centreon, only through a host that is located between the Ansible network and the DMZ where the Centreon is located. For these scenarios, you will either have to use a specific host or use the services client that Centreon will monitor.

## Screencasts

(Coming soon)
