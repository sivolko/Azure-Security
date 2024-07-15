# 🗼 Network Security

<mark style="color:purple;">**LAB 1**</mark>

<details>

<summary>Design and Implement a VNET(Virtual Network )</summary>

Company : Troubleshooter Club LTD&#x20;

Process of migrating infrastructure and applications to Azure . As Network engineer , we need to plan & implement 3 VNET and Subnets



Architecture Diagram&#x20;

<img src=".gitbook/assets/image (17).png" alt="" data-size="original">

**Objectives**&#x20;

* Task 1 : Create the troubleshooterclub resource group&#x20;
* Task2: Create the coreServicesVnet virtual network and subnets&#x20;
* Task3: Create the ManufacturingVnet virtual network and subnets&#x20;
* Task4: Create the ResearchVnet virtual network and subnets
* Task5: Verify the creation of the virtual networks and subnets&#x20;



</details>

<details>

<summary>Consideration for Public DNS services</summary>

* Name of Zone must be unique within the RG and Zone must not exist already&#x20;
* Same Zone name can be reused in a different RG or a different Azure Subscription&#x20;
* Where Multiple zones shares the same name, each instance is assigned different name server addresses&#x20;
* Root/Parent domain is registered at the registrar and pointed to Azure NS&#x20;
* Child domains are registered in AzureDNS directly

</details>
