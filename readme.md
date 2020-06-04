# ansible-role-disk-volume

Ansible role for managing disk volumes

## Usage example

Create requirements file `requirements.yml`

```yml
---
  - name: disk-volume
    src: git@github.com:tinslice/ansible-role-disks.git
```

Install role

```bash
ansible-galaxy install -f -r local-requirements.yml -p roles/
```

Create playbook file `playbook.yml`

```yml
---
- hosts: "{{ target_host }}"
  vars:
    cloud_provider: gcp # cloud provider (defaults to 'gcp'). For now only 'gcp' support is implemented 
    disk-volumes:
      - name: mydisk                   # disk name
        mount: /mnt/diskmount          # mount location
        state: present                 # disk state                        | possible values: [ 'present', 'absent' ]
        unit: KiB                      # default unit used by parted command (default 'KiB')
        fstype: ext4                   # file system type (default 'ext4') | possible values: [ 'aix', 'amiga', 'bsd', 'dvh', 'gpt', 'loop', 'mac', 'msdos', 'pc98', 'sun' ]
        force: false                   # create new filesystem on devices that already has filesystem (default false)
        resizefs: true                 # if size differ, grow the filesystem into the space (default true)
        fsopts: []                     # list options for mkfs (default omit)
        disable_periodic_fsck: false   # enable periodic fsck check (default false)
        user: root                     # mount location user (default 'root')
        group: syslog                  # mount location group (default 'root')
        mount_state: mounted           # mount state (default item.state)  | possible values: [ 'absent', 'mounted', 'present', 'unmounted' ]
        mount_options: defaults,nofail # mount options (default 'defaults,nofail') 
  become: yes
    roles:
      - disks
```

Run playbook

```bash
ansible-playbook playbook.yml -i <inventory> -e target_host=<target-host> 
```

Resources:

- <https://docs.ansible.com/ansible/latest/modules/parted_module.html>
- <https://docs.ansible.com/ansible/latest/modules/filesystem_module.html>
- <https://docs.ansible.com/ansible/latest/modules/mount_module.html>
