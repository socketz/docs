---
title: Hosted Private Cloud Reversibility Policy
slug: hosted-private-cloud-reversibility-policy
section: Reversibility policies
order: 2
---

**Last updated 5th May 2021**

## Objective

This document is the reversibility policy for the [Hosted Private Cloud Premier Powered by VMware](https://www.ovhcloud.com/en-gb/enterprise/products/hosted-private-cloud/) product.

This policy aims at implementing the global reversibility principles and requirements of [SWIPO IaaS Code of Conduct for Cloud Providers](https://swipo.eu/download-section/copyrighted-downloads/){.external}.

## Features map

Hosted Private Cloud features are divided into three categories:

- The [core features](#core-features) for which we guarantee the ability to migrate
- The [OVHcloud implementation](#ovhcloud-implementation), whose migration will require adaptations to a new environment.
- [Specific functionalities](#specific-functionalities), whose migration as such is impossible to guarantee as they are tied to OVHcloud environment or specific developments.

### Core features <a name="core-features"></a>

|Feature|Description|Available formats|
|---|---|---|
|Software-defined Compute|Set of virtual machines managed by VMware vSphere|Every file format supported by vSphere, such as .vmsd, .vmx ...|
|Software-defined Storage|Set of datastores attached to virtual machines.|N/A|
|Software defined Network|Network virtualisation service based on NSX|N/A|

The following migration models and available documentation apply to all the features described in the table above.

|Migration model|Available documentation|
|---|---|
|**Inbound migration**:<br>- Sign up for an Hosted Private Cloud project<br>-Order the appropriate number of hosts and datastores on the project to achieve capacity comparable to the original infrastructure.<br>-Migrate via a specialized tool (e.g. Veeam, Zerto, HCX ...)<br><br>**Outbound migration**:<br>- Set up a vsphere hypervisor in the target environment<br>- Plan the capacities of the target environment in a manner comparable to the original environment<br>- Migrate via a specialized tool (e.g. Veeam, Zerto, HCX ...)|[Standard vSphere documentation](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.vm_admin.doc/GUID-CEFF6D89-8C19-4143-8C26-4B6D6734D2CB.html) applies.<br><br>[Deploying an OVF Linux, Windows Server and Windows SQL Server template](https://docs.ovh.com/gb/en/private-cloud/applying-ovh-template/)<br><br>[Deploying a virtual machine with vSphere](https://docs.ovh.com/gb/en/private-cloud/deploying-a-virtual-machine/)<br><br>[Create a cluster and enable EVC](https://docs.ovh.com/fr/private-cloud/create-cluster-enable-evc/)(FR)|

### OVHcloud implementation <a name="ovhcloud-implementation"></a>

|Feature|Description|Available formats|Migration model|Available documentation|
|---|---|---|---|---|
|vRack|vRack, or Virtual rack, is a private VLAN technology that enables connection between OVHcloud services|N/A|**Inbound migration**: Hosted private Cloud is included by default into vRack.<br><br>**Outbound migration**: Record the network architecture and reproduce it with VLAN.|[Using Private Cloud with vRack](https://docs.ovh.com/gb/en/private-cloud/using-private-cloud-with-vrack/)<br><br>[How to create a V(x)LAN](https://docs.ovh.com/gb/en/private-cloud/creation-vlan-vxlan/)|
|vROps|Standard VMware monitoring solution.|N/A|**Inbound migration**: ROps is included by default with every Hosted Private Cloud.<br><br>**Outbound migration**: Install & configure vROps in a vSphere environment.|[First connection on vROps](https://docs.ovh.com/fr/private-cloud/first-connection-on-vrops/)|
|Managed Veeam backup|Backup-as-a-Service solution for your VMs|VBK, VIB, VBM|**Inbound migration**: Activate a Veeam backup option in the [OVHcloud Control Panel](https://www.ovh.com/auth/?action=gotomanager&from=https://www.ovh.co.uk/&ovhSubsidiary=GB).<br><br>**Outbound migration**: Import backups manually, then restore backups|[Activating and using Veeam Managed Backup](https://docs.ovh.com/gb/en/private-cloud/veeam-backup-as-a-service/)<br><br>[Importing backups](https://helpcenter.veeam.com/docs/backup/hyperv/importing_backups.html?ver=110)|
|Zerto|Business continuity and disaster recovery platform.|N/A|**Inbound migration**: Activate the zerto service either in the [OVHcloud Control Panel](https://www.ovh.com/auth/?action=gotomanager&from=https://www.ovh.co.uk/&ovhSubsidiary=GB) or directly in the provided Zerto Replication Interface.<br><br>**Outbound migration**: Export zerto VPG settings and import those settings in the new environment.|[Setting up Zerto Virtual Replication for your DRP](https://docs.ovh.com/gb/en/private-cloud/zerto-virtual-replication-vmware-vsphere-drp/)<br><br>[Exporting Zerto VPG settings](https://www.zerto.com/myzerto/knowledge-base/exporting-and-importing-vpg-settings-with-zerto-diagnostic-tool/)|

### Specific functionalities <a name="specific-functionalities"></a>

|Feature|Description|Available formats|Migration model|Available documentation|
|---|---|---|---|---|
|vScope monitoring|Health and resource usage monitoring tool designed by OVHCloud for Privrate Cloud.|N/A|N/A ; vScope is a static interface.|[How to use vScope](https://docs.ovh.com/gb/en/private-cloud/how_to_use_vscope/)|
|Anti-DDoS|The anti-DDoS is a set of equipment and means put in place to absorb distributed denial of service attacks. It includes an analysis of traffic, the "aspiration" towards a specialized network, and mitigation, ensured by VAC technology developed by OVHcloud.|N/A|**Inbound migration**: The Anti-DDoS is a component of our infrastructure, enabled by default. No action is required.<br><br>**Outbound migration**: Order and configure an anti-DDoS with the new provider.|[OVHcloud anti-DDoS protection](https://www.ovh.co.uk/anti-ddos/)<br><br>[Anti-DDoS Technology](https://www.ovh.co.uk/anti-ddos/anti-ddos-technology.xml)|
|SDDC Security advanced|Package of features improving security, such as Zero trust implementation, MFA, IDS for vSphere access ...|N/A|**Inbound migration**: Order the activation of security advanced in the [OVHcloud Control Panel](https://www.ovh.com/auth/?action=gotomanager&from=https://www.ovh.co.uk/&ovhSubsidiary=GB).<br><br>**Outbound migration**: Order and configure appropriate security features with the new provider.|[SDDC Advanced Security Pack](https://www.ovhcloud.com/en-gb/enterprise/products/hosted-private-cloud/safety-compliance/sddc/)|

### Architecture listing

Information about the architecture (such as servers, storage ...) is centralized and visible in the vSphere console.

### Available migration tools

[Importing/exporting virtual machines](https://docs.ovh.com/fr/private-cloud/ovf-tool/) (FR).

[Converting a physical / virtual machine into a cloud infrastructure](https://docs.ovh.com/fr/private-cloud/vmware-vcenter-converter/) (FR).

### Partner services

OVHcloud Partners are listed with the "Cloud Migration" keyword in the dedicated [directory](https://partner.ovhcloud.com/en-gb/directory/).

### Cost and fees

No additional billing is planned from OVHcloud for the migration features listed here.

### Retention of data after contract termination

The data is retained until the end of the month after the termination of the service and then deleted permanently.

## Go further

[Migrating a PCC to Hosted Private Cloud](https://docs.ovh.com/gb/en/private-cloud/sddc-migration/)