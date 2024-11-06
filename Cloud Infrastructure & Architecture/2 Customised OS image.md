
# Procedure for Creating and Launching a Cloud OS Image in OpenStack

## Step 1: Download the Cloud OS Image
1. Search for "Cloud OS image download" on Google.
2. Select a suitable image, such as NetBSD.

## Step 2: Create an Image in OpenStack
1. Log in to the OpenStack dashboard.
2. Click on the "Images" tab.
3. Click on the "Create Image" button.
4. Provide the image name ("NetBSD").
5. Select the image source (the downloaded image file).
6. Select the image format (QEMU).
7. Set the RAM size (2 GB) and disk size (4 GB).
8. Set the image sharing visibility to "Public".
9. Click "Create Image".

## Step 3: Launch an Instance from the Customized Image
1. Go to the "Instances" tab.
2. Click on the "Launch Instance" button.
3. Select the customized image ("NetBSD").
4. Provide an instance name ("My Instance").
5. Select a flavor ("m1.small").
6. Select an external network.
7. Click "Launch Instance".

### Alternative Method: Launching an Instance using the Command Line
Run this in the terminal:
```bash
microstack launch NetBSD -n Myins --flavor m1.small
```

---

## RESULT:
Successfully created a customized Ubuntu OS image, uploaded it to the OpenStack environment, and deployed a VM using Nova.
