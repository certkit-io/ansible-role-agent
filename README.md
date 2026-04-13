# Ansible Role: certkit_io.agent

An Ansible role for Linux that installs and configures [`certkit-agent`](https://github.com/certkit-io/certkit-agent). This allowed automated deployment of Certkit managed certificates across many machines.

Published in Ansible Galaxy here: [certkit_io.agent](https://galaxy.ansible.com/ui/standalone/roles/certkit_io/agent/).

## Overview

- Installs `certkit-agent` to `/usr/local/bin/certkit-agent`.
- Downloads the latest release by default from [certkit-agent releases](https://github.com/certkit-io/certkit-agent/releases).
- Will update certkit-agent to newest version by default.
  - Locking to a specific version is supported.
  - By default, the role will not downgrade to an older agent version.
- Registers newly installed agents with your Certkit account.

## Requirements

- Linux host with systemd.
- Internet access from managed host to:
  - `api.github.com`
  - `github.com`
  - `app.certkit.io`

## Role Variables

- `certkit_registration_key` (required): Registration key from [Certkit](https://app.certkit.io).
  - Looks like `abcd.xyz123`
- `certkit_service_name` (optional): Service name used by `certkit-agent install`.
  - Default: `certkit-agent`
- `certkit_config_path` (optional): Config path used by `certkit-agent install`.
  - Default: `/etc/certkit-agent/config.json`
- `certkit_agent_version` (optional): Version selector to install.
  - Default: `latest`
  - Accepted formats: `latest`, `v1.5.0`, or `1.5.0`
- `certkit_allow_downgrade` (optional): Allow downgrading when the requested version is lower than the installed version.
  - Default: `false`
- `certkit_force_reregister` (optional): Force agent re-registration by deleting existing config and running `certkit-agent register`. In general, this should not be needed. Only use if you need to replace an invalid registration key.
  - Default: `false`

## Example Playbook

```yaml
- hosts: all
  become: true
  tasks:
    - include_role:
        name: certkit_io.agent
      vars:
        certkit_registration_key: "abcd.xyz123"
```

Lock to a specific version:

```yaml
- hosts: all
  become: true
  tasks:
    - include_role:
        name: certkit_io.agent
      vars:
        certkit_registration_key: "abcd.xyz123"
        certkit_agent_version: "v1.5.0"
```

## Notes

- This role mirrors the Linux installation steps from [`certkit-agent/scripts/install.sh`](https://github.com/certkit-io/certkit-agent/blob/main/scripts/install.sh). The end result should be the same, but automated with Ansible.

## License

MIT
