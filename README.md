# Apache Web Server Project - Using RHEL, Ansible

This project demonstrates a simple **Apache web server setup** on **RHEL** using **Ansible**. It serves a basic `index.html` page hosted on Apache, which is managed and deployed using Ansible automation.

## Setup

### Step 1: Install Required Packages on RHEL

Before setting up Apache and Ansible, you need to install the required packages on RHEL.

1. **Update the system packages**:
   ```bash
   sudo dnf update -y
   ```

2. **Install Apache HTTP Server**:
   ```bash
   sudo dnf install httpd -y
   ```

3. **Start and Enable Apache**:
   ```bash
   sudo systemctl start httpd
   sudo systemctl enable httpd
   ```

4. **Install Ansible**:
   ```bash
   sudo dnf install ansible-core -y
   ```

5. **Verify Ansible Installation**:
   ```bash
   ansible --version
   ```

### Step 2: Create the Ansible Playbook for Apache Installation

We use an **Ansible playbook** to automate the installation and configuration of Apache. This ensures consistent deployment and reduces manual setup time.

1. **Create the `webserver.yml` Playbook**:
   ```bash
   sudo nano ~/ansible/webserver.yml
   ```

2. **Add the following content to `webserver.yml`**:
   ```yaml
   - name: Install and configure Apache
     hosts: localhost
     become: yes
     tasks:
       - name: Install Apache
         dnf:
           name: httpd
           state: present
       - name: Start and enable Apache
         systemd:
           name: httpd
           state: started
           enabled: yes
       - name: Allow HTTP traffic through firewall
         firewalld:
           service: http
           permanent: yes
           state: enabled
       - name: Reload firewall
         command: firewall-cmd --reload
   ```

3. **Run the Ansible Playbook**:
   ```bash
   ansible-playbook ~/ansible/webserver.yml
   ```

### Step 3: Create a Custom `index.html` Page

Once Apache is installed and running, create a custom **`index.html`** page in the default web directory.

1. **Create the `index.html` File**:
   The `index.html` file is placed in `/var/www/html/` (Apache’s default directory for web content).

   Example content for `index.html`:
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Welcome to Apache!</title>
   </head>
   <body>
       <h1>Welcome to Apache on RHEL!</h1>
       <p>This website is hosted on Apache!</p>
   </body>
   </html>
   ```

### Step 4: Start Apache and Enable It to Run on Boot

1. **Start Apache**:
   To start Apache, use the following command:
   ```bash
   sudo systemctl start httpd
   ```

2. **Enable Apache to Start on Boot**:
   To ensure Apache starts on boot:
   ```bash
   sudo systemctl enable httpd
   ```

### Step 5: Configure the Firewall

To allow HTTP traffic through the firewall, run the following commands:

```bash
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload
```

### Step 6: Access the Apache Server

Once Apache is set up and running, you can access the web page by navigating to the IP address of the server in a browser:
```
http://your-server-ip/
```

### Files Included:
- **`webserver.yml`**: Ansible playbook used to install and configure Apache.
- **`index.html`**: Simple webpage served by Apache, placed in `/var/www/html/`.
- **`httpd.conf`**: Apache configuration file (optional, depending on your setup).

### Future Improvements:
- Set up **virtual hosts** to host multiple websites on the same server.
- Add **SSL encryption** using Let’s Encrypt to enable HTTPS.
- Customize the Apache configuration further, such as enabling PHP or additional security features.

