# Fortigate HA AP (Active Passive)

## Introduction

This template deploys a 2 x 4 NIC Fortigate Firewall resource in an HA Active Passive configuration. An internal loadbalancer is also created to handle all outbound traffic to the internal Interface of the firewall.

An external firewall is created to handle balancing traffic for public services exposed to the active firewall node.

Each firewall is given two public IP. One for management and one to help troubleshoot external service access.

## Security Controls

The following security controls can be met through configuration of this template:

* AC-2, AC-3, AC-4, AC-5, AC-6, AC-8, AU-11, AU-12, AU-2, AU-3, AU-4, AU-8, AU-9, CM-3, CM-6, IA-2, IA-3, IA-5, SA-22, SC-10, SC-12, SC-13, SC-7, SC-8, SI-2, SI-4

## Dependancies

* [Resource Groups](https://github.com/canada-ca/accelerators_accelerateurs-azure/blob/master/Templates/arm/resourcegroups/latest/readme.md)
* [Keyvault](https://github.com/canada-ca/accelerators_accelerateurs-azure/blob/master/Templates/arm/keyvaults/latest/readme.md)
* [VNET-Subnet](https://github.com/canada-ca/accelerators_accelerateurs-azure/blob/master/Templates/arm/vnet-subnet/latest/readme.md)

## Parameter format

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "fwObject": {
            "value": {
                "vmKeyVault": {
                    "keyVaultResourceGroupName": "PwS2-validate-fortigateAP-RG",
                    "keyVaultName": "PwS2-validate-[unique]"
                },
                "adminUsername": "fwadmin",
                "adminSecret": "fwpassword",
                "FortiGateNamePrefix": "validate",
                "FortiGateImageSKU": "fortinet_fg-vm",
                "FortiGateVersion": "latest",
                "instanceType": "Standard_F4",
                "publicIPNewOrExisting": "new",
                "publicIP2NewOrExisting": "new",
                "publicIP3NewOrExisting": "new",
                "publicIPAddressName": "FGTAPClusterPublicIP",
                "publicIPAddressResourceGroup": "PwS2-validate-fortigateAP-RG",
                "publicIP2AddressName": "FGTAMgmtPublicIP",
                "publicIP2AddressResourceGroup": "PwS2-validate-fortigateAP-RG",
                "publicIP3AddressName": "FGTBMgmtPublicIP",
                "publicIP3AddressResourceGroup": "PwS2-validate-fortigateAP-RG",
                "publicIPAddressType": "Static",
                "vnetName": "PwS2-validate-fortigateAP-VNET",
                "vnetResourceGroup": "PwS2-validate-fortigateAP-RG",
                "Subnet1Name": "outside",
                "Subnet2Name": "inside",
                "Subnet3Name": "HASync",
                "Subnet4Name": "management",
                "firewall1Config": "Y29uZmlnIHN5c3RlbSBnbG9iYWwNCiAgICBzZXQgYWRtaW4tc3BvcnQgODQ0Mw0KICAgIHNldCBhbGlhcyAiUHdQMEZXQ29yZTAxIg0KICAgIHNldCBob3N0bmFtZSAiUHdQMEZXQ29yZTAxIg0KICAgIHNldCB0aW1lem9uZSAwNA0KZW5kDQpjb25maWcgc3lzdGVtIGludGVyZmFjZQ0KICAgIGVkaXQgInBvcnQxIg0KICAgICAgICBzZXQgdmRvbSAicm9vdCINCiAgICAgICAgc2V0IG1vZGUgZGhjcA0KICAgICAgICBzZXQgYWxsb3dhY2Nlc3MgcGluZyBodHRwcyBzc2ggaHR0cCBmZ2ZtDQogICAgICAgIHNldCB0eXBlIHBoeXNpY2FsDQogICAgICAgIHNldCBkZXNjcmlwdGlvbiAib3V0c2lkZSINCiAgICAgICAgc2V0IGFsaWFzICJvdXRzaWRlIg0KICAgICAgICBzZXQgc25tcC1pbmRleCAxDQogICAgbmV4dA0KICAgIGVkaXQgInBvcnQyIg0KICAgICAgICBzZXQgdmRvbSAicm9vdCINCiAgICAgICAgc2V0IG1vZGUgZGhjcA0KICAgICAgICBzZXQgdHlwZSBwaHlzaWNhbA0KICAgICAgICBzZXQgZGVzY3JpcHRpb24gImluc2lkZSINCiAgICAgICAgc2V0IGFsaWFzICJpbnNpZGUiDQogICAgICAgIHNldCBzbm1wLWluZGV4IDINCiAgICAgICAgc2V0IGRlZmF1bHRndyBkaXNhYmxlDQogICAgbmV4dA0KICAgIGVkaXQgInBvcnQzIg0KICAgICAgICBzZXQgdmRvbSAicm9vdCINCiAgICAgICAgc2V0IG1vZGUgZGhjcA0KICAgICAgICBzZXQgdHlwZSBwaHlzaWNhbA0KICAgICAgICBzZXQgZGVzY3JpcHRpb24gImhhc3luY3BvcnQiDQogICAgICAgIHNldCBhbGlhcyAiaGFzeW5jcG9ydCINCiAgICAgICAgc2V0IHNubXAtaW5kZXggMw0KICAgICAgICBzZXQgZGVmYXVsdGd3IGRpc2FibGUNCiAgICBuZXh0DQogICAgZWRpdCAicG9ydDQiDQogICAgICAgIHNldCB2ZG9tICJyb290Ig0KICAgICAgICBzZXQgbW9kZSBkaGNwDQogICAgICAgIHNldCBhbGxvd2FjY2VzcyBwaW5nIGh0dHBzIHNzaCBodHRwIGZnZm0NCiAgICAgICAgc2V0IHR5cGUgcGh5c2ljYWwNCiAgICAgICAgc2V0IGRlc2NyaXB0aW9uICJtYW5hZ2VtZW50Ig0KICAgICAgICBzZXQgYWxpYXMgIm1hbmFnZW1lbnQiDQogICAgICAgIHNldCBzbm1wLWluZGV4IDQNCiAgICAgICAgc2V0IGRlZmF1bHRndyBkaXNhYmxlDQogICAgbmV4dA0KZW5kDQpjb25maWcgc3lzdGVtIGhhDQogIHNldCBncm91cC1uYW1lICJBenVyZUhBIg0KICBzZXQgbW9kZSBhLXANCiAgc2V0IGhiZGV2ICJwb3J0MyIgMTAwDQogIHNldCBzZXNzaW9uLXBpY2t1cCBlbmFibGUNCiAgc2V0IHNlc3Npb24tcGlja3VwLWNvbm5lY3Rpb25sZXNzIGVuYWJsZQ0KICBzZXQgaGEtbWdtdC1zdGF0dXMgZW5hYmxlDQogIGNvbmZpZyBoYS1tZ210LWludGVyZmFjZXMNCiAgICBlZGl0IDENCiAgICAgIHNldCBpbnRlcmZhY2UgInBvcnQ0Ig0KICAgICAgc2V0IGdhdGV3YXkgMTAuMC40LjENCiAgICBuZXh0DQogIGVuZA0KICBzZXQgb3ZlcnJpZGUgZGlzYWJsZQ0KICBzZXQgcHJpb3JpdHkgMjU1DQogIHNldCB1bmljYXN0LWhiIGVuYWJsZQ0KICBzZXQgdW5pY2FzdC1oYi1wZWVyaXAgMTAuMC4zLjUNCmVuZA==",
                "firewall2Config": "Y29uZmlnIHN5c3RlbSBnbG9iYWwNCiAgICBzZXQgYWRtaW4tc3BvcnQgODQ0Mw0KICAgIHNldCBhbGlhcyAiUHdQMEZXQ29yZTAyIg0KICAgIHNldCBob3N0bmFtZSAiUHdQMEZXQ29yZTAyIg0KICAgIHNldCB0aW1lem9uZSAwNA0KZW5kDQpjb25maWcgc3lzdGVtIGludGVyZmFjZQ0KICAgIGVkaXQgInBvcnQxIg0KICAgICAgICBzZXQgdmRvbSAicm9vdCINCiAgICAgICAgc2V0IG1vZGUgZGhjcA0KICAgICAgICBzZXQgYWxsb3dhY2Nlc3MgcGluZyBodHRwcyBzc2ggaHR0cCBmZ2ZtDQogICAgICAgIHNldCB0eXBlIHBoeXNpY2FsDQogICAgICAgIHNldCBkZXNjcmlwdGlvbiAib3V0c2lkZSINCiAgICAgICAgc2V0IGFsaWFzICJvdXRzaWRlIg0KICAgICAgICBzZXQgc25tcC1pbmRleCAxDQogICAgbmV4dA0KICAgIGVkaXQgInBvcnQyIg0KICAgICAgICBzZXQgdmRvbSAicm9vdCINCiAgICAgICAgc2V0IG1vZGUgZGhjcA0KICAgICAgICBzZXQgdHlwZSBwaHlzaWNhbA0KICAgICAgICBzZXQgZGVzY3JpcHRpb24gImluc2lkZSINCiAgICAgICAgc2V0IGFsaWFzICJpbnNpZGUiDQogICAgICAgIHNldCBzbm1wLWluZGV4IDINCiAgICAgICAgc2V0IGRlZmF1bHRndyBkaXNhYmxlDQogICAgbmV4dA0KICAgIGVkaXQgInBvcnQzIg0KICAgICAgICBzZXQgdmRvbSAicm9vdCINCiAgICAgICAgc2V0IG1vZGUgZGhjcA0KICAgICAgICBzZXQgdHlwZSBwaHlzaWNhbA0KICAgICAgICBzZXQgZGVzY3JpcHRpb24gImhhc3luY3BvcnQiDQogICAgICAgIHNldCBhbGlhcyAiaGFzeW5jcG9ydCINCiAgICAgICAgc2V0IHNubXAtaW5kZXggMw0KICAgICAgICBzZXQgZGVmYXVsdGd3IGRpc2FibGUNCiAgICBuZXh0DQogICAgZWRpdCAicG9ydDQiDQogICAgICAgIHNldCB2ZG9tICJyb290Ig0KICAgICAgICBzZXQgbW9kZSBkaGNwDQogICAgICAgIHNldCBhbGxvd2FjY2VzcyBwaW5nIGh0dHBzIHNzaCBodHRwIGZnZm0NCiAgICAgICAgc2V0IHR5cGUgcGh5c2ljYWwNCiAgICAgICAgc2V0IGRlc2NyaXB0aW9uICJtYW5hZ2VtZW50Ig0KICAgICAgICBzZXQgYWxpYXMgIm1hbmFnZW1lbnQiDQogICAgICAgIHNldCBzbm1wLWluZGV4IDQNCiAgICAgICAgc2V0IGRlZmF1bHRndyBkaXNhYmxlDQogICAgbmV4dA0KZW5kDQpjb25maWcgc3lzdGVtIGhhDQogIHNldCBncm91cC1uYW1lICJBenVyZUhBIg0KICBzZXQgbW9kZSBhLXANCiAgc2V0IGhiZGV2ICJwb3J0MyIgMTAwDQogIHNldCBzZXNzaW9uLXBpY2t1cCBlbmFibGUNCiAgc2V0IHNlc3Npb24tcGlja3VwLWNvbm5lY3Rpb25sZXNzIGVuYWJsZQ0KICBzZXQgaGEtbWdtdC1zdGF0dXMgZW5hYmxlDQogIGNvbmZpZyBoYS1tZ210LWludGVyZmFjZXMNCiAgICBlZGl0IDENCiAgICAgIHNldCBpbnRlcmZhY2UgInBvcnQ0Ig0KICAgICAgc2V0IGdhdGV3YXkgMTAuMC40LjENCiAgICBuZXh0DQogIGVuZA0KICBzZXQgb3ZlcnJpZGUgZGlzYWJsZQ0KICBzZXQgcHJpb3JpdHkgMQ0KICBzZXQgdW5pY2FzdC1oYiBlbmFibGUNCiAgc2V0IHVuaWNhc3QtaGItcGVlcmlwIDEwLjAuMy40DQplbmQNCg==",
                "tagValues": {
                    "Owner": "build.pipeline@tpsgc-pwgsc.gc.ca",
                    "CostCenter": "PSPC-EA",
                    "Enviroment": "Validate",
                    "Classification": "Unclassified",
                    "Organizations": "PSPC-CCC-E&O"
                }
            }
        }
    }
}
```

## Parameter Values

### Main Template

| Name               | Type   | Required | Value                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ------------------ | ------ | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| containerSasToken  | string | No       | SAS Token received as a parameter                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| _artifactsLocation | string | No       | Dynamically derived from the deployment template link uri                                                                                                                                                                                                                                                                                                                                                                                                                      |
| _debugLevel        | string | No       | Specifies the type of information to log for debugging. The permitted values are none, requestContent, responseContent, or both requestContent and responseContent separated by a comma. The default is none. When setting this value, carefully consider the type of information you are passing in during deployment. By logging information about the request or response, you could potentially expose sensitive data that is retrieved through the deployment operations. |
| fwObject           | object | Yes      | Array of [Fortigate2NIC objects](#fortigate2nic-object)                                                                                                                                                                                                                                                                                                                                                                                                                        |

### Fortigate2NIC Object

| Name                          | Type   | Required | Value                                                                                                                                                           |
| ----------------------------- | ------ | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| vmKeyVault                    | object | Yes      | Keyvault resource information - [Keyvault Object](#keyvault-object)                                                                                             |
| FortiGateName                 | string | Yes      | Name of the firewall Virtual Machine.                                                                                                                           |
| adminUsername                 | string | Yes      | Name of the Fortigate admin user                                                                                                                                |
| adminSecret                   | string | Yes      | Name of the keyvault secret containing the Fortigate admin user password                                                                                        |
| FortiGateImageSKU             | string | Yes      | SKU for the fortigate image - fortinet_fg-vm or fortinet_fg-vm_payg                                                                                             |
| FortiGateImageVersion         | string | Yes      | Version of the firewall marketplace image - 5.6.6, 6.0.3, 6.0.4 or latest. Recommended values: latest                                                           |
| instanceType                  | string | Yes      | Virtual Machine size selection. Recommended value: Standard_F4s                                                                                                 |
| publicIPAddressType           | string | Yes      | Type of public IP address - dynamic or static. Recommended value: static                                                                                        |
| publicIPNewOrExisting         | string | Yes      | Indicates whether a Public IP is required - none, new or existing. Recommended value: none unless one is required. In this case: new                            |
| publicIP2NewOrExisting        | string | Yes      | Indicates whether a Public IP 2 is required - none, new or existing. Recommended value: none unless one is required. In this case: new                          |
| publicIP3NewOrExisting        | string | Yes      | Indicates whether a Public IP 3 is required - none, new or existing. Recommended value: none unless one is required. In this case: new                          |
| publicIP4NewOrExisting        | string | Yes      | Indicates whether a Public IP 4 is required - none, new or existing. Recommended value: none unless one is required. In this case: new                          |
| publicIP5NewOrExisting        | string | Yes      | Indicates whether a Public IP 5 is required - none, new or existing. Recommended value: none unless one is required. In this case: new                          |
| publicIPAddressResourceGroup  | string | Yes      | Resource Group of the public IP                                                                                                                                 |
| publicIP2AddressResourceGroup | string | Yes      | Resource Group of the public IP 2                                                                                                                               |
| publicIP3AddressResourceGroup | string | Yes      | Resource Group of the public IP 3                                                                                                                               |
| publicIP4AddressResourceGroup | string | Yes      | Resource Group of the public IP 4                                                                                                                               |
| publicIP5AddressResourceGroup | string | Yes      | Resource Group of the public IP 5                                                                                                                               |
| publicIPAddressType           | string | Yes      | Type of public IP. - Static or Dynamic                                                                                                                          |
| vnetName                      | string | Yes      | Virtual Network name                                                                                                                                            |
| vnetResourceGroup             | string | Yes      | Name of the Resource Group containing the VNET defined above                                                                                                    |
| Subnet1Name                   | string | Yes      | Name of the external subnet                                                                                                                                     |
| Subnet2Name                   | string | Yes      | Name of the internal subnet                                                                                                                                     |
| Subnet3Name                   | string | Yes      | Name of the internal subnet                                                                                                                                     |
| Subnet4Name                   | string | Yes      | Name of the internal subnet                                                                                                                                     |
| firewall1Config               | string | Yes      | Base64 encoded multipart/mixed string of [Firewall Config and License](#firewall-config-and-license). Leave empty ("") if no firewall configuration is required |
| firewall2Config               | string | Yes      | Base64 encoded multipart/mixed string of [Firewall Config and License](#firewall-config-and-license). Leave empty ("") if no firewall configuration is required |
| tagValues                     | object | Yes      | Object of tag values                                                                                                                                            |

#### Keyvault Object

| Name                      | Type                           | Required | Value                                                                   |
| ------------------------- | ------------------------------ | -------- | ----------------------------------------------------------------------- |
| keyVaultResourceGroupName | PwS2-validate-Fortigate2NIC-RG | Yes      | Name of the Resource Group containing the keyvault                      |
| keyVaultName              | PwS2-validate-[unique]         | Yes      | Name of keyvault resource - [Name format options](#name-format-options) |

##### Name Format Options

When specifying the name of a keyvault simply include the token [unique] (including the []) as part of the name. The template will replace the [unique] word with a unique string of characters. For example:

| Name                   | Result                     |
| ---------------------- | -------------------------- |
| key-[unique]-deploy    | key-sd8kjdf678k9-deploy    |
| keyvault-test-[unique] | keyvault-test-sd8kjdf678k9 |

This is helpfull to ensure there will be no keyvault duplicates in Azure as it need to be unique.

#### Firewall Config and License

Two part mime message comprised of the desired firewall configuration and the fortigate license. Here is an example of the mime message:

```mime
Content-Type: multipart/mixed; boundary="===============0740947994048919689=="
MIME-Version: 1.0

--===============0740947994048919689==
Content-Type: text/plain; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="config"

config system global
    set admin-sport 8443
    set alias "PwP0FWCore01"
    set hostname "PwP0FWCore01"
    set timezone 04
end
config system interface
    edit "port1"
        set vdom "root"
        set mode dhcp
        set allowaccess ping https ssh http fgfm
        set type physical
        set description "outside"
        set alias "outside"
        set snmp-index 1
    next
    edit "port2"
        set vdom "root"
        set mode dhcp
        set type physical
        set description "inside"
        set alias "inside"
        set snmp-index 2
        set defaultgw disable
    next
    edit "port3"
        set vdom "root"
        set mode dhcp
        set type physical
        set description "hasyncport"
        set alias "hasyncport"
        set snmp-index 3
        set defaultgw disable
    next
    edit "port4"
        set vdom "root"
        set mode dhcp
        set allowaccess ping https ssh http fgfm
        set type physical
        set description "management"
        set alias "management"
        set snmp-index 4
        set defaultgw disable
    next
end
config system ha
  set group-name "AzureHA"
  set mode a-p
  set hbdev "port3" 100
  set session-pickup enable
  set session-pickup-connectionless enable
  set ha-mgmt-status enable
  config ha-mgmt-interfaces
    edit 1
      set interface "port4"
      set gateway 10.0.4.1
    next
  end
  set override disable
  set priority 255
  set unicast-hb enable
  set unicast-hb-peerip 10.0.3.5
end

--===============0740947994048919689==
Content-Type: text/plain; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="license"

-----BEGIN FGT VM LICENSE-----
QAAAAAxO36IkxkoqbVbYEKvz9qpFD+WwKJ5ilbkuiUvK8iZaCBTZ2gicYuVV1oez
O/QM4hnMFQePlvlga/gyI9iIRdnQGQAAXhNlYZQnMJLXyfS7Up6FAhQeb8bOLWu2
TjQ4y76kfL/D30OvlpCZjbQgIWwDJu4NPsZYAob2PDVg+JEhk9dAO9mqTu+gF954
OAlFW7ljUzpjyRU/sfaPwISps+O/lMi+7+vVkbSBputtdSgtIkSKqEeCkdEuoKCd
6OQbWoI9m36FOztiZhlQ3gwrJ3dV94rxvnqkxzaWIQFvByKTaVp9YpTTIGR9aa02
k5N9i6QY+OOLipKFHKlhPBicygePkm5HKGiDoi/cvYrq9kgfcgvV5YglV3d/wl50
iYx/jIOypr8kMrAVcRCrz46Nz/K5U7pC5qL8X2caT+AXVm6dOCNYwsQ/hjZl06Qg
J/StdPQzyY6KuaGryuK8ThmftycVY1adMMJeuGoj3YWoNKWI1aKkjbdFJ6qFk+0J
GMGv4KeM7/UooMiKvyST5Kupfjel/CgpjbRb6tryAvXZBszizvQmmW0CLcLxmmnf
92sSJeQZ1+qh9+EOHRK6XusKRkPfjgFsjQ41KyvdFe9rR0WIaiovSzAk2WWZDWIR
W7Rhzm9Z7PhKMi9MTl+q2gOA02I1f9hc3NVF5jdEzdnrk4AoY8FnbLvkxYSYfFGl
PfDcpqJaM8LrpJAODmqCyQSlYeBXZWnRuxkU/mN6DtkhbsTrY+KO9PVueEUTHtEf
Cu7iNZkv6E/aislDjeYgvbKCScoQZcxqX5c0a9GnoPjbMOQDlnGUu6BqK0YCvxgD
51y7om0ioi1w0U2M37xCDaeYSQDEfdMIhtIsenY1SofIpfoUdqv6ngG2jGti58pA
uH+sdb3Nuc0=
-----END FGT VM LICENSE-----

--===============0740947994048919689==--
```

## History

| Date     | Release                                                                            | Change                                                                                         |
| -------- | ---------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| 20190614 | [20190614](https://github.com/canada-ca-azure-templates/fortigateha/tree/20190614) | 1st release of template                                                                        |
| 20190619 | [20190619](https://github.com/canada-ca-azure-templates/fortigateha/tree/20190619) | Adding Internal and External LB to template.                                                   |
|          |                                                                                    | Add new template to update External LB Rules and Probes after the firewall deployment is done. |
