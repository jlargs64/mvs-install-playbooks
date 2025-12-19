# Gameframe Pi Lite Playbooks

Ansible playbooks for deploying MVS 3.8j TK5 (Hercules Mainframe Emulator) on
Raspberry Pi.

## Prerequisites

- Ansible installed on your local machine
- SSH access to the target Raspberry Pi
- Target host configured in `inventory.ini` (copy from `inventory.ini.example` and update with your values)

## Running the Playbook

### Run all tasks

```bash
ansible-playbook -i inventory.ini playbook.yaml
```

### Run only update tasks

```bash
ansible-playbook -i inventory.ini playbook.yaml --tags updates
```

### Skip update tasks

```bash
ansible-playbook -i inventory.ini playbook.yaml --skip-tags updates
```

## Managing the MVS Service

### Check service status

```bash
systemctl status mvs
```

### Start the service

```bash
sudo systemctl start mvs
```

### Stop the service

```bash
sudo systemctl stop mvs
```

### Restart the service

```bash
sudo systemctl restart mvs
```

### Enable service at boot

```bash
sudo systemctl enable mvs
```

### Disable service at boot

```bash
sudo systemctl disable mvs
```

## Viewing Logs

### View recent logs

```bash
journalctl -u mvs
```

### Follow logs in real-time

```bash
journalctl -u mvs -f
```

### View logs since last boot

```bash
journalctl -u mvs -b
```

### View logs with timestamps

```bash
journalctl -u mvs -o short-iso
```

### View last N lines

```bash
journalctl -u mvs -n 50
```

### View logs for specific time range

```bash
journalctl -u mvs --since "2025-01-01" --until "2025-01-02"
```

### Connect to the terminal

Using your favorite terminal emulator to connect to your new maineframe! I use
x3270 on mac, which uses the c3270 command like so:
`c3270 -defaultfgbg <your-pi-ip>:3270`.

The default port is 3270 which can be changed in the
`{{ mvs_install_dir }}/conf/tk5.conf` file.

## Configuration

- **Service name**: mvs
- **Install directory**: `/opt/mvs-tk5`
- **Service user**: mvs
- **Target hosts**: defined in `inventory.ini`
- **Variables**: see `group_vars/all.yml`
