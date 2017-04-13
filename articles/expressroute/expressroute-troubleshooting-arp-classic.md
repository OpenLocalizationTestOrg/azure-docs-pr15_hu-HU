<properties
   pageTitle="Készült ExpressRoute hibaelhárítási útmutató: az első ARP táblák |} Microsoft Azure"
   description="Ez az oldal utasításokat az ARP táblázatok egy készült ExpressRoute áramkör ismerteti."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carolz"
   editor="tysonn"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>

# <a name="expressroute-troubleshooting-guide-getting-arp-tables-in-the-classic-deployment-model"></a>Készült ExpressRoute hibaelhárítási útmutató: a klasszikus telepítési modell tábla ARP beolvasása

> [AZURE.SELECTOR]
[A PowerShell - erőforrás-kezelő](expressroute-troubleshooting-arp-resource-manager.md)
[PowerShell - klasszikus](expressroute-troubleshooting-arp-classic.md)

Ez a cikk végigvezeti a lépéseket a cím felbontás Protocol (ARP) tábla beolvasása a készült Azure ExpressRoute áramkör.

>[AZURE.IMPORTANT] A dokumentum célja diagnosztizálása és egyszerű problémák megoldása. Nem célja helyettesítheti a Microsoft ügyfélszolgálatával. Ha az alábbi útmutatás használatával nem oldja meg a problémát, nyissa meg a támogatási kérelmet [segítséget a Microsoft Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)+ támogatás.

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>A cím felbontás Protocol (ARP) és ARP tábla
ARP az egy réteg 2 protokoll, a [826](https://tools.ietf.org/html/rfc826)meghatározott. Feleltesse meg egy Ethernet-címét (MAC) IP-címének ARP szolgál.

Egy táblázat ARP megfeleltetés IPv4-címet és egy adott peering MAC-címét. A ARP táblában egy készült ExpressRoute áramkör peering a minden kapcsolat (elsődleges és másodlagos) az alábbi információk találhatók:

1. A helyszíni útválasztó felület IP-címet a MAC-cím megfeleltetése
2. Megfeleltetésének készült ExpressRoute útválasztó felület IP-címet a MAC-cím
3. A leképezés kora

ARP táblázatok segíthetnek a réteg 2 konfigurációjának ellenőrzése és az egyszerű réteg 2 kapcsolódási problémák megoldásához.

Következő képen egy ARP táblázatot:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


A következő szakasz hogy miként tekintheti meg él készült ExpressRoute útválasztó láthatók ARP táblákhoz információt tartalmaz.

## <a name="prerequisites-for-using-arp-tables"></a>Előfeltételekről a ARP táblázatok használata

Győződjön meg arról, hogy a következő a folytatás előtt:

 - Érvényes készült ExpressRoute áramkör, amely legalább egy peering van beállítva. A kapcsolat teljes mértékben kell beállítania a kapcsolat szolgáltatója. (Vagy a kapcsolat szolgáltatója) konfigurálnia kell a peerings (Azure saját, Azure nyilvános vagy Microsoft) közül a kapcsolaton.

 - IP-címtartományok használható parancsmagokról a peerings (Azure saját, Azure nyilvános, és a Microsoft) beállítása. Tekintse át a IP-cím hozzárendelés [készült ExpressRoute útválasztási követelmények lap](expressroute-routing.md) első megértéséhez hogyan vannak megfeleltetve IP-címek a aise és a készült ExpressRoute oldalán felületek szereplő példák. A [készült ExpressRoute peering beállítása lapon](expressroute-howto-routing-classic.md)megtekintésével elérheti a peering konfigurációs adatait.

 - A hálózati csoport- vagy kapcsolódási szolgáltatóról a MAC címek adatainak az alábbi IP-címek használt kapcsolatok.

 - A legújabb Windows PowerShell-modult az Azure (1,50 vagy újabb verzió).

## <a name="arp-tables-for-your-expressroute-circuit"></a>A készült ExpressRoute áramkör ARP-táblázatok
Ez a témakör a PowerShell használatával peering hibatípusonként ARP táblázatok megjelenítése utasításokat. A folytatás előtt, vagy a saját kapcsolat szolgáltatóját kell konfigurálni a peering. Minden áramkör két elérési útját (elsődleges és másodlagos) tartalmaz. Érdemes a ARP tábla minden egyes görbéhez egymástól függetlenül.

### <a name="arp-tables-for-azure-private-peering"></a>A magánjellegű Azure peering ARP táblában
A következő parancsmagot biztosít a ARP táblák magánjellegű Azure peering:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

Az alábbiakban minta kimeneti az elérési utak egyikét:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>Az Azure nyilvános peering táblák ARP:
A következő parancsmagot biztosít a ARP táblák Azure nyilvános peering:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

Az alábbiakban minta kimeneti az elérési utak egyikét:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Az alábbiakban az egyik javaslatot minta kimeneti:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>A Microsoft peering ARP táblázatok
A következő parancsmagot biztosít a ARP táblák Microsoft peering:

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


Minta kimeneti az egyik javaslatot az alább látható:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Ez az információ használata
Egy peering ARP táblázata Layer 2 konfigurálása és a csatlakozás ellenőrzéséhez használható. Ez a szakasz áttekintést nyújt arról, hogyan ARP táblázatok különböző forgatókönyvekben.

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a>Amikor egy műveleti (várható) állapotban van egy áramkör ARP táblázat

 - A ARP táblának érvényes IP- és a MAC cím a helyszíni mellett egy bejegyzést, és a Microsoft oldalának hasonló bejegyzést.
 - A helyi IP-címének az utolsó bájt értéke mindig páratlan szám.
 - A Microsoft IP-cím az utolsó bájt értéke mindig páros szám.
 - MAC ugyanazt a címet az összes három peerings (elsődleges és másodlagos), a Microsoft oldalon megjelenik.


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-the-connectivity-provider-side-has-problems"></a>Ha a helyszíni, vagy ha a kapcsolat-szolgáltató oldalán hibát talál ARP táblázat

 Csak egy bejegyzés jelenik meg a ARP táblázatban. A hozzárendelés között a MAC és az IP-címet, amely a Microsoft oldalon megjelenik.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] Jelennek meg a problémát tapasztal, nyissa meg a támogatási kérelmet a csatlakozási szolgáltatónál, a probléma megoldásáért.


### <a name="arp-table-when-the-microsoft-side-has-problems"></a>Ha a Microsoft oldal problémák ARP táblázat

 - Nem jelenik meg egy ARP táblázat egy peering, ha problémák lépnek fel a Microsoft oldalon megjelenik.
 -  [Microsoft Azure súgó](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)+ támogatási nyissa meg a támogatási kérelmet. Adja meg, hogy réteg 2 csatlakozási problémát.

## <a name="next-steps"></a>Következő lépések

 - A készült ExpressRoute áramkör Layer 3 konfigurációjának ellenőrzése:
     - Első útvonal összefoglaló az állapot BGP tanfolyamainkat meghatározásához.
     - Ismerkedés a egy útvonal táblázatból megállapíthatja, hogy mely prefixumokban vannak közzététel készült ExpressRoute keresztül.
 - Adatátvitel ellenőrzése és kicsinyítés megtekintésével bájt.
 - Ha továbbra is problémákat tapasztal nyissa meg a támogatási kérelmet [segítséget a Microsoft Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) + támogatás.
