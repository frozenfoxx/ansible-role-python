# ansible-role-python

Ansible role to install and manage a system installation of [Python](https://www.python.org).

## Requirements

### Supported Operating Systems

- Debian (Bullseye, Bookworm)
- Ubuntu (Focal, Jammy, Noble)
- Arch Linux

### Ansible Version

- Minimum Ansible version: 2.9

### Collections

For Arch Linux support, ensure you have the `community.general` collection installed:

```bash
ansible-galaxy collection install community.general
```

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
# Debian/Ubuntu specific packages
python_debian_packages:
  - python3
  - python3-pip
  - python3-venv

# Arch Linux specific packages
python_archlinux_packages:
  - python
  - python-pip

# Additional system packages to install (OS-agnostic)
# Override this in your playbook for OS-specific packages
python_system_packages: []

# Python version preferences (optional)
# Leave empty to use distribution default
python_version: ""

# Whether to ensure pip is upgraded to latest
python_upgrade_pip: false
```

### Per-OS Overrides

The role loads OS-specific variable files from `vars/` via `first_found`, in this order of precedence:

1. `vars/<distribution>-<version>.yml` (e.g. `ubuntu-22.04.yml`)
2. `vars/<distribution>-<major_version>.yml` (e.g. `debian-12.yml`)
3. `vars/<distribution>.yml` (e.g. `ubuntu.yml`)
4. `vars/<os_family>.yml` (e.g. `debian.yml`, `archlinux.yml`)
5. `vars/default.yml` (fallback)

The shipped files are empty placeholders. Drop overrides into the appropriate file when a distribution or version needs different values than `defaults/main.yml` provides.

## Dependencies

None

## Usage

### Basic Usage

```yaml
- hosts: servers
  roles:
    - role: frozenfoxx.python
```

### Custom Additional Packages

```yaml
- hosts: servers
  roles:
    - role: frozenfoxx.python
      vars:
        python_system_packages:
          - python3-requests
          - python3-yaml
        python_upgrade_pip: true
```

### OS-Specific Package Installation

For Debian/Ubuntu systems:

```yaml
- hosts: debian_servers
  roles:
    - role: frozenfoxx.python
      vars:
        python_system_packages:
          - python3-dev
          - python3-setuptools
```

For Arch Linux systems:

```yaml
- hosts: arch_servers
  roles:
    - role: frozenfoxx.python
      vars:
        python_system_packages:
          - python-virtualenv
          - python-setuptools
```

## Tags

This role supports the following tags:

- `python` - Applied to all tasks

You can run only Python installation tasks:

```bash
ansible-playbook playbook.yml --tags python
```

## License

Apache-2.0

## Author Information

This role was created by frozenfoxx.
