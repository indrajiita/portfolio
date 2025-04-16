# CryoSPARC Administrator Guide
---

## Table of Contents

  - [Scope](#scope)
  - [Environment](#environment)
  - [Procedures](#procedures)
    - [Renew SSL Certificate](#renew-ssl-certificate)
    - [Restart CryoSPARC](#restart-cryosparc)
    - [Backup CryoSPARC Database](#backup-cryosparc-database)
    - [Maintenance Mode](#maintenance-mode)
    - [Full Shutdown](#full-shutdown)
    - [Manual Update](#manual-update)
    - [Install Manual Cluster Worker Updates](#install-manual-cluster-worker-updates)
    - [Create User via Web GUI](#create-user-via-web-gui)

## Scope

This document provides guidelines for updating, restarting, and configuring CryoSPARC, a scientific software platform used in cryo-electron microscopy (cryo-EM) research and data processing pipelines.


## Environment

- **Location**: On-prem (worker)/Cloud (server)
- **Deployment Type**: Manual
- **Cloud Instance**: i-0123456789abcdef (cryosparc-prod)
- **Access**:
  - Hostname: cryo-hpc-node1
  - IP: 10.0.0.1
  - Admin Panel: https://cryosparc.example.org

## Procedures

### Renew SSL Certificate

#### Prerequisites
Review the internal guide on requesting new SSL certificates.

#### Steps
1. SSH into the server (e.g., `ssh admin@cryo-hpc-node1`).
2. Open the configuration file `/etc/nginx/conf.d/cryosparc.conf`.
3. Update `ssl_certificate` and `ssl_certificate_key` paths.
4. Restart Nginx: `systemctl restart nginx`.

### Restart CryoSPARC

1. Restart CryoSPARC by running the following commands as root:
```bash
systemctl stop cryosparc-supervisor.service
systemctl start cryosparc-supervisor.service
systemctl status cryosparc-supervisor.service
```
2. Verify the services:
```bash
/apps/cryosparc_master/bin/cryosparcm status
```

### Backup CryoSPARC Database

1. Use `mongodump` via the CryoSPARC CLI:
```bash
cryosparcm backup --dir=/backup/location --file=backup_filename.archive
```
2. Check CRYOSPARC_DB_PATH in `config.sh` for the default path.

### Maintenance Mode

Maintenance mode prevents new job submissions.

To enable the maintenance mode, run the following commands:
```bash
cryosparcm maintenancemode on
cryosparcm maintenancemode status
```

To disable the maintenance mode, run the following commands:
```bash
cryosparcm maintenancemode off
cryosparcm maintenancemode status
```

**Note:** SLURM job failure may indicate a bad worker path; verify the settings via the web UI.

### Full Shutdown

1. Stop the service:
```bash
systemctl stop cryosparc-supervisor.service
```
2. Identify remaining processes:
```bash
ps -weo pid,ppid,start,cmd | grep -E 'cryosparc|mongo' | grep -v grep
```
3. Kill any remaining relevant processes using `kill <PID>`.
4. Re-run `ps` to confirm all are terminated.

### Manual Update

**Before Update:**
- Notify users.
- Enable maintenance mode.
- Wait for active jobs to complete, or terminate them manually if needed.
- Backup the database.
- Perform a full shutdown.

**Check for updates:**
```bash
cryosparcm update --check
cryosparcm update --list
```

**Run the update:**
```bash
cd /apps/cryosparc_master/
/apps/cryosparc_master/bin/cryosparcm update
```

### Install Manual Cluster Worker Updates

1. Stop the instance:
```bash
cryosparcm stop
```
2. Copy worker package to GPU node:
```bash
cp /apps/cryosparc_master/cryosparc_worker.tar.gz /usr/local/cryosparc_worker/
```
3. SSH to GPU node and update:
```bash
cd /usr/local/cryosparc_worker/
bin/cryosparcw update
```
4. Restart master:
```bash
systemctl start cryosparc-supervisor.service
cryosparcm maintenancemode off
```

### Create User via Web GUI

1. Click <img src="https://github.com/indrajiita/portfolio/blob/main/Technical%20Writing/media1/key_icon.png?raw=true" width="17">.
2. Navigate to **User Management**.
3. Fill out the form and submit it.
4. Provide the user with the registration token.
