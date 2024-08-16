# Step 1: Create your Project folder
Create your Project folder on your Windows PC with the following structure:
```
project-root/
│
├── docker-compose.yml # Docker Compose configuration file
├── odoo.conf # Custom Odoo configuration file
│
├── addons/ # Directory for custom Odoo addons
├── enterprise-addons/ # Directory for Odoo Enterprise addons
```

# Step 2: Open Command Prompt or PowerShell
You can use either Command Prompt or PowerShell to execute the Git commands. Press Win + R, type cmd or powershell, and press Enter. Then Navigate to the Desired Directory where you want to clone the Odoo Enterprise repository
```
cd C:\project-root\enterprise-addons
```


# Step 4: Clone Odoo Enterprise Git Repository
Use the git clone command to clone the repository into a subdirectory named enterprise-addons. Ensure you have Git installed and configured on your Windows machine.
```
git clone git@github.com:odoo/enterprise.git enterprise-addons
```
> [!TIP]
> Add your custom addones in the `addons` Directory which located inside the `project-root/` if required


# Step 5: Prepare `docker-compose.yml` file
Create `docker-compose.yml` File which located inside the `project-root/` file with the following content:

```
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

# Step 6: Create/Update Configuration File (`odoo.conf`)
Make sure your `odoo.conf` includes the necessary configurations for addons:
```
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

# Step 7: Start Docker Compose
Run the following command in the directory where your `docker-compose.yml` file is located:
```
docker-compose up -d
```

# Step 7: Verify the Setup
## 7.1: Check Running Containers
```
docker ps
```

## 7.2: Access Odoo
Open a web browser and navigate to `http://localhost:8069`.

## 7.3: Check Logs for Errors
```
docker logs <odoo_container_id>
```


