# Scope
This procedure outlines the steps required to change the NFS permissions on a WEKA filesystem in an HPC cluster, allowing for access from non-cluster nodes (for example, cloud instances).

# Approval
| Role         | Person Responsible |
|-------------|-------------------|
| **Author**  | John Doe  |
| **Reviewer** | Andrei Orlov |
| **Approver** | Jane Smith |

# Definitions & Acronyms
| Term/Acronym | Definition |
|--------------|------------|
| GUI |  Graphical User Interface. A visual interface that allows users to interact with software using graphical elements like buttons and menus. |
| CLI |  CLI | Command-Line Interface. A text-based interface that lets users interact with software by typing commands. |
| ADM Account |  An administrator-level user account with elevated permissions for managing system settings. |

# Responsibilities
The HPC Support Team performs this procedure, periodically reviews and updates it as necessary, and maintains proper support documentation. Specific responsibilities are defined in the table below.

| Role/Group | Responsibilities |
|------------|-----------------|
| HPC Support Engineers | Perform the procedure. |
| Technical Writer | Review and maintain documentation. |

# Prerequisites
N/A

# Procedure

## Justification
It may be necessary to grant access to shared filesystems on non-cluster nodes. By default, the NFS protocol restricts access to unknown hosts or networks, blocking these systems from mounting shares. This process ensures that the necessary permissions are granted.

## Communication Plan
Not required as the process does not involve service interruption.

## Implementation Plan
There are two methods to modify permissions in WEKA: the **GUI** and the **CLI**.

### Method 1: Using the GUI
1. Access the WEKA GUI: [http://weka.sample.com:14001/ui](http://weka.sample.com:14001/ui).
2. Log in with your ADM account.
3. Go to **Manage > Protocols**.<br><img src="https://github.com/indrajiita/test/blob/main/Technical%20Writing/media1/Weka1.png?raw=true" width="400">
4. On the sidebar, select **NFS** and click **Permissions**.<br><img src="https://github.com/indrajiita/test/blob/main/Technical%20Writing/media1/Weka2.png?raw=true" width="400">
5. Since clients are not separated into groups, all clients are listed under the **sample-cluster** group.
6. Click on either **Add DNS** or **Add IP**, depending on your requirements.<br><img src="https://github.com/indrajiita/test/blob/main/Technical%20Writing/media1/Weka3.png?raw=true" width="200">
7. Complete the form with the appropriate **hostname or IP address**. Ensure the correct **network mask** is set if adding a network.
8. Click **Save**.
9. Verify that the changes have been applied by reviewing the updated list of clients.

### Method 2: Using the CLI
1. SSH into any WEKA node with an ADM account:
   ```bash
   ssh sample-weka-node-001
   ```
2. Log into WEKA CLI:
   ```bash
   weka user login
   ```
3. Enter your ADM username and password when prompted.
4. Depending on your requirements, use one of the following commands to add a new client or network:
- Add by DNS:
   ```bash
   weka nfs rules add dns sample-cluster client-hostname
   ```
- Add by IP:
   ```bash
   weka nfs rules add ip sample-cluster 192.168.1.1/255.255.255.0
   ```
5. Confirm that the new client or network has been added. Run either of the following commands:
- For DNS:
   ```bash
   weka nfs client-group | grep client-hostname
   ```
- For IP:
   ```bash
   weka nfs client-group | grep client-ip
   ```

# Backup Plan
Not required as the process does not involve service interruption.

# References
N/A

# Training
All administrators must receive the appropriate level of training required to change permissions on WEKA NFS shared filesystems in accordance with their assigned roles.

# Attachments
N/A

# Additional Details
N/A
