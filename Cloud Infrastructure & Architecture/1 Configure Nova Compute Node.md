
# MicroStack Installation and VM Launch Guide

## Steps to Install and Verify MicroStack

### 1. Update the package list:
```bash
sudo apt-get update
```

### 2. Install MicroStack:
```bash
sudo snap install microstack --beta
```

### 3. Verify the installation:
```bash
snap list microstack
```

### 4. Initialize MicroStack:
```bash
sudo microstack init --auto --control
```

### 5. Check the OpenStack version:
```bash
microstack.openstack --version
```

## Launch a VM with Cirros

### 6. Launch a Cirros VM:
```bash
microstack launch cirros -n MyVm  # For Dashboard link
```

### 7. Retrieve the password for the dashboard:
```bash
sudo snap get microstack config.credentials.keystone.password  # For Password
```

---
