<p align="center">
    <img src="files/gcp.png" height="150">
</p>

# Ensure Access & Identity in Google Cloud: Challenge Lab


# Task 1
Create a custom security role using YAML file

Create a file for YAML file using any editor.<br>
Here we are using Nano Text Editor
```md
nano role-definition.yaml
```
Replace <b>[Custom Securiy Role]</b> with <b>Custom Securiy Role</b> from your project.<br>
Add the following lines and save the file [Ctrl+X - Y - Enter]

```md
title: "[Custom Securiy Role]"
description: "Permissions"
stage: "ALPHA"
includedPermissions:
- storage.buckets.get
- storage.objects.get
- storage.objects.list
- storage.objects.update
- storage.objects.create
```
Create Security Role by applying the YAML file<br>
Replace <b>[Custom Securiy Role]</b> with <b>Custom Securiy Role</b> from your project.<br>
```md
gcloud iam roles create [Custom Securiy Role] --project $DEVSHELL_PROJECT_ID \
--file role-definition.yaml
```

# Task 2 and 3
Create a service account from Console [You can do it from CLI too]<br>
Copy <b>[Service Account]</b> name given for your project.<br>
* Navigate to IAM & Admin - Service Accounts
* Create Service Account
* Use the copied name from project
* create and Continue
<br>

Bind a custom security role to a service account.<br>
* Select Role and add these roles
  * Monitoring Viewer
  * Monitoring Metric Writer
  * Logging log Writer
  * Custom - [Custom Securiy Role]
* Continue
* Done

# Task 4

Create and configure a new Kubernetes Engine private cluster.<br>
Replace <b>[Cluster Name],[Service Account] & [Project ID]</b> with <b>Cluster Name,Service Account & Project ID</b> from your project.


```md
gcloud container clusters create [Cluster Name] --num-nodes 1 --master-ipv4-cidr=172.16.0.64/28 --network orca-build-vpc --subnetwork orca-build-subnet --enable-master-authorized-networks  --master-authorized-networks 192.168.10.2/32 --enable-ip-alias --enable-private-nodes --enable-private-endpoint --service-account [Service Account]@[Project ID].iam.gserviceaccount.com --zone us-east1-b
```
Kubernetes Engine creation takes around 5 minutes.

# Task 5
Deploy an application to a private Kubernetes Engine cluster.<br>
* Navigate to Compute Engine VM Instances
* Find orca-jumphost and SSH 

Get Kubernetes credentials using given command.
Replace [Project ID] and [Cluster Name] with Project ID and Cluster Name of your project.

```md
gcloud container clusters get-credentials [Cluster Name] --internal-ip --zone us-east1-b --project [Project ID]
```  
Deploy app

```md
 kubectl create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0
```

