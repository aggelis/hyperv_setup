# hyperv_setup

## Create an internal Virtual Switch

> New-VMSwitch -Name DTN-TESTBED -SwitchType Internal

## Get Interface Index 

Use the following command to get the interface index of the virtual switch created in 
previous step

> Get-NetAdapter

## Set IP Addreass on internal VSwitch 

Set the ip address on Created VSwitch. This will be used as the gateway for all VMs that will be attached 
in this VSwitch

> New-NetIPAddress –IPAddress 10.10.40.1 -PrefixLength 24 -InterfaceIndex 28

## Set up NAT for VSWITCH network 

>  New-NetNat -Name Nat-DTNTB -InternalIPInterfaceAddressPrefix 10.10.40.0/24


## Create ssh port forwarding 

For example we have created two VMs with IPs 10.10.40.100 and 10.10.40.101. We setup SSH port forwarding to 
these VMs as follows 

> Add-NetNatStaticMapping -ExternalIPAddress "0.0.0.0/24" -ExternalPort 40100 -Protocol TCP -InternalIPAddress "10.10.40.100" -InternalPort 22 -NatName Nat-DTNTB

```
StaticMappingID               : 0
NatName                       : Nat-DTNTB
Protocol                      : TCP
RemoteExternalIPAddressPrefix : 0.0.0.0/0
ExternalIPAddress             : 0.0.0.0
ExternalPort                  : 40100
InternalIPAddress             : 10.10.40.100
InternalPort                  : 22
InternalRoutingDomainId       : {00000000-0000-0000-0000-000000000000}
Active                        : True
```

> Add-NetNatStaticMapping -ExternalIPAddress "0.0.0.0/24" -ExternalPort 40101 -Protocol TCP -InternalIPAddress "10.10.40.101" -InternalPort 22 -NatName Nat-DTNTB

```
StaticMappingID               : 1
NatName                       : Nat-DTNTB
Protocol                      : TCP
RemoteExternalIPAddressPrefix : 0.0.0.0/0
ExternalIPAddress             : 0.0.0.0
ExternalPort                  : 40101
InternalIPAddress             : 10.10.40.101
InternalPort                  : 22
InternalRoutingDomainId       : {00000000-0000-0000-0000-000000000000}
Active                        : True
```

REFERENCES 

- [How to create a nat VSWITCH network](https://activedirectorypro.com/how-to-create-a-nat-switch-on-hyper-v/)
- [How to add static mappings (port forwarding) 1](https://stackoverflow.com/questions/34238308/set-up-port-forwarding-on-windows-10-nat-virtual-switch)
- [How to add static mappings (port forwarding) 2](https://superuser.com/questions/1445288/port-forwarding-with-hyper-v-virtual-machine-on-windows-10)
