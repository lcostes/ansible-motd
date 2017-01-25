motd
====

This role creates the Message Of The Day (motd) file with some additional
information about the distro and the hardware.


Example
-------

```
# This is a playlist example
- host: myhost
  # Load the role to produce the /etc/motd file
  roles:
    - motd
```

This playbook produces the `/etc/motd' file looking like this:

```
[root@localhost ~]# cat /etc/motd
     _              _ _     _
    / \   _ __  ___(_) |__ | | ___
   / _ \ | '_ \/ __| | '_ \| |/ _ \
  / ___ \| | | \__ \ | |_) | |  __/
 /_/   \_\_| |_|___/_|_.__/|_|\___|

 FQDN:    localhost.localdomain
 Distro:  CentOS 6.6 Final
 Virtual: YES

 CPUs:    1
 RAM:     0.49GB

```


Role variables
--------------

```
# Default ASCII art shown at the beginning of the motd
motd_ascii_art: |-2
       _              _ _     _
      / \   _ __  ___(_) |__ | | ___
     / _ \ | '_ \/ __| | '_ \| |/ _ \
    / ___ \| | | \__ \ | |_) | |  __/
   /_/   \_\_| |_|___/_|_.__/|_|\___|

# Whether to hide the Virtual info
motd_hide_virtual: no

# Whether to add extra new line behind the Virtual info
motd_virtual_newline: yes

# Number of initial space
motd_initial_spaces: 1

# Indent size
motd_indent: "{{ 7 if motd_hide_virtual else 8 }}"

# Default information to show under the ASCII art
motd_info__default:
  - FQDN: "{{ ansible_fqdn }}"
  - Distro: "{{ ansible_distribution }} {{ ansible_distribution_version }} {{ ansible_distribution_release }}"
  - "{{
        { '': ' ' if motd_virtual_newline else '' }
          if motd_hide_virtual
          else
        { 'Virtual': ('YES\n' if motd_virtual_newline else 'YES') if ansible_virtualization_role == 'guest' else ('NO\n' if motd_virtual_newline else 'NO') } }}"
  - CPUs: "{{ ansible_processor_vcpus }}"
  - RAM: "{{ (ansible_memtotal_mb / 1000) | round(1) }}GB"

# Custom information to show under the ASCII art
motd_info__custom: []

# Final information to show under the ASCII art
motd_info: "{{
  motd_info__default +
  motd_info__custom
}}"
```


License
-------

MIT


Author
------

Jiri Tyr
