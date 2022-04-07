# Deploy Bookstack on AKS with Azure MySQL backend and Velero Backups!
Here’s how this happened. I am working for a startup and we needed a great wiki. We thought that SharePoint might fit the bill since we are a Microsoft 365 shop, right? Nope, it turns out that SharePoint wiki features have been severely gutted from what I remembered it could do (Microsoft Viva’s domain, maybe?). 
In any case, we wanted to save money so I started looking around and found this awesome product called [Bookstack](https://www.bookstackapp.com/). This app has a lot but mainly I liked the way it has native Draw.IO integration and had a pretty decent WYSIWYG editor. 
In any case, I decided I wanted to ensure that the implementation was a solid as possible on Azure. So I looked into deploying it on AKS. 
# Helm Chart
In this repo, you’ll find a heavily modified Helm chart for deploying the bookstack cluster on AKS. First you’ll need to add the repo then you can deploy the chart **==(after you modify everything in between the carrots < >)==**. 

`helm repo add k8s-at-home https://k8s-at-home.com/charts`

`helm upgrade --install bookstack k8s-at-home/bookstack --create-namespace --namespace bookstack --values bookstack-values.yaml`  

*[values file](bookstack-values.yaml) found within this GitHub repository*
## What’s in the Chart?
1.	It deploys bookstack
2.	It deploys the backend of bookstack on an Azure MySQL Database PaaS instance instead of internally to the AKS cluster. NOTE: you’ll need to set this up PRIOR to running the helm chart install command
3.	It creates a persistent volume on the two directories that could be lost if you lose the cluster altogether

# Velero Backups
Velero is a beast and has many configurations. The main thing I wanted to accomplish here was to backup the persistent volumes I configured in the helm chart so that if ever I lose the cluster, I don’t lose uploaded attachments or images.
After your deploy the helm chart, be sure to check the install for Velero to enable backups on your bookstack cluster. It’s found in the *[/velero](/velero) directory* in this repository
