# Package

[![Ansible Galaxy][galaxy_image]][galaxy_link]
[![Build Status][travis_image]][travis_link]
[![Latest tag][tag_image]][tag_url]
[![Gitter chat][gitter_image]][gitter_url]

A role for managing packages on different operating systems.

This role currently supports apt, yum, brew, zypper, pacman and portage. Feel
free to send a pull or feature request to add your favorite package manager!

**Attention:**

- This role handles name differences between package managers but not between
  distributions using the same package manager.
- Test coverage is rather small so please do report bugs!

## Requirements

- Hosts should be bootstrapped for ansible usage (have python,...)
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

The package list allows you to define which packages must be managed. Each item
in the list can have following attributes:

| Variable | Description | required |
|----------|-------------|----------|
| `name` | Package name | yes |
| `state` | Package state | no |
| `apt` | Package name for apt | no |
| `apt_ignore` | Ignore package for apt | no |
| `yum` | Package name for yum | no |
| `yum_ignore` | Ignore package for yum | no |
| `brew` | Package name for brew | no |
| `brew_ignore` | Ignore package for brew | no |
| `zypper` | Package name for zypper | no |
| `zypper_ignore` | Ignore package for zypper | no |
| `pacman` | Package name for pacman | no |
| `pacman_ignore` | Ignore package for pacman | no |
| `portage` | Package name for portage | no |
| `portage_ignore` | Ignore package for portage | no |

By default `package_state` and `item.name` are used when managing the packages.
If however `item.state` is defined or a more specific package name (eg
`item.apt`) these will be used instead. If you want a package to be ignored for
some package managers you can add `***_ignore`: yes.

##### `package_list` example

```yaml
package_list:
  - name: package
  - name: package1
    state: absent
  - name: package2
    apt: package2_apt_name
  - name: package3
    apt_ignore: yes
    yum: package3_yum_name
    pacman: package3_pacman_name
    portage: package3_portage_name
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

## Contributing

All assistance, changes or ideas [welcome][issues]!

## Author

By [G. Roggemans][groggemans]

## License
MIT

[galaxy_image]:         http://img.shields.io/badge/galaxy-GROG.package-660198.svg?style=flat
[galaxy_link]:          https://galaxy.ansible.com/GROG/package
[travis_image]:         https://travis-ci.org/GROG/ansible-role-package.svg?branch=master
[travis_link]:          https://travis-ci.org/GROG/ansible-role-package
[tag_image]:            https://img.shields.io/github/tag/GROG/ansible-role-package.svg
[tag_url]:              https://github.com/GROG/ansible-role-package/tags
[gitter_image]:         https://badges.gitter.im/GROG/chat.svg
[gitter_url]:           https://gitter.im/GROG/chat

[issues]:               https://github.com/GROG/ansible-role-package/issues
[groggemans]:           https://github.com/groggemans
