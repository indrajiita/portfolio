# CryoSPARC Administrator Guide
---

## Scope

This document provides guidelines for updating, restarting, and configuring CryoSPARC, a scientific software platform used in cryo-electron microscopy (cryo-EM) research and data processing pipelines.


## Environment

- **Location**: On-premises (worker) / Cloud (server)
- **Deployment Type**: Manual
- **Cloud Instance**: i-0123456789abcdef (cryosparc-prod)
- **Access**:
  - Hostname: cryo-hpc-node1
  - IP: 10.0.0.1
  - Admin Panel: https://cryosparc.example.org

## Procedures

### Renew SSL Certificate

#### Prerequisites
Review your internal guide on requesting new SSL certificates.

#### Steps
1. SSH into the server (e.g., `ssh admin@cryo-hpc-node1`).
2. Open the configuration file `/etc/nginx/conf.d/cryosparc.conf`.
3. Update `ssl_certificate` and `ssl_certificate_key` paths.
4. Restart Nginx: `systemctl restart nginx`.

### Restart CryoSPARC

Commands (run as root):
```bash
systemctl stop cryosparc-supervisor.service
systemctl start cryosparc-supervisor.service
systemctl status cryosparc-supervisor.service
```
Check services:
```bash
/apps/cryosparc_master/bin/cryosparcm status
```

### Backup CryoSPARC Database

Use `mongodump` via the CryoSPARC CLI:
```bash
cryosparcm backup --dir=/backup/location --file=backup_filename.archive
```

Check CRYOSPARC_DB_PATH in `config.sh` for the default path.

### Maintenance Mode

Enable to prevent new job submissions:
```bash
cryosparcm maintenancemode on
cryosparcm maintenancemode status
cryosparcm maintenancemode off
```

**Note:** SLURM job failure may indicate a bad worker path; verify via the web UI settings.

### Complete Shutdown

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
- Wait for or kill active jobs.
- Backup the database.
- Perform full shutdown.

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

### Create User via CLI

Create user:
```bash
cryosparcm createuser \
  --email "user@example.com" \
  --username "username" \
  --firstname "First" \
  --lastname "Last" \
  --password "password"
```

### Create User via Web GUI

1. Click the key icon.
2. Navigate to **User Management**.
3. Fill out the form and submit.
4. Provide the user with the registration token.