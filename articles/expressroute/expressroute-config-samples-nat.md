<properties
   pageTitle="Készült ExpressRoute ügyfél útválasztó konfigurációs minták |} Microsoft Azure"
   description="Ezen az oldalon a Cisco és boróka útválasztó útválasztó konfigurációs minták biztosít."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>

# <a name="router-configuration-samples-to-setup-and-manage-nat"></a>Útválasztó konfigurációs minták beállítása és kezelése a hálózati Címfordítást

Ezen az oldalon Cisco ASA és a boróka SRX-Leírásban sorozat útválasztó nyújt a hálózati Címfordítást konfigurációs minták. Ezek a minták útmutatást csak szánt, és nem fogja használni, mint. Dolgozhat a szállítójával, ha a megfelelő beállításokat a hálózathoz tartozik. 

>[AZURE.IMPORTANT] Ezen a lapon a minták célja, hogy tisztán az útmutatást. A szállítójával értékesítési / technikai csapat- és a hálózati csoport, így felel meg az igényeinek megfelelő beállítással kell dolgozni. A Microsoft nem támogatja a lapon szereplő konfigurációk kapcsolatos problémákat. Az eszköz szállítói kapcsolatba kell lépnie az ügyfélszolgálati eseteit.

Az alábbi útválasztó konfigurációs minták Azure nyilvános és a Microsoft peerings vonatkoznak. A magánjellegű Azure peering nem hálózati Címfordítást kell adnia. További részletekért tekintse át a [készült ExpressRoute peerings](expressroute-circuit-peerings.md) és [készült ExpressRoute hálózati Címfordítást követelmények](expressroute-nat.md) .

**Megjegyzés:** Az internetes és készült ExpressRoute elérhetőségének külön hálózati Címfordítást IP-készletek kell használnia. Az internetes és készült ExpressRoute használata ugyanazon a hálózati Címfordítást IP-készlet aszimmetrikus Útválasztás és a kapcsolat fokú eredményezi.

## <a name="cisco-asa-firewalls"></a>Cisco ASA tűzfal

### <a name="pat-configuration-for-traffic-from-customer-network-to-microsoft"></a>A Microsoft ügyfélhálózat érkező forgalmat PÉTERHEZ konfigurációja

    object network MSFT-PAT
      range <SNAT-START-IP> <SNAT-END-IP>
    
    
    object-group network MSFT-Range
      network-object <IP> <Subnet_Mask>
    
    object-group network on-prem-range-1
      network-object <IP> <Subnet-Mask>
    
    object-group network on-prem-range-2
      network-object <IP> <Subnet-Mask>
    
    object-group network on-prem
      network-object object on-prem-range-1
      network-object object on-prem-range-2
    
    nat (outside,inside) source dynamic on-prem pat-pool MSFT-PAT destination static MSFT-Range MSFT-Range

### <a name="pat-configuration-for-traffic-from-microsoft-to-customer-network"></a>A forgalmat a Microsoft ügyfélhálózatához való PÉTERHEZ konfigurálása

#### <a name="interfaces-and-direction"></a>Kapcsolatok és irányát:
    Source Interface (where the traffic enters the ASA): inside
    Destination Interface (where the traffic exits the ASA): outside

#### <a name="configuration"></a>Konfigurációs:
Hálózati Címfordítást készlet:

    object network outbound-PAT
        host <NAT-IP>

Tároló kiszolgáló:

    object network Customer-Network
        network-object <IP> <Subnet-Mask>

Ügyfél IP-címek objektumcsoport

    object-group network MSFT-Network-1
        network-object <MSFT-IP> <Subnet-Mask>
    
    object-group network MSFT-PAT-Networks
        network-object object MSFT-Network-1

Hálózati Címfordítást parancsok:

    nat (inside,outside) source dynamic MSFT-PAT-Networks pat-pool outbound-PAT destination static Customer-Network Customer-Network


## <a name="juniper-srx-series-routers"></a>Boróka SRX-Leírásban sorozat útválasztó 

### <a name="1-create-redundant-ethernet-interfaces-for-the-cluster"></a>1. a fürt felesleges Ethernet kapcsolatok létrehozása

    interfaces {
        reth0 {
            description "To Internal Network";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 1;
            }
            unit 100 {
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
        reth1 {
            description "To Microsoft via Edge Router";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 2;
            }
            unit 100 {
                description "To Microsoft via Edge Router";
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
    }


### <a name="2-create-two-security-zones"></a>2. Hozzon létre két biztonsági zónák

 - Az adatvédelmi belső hálózati és Untrust zónába él útválasztó szemben lévő külső hálózatot
 - Megfelelő felületek hozzárendelése zónában
 - A kapcsolatok szolgáltatások engedélyezése


    biztonsági zónák {{biztonsági zónák adatvédelmi {host bejövő-forgalom {rendszer-szolgáltatások {ping;                  } {bgp; protokollok                  {reth0.100;}} felületek              }} {host bejövő-forgalom {rendszer-szolgáltatások {ping; biztonsági zónák Untrust                  } {bgp; protokollok                  {reth1.100;}} felületek              }          }      }  }


### <a name="3-create-security-policies-between-zones"></a>3. Hozzon létre eszközbiztonsági házirendek zónák között
 
    security {
        policies {
            from-zone Trust to-zone Untrust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
            from-zone Untrust to-zone Trust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
        }
    }


### <a name="4-configure-nat-policies"></a>4. a hálózati Címfordítást házirendek beállítása
 - Hozzon létre két hálózati Címfordítást készleteket. Egy használandó kimenő Microsoft és más hálózati Címfordítást forgalmat a Microsoft az ügyfélnek.
 - A megfelelő forgalom hálózati Címfordítást szabályok létrehozása

        security {
            nat {
                source {
                    pool SNAT-To-ExpressRoute {
                        routing-instance {
                            External-ExpressRoute;
                        }
                        address {
                            <NAT-IP-address/Subnet-mask>;
                        }
                    }
                    pool SNAT-From-ExpressRoute {
                        routing-instance {
                            Internal;
                        }
                        address {
                            <NAT-IP-address/Subnet-mask>;
                        }
                    }
                    rule-set Outbound_NAT {
                        from routing-instance Internal;
                        to routing-instance External-ExpressRoute;
                        rule SNAT-Out {
                            match {
                                source-address 0.0.0.0/0;
                            }
                            then {
                                source-nat {
                                    pool {
                                        SNAT-To-ExpressRoute;
                                    }
                                }
                            }
                        }
                    }
                    rule-set Inbound-NAT {
                        from routing-instance External-ExpressRoute;
                        to routing-instance Internal;
                        rule SNAT-In {
                            match {
                                source-address 0.0.0.0/0;
                            }
                            then {
                                source-nat {
                                    pool {
                                        SNAT-From-ExpressRoute;
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }


### <a name="5-configure-bgp-to-advertise-selective-prefixes-in-each-direction"></a>5. egyes irányba szelektív prefixumokban meghirdetése BGP beállítása

Keresse meg a minták [Útválasztás konfigurációs minták](expressroute-config-samples-routing.md) lapon.

### <a name="6-create-policies"></a>6. házirendek létrehozása

    routing-options {
                  autonomous-system <Customer-ASN>;
    }
    policy-options {
        prefix-list Microsoft-Prefixes {
            <IP-Address/Subnet-Mask;
            <IP-Address/Subnet-Mask;
        }
        prefix-list private-ranges {
            10.0.0.0/8;
            172.16.0.0/12;
            192.168.0.0/16;
            100.64.0.0/10;
        }
        policy-statement Advertise-NAT-Pools {
            from {
                protocol static;
                route-filter <NAT-Pool-Address/Subnet-mask> prefix-length-range /32-/32;
            }
            then accept;
        }
        policy-statement Accept-from-Microsoft {
            term 1 {
                from {
                    instance External-ExpressRoute;
                    prefix-list-filter Microsoft-Prefixes orlonger;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
        policy-statement Accept-from-Internal {
            term no-private {
                from {
                    instance Internal;
                    prefix-list-filter private-ranges orlonger;
                }
                then reject;
            }
            term bgp {
                from {
                    instance Internal;
                    protocol bgp;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
    }
    routing-instances {
        Internal {
            instance-type virtual-router;
            interface reth0.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Microsoft;
            }
            protocols {
                bgp {
                    group customer {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-ASN-1>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
        External-ExpressRoute {
            instance-type virtual-router;
            interface reth1.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Internal;
            }
            protocols {
                bgp {
                    group edge-router {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-Public-ASN>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
    }

## <a name="next-steps"></a>Következő lépések

Lásd: további információt a [Készült ExpressRoute – gyakori kérdések](expressroute-faqs.md) .
