<properties
   pageTitle="Készült ExpressRoute ügyfél útválasztó konfigurációs minták |} Microsoft Azure"
   description="Ezen az oldalon útválasztó config minták nyújt a Cisco és boróka útválasztó."
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

# <a name="router-configuration-samples-to-setup-and-manage-routing"></a>Útválasztó konfigurációs minták beállítása és kezelése a továbbítás

Ezen az oldalon nyújt a felület és a útválasztási konfigurációs minták Cisco IOS-XE és a boróka MX sorozat útválasztó. Ezek a minták útmutatást csak szánt, és nem fogja használni, mint. Dolgozhat a szállítójával, ha a megfelelő beállításokat a hálózathoz tartozik. 

>[AZURE.IMPORTANT] Ezen a lapon a minták célja, hogy tisztán az útmutatást. A szállítójával értékesítési / technikai csapat- és a hálózati csoport, így felel meg az igényeinek megfelelő beállítással kell dolgozni. A Microsoft nem támogatja a lapon szereplő konfigurációk kapcsolatos problémákat. Az eszköz szállítói kapcsolatba kell lépnie az ügyfélszolgálati eseteit.

Az alábbi útválasztó konfigurációs minták összes peerings vonatkoznak. További információt a továbbítás tekintse át a [készült ExpressRoute peerings](expressroute-circuit-peerings.md) és [készült ExpressRoute útválasztási követelményeknek](expressroute-routing.md) .

## <a name="cisco-ios-xe-based-routers"></a>Cisco IOS-XE útválasztó alapján

Ebben a szakaszban a minták bármely az IOS-XE operációs rendszerű útválasztó vonatkoznak.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. felületek és a részleges kapcsolatok konfigurálása

A minden Microsoft való csatlakozás útválasztó peering per sub felhasználói felülete van szükség. Sub felhasználói felülete azonosítania kell magát egy virtuális Azonosítót vagy halmozott két virtuális azonosítókhoz és az IP-címet.

#### <a name="dot1q-interface-definition"></a>Dot1Q felület meghatározása

Ez a példa biztosít a alszint felület definícióját egy alszint felület egy egyetlen virtuális azonosítójával. A virtuális Azonosítóra egy peering egyedi. Az utolsó bájt a IPv4-címek mindig páratlan szám.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

#### <a name="qinq-interface-definition"></a>QinQ felület meghatározása

Ez a példa két virtuális lehetővé tevő dokumentumazonosítók egy alszint felületen alszint felület definíciója biztosít. A külső virtuális Azonosítót (s-kódot), ha a használt változatlan marad, az összes peerings keresztül. A belső virtuális-azonosító (c-címke) egy peering egyedi. Az utolsó bájt a IPv4-címek mindig páratlan szám.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>
    
### <a name="2-setting-up-ebgp-sessions"></a>2. a munkamenetek eBGP beállítása

Az összes peering Microsoft BGP munkamenetet beállítása Az alábbi példa lehetővé teszi a Microsoft BGP munkamenetet beállítása. Ha a IPv4-címet használta az sub felületén a.b.c.d volt, a BGP szomszédos (Microsoft) IP-címe lesz a.b.c.d+1. Az utolsó bájt a BGP szomszédos IPv4-címet a program mindig páros szám.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. beállítása prefixumokban a BGP munkamenet fölé felhasználási

Beállíthatja, hogy az útválasztóra meghirdetése választó prefixumokban a Microsoftnak. Ehhez az alábbi minta használatával.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a>4. útvonal megfeleltetések

Használhatja az útvonal-megfeleltetések, és a előtag szűrő prefixumokban propagálja a hálózatba való látható. A feladat elvégzéséhez az alábbi minta is használhatja. Győződjön meg arról, hogy megfelelő előtagot listák beállítása.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
      neighbor <IP#2_used_by_Azure> route-map <MS_Prefixes_Inbound> in
     exit-address-family
    !
    route-map <MS_Prefixes_Inbound> permit 10
     match ip address prefix-list <MS_Prefixes>
    !


## <a name="juniper-mx-series-routers"></a>MX boróka sorozat útválasztó 

Ebben a szakaszban a minták bármely boróka MX adatsor útválasztó alkalmazni.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. felületek és a részleges kapcsolatok konfigurálása

#### <a name="dot1q-interface-definition"></a>Dot1Q felület meghatározása

Ez a példa biztosít az almenüből felület definícióját alszint felhasználói felülete egy egyetlen virtuális azonosítójával. A virtuális Azonosítóra egy peering egyedi. Az utolsó bájt a IPv4-címek mindig páratlan szám.

    interfaces {
        vlan-tagging;
        <Interface_Number> {
            unit <Number> {
                vlan-id <VLAN_ID>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }
            }
        }
    }


#### <a name="qinq-interface-definition"></a>QinQ felület meghatározása

Ez a példa két virtuális lehetővé tevő dokumentumazonosítók egy alszint felületen alszint felület definíciója biztosít. A külső virtuális Azonosítót (s-kódot), ha a használt változatlan marad, az összes peerings keresztül. A belső virtuális-azonosító (c-címke) egy peering egyedi. Az utolsó bájt a IPv4-címek mindig páratlan szám.

    interfaces {
        <Interface_Number> {
            flexible-vlan-tagging;
            unit <Number> {
                vlan-tags outer <S-tag> inner <C-tag>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }                           
            }                               
        }                                   
    }                           

### <a name="2-setting-up-ebgp-sessions"></a>2. a munkamenetek eBGP beállítása

Az összes peering Microsoft BGP munkamenetet beállítása Az alábbi példa lehetővé teszi a Microsoft BGP munkamenetet beállítása. Ha a IPv4-címet használta az sub felületén a.b.c.d volt, a BGP szomszédos (Microsoft) IP-címe lesz a.b.c.d+1. Az utolsó bájt a BGP szomszédos IPv4-címet a program mindig páros szám.

    routing-options {
        autonomous-system <Customer_ASN>;
    }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. beállítása prefixumokban a BGP munkamenet fölé felhasználási

Beállíthatja, hogy az útválasztóra meghirdetése választó prefixumokban a Microsoftnak. Ehhez az alábbi minta használatával.

    policy-options {
        policy-statement <Policy_Name> {
            term 1 {
                from protocol OSPF;
        route-filter <Prefix_to_be_advertised/Subnet_Mask> exact;
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }


### <a name="4-route-maps"></a>4. maps útvonal

Útvonal-térképek és a szűrő prefixumokban a hálózatba propagált listák előtagot. A feladat elvégzéséhez az alábbi minta is használhatja. Győződjön meg arról, hogy megfelelő előtagot listák beállítása.

    policy-options {
        prefix-list MS_Prefixes {
            <IP_Prefix_1/Subnet_Mask>;
            <IP_Prefix_2/Subnet_Mask>;
        }
        policy-statement <MS_Prefixes_Inbound> {
            term 1 {
                from {
        prefix-list MS_Prefixes;
                }
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                import <MS_Prefixes_Inbound>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

## <a name="next-steps"></a>Következő lépések

Lásd: további információt a [Készült ExpressRoute – gyakori kérdések](expressroute-faqs.md) .
