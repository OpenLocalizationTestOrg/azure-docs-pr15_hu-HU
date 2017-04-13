<properties 
   pageTitle="Készült ExpressRoute hibaelhárítás útmutató – az első ARP táblák |} Microsoft Azure"
   description="Ezen az oldalon nyújt útmutatást ahhoz, hogy az ARP táblázatok egy készült ExpressRoute áramkör"
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

#<a name="expressroute-troubleshooting-guide---getting-arp-tables-in-the-resource-manager-deployment-model"></a>Készült ExpressRoute hibaelhárítási útmutatója – az első ARP táblázatok az erőforrás-kezelő telepítési modell

> [AZURE.SELECTOR]
[A PowerShell - erőforrás-kezelő](expressroute-troubleshooting-arp-resource-manager.md)
[PowerShell - klasszikus](expressroute-troubleshooting-arp-classic.md)

Ez a cikk végigvezeti a lépéseket követve megtudhatja, a készült ExpressRoute áramkör ARP táblázatokban. 

>[AZURE.IMPORTANT] A dokumentum célja diagnosztizálása és egyszerű problémák megoldása. Nem célja helyettesítheti a Microsoft ügyfélszolgálatával. Meg kell nyitnia a támogatási jegyek a [Microsoft támogatási](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Ha nem tudja megoldani a problémát, használja az alábbi útmutatást.

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>A cím felbontás Protocol (ARP) és ARP tábla
Cím felbontás Protocol (ARP) egy 2 protokollnak meghatározása a [826](https://tools.ietf.org/html/rfc826). ARP feleltesse meg az Ethernet címét (MAC) ip-címmel szolgál.

A ARP táblázat megfeleltetés IPv4-címet és egy adott peering MAC-címét. Az egy készült ExpressRoute áramkör peering ARP táblázat a következő információkkal minden kapcsolat (elsődleges és másodlagos)

1. A helyszíni útválasztó felület IP-címet a MAC-cím megfeleltetése
2. Megfeleltetés készült ExpressRoute útválasztó felület IP-cím a MAC-cím
3. A hozzárendelés kora

ARP táblázatok segítségével, hogy ellenőrizze a réteg 2 konfigurációját és egyszerű hibaelhárítási réteg 2 csatlakozási problémákat tapasztal. 

Példa ARP tábla: 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


A következő szakasz meg, hogy miként tekintheti meg él készült ExpressRoute útválasztó látják ARP-táblázatok adatait találja. 

## <a name="prerequisites-for-learning-arp-tables"></a>Tanulási ARP táblázatok előfeltételei

Győződjön meg arról, hogy a következő további végrehajtási előtt

 - Érvényes készült ExpressRoute áramkör, legalább egy peering konfigurált. A kapcsolat teljes mértékben kell beállítania a kapcsolat szolgáltatója. (Vagy a kapcsolat szolgáltatója) kell konfigurálta a peerings (Azure saját, Azure nyilvános és Microsoft) közül a kapcsolaton.
 - IP-címtartományok a peerings (Azure saját, Azure nyilvános és Microsoft) beállításához használt. Tekintse át a ip-cím hozzárendelés [készült ExpressRoute útválasztási követelmények lap](expressroute-routing.md) első megértéséhez hogyan vannak megfeleltetve IP-címek felületek az oldalon, és a készült ExpressRoute oldalon szereplő példák. Peering konfigurációja információkat kaphat az [készült ExpressRoute peering beállítása lapon](expressroute-howto-routing-arm.md)áttekintésével.
 - A hálózati csoport adatait / felületek MAC azoknak a kapcsolat szolgáltatója használja az alábbi IP-címek.
 - A legújabb PowerShell-modult az Azure (1,50 vagy újabb verzió) kell rendelkeznie.

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a>A készült ExpressRoute áramkör az ARP tábla beolvasása
Ez a szakasz útmutatás miként tekintheti meg a ARP táblák / peering a PowerShell használatával. Ön vagy a kapcsolat szolgáltatója kell állította be további halad előtt peering. Minden áramkör két elérési útját (elsődleges és másodlagos) tartalmaz. Érdemes a ARP tábla minden egyes görbéhez egymástól függetlenül.

### <a name="arp-tables-for-azure-private-peering"></a>A magánjellegű Azure peering ARP táblában
A következő parancsmagot biztosít a ARP táblák magánjellegű Azure peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary
        
        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

Minta kimeneti az egyik javaslatot az alább látható

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>Azure nyilvános peering ARP táblában
A következő parancsmagot biztosít a ARP táblázatok Azure nyilvános peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary
        
        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


Minta kimeneti az egyik javaslatot az alább látható

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>A Microsoft peering ARP táblázatok
A következő parancsmagot biztosít a ARP táblázatok Microsoft peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary
        
        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


Minta kimeneti az egyik javaslatot az alább látható

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Ez az információ használata
Egy peering ARP táblázat is használható megállapításához réteghez 2 konfigurációs és kapcsolódási ellenőrzése. Ez a témakör a különböző forgatókönyvek ARP táblázatok képének áttekintése.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>Ha egy áramkör működési állapota (várható állapot) ARP táblázat

 - ARP táblázat egy érvényes IP-cím és a MAC-cím a helyszíni oldalának és a Microsoft oldalának hasonló bejegyzés fog rendelkezni. 
 - A helyi IP-címének az utolsó bájt mindig páratlan szám.
 - A Microsoft IP-címének az utolsó bájt értéke mindig páros szám.
 - MAC ugyanazt a címet az összes 3 peerings (elsődleges és másodlagos), a Microsoft oldalon jelenik meg. 


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>ARP táblázat, amikor a helyszíni / kapcsolódási szolgáltató oldalán kapcsolatban probléma merült fel

 - Csak egy bejegyzés ARP táblázat fog megjelenni. Ez a MAC és IP-címet használja a Microsoft egymás között a hozzárendelés jelennek meg. 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] A kapcsolat szolgáltatónál szeretné hibakeresése ilyen problémák nyissa meg a támogatási kérelmet. 


### <a name="arp-table-when-microsoft-side-has-problems"></a>Ha Microsoft egymás problémák ARP táblázat

 - Nem jelenik meg egy ARP táblázat egy peering, ha problémák lépnek fel a Microsoft oldalon megjelenik. 
 -  Nyissa meg a támogatási jegyek [Microsoft támogatja](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Adja meg, hogy réteghez 2 csatlakozási problémát. 

## <a name="next-steps"></a>Következő lépések

 - A készült ExpressRoute áramkör Layer 3 konfigurációjának ellenőrzése
     - Első útvonal összefoglaló BGP munkamenetek állapotának meghatározása 
     - Első útvonal táblázatból megállapíthatja, hogy melyik prefixumokban vannak közzététel keresztül készült ExpressRoute
 - Adatátvitel érvényesítése bájt be / ki megtekintésével
 - Ha továbbra is problémákat tapasztal, nyissa meg a támogatási jegyek a [Microsoft támogatási](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .
