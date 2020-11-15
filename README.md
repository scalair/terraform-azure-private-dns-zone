# Terraform Azure Private DNS Zone

This module allows to:

- Create a private DNS zone in Azure
- Link it with provided virtual networks
- Manage records within it (a, aaaa, cname, mx, prt, srv and txt)

More info [here](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/private_dns_zone).

## Usage example

Only `source`, `name` and `resource_group_name` are required:

```hcl
module "private_dns_zone" {
  source = "github.com/scalair/terraform-azure-private-dns-zone"

  name                = "private.zone.com" // must be a valid domain name
  resource_group_name = "rg_name"

  registration_enabled = true // Enables automatic creation of DNS records for the VMs in the linked Vnets.

  linked_vnets = {
      "vnet_name": "/vnet/id",
  }

  a_records = {
    "a_record_1" = {
      ttl = 300,
      records = ["10.0.180.17"]
    }
  }

  aaaa_records = {
    "aaaa_record_1" = {
      ttl = 300,
      records = ["fd5d:70bc:930e:d008:0000:0000:0000:7334", "fd5d:70bc:930e:d008::7335"]
    }
  }

  cname_records = {
    "cname_record_1" = {
      ttl = 300,
      records = "example.com"
    }
  }

  mx_records = {
    "mx_record_1" = {
      ttl = 300,
      records = [
        {
          preference = 10,
          exchange = "mx1.contoso.com"
        }
      ]
    }
  }

  ptr_records = {
    "ptr_record_1" = {
      ttl = 300,
      records = ["test.example.com"]
    }
  }

  srv_records = {
    "srv_record_1" = {
      ttl = 300,
      records = [
        {
          priority = 1,
          weight   = 5,
          port     = 8080,
          target   = "target1.contoso.com"
        }
      ]
    }
  }

  txt_records = {
    "txt_record_1" = {
      ttl = 300,
      records = [
        {
          value = "v=spf1 mx ~all"
        }
      ]
    }
  }

  tags = {}
}
```
