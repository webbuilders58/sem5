
# Cinder Setup with DevStack

## Step-by-step Instructions

### 1. Switch to root user:
```bash
su -
```

### 2. Add the 'ubuntu' user to the sudo group:
```bash
usermod -aG sudo ubuntu
```

### 3. Edit sudoers file:
```bash
visudo
```
Add the following lines:
```bash
ubuntu ALL=(ALL)NOPASSWD:ALL
stack ALL=(ALL)NOPASSWD:ALL
```

### 4. Install necessary packages:
```bash
sudo apt install mysql-client-core-8.0
sudo apt update && sudo apt upgrade -y
sudo apt install -y git
```

### 5. Add and configure the 'stack' user:
```bash
sudo adduser stack
sudo usermod -aG sudo stack
su - stack
```

### 6. Clone DevStack and configure:
```bash
git clone https://opendev.org/openstack/devstack.git
cd devstack/
nano local.conf
```
Add the following content to `local.conf`:
```bash
[[local|localrc]]
ADMIN_PASSWORD=secret
DATABASE_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD
GLANCE_HOSTPORT=12.0.0.1:9292
enable_service cinder cinderv2 cinderv3
enable_service n-api n-cpu n-cond n-sch n-novnc n-api-meta
enable_service glance
enable_service swift
enable_service horizon
HOST_IP=10.0.2.15
```

### 7. Run the stack setup:
```bash
./stack.sh   # This will take a long time
```

### 8. Access the OpenStack dashboard:
- URL: `http://10.0.2.15/dashboard`
- Username: `admin`
- Password: `secret`

### 9. Source OpenRC for admin:
```bash
source ~/devstack/openrc admin admin
```

### 10. Verify environment variables:
```bash
env | grep OS_
```

### 11. List OpenStack services:
```bash
openstack service list
```

### 12. Create and list a volume:
```bash
openstack volume create --size 1 my_volume
openstack volume list
```

### 13. Unstack and clean up DevStack:
```bash
./unstack.sh
./clean.sh
```
