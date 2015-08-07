# Package

[![Ansible Galaxy](http://img.shields.io/badge/galaxy-GROG.package-660198.svg?style=flat)](https://galaxy.ansible.com/list#/roles/4689)
[![Build Status](https://travis-ci.org/GROG/ansible-role-package.svg?branch=master)](https://travis-ci.org/GROG/ansible-role-package)

A role for managing packages on different operating systems.

This role currently supports apt, yum, brew, zypper and pacman. Feel free to send a
pull or feature request to add your favorite package manager!

**Attention:**

- This role handles name differences between package managers but not between
  distributions using the same package manager.
- Test coverage is rather small so please do report bugs!

## Requirements

- Hosts should be bootstrapt for ansible usage (have python,...)
- Root privileges, eg `become: yes`

## Role Variables

| Variable | Description | Default value |
|----------|-------------|---------------|
| `package_list` | List of packages **(see details!)** | `[]` |
| `package_list_host`| List of packages **(see details!)**  | `[]` |
| `package_list_group` | List of packages **(see details!)** | `[]` |
| `package_state` | Default package state | 'present' |
| `package_update_cache` | Update the cache? | `yes` |
| `package_cache_valid_time` | How long is the package cache valid? (seconds) | 3600 |

#### `package_list` details

`package_list`, `package_list_host` and `package_list_group` are merged when
managing the packages. You can use the host and group lists to specify
packages per host or group.

The package list allows you to define which packages must be managed. By
default `item.name` will be installed/removed. If however a more specific name
is available for the current package manager (eg `item.apt`) this will be used.

You can also define the state per package (`item.state`). If not defined, the
default will be used (`package_state`).

```yaml
package_list:
  - name: package               # Package with same name in all package managers
  - name: package1              # Package with deviating state
    state: 'absent'
  - name: package2              # Package with different name in apt
    apt: package2_apt_name
  - name: package3              # Example for all supported package managers
    apt: package3_apt_name
    yum: package3_yum_name
    brew: package3_brew_name
    zypper: package3_zypper_name
    pacman: package3_pacman_name
```

## Dependencies

None.

## Example Playbook

```yaml
---
- hosts: servers
  roles:
  - { role: GROG.package,
      become: yes,
        package_list: [
          { name: htop,
            brew: htop-osx },
          { name: tree }
        ]
    }
```

## License

LGPLv3

## Contributing

All assistance, changes or ideas [welcome](https://github.com/GROG/ansible-role-package/issues)!

## Author Information

By [G. Roggemans](https://github.com/groggemans)
