# SDG National Reporting Initiative Ansible Scripts

This repository contains the [Ansible](https://www.ansible.com/) scripts for the SDG National Reporting Intiative. We use Ansible to help automate the provisioning and restoration of technical infrastructure.

## Directory Layout

To understand the directory layout, please familiarize yourself with [Content Organization](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html#content-organization) section of the [Best Practices](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html#content-organization) guide from the offical Ansible documentation.

## Configuration

Check the `defaults` directory for each role to get a list of the configurable variables for that role (some of the variables don't have default values and will need to be set before you can run the playbooks). You can override them either by creating a `vars/main.yml` file under the corresponding role or by setting them in the proper group file under the `group_vars` directory.

For connection configurations, set the variables in the inventory files under `inventories`.

## Playbooks

There are currently two playbooks:

### jenkins_playbook.yml

This playbook installs Jenkins and restores any configurations and plugins.

### sdg_api_playbook.yml

This playbook sets up and deploys the API service backing the SDG National Reporting Initiative Website.
