<properties
	pageTitle="Troubleshooting VM allocation failures | Microsoft Azure"
	description="Troubleshoot allocation failures when you create, restart, or resize a VM in Azure"
	services="virtual-machines, azure-resource-manager"
	documentationCenter=""
	authors="jiangchen79"
	manager="felixwu"
	editor=""
	tags="top-support-issue"/>

<tags
	ms.service="virtual-machines"
	ms.workload="na"
	ms.tgt_pltfrm="ibiza"
	ms.devlang="na"
	ms.topic="article"
	ms.date="02/22/2016"
	ms.author="cjiang"/>


# Troubleshoot issues in creating or adding new Azure Virtual Machines (VMs), or in resizing or restarting existing VMs

You may encounter issues or errors when you try to deploy, add, or create a new virtual machine (VM), or when you try to restart or resize an existing VM. To resolve the most common issues, try one or more of the following methods:

- Review the Audit logs to determine the reason for the failure. In the Azure portal, click **Browse** > **Virtual machines** > *your Windows virtual machine* > **Settings** -> **Audit logs**.

-	See the [Troubleshoot specific error](#troubleshoot-specific-error) codes section for suggested actions by error code.

-	Try your request by using a smaller VM size. To do this, click **Browse** > **Virtual machines** > *your Windows virtual machine* > **Settings** > **Size**.  For detailed steps, see [Resize the virtual machine](https://msdn.microsoft.com/library/dn168976.aspx).

- Consider the option of creating a new cloud service to host the new VM. For more information, see [Connecting virtual networks in Azure across different regions](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/) and [Regional virtual networks](http://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

-	If you’re creating a new VM by using a custom image, see the following articles to verify that you followed the necessary steps to create the image:

	- [Create an Azure image from a running Windows machine – classic](virtual-machines-capture-image-windows-server.md)
	- [Create an Azure image from a running Windows machine – ARM](virtual-machines-windows-capture-image-resource-manager.md)
	- [Create an Azure image from a running Linux machine](virtual-machines-linux-capture-image.md)

For additional help, you can contact Azure experts through [Azure forums](http://azure.microsoft.com/support/forums/). You can also file an Azure support case. To do this, go to the [Azure Support site](http://azure.microsoft.com/support/options/), and then click **Get Support**.

## Troubleshoot specific error codes

**Upgrade_VMSizeNotSupported* or GeneralError***

**Potential cause**: If the specific VM size that’s requested in a cluster has insufficient capacity, the operation may fail.

**Suggested action**:

1.	Try submitting your request after several minutes, or try your request by using a different VM size or a smaller VM size.

2.	If using a different VM size is not an option, but if it's acceptable to use a different virtual IP (VIP) address, create a new cloud service to host the new VM, and then add the new cloud service to the regional virtual network where the existing VMs are running.

	If your existing cloud service does not use a regional virtual network, you can still create a new virtual network for the new cloud service and then [connect your existing virtual network to the new virtual network](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). For more information, see [regional virtual networks](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

3.	If you’re using availability sets, you can:

	A.	Stop all VMs in the availability set, and then restart the first one. This will make sure that a new allocation attempt is run and that a new cluster that has available capacity can be selected.

	B.	If the VM can be part of a different availability set, create a new VM in a different availability set (in the same region). This new VM can then be added to the same virtual network.

**New_General* or New_VMSizeNotSupported***

**Potential cause**: Because of deployment request constraints, the VM size (or combination of VM sizes) required by this deployment cannot be provisioned.  

If you’re using staging deployment and the production deployment of a cloud service, they are hosted on the same cluster. When you add the second deployment, the corresponding allocation request will be tried in the same cluster that hosts the first deployment. However, the cluster may have insufficient capacity.

Any computer resource that’s assigned to an affinity group is tied to one cluster. New computer resource requests in that affinity group are tried in the same cluster where the existing resources are hosted. However, the same cluster may have insufficient capacity. This is true whether the new resources are created through a new cloud service or through an existing cloud service.

**Suggested action**:

1.	If possible, try relaxing constraints such as virtual network bindings, and deploying to a hosted service with no other deployment in it.

2.	Try deploying to a different region or by using a smaller VM size.

3.	If you’re performing staging and production deployment for the same cloud service, delete the first deployment and the original cloud service, and then redeploy the cloud service. This action may land the first deployment in a cluster that has sufficient free resources to fit both deployments, or in a cluster that supports the VM sizes that you requested.
4.	If an affinity group is unnecessary, do not use an affinity group, or group your computer resources into multiple affinity groups. Alternatively, you can [migrate your affinity-group-based virtual network to a regional virtual network](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/) and then add the desired resources again.
