# ansible-role-disk-volume

Ansible role for managing disk volumes

Role configuration:

- `disk-volumes`

  - `name` [required] disk name
  - `mount` [required] mount location
  - `state` [required] disk state. Values: [ 'present', 'absent' ]
  - `unit` [optional] default unit used by parted command ( defaults to 'KiB' )
  - `fstype` [optional] file system type (defaults to 'ext4'). Values: [ 'aix', 'amiga', 'bsd', 'dvh', 'gpt', 'loop', 'mac', 'msdos', 'pc98', 'sun' ]
  - `force` [optional] force creation of new filesystem on devices that already has filesystem (defaults to false)
  - `resizefs` [optional] if size differ, grow the filesystem into the space (defaults to true)
  - `fsopts` [optional] list of options for mkfs
  - `disable_periodic_fsck` [optional] enable periodic fsck check (defaults to false)
  - `user` [optional] Owner of the mount location (defaults to 'root')
  - `group` [optional] Group owner of the mount location (defaults to 'root')
  - `mount_state` [optional] mount state (defaults to the same value as `state`). Values: [ 'absent', 'mounted', 'present', 'unmounted' ]
  - `mount_options` [optional] mount options (defaults to 'defaults,nofail')
  - `mount_mode` [optional] mount mode (defaults to 'by-id'). Values: [ 'by-id', 'uuid' ]

## Usage example

Create requirements file `requirements.yml`

```yml
---
  - name: disk-volume
    src: https://github.com/tinslice/ansible-role-disk-volume.git
```

Install role

```bash
ansible-galaxy install -f -r requirements.yml -p roles/
```

Create playbook file `playbook.yml`

```yml
---
- hosts: "{{ target_host }}"
  vars:
    disk-volumes:
      - name: mydisk 
        mount: /mnt/diskmount 
        state: present 
        fstype: ext4 
        resizefs: true  
        mount_state: mounted 
        mount_options: defaults,nofail  
  become: yes
    roles:
      - disk-volume
```

Run playbook

```bash
ansible-playbook playbook.yml -i <inventory> -e target_host=<target-host> 
```

Resources:

- <https://docs.ansible.com/ansible/latest/modules/parted_module.html>
- <https://docs.ansible.com/ansible/latest/modules/filesystem_module.html>
- <https://docs.ansible.com/ansible/latest/modules/mount_module.html>
