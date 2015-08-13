# Node.js Ansible Role

This role assists with deploying a Node.js application on Fedora 22, CentOS 7, or a Docker container. It currently performs the following tasks:

* Installs Node.js, NPM, and any other requested packages
* Creates a user account and group used by the application
* Clones the application's Git repository
* Runs ``npm install`` in the Git working directory or specific commands requested by the user
* Manages the application process using Systemd in a VM or physical machine, or using Supervisor in a Docker container
* Optionally checks a user provided HTTP endpoint for a test string to confirm the application has been deployed correctly

The role uses the following [Ansible tags](http://docs.ansible.com/ansible/playbooks_tags.html) to control the provisioning process:

* ``install`` - install software using package managers and (not recommended) using source 
* ``configure`` - add user accounts, groups, make filesystem related changes, clone git repositories, run application commands, etc.
* ``deploy`` - enable and restart a Systemd unit if a VM or physical machine is being used or in the case of Docker, add a Supervisor config
* ``test`` - check the application's HTTP endpoint for a response to confirm it is working

## Role Variables

Please refer to the [defaults/main.yml](https://github.com/avtar/ansible-nodejs/blob/master/defaults/main.yml) file for a list of variables along with additional documentation.

## Requirements

The following roles are required:

*  [facts](https://github.com/avtar/ansible-facts/)

## Example

Here is an example playbook that uses this role:

```
- hosts: localhost
  user: root

  vars:
    nodejs_app_name: preferences-server
    nodejs_app_git_repo: https://github.com/gpii/universal.git
    nodejs_app_git_branch: master
    nodejs_app_rpm_packages:
      - nodejs-grunt-cli
    nodejs_app_commands:
      - npm install
      - grunt dedupe-infusion
    nodejs_app_start_script: gpii.js
    nodejs_app_env_vars:
      - NODE_ENV=development.all.local
    nodejs_app_host_address: 127.0.0.1
    nodejs_app_tcp_port: 8081
    nodejs_app_test_http_endpoint: /preferences/carla
    nodejs_app_test_http_status_code: 200
    nodejs_app_test_string: registry.gpii.net

  roles:
    - facts
    - nodejs
```
