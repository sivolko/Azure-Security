---
description: Please refer official documentation for latest changes and installation
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: false
  pagination:
    visible: true
---

# Installation

{% embed url="https://developer.hashicorp.com/terraform/tutorials/azure-get-started/install-cli" %}

{% hint style="info" %}
Azure Cli should be installed in local system to use Azure resources via API&#x20;
{% endhint %}

{% tabs %}
{% tab title="Debain based Linux Installation " %}
```bash
sudo apt update && sudo apt install -y gnupg software-properties-common
```
{% endtab %}

{% tab title="Second Tab" %}
```
// Some code
```
{% endtab %}
{% endtabs %}

{% code title="Install  HashiCorp GPG key" fullWidth="false" %}
```bash
wget -O- https://apt.releases.hashicorp.com/gpg | \
gpg --dearmor | \
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg

```
{% endcode %}

{% code title="Verify the key's fingerprint." %}
```bash
gpg --no-default-keyring \
--keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
--fingerprint

```
{% endcode %}

{% code title="Add the official HashiCorp repository to your system." %}
```bash
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list

```
{% endcode %}

{% code title="Download the package from HashiCorp." %}
```bash
sudo apt update
```
{% endcode %}

{% code title="Install Terraform " %}
```bash
sudo apt install terraform
```
{% endcode %}

{% code title="Verify Installation " %}
```bash
terraform -help
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p>Installation Verification </p></figcaption></figure>

## <mark style="background-color:purple;">VS Code Extension Installation</mark>&#x20;
