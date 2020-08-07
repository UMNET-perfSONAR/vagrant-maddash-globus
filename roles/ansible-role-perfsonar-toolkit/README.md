perfSONAR-toolkit Ansible role
==============================

This roles installs and configure a perfSONAR toolkit.

Requirements
------------

This role is meant to work with any perfSONAR supported distro:

  - CentOS: http://docs.perfsonar.net/install_centos.html
  - Debian/Ubuntu: http://docs.perfsonar.net/install_debian.html

The hosts must be manageable through Ansible including access to some Ansible modules.  We recommend that you bootstrap Ansible on your hosts prior to running this role.  This can be done manually or through roles provided by [DebOps][debops] or some bootstrap roles available on Ansible Galaxy like [robertdebock.bootstrap][rdbs] as a very first role.

Role Variables
--------------

In addition to the variables in ansible-perfsonar-role-testpoint, the following variables can/should be defined for your host setup:

  - `perfsonar_web_user` defaults to psadmin
  - `perfsonar_web_passwd` for the web UI

  - Some other variables are defined at the end of `default/main.yml` and in `vars/Debian.yml` and `vars/RedHat.yml` (those contains distro specific settings), but shouldn't need to be altered for a regular install.

Role Tags
---------

Some tags are used in the role, they are meant to run only or skip part of the process.  The following tags are existing:

  - `ps::install` : only install perfSONAR packages and their dependencies
  - `ps::config` : only configure any already installed perfSONAR package
  - `ps::running` : checks that your perfSONAR node is running as intended

Examples,

  - If you only want to install the perfSONAR software without configuring it automatically, you can run:

        ansible-playbook site.yml --tags "ps::install"

  - If you have already installed the perfSONAR packages and you only want to change an already existing config, you can run:

        ansible-playbook site.yml --skip-tags "ps::install"

Dependencies
------------

This role depends on the perfsonar-testpoint role.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - role: perfsonar-toolkit

License
-------

Apache 2.0

Author Information
------------------

This role is provided by the perfSONAR team.  See http://www.perfsonar.net and https://github.com/perfsonar/ for more information.


[debops]: https://debops.org/
[rdbs]: https://galaxy.ansible.com/robertdebock/bootstrap/
[debian-optional]: http://docs.perfsonar.net/install_debian.html#optional-packages
[centos-optional]: http://docs.perfsonar.net/install_centos.html#optional-packages
