---
description: This contains cloud Native Meetup code snippets
---

# 👺 Code Like Hacker : Secure Terraform Practices

## Admin access should be restricted from the specific IP&#x20;

* ISSUE :  Any Firewall allowing traffic from all IP address to standard n/w port on which admin services traditionally listen such as \
  &#x20;SSH - port#22 : Lead to unauthorised access
* Potential Impact :- \
  &#x20;Privilege Escalation or elevation \
  &#x20;Vulnerability&#x20;

Example&#x20;

#### An ingress rule allowing all inbound SSH traffic for AWS:

```hcl
// Non-compliant code 

resource "aws_security_group" "noncompliant" {
  name        = "allow_ssh_noncompliant"
  description = "allow_ssh_noncompliant"
  vpc_id      = aws_vpc.main.id

  ingress {
    description      = "SSH rule"
    from_port        = 22
    to_port          = 22
    protocol         = "tcp"
    cidr_blocks      = ["0.0.0.0/0"]  # Noncompliant
  }
}
```

#### A security rule allowing all inbound SSH traffic for Azure

```hcl
// Non-compliant code 

resource "azurerm_network_security_rule" "noncompliant" {
  priority                    = 100
  direction                   = "Inbound"
  access                      = "Allow"
  protocol                    = "Tcp"
  source_port_range           = "*"
  destination_port_range      = "22"
  source_address_prefix       = "*"  # Noncompliant
  destination_address_prefix  = "*"
}
```

### Compliant Solution&#x20;

It is recommended to restrict access to remote administration services to only trusted IP addresses. In practice, trusted IP addresses are those held by system administrators or those of [bastion-like](https://aws.amazon.com/quickstart/architecture/linux-bastion/?nc1=h_ls) servers.

#### An ingress rule allowing inbound SSH traffic from specific IP addresses for AWS:

```hcl
// Compliant-code 

resource "aws_security_group" "compliant" {
  name        = "allow_ssh_compliant"
  description = "allow_ssh_compliant"
  vpc_id      = aws_vpc.main.id

  ingress {
    description      = "SSH rule"
    from_port        = 22
    to_port          = 22
    protocol         = "tcp"
    cidr_blocks      = ["1.2.3.0/24"]
  }
}

```

#### A security rule allowing inbound SSH traffic from specific IP addresses for Azure&#x20;

```hcl
// Compliant-code 

resource "azurerm_network_security_rule" "compliant" {
  priority                    = 100
  direction                   = "Inbound"
  access                      = "Allow"
  protocol                    = "Tcp"
  source_port_range           = "*"
  destination_port_range      = "22"
  source_address_prefix       = "1.2.3.0"
  destination_address_prefix  = "*"
}
```

* CWE - [CWE-284 - Improper Access Control](https://cwe.mitre.org/data/definitions/284)

