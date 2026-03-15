Role Name
=========

data-migration

Migrates files from source to destination.  Multiple source/destionations can be used.  Three methods can be used for migrating the data.


Set data-migration_mode = 


s - Uses ansible.posix.synchronize module.  Make sure data is mounted to the ansible control node.

c - Uses ansible.builtin.copy module.  Be sure the data is mounted on the remote host

t - Uses the created shell script


data-migration_mountpoint

Set the data-migration_source_mountpoint to the location of the mountpoint of the data to migrate.

Requirements
------------

Make sure the rsync package exists on the control node if you're using rsync to migrate the data via network to the host.

Role Variables
--------------

Default variables

# data-migration_mode values

#  s = Use syncrhonize module

#  c = Use copy module 
#
#  t = Use created shell script to migrate the data

data-migration_mode: s

data-migration_script_filename: copy_shell_script.sh

# Script filepath
data_migration_script_destination: "/root/{{ data_migration_script_filename }}"

# Where data is mounted
data_migration_source_mountpoint:

# list of sources and distinations to migrate files from/to
data_migration_directories:
  - src: "{{ data-migration_source_mountpoint }}/"
    dest:
    owner:
    group
  - src: "{{ data-migration_source_mountpoint }}/"
    dest:
    owner:
    group:

vars

FreeBSD.yml:
data_migration_shebang: "#!/bin/sh"


RedHat.yml

data_migration_shebang: "#!/bin/bash"
Dependencies
------------

ansible.posix collection

Example Playbook
----------------

Here's an example using copy mode

* Note the "my_source_location" must exist on the remote host.  This allows this
  copy operation to be as fast as possible.

- hosts: node5.example.com
  
  vars:

    my_source_location: /home/migration_directory

    data_migration_mode: c
    data_migration_source_mountpoint: "{{ my_source_location }}"
    data_migration_destination_dirs:
      - /home/data

    data_migration_directories:
      - src: "{{ data_migration_source_mountpoint }}"
        dest: "/home/data"
        owner: rich
        group: rich


  roles:
 
    - name:  data-migration

Example using synchronize mode

* Note the "my_source_location" must exist on the ansible controller.

- hosts: servers

  vars:

    my_source_location: /mnt/nfs/priv/ansible/data
 
    data_migration_mode: s
    data_migration_source_mountpoint: "{{ my_source_location }}"
    data_migration_destination_dirs:
      - /home/data/sync

    data_migration_directories:
      - src: "{{ data_migration_source_mountpoint }}"
        dest: "/home/data/sync"
        owner: rich
        group: rich

  roles:

    - name: data-migration


Example using script mode

- hosts: servers

  vars:


    my_source_location: /home/migration_directory

    data_migration_mode: t
    data_migration_script_file_name: script_copy.sh
    data_migration_script_destination: "/root/{{ data_migration_script_file_name }}"

    data_migration_source_mountpoint: "{{ my_source_location }}"
    data_migration_destination_dirs:
      - /home/data/script

    data_migration_directories:
      - src: "{{ data_migration_source_mountpoint }}"
        dest: "/home/data/script"
        owner: rich
        group: richoles:

  roles:

    - name: data-migration

License
-------

BSD

Author Information
------------------

rich ohara
