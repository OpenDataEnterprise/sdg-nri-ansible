# SDG National Reporting Initiative Ansible Scripts

This repository contains the [Ansible](https://www.ansible.com/) scripts for the SDG National Reporting Intiative. We use Ansible to help automate the provisioning and restoration of technical infrastructure. These scripts also serve as a form of documentation on how to set up various infrastructure assets for the project.

## Requirements

You'll need to run the playbooks on a system with [Ansible installed](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) (tested with version 2.4.3.0). If you want to test locally against a [Vagrant](https://www.vagrantup.com/) box, you will need to have [Vagrant installed](https://www.vagrantup.com/docs/installation/) on your system (see the *Testing with Vagrant* section below).

## How to Use

`ansible-playbook <playbook_filepath> -i <inventory_filepath>`

This project makes heavy use of [tags](https://docs.ansible.com/ansible/latest/user_guide/playbooks_tags.html) to control which tasks are run. Of particular note should be the `always` and `never` tags:

- Tasks tagged `always` will always run unless one of the task's other tags is explicitly skipped.
- Tasks tagged `never` will only run if one of the task's other is explicitly stated.

A typical usage is to set up a new API machine to production while skipping SSL certificate registration with Let's Encrypt:

`ansible-playbook ansible/deploy_sdg_api -i ansible/inventories/production --tags production --skip-tags ssl-cert`

## Variables

Many of the tasks require certain variables to be filled out. To see which values you need to provide, check `defaults/main.yml` for each role; some of these variables will have default values, and others will be empty, requiring you to provide a value. You can fill out the default values, but it is better to copy these values into a dedicated variable file. There are many ways to provide the variables for running tasks, but the best way is to use [group variables](https://docs.ansible.com/ansible/2.3/intro_inventory.html#group-variables).

In the `group_vars` directory, simply create an extensionless file named after the inventory you wish to provide variables for. For example, if I wanted to provide variables when running against the `production` inventory, I would create the file `group_vars/production` and define my variables in there.

You can provide variables that are available for every inventory by defining them in `group_vars/all`.

## Testing with Vagrant

You can test most of the playbook roles using [Vagrant](https://www.vagrantup.com/). There is a `Vagrantfile` included in the root of the repo which you will need to configure with the playbook you want to test. The Ansible provisioning configured in the Vagrantfile is set to ignore any tasks labeled with the `production` tag, in case there are tasks that you can't test locally and need to ignore (you can learn more in the [Ansible tags documentation](https://docs.ansible.com/ansible/latest/user_guide/playbooks_tags.html)).

## Directory Layout

To understand the directory layout structure, please familiarize yourself with the [Content Organization](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html#content-organization) section of the [Best Practices](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html#content-organization) guide from the offical Ansible documentation.

## Configuration

Check the `defaults` directory under each role to get a list of the configurable variables for that role (some of the variables don't have default values and will need to be set before you can run the playbooks).

You can override the default values either by creating a `vars/main.yml` file under the corresponding role or by setting them in the proper group file under the `group_vars` directory.

For connection configurations, see the variables in the inventory files under `inventories`.

## Playbooks

There are currently two playbooks:

### jenkins_playbook.yml

This playbook installs Jenkins and restores any configurations and plugins.

### sdg_api_playbook.yml

This playbook sets up and deploys the API service backing the SDG National Reporting Initiative Website.
