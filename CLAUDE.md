# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains Ansible playbooks for deploying MVS 3.8j TK5 (Hercules Mainframe Emulator) on Raspberry Pi devices. The deployment creates a systemd service that runs the emulator in unattended mode.

## Architecture

The playbook consists of two main plays:

1. **Setup pi**: System-level updates for the Raspberry Pi (tagged with "updates")
   - Updates package cache and upgrades system packages
   - Can be run independently or skipped using tags

2. **Setup Hercules TK5**: Mainframe emulator deployment
   - Downloads MVS TK5 from external source
   - Creates dedicated system user (`mvs`) for running the service
   - Extracts emulator files to `/opt/mvs-tk5`
   - Configures unattended mode (CONSOLE mode)
   - Deploys systemd service using `mvs.service.j2` template
   - Enables and starts the service automatically

The systemd service is configured with:
- Automatic restart on failure (10s delay)
- Journal logging with identifier `mvs-tk5`
- Runs as unprivileged `mvs` user
- Working directory set to installation path

## Configuration

All configuration is Ansible-based:
- **inventory.ini**: Target hosts (currently `preprod_pi` group)
- **group_vars/all.yml**: Global variables (`mvs_user`, `mvs_install_dir`)
- **mvs.service.j2**: Systemd service template using Jinja2 variables

Variables are templated throughout, so changes to user or install directory propagate automatically.

## Common Commands

### Run the playbook
```bash
ansible-playbook -i inventory.ini playbook.yaml
```

### Run only system updates
```bash
ansible-playbook -i inventory.ini playbook.yaml --tags updates
```

### Skip system updates
```bash
ansible-playbook -i inventory.ini playbook.yaml --skip-tags updates
```

### Manage the deployed service (on target host)
```bash
systemctl status mvs
journalctl -u mvs -f
```

## Important Notes

- The playbook uses `become: true` for privilege escalation
- Download source is hardcoded to external URL (prince-webdesign.nl)
- Service is configured for automatic restart on failure, which is important for reliability
- Logs are sent to systemd journal with identifier `mvs-tk5`
- The handler pattern is used for service restarts when the systemd unit changes
