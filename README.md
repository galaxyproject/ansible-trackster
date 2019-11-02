This role is used to setup [Trackster][trackster] genome browser in Galaxy.

Requirements
------------
The role has been developed and tested on Ubuntu 14.04 and 16.04. It uses
`sudo`

Variables
---------

 - `galaxy_manage_trackster`: (default: `yes`) if set, the role will run
 - `galaxy_user_name`: (default: `galaxy`) system username used for Galaxy
 - `galaxy_server_dir`: (default: `/home/galaxy/galaxy`) the base path under which the
    galaxy file system is planned to be placed
 - `bedtools_version`: (default: `2.28.0`) the version of BED tools to download
    and `make`
 - `galaxy_custom_tools_dir`: System path where Trackster utility tools will be
    installed. This path should be on the galaxy `$PATH` (default: `/home/galaxy/galaxy/.venv/bin`)

Dependencies
------------
None.

Example Playbook
----------------
To use the role, wrap it into a playbook as follows (the following assumes the
role has been placed into directory `roles/trackster`):

    - hosts: galaxy-builder
      become: yes
      vars:
        galaxy_custom_tools_dir: /home/galaxy/galaxy/.venv/bin
        galaxy_server__dir: /home/galaxy/galaxy
      pre_tasks:
        - name: Assure galaxy dir exists
          file: path={{ galaxy_server_dir }} state=directory owner={{ galaxy_user_name }} group={{ galaxy_user_name }}
          become_user: root
      roles:
        - role: trackster

Next, create a `hosts` file:

    [galaxy-builder]
    130.56.250.204 ansible_ssh_private_key_file=key.pem ansible_ssh_user=ubuntu # substitute with you target ip

Finally, run the playbook as follows:

    $ ansible-playbook playbook.yml -i hosts

[trackster]: https://wiki.galaxyproject.org/Learn/Visualization
[cm]: https://wiki.galaxyproject.org/CloudMan/
[acm]: https://github.com/galaxyproject/ansible-cloudman
