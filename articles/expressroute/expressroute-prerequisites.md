<properties
   pageTitle="Készült ExpressRoute elfogadása előfeltételei |} Microsoft Azure"
   description="Ezen az oldalon itt az Azure készült ExpressRoute áramkör sorrendbe is teljesítendő követelmények listáját."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>


# <a name="expressroute-prerequisites--checklist"></a>Készült ExpressRoute Előfeltételek és a feladatlista  

A Microsoft-felhőszolgáltatások készült ExpressRoute használatával szeretne csatlakozni, kell ellenőrizze, hogy teljesülnek-e az alábbi követelményeket, az alábbi szakaszokban szerepel.

[AZURE.INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Azure-fiók

- Egy érvényes és az aktív Microsoft Azure-fiókjába. Ez a készült ExpressRoute áramkör beállításához szükséges. Készült ExpressRoute áramkörök belül Azure előfizetések erőforrások. Az Azure-előfizetés a követelmény, akkor is, ha kapcsolat Azure Microsoft felhőszolgáltatásokhoz, például az Office 365-szolgáltatások és CRM online korlátozott.
- Egy aktív Office 365-előfizetés (ha az Office 365 szolgáltatásainak használata). Az [adott követelményeket az Office 365-ben](#office-365-specific-requirements) című a jelen cikk további információt.

## <a name="connectivity-provider"></a>Kapcsolat-szolgáltató
- Egy [készült ExpressRoute kapcsolódási partner](expressroute-locations.md#partners) csatlakozni a Microsoft cloud dolgozhat. Beállíthatja, hogy a helyszíni hálózaton és a Microsoft közötti kapcsolatot [háromféleképpen](expressroute-introduction.md#howtoconnect). 
- Ha a szolgáltató nem egy készült ExpressRoute kapcsolódási partnernek, továbbra is csatlakozhat a Microsoft cloud egy [felhőalapú exchange szolgáltató](expressroute-locations.md#nonpartners)keresztül.

## <a name="network-requirements"></a>Hálózati vonatkozó követelmények
- **Felesleges kapcsolatot**: nem redundancia követelmény a fizikai kapcsolattól Ön és a szolgáltató között. A Microsoft kell-e be kell állítani a Microsoft útválasztó és a peering útválasztó között, akkor, ha csak [egyetlen fizikai kapcsolat az a felhő exchange](expressroute-faqs.md#onep2plink)felesleges BGP munkamenetek. 
- **Útválasztás**: attól függően, hogy hogyan a Microsoft Cloud kapcsolódni, akkor vagy szolgáltatójától kell beállítása és kezelése a BGP munkamenetek [útválasztási](expressroute-circuit-peerings.md)tartományok. Néhány Ethernet kapcsolat szolgáltatója vagy a felhőben exchange szolgáltató ajánlhat fel BGP-kezelés, mint egy érték-szolgáltatás hozzáadása.
- **Hálózati Címfordítást**: Microsoft csak fogadja el a nyilvános IP-címek Microsoft peering keresztül. Saját IP-címek a helyszíni hálózaton használnak, ha Ön vagy a szolgáltató szükséges lefordítása a saját IP-címek nyilvános IP-címét a [a hálózati Címfordítást használ](expressroute-nat.md).
- **QoS**: Skype for Business van szükség különböző szolgáltatások (például hang, videó-és szöveg) megkülönböztetni QoS kezelése. Ön és a szolgáltató [QoS követelmények](expressroute-qos.md)kell követnie.
- **Hálózati biztonság**: a Microsoft Cloud készült ExpressRoute keresztül való csatlakozáskor [hálózati biztonsági](../best-practices-network-security.md) figyelembe.
 
## <a name="office-365"></a>Az Office 365-ben

Ha ahhoz, hogy az Office 365 készült ExpressRoute, olvassa el az Office 365 követelményeiről további információt a következő dokumentumokat.


- [Az Office 365-höz készült ExpressRoute áttekintése](https://support.office.com/en-us/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
- [Office 365-höz készült ExpressRoute Útválasztás](https://support.office.com/en-us/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
- [Az Office 365 URL-EK és IP-címtartományai](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
- [Hálózattervezés és teljesítményhangolás az Office 365-ben](https://support.office.com/en-us/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
- [Hálózati sávszélesség-kalkulátorok és eszközök](https://support.office.com/en-us/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
- [Az Office 365 integrációja helyszíni környezetekbe](https://support.office.com/en-us/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)

## <a name="crm-online"></a>CRM Online 
Ha CRM Online-ban készült ExpressRoute a engedélyezése, olvassa el a CRM Online további információt a következő dokumentumok

- [CRM Online URL-ek](https://support.microsoft.com/kb/2655102) és [IP-címtartományok](https://support.microsoft.com/kb/2728473)

## <a name="next-steps"></a>Következő lépések

- Készült ExpressRoute kapcsolatos további tudnivalókért olvassa el a [Készült ExpressRoute – gyakori kérdések](expressroute-faqs.md)című témakört.
- -Készült ExpressRoute kapcsolódási szolgáltató keresése. Lásd: [készült ExpressRoute partnerek és peering helyek](expressroute-locations.md).
- Keresse meg az [Útválasztás](expressroute-routing.md), [hálózati Címfordítást](expressroute-nat.md) és [QoS](expressroute-qos.md).
- Állítsa be a készült ExpressRoute kapcsolatot.
    - [Készült ExpressRoute áramkör létrehozása](expressroute-howto-circuit-classic.md)
    - [Útválasztás konfigurálása](expressroute-howto-routing-classic.md)
    - [Hivatkozás egy VNet egy készült ExpressRoute áramkör](expressroute-howto-linkvnet-classic.md)

