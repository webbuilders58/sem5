

sudo apt install apache2
sudo systemctl enable apache2
sudo systemctl start apache2


sudo apt install memcached python3-memcache

sudo nano /etc/memcached.conf
Find the following line:
-l 127.0.0.1 (check it is on localhost)

sudo systemctl enable memcached
sudo systemctl start memcached


### Steps to Set Up a Horizon Node (OpenStack Dashboard)

#### 1. **Install Horizon (OpenStack Dashboard) Packages**

   First, you need to install the Horizon package on the node where the dashboard will run.

   **For Ubuntu/Debian-based systems:**
   ```
   sudo apt update
   sudo apt install openstack-dashboard
   ```


#### 2. **Edit the Configuration File**

   The main configuration file for Horizon is located at `/etc/openstack-dashboard/local_settings.py`. Open this file in a text editor:

   ```bash
   sudo nano /etc/openstack-dashboard/local_settings.py
   ```

   Now follow these steps:

   - **Set the OpenStack controller node address** (the node where OpenStack services like Keystone run):
     ```python
     OPENSTACK_HOST = "<ip addr of controller node>"
     ```

     If you're not using "controller" as the hostname for your OpenStack controller, replace it with the actual hostname or IP address.

     - For development environments, you can allow access from all hosts (this is **not recommended** for production):
     ```python
     ALLOWED_HOSTS = ['*']
     ```

   - **Configure session storage using memcached**:
     Ensure that Horizon uses memcached to store session information:
     ```python
     SESSION_ENGINE = 'django.contrib.sessions.backends.cache'

     CACHES = {
         'default': {
             'BACKEND': 'django.core.cache.backends.memcached.MemcachedCache',
             'LOCATION': '127.0.0.1:11211',
         }
     }
     ```

     Replace `controller` with the actual hostname or IP address of the memcached server if it differs.

   - **Set the Keystone API version to v3**:
     ```python
     OPENSTACK_KEYSTONE_URL = "http://%s/identity/v3" % OPENSTACK_HOST
     ```

     If Keystone is running on port 5000, use:
     ```python
     OPENSTACK_KEYSTONE_URL = "http://%s:5000/identity/v3" % OPENSTACK_HOST
     ```

   - **Enable Keystone domain support**:
     ```python
     OPENSTACK_KEYSTONE_MULTIDOMAIN_SUPPORT = True
     ```

   - **Set API versions** for various OpenStack services:
     ```python
     OPENSTACK_API_VERSIONS = {
         "identity": 3,
         "image": 2,
         "volume": 3,
     }
     ```

   - **Set the default domain and role** for users created via the dashboard:
     ```python
     OPENSTACK_KEYSTONE_DEFAULT_DOMAIN = "Default"
     OPENSTACK_KEYSTONE_DEFAULT_ROLE = "user"
     ```

   - **Networking configuration (optional)**: If you are not using advanced Layer 3 networking features, you can disable these options:
     ```python
     OPENSTACK_NEUTRON_NETWORK = {
         'enable_router': False,
         'enable_quotas': False,
         'enable_ipv6': False,
         'enable_distributed_router': False,
         'enable_ha_router': False,
         'enable_fip_topology_check': False,
     }
     ```

   - **Set the correct timezone** (optional):
     ```python
     TIME_ZONE = "UTC"  # Replace "UTC" with your local timezone if needed
     ```

#### 3. **Update Apache Configuration**

   Horizon runs on an Apache web server. Ensure the following line is included in `/etc/apache2/conf-available/openstack-dashboard.conf`:

   ```bash
   WSGIApplicationGroup %{GLOBAL}
   ```

   If this line is missing, add it and save the file.

#### 4. **Reload Apache Web Server**

   After updating the Horizon configuration, reload the Apache service to apply the changes:

   ```bash
   sudo systemctl reload apache2 (if not works try sudo systemctl restart apache2) 
   ```

#### 5. **Access Horizon Dashboard**

   Once everything is configured, you can access the Horizon dashboard through your browser. The URL will be:
   ```
   http://<horizon-node-ip>/horizon
   ```

   Use your OpenStack credentials to log in (these are the credentials for Keystone).

---

### Additional Notes:
- If you’re setting up **Horizon** in a multi-node OpenStack deployment, make sure the **controller node** is accessible and all necessary services (e.g., Keystone, Neutron, Glance) are up and running.
- If you run into any issues with accessing the dashboard or loading pages, you can check the logs for errors:
  ```bash
  sudo tail -f /var/log/apache2/error.log
  ```
25 visits · 1 online

