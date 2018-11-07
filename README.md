This role is used to setup [Trackster][trackster] genome browser in Galaxy.

Requirements
------------
The role has been developed and tested on Ubuntu 14.04 and 16.04. It uses
`sudo` (but could easily be adjusted not to rely on it).

Variables
---------
If using this role in the context of [Galaxy CloudMan][cm], note that some of these
variables need to match equally named ones from the [`ansible-cloudman`][acm]
role.

 - `galaxy_manage_trackster`: (default: `yes`) if set, the role will run
 - `galaxy_user_name`: (default: `galaxy`) system username used for Galaxy
 - `galaxyFS_base_dir`: (default: `/mnt/galaxy`) the base path under which the
    galaxy file system is planned to be placed
 - `len_file_path`: (default: `"{{ galaxyFS_base_dir }}/galaxy-app/config/len"`)
    the default location where Trackster's `.len` files should be placed
 - `bedtools_version`: (default: `2.25.0`) the version of BED tools to download
    and `make`
 - `galaxy_custom_tools_dir`: System path where Trackster utility tools will be
    installed. This path should be on the `$PATH`.

Dependencies
------------
None.

Example Playbook
----------------
To use the role, wrap it into a playbook as follows (the following assumes the
role has been placed into directory `roles/trackster`):

    - hosts: galaxyFS-builder
      become: yes
      vars:
        galaxy_custom_tools_dir: /usr/local/bin
        galaxyFS_base_dir: /home/galaxy/galaxy
        len_file_path: "{{ galaxyFS_base_dir }}/config/len"
      pre_tasks:
        - name: Assure galaxyFS dir exists
          file: path={{ galaxyFS_base_dir }} state=directory owner={{ galaxy_user_name }} group={{ galaxy_user_name }}
          become_user: root
      roles:
        - role: trackster

Next, create a `hosts` file:

    [galaxyFS-builder]
    130.56.250.204 ansible_ssh_private_key_file=key.pem ansible_ssh_user=ubuntu

Finally, run the playbook as follows:

    $ ansible-playbook playbook.yml -i hosts

[trackster]: https://wiki.galaxyproject.org/Learn/Visualization
[cm]: https://wiki.galaxyproject.org/CloudMan/
[acm]: https://github.com/galaxyproject/ansible-cloudman
