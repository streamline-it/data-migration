Role Name
=========

data-migration

Migrates files from source to destination.  Multiple source/destionations can be used.  Two methods can be used for migrating the data.


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

# data-migration_mode values

#  s = Use syncrhonize module

#  c = Use copy module 
#
#  t = Use created shell script to migrate the data

data-migration_mode: s

# Where script exists to be executed
data_script_destination: /root/copy_shell_script.sh

# Where data is mounted
data-migration_source_mountpoint:

# list of sources and distinations to migrate files from/to
data-migration_directories:
  - src: "{{ data-migration_source_mountpoint }}/"
    dest:
  - src: "{{ data-migration_source_mountpoint }}/"
    dest:


Dependencies
------------

ansible.posix collection

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers

      vars:

        data-migration_mode: c

        migration_directories:
          - src: "{{ data-migration_source_mountpoint }}/pub"
            dest: /data/
          - src: "{{ data-migration_source_mountpoint }}/priv"
            dest: /data

      roles:
         - name: data-migration

License
-------

BSD

Author Information
------------------

N/A

