# Odoo Docker Setup with Custom and Enterprise Addons

## Overview
This guide provides step-by-step instructions for setting up an Odoo instance with Docker, including custom and enterprise addons.

## Project Structure
Create your project folder on your Windows PC with the following structure:
```
project-root/
│
├── docker-compose.yml # Docker Compose configuration file
├── odoo.conf # Custom Odoo configuration file
│
├── addons/ # Directory for custom Odoo addons
├── enterprise-addons/ # Directory for Odoo Enterprise addons
```


## Steps to Setup
### Step 1: Create Project Folder

1. Create a folder named `project-root` on your Windows PC.
2. Inside `project-root`, create the following subdirectories and files:
   - `docker-compose.yml`
   - `odoo.conf`
   - `addons/` (for custom addons)
   - `enterprise-addons/` (for enterprise addons)

### Step 2: Open Command Prompt or PowerShell

1. Press `Win + R`, type `cmd` or `powershell`, and press Enter.
2. Navigate to the `project-root` directory:

```shell
   cd C:\project-root
```

### Step 3: Clone Odoo Enterprise Git Repository
1. Ensure you have Git installed and configured on your Windows machine.
2. Clone the Odoo Enterprise repository into the `enterprise-addons` directory:
```shell
   git clone git@github.com:odoo/enterprise.git enterprise-addons
```

> [!TIP]
> Place your custom addons in the `addons/` directory inside the `project-root/`.


### Step 4: Create `docker-compose.yml`
Create a `docker-compose.yml` file inside the `project-root/` directory with the following content:
```yaml
version: '3.8'

services:
  db:
    image: postgres:15
    environment:
      POSTGRES_USER: odoo
      POSTGRES_PASSWORD: odoo
      POSTGRES_DB: postgres
    volumes:
      - odoo-db:/var/lib/postgresql/data
    networks:
      - odoo-network

  odoo:
    image: odoo
    ports:
      - "8069:8069"
    volumes:
      - C:/project-root/addons:/mnt/extra-addons  # Custom addons
      - C:/project-root/enterprise-addons:/mnt/enterprise-addons  # Enterprise addons
      - C:/project-root/odoo.conf:/etc/odoo/odoo.conf  # Custom config file
    depends_on:
      - db
    networks:
      - odoo-network

networks:
  odoo-network:

volumes:
  odoo-db:
```


### Step 5: Create/Update Configuration File (`odoo.conf`)
Ensure your `odoo.conf` file includes the necessary configurations for addons:
```ini
[options]
; This is the Odoo configuration file

# Database connection
db_host = db
db_port = 5432
db_user = odoo
db_password = odoo

# Addons paths
addons_path = /mnt/extra-addons,/mnt/enterprise-addons,/usr/lib/python3/dist-packages/odoo/addons

# Log file
logfile = /var/log/odoo/odoo.log

# Other settings
admin_passwd = admin
```

### Step 6: Start Docker Compose
Run the following command in the directory where your `docker-compose.yml` file is located:
```shell
docker-compose up -d
```

### Step 7: Verify the Setup
7.1. Check Running Containers
```shell
docker-compose up -d
```

7.2. Access Odoo
Open a web browser and navigate to `http://localhost:8069`.

7.3. Check Logs for Errors
```shell
docker logs <odoo_container_id>
```
Replace `<odoo_container_id>` with the ID of your Odoo container, which you can find using the docker ps command.

## Required Software

To set up and run Odoo with Docker and custom addons on your Windows PC, you need the following software:

### 1. [Docker Desktop](https://www.docker.com/products/docker-desktop)
### 2. [Git](https://git-scm.com/)
### 3. [Text Editor or IDE](https://code.visualstudio.com/)
### 4. [PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-windows)
### Optional: [Windows Terminal](https://aka.ms/terminal)


## Contact
If you have any questions or need further assistance, please feel free to contact me:

- **Email:** [t.ahmed@stride.ae](mailto:t.ahmed@stride.ae)
- **GitHub:** [StrideIT](https://github.com/StrideIT)

Thank you for checking out this guide!
