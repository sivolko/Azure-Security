---
description: >-
  This contains SAST (static Application Security Testing) rules for shift left
  approach and maintain code smell and clean coding
---

# Azure  SAST Rules

RBAC Rules&#x20;

<details>

<summary>Azure custom roles should not grant subscription "Owner" capabilities </summary>

<mark style="color:red;">**Noncompliant code example**</mark>

{% code lineNumbers="true" %}
```json
//Noncompliant code 

resource "azurerm_role_definition" "example" { # Sensitive
  name        = "example"
  scope       = data.azurerm_subscription.primary.id

  permissions {
    actions     = ["*"]
    not_actions = []
  }

  assignable_scopes = [
    data.azurerm_subscription.primary.id
  ]
}


// compliant solution 

resource "azurerm_role_definition" "example" {
  name        = "example"
  scope       = data.azurerm_subscription.primary.id

  permissions {
    actions     = ["Microsoft.Compute/*"]
    not_actions = []
  }

  assignable_scopes = [
    data.azurerm_subscription.primary.id
  ]
}
```
{% endcode %}

</details>

<details>

<summary>Administration services access should be restricted to specific IP addresses.</summary>

#### <mark style="color:red;">What is the potential impact?</mark>

Since Administrative services run with the elevated privileges and thus a vulnerability could have a high impact on the system along with credentials might be leaked through the phishing or similar technique.&#x20;

&#x20;<mark style="color:red;">**Solution**</mark>&#x20;

Restrict access to the remote administrative services to only <mark style="color:purple;">trusted IP</mark> address.\
What should be termed as "Trusted IP "?

Any IP address which is held by system Administrative&#x20;

eg:-&#x20;

```json
// Nonecompliant code example 

{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "name": "networkSecurityGroups/example",
      "type": "Microsoft.Network/networkSecurityGroups/securityRules",
      "apiVersion": "2022-11-01",
      "properties": {
        "protocol": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "*",
        "access": "Allow",
        "direction": "Inbound"
      }
    }
  ]
}

resource securityRules 'Microsoft.Network/networkSecurityGroups/securityRules@2022-11-01' = {
  name: 'securityRules'
  properties: {
    direction: 'Inbound'
    access: 'Allow'
    protocol: '*'
    destinationPortRange: '*'
    sourceAddressPrefix: '*'
  }
}


// compliant solution

{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "name": "networkSecurityGroups/example",
      "type": "Microsoft.Network/networkSecurityGroups/securityRules",
      "apiVersion": "2022-11-01",
      "properties": {
          "protocol": "*",
          "destinationPortRange": "22",
          "sourceAddressPrefix": "10.0.0.0/24",
          "access": "Allow",
          "direction": "Inbound"
      }
    }
  ]
}

resource securityRules 'Microsoft.Network/networkSecurityGroups/securityRules@2022-11-01' = {
  name: 'securityRules'
  properties: {
    direction: 'Inbound'
    access: 'Allow'
    protocol: '*'
    destinationPortRange: '22'
    sourceAddressPrefix: '10.0.0.0/24'
  }
}

```

</details>

