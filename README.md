# Ansible Project: Install and Configure Nginx with Restricted Access

## Project Overview

This project uses **Ansible** to automate the installation and configuration of **Nginx** on a Linux machine. It ensures restricted access to the web server by allowing only specific IPs. The project creates a simple "Welcome to Nginx" page, sets up a custom Nginx configuration using Jinja2 templates, and validates the Nginx configuration before enabling the service.

## Features

- Installs the latest version of **Nginx**.
- Configures Nginx using a **Jinja2** template to:
  - Set a custom server name.
  - Restrict access to specified IP addresses.
- Validates Nginx configuration before applying changes.
- Starts and enables the Nginx service.
- Deploys a custom welcome page.

## Requirements

- **Ansible Controller**:
  - Installed Ansible (2.9+ recommended).
  - Python 3 installed on the managed node.
- **Managed Node**:
  - A Linux-based machine (e.g., Ubuntu).
  - SSH access configured with a valid user.

## File Structure

```
.
├── playbook.yaml       # Ansible playbook to install and configure Nginx
├── hosts               # Inventory file with target node details
├── default.j2          # Jinja2 template for Nginx configuration
```

### File Descriptions

1. **`playbook.yaml`**: Contains the tasks to:
   - Install Nginx.
   - Configure Nginx with a custom template.
   - Validate the configuration.
   - Start and enable the Nginx service.
   - Deploy the welcome page.

2. **`hosts`**: Inventory file specifying the target node. Example:
   ```ini
   node1 ansible_host=10.175.175.89 ansible_connection=ssh ansible_user=ansible-admin
   ```

3. **`default.j2`**: Jinja2 template for Nginx configuration. Example:
   ```nginx
   server {
       listen 80;
       server_name {{ server_name }};

       location / {
           root /var/www/html;
           index index.html;

           # Allow access only from specified IPs
           {% for ip in allowed_ips %}
           allow {{ ip }};
           {% endfor %}

           # Deny access by default
           deny all;
       }
   }
   ```

## How to Run

1. **Clone the Project**:
   ```bash
   git clone https://github.com/ahmedmaged6/ansible-nginx-setup
   cd ansible-nginx-setup
   ```

2. **Update the Inventory File**:
   Replace the values in `hosts` with your managed node's details.

3. **Run the Playbook**:
   ```bash
   ansible-playbook -i hosts playbook.yaml
   ```

4. **Verify the Setup**:
   - Visit the managed node's IP in your browser (e.g., `http://10.175.175.89`).
   - Ensure only the specified IPs can access the server.

## Customization

- **Change the Server Name**:
  Update the `server_name` variable in `playbook.yaml`.

- **Modify Allowed IPs**:
  Add or remove IPs in the `allowed_ips` list in `playbook.yaml`.

- **Edit Welcome Page**:
  Update the content in the `Create Welcome Page` task.

## Sample Output

After running the playbook:
- **Play Recap**:
  ```
  PLAY RECAP
  node1                      : ok=6    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
  ```
- Access the welcome page to see:
  ```html
  <html>
  <head><title>Welcome to Nginx</title></head>
  <body>
    <h1>Welcome to nginx!</h1>
  </body>
  </html>
  ```

## Notes

- This playbook uses the default Python interpreter on the managed node.
- Ensure that your node's firewall allows HTTP traffic (port 80).

---
