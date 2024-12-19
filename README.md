
# Icinga Agent Installation and Registration Playbook

This Ansible playbook installs, configures, and registers an Icinga agent on remote hosts, ensuring the agent is correctly configured and connected to the Icinga monitoring server.

## Prerequisites

- Ansible (version 2.9 or higher)
- Icinga server (accessible from the hosts)
- Vault server for storing credentials (if applicable)
- SSH access to the target hosts
- Ansible roles and dependencies as required by the tasks

## Requirements

1. Ansible must be able to run on the target hosts with appropriate privileges.
2. The playbook retrieves SSH credentials from Vault (if applicable), or uses root access if Vault is not available.
3. The Icinga server should be accessible via port 5665.
4. The necessary Icinga templates and configuration files must be available, especially for the `icinga_host` and agent registration.

## Playbook Overview

The playbook performs the following tasks:

1. **Retrieves SSH credentials** from Vault (if configured).
2. **Checks if the Icinga server is accessible** on port 5665.
3. **Installs Icinga agent** on the target host.
4. **Configures sudoers** for Icinga operations.
5. **Checks if the host is registered on the Icinga server**; if not, it creates a new host object.
6. **Generates a registration script** and runs it to register the agent with the Icinga server.
7. **Starts and enables the Icinga agent** on the target host.

## Variables

- `icinga.example.com`: Replace with your actual Icinga server URL.
- `password`: The Icinga admin password used in HTTP basic authentication for API requests.
- `ansible_user`: The user that Ansible will use to connect to the target host.
- `ansible_become_password`: The sudo password for privilege escalation.

## Tasks

### 1. Get Credentials from Vault (if configured)
This task attempts to retrieve SSH credentials from Vault. If Vault is not configured or accessible, it falls back to root access.

### 2. Check Access to Icinga Server
The playbook checks if port 5665 is open on the Icinga server before proceeding with further configuration.

### 3. Include OS-Specific Tasks
Depending on the operating system of the target host (Ubuntu, RedHat), specific tasks for installing the Icinga agent will be included.

### 4. Create Sudoers File for Icinga Agent
The playbook creates a sudoers file (`/etc/sudoers.d/icinga`) to allow the Icinga agent to run necessary commands.

### 5. Register Host on Icinga Server
It checks if the host is already registered on the Icinga server and creates a new host object if needed.

### 6. Run Agent Registration Script
The playbook generates a registration script and runs it to register the agent with the Icinga server.

### 7. Enable and Start Icinga2 Service
Finally, the Icinga agent is started and enabled to run on system boot.

## Usage

1. Clone or copy this repository to your local machine.
2. Configure your Icinga server and Vault settings as needed.
3. Run the playbook with Ansible:

   ```bash
   ansible-playbook -i inventory.ini main.yml
   ```

Replace `inventory.ini` with your actual Ansible inventory file.

## Notes

- Ensure that the Icinga server's API is accessible from the target hosts.
- Update the Vault URL and paths as required for your environment.
- The playbook assumes the Icinga server uses HTTP basic authentication for API access.
  
## License

This project is licensed under the MIT License
