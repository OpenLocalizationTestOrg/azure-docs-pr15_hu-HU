<properties
   pageTitle="Azure biztonsági kezelése és a figyeléshez áttekintése |} Microsoft Azure"
   description=" Azure biztonsági mechanizmusok kezelését, és Azure felhőszolgáltatások és a virtuális gépeken futó ellenőrzésére támogatási biztosít.  Ez a cikk áttekintést nyújt ezen core biztonsági funkciók és szolgáltatások. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="azure-security-management-and-monitoring-overview"></a>Azure biztonsági kezelése és a figyeléshez – áttekintés

Azure biztonsági mechanizmusok kezelését, és Azure felhőszolgáltatások és a virtuális gépeken futó ellenőrzésére támogatási biztosít. Ez a cikk áttekintést nyújt ezen core biztonsági funkciók és szolgáltatások. Hivatkozások találhatók, további részleteket az egyes ad foglalkozó cikkekre.

A Microsoft cloud services biztonsága pedig egy partnerség felelősség megosztott és a Microsoft között. Megosztott felelősség azt jelenti, hogy Microsoft felelős a a Microsoft Azure és az adatok biztonsága fizikai középre igazítása (való biztonsági védelem például zárolt jelvény bejegyzés ajtók, kerítések és őrök). Azure emellett felhő biztonsági nagy ügyfelei biztonság, adatvédelem és megfelelőség igényeinek megfelelő szoftver rétegben erős szintek.

Ön a tulajdonosa az adatok és identitások, a védelmet őket, a biztonsági erőforrásait a helyszíni és felhőbeli összetevők fölé, amelyhez hozzáférése van a biztonsági felelősség. A Microsoft biztosít a biztonsági funkciók és az adatokat, és az alkalmazások védelméhez képességeit. A biztonsági felelősséget felhőalapú szolgáltatás típusú alapul.

A következő diagramon áttekintheti a egyenleg felelősséget vállal Microsoft, mind az ügyfélnek.

![Megosztott felelősség][1]

Egy mélyebb merülési be biztonsági kezelése olvassa el a [biztonsági kezelése Azure-ban](azure-security-management.md)című témakört.

Íme az alapvető funkcióit, a jelen cikkben nem vonatkozik:

- Szerepköralapú hozzáférés-vezérlés
- Antimalware
- Többtényezős hitelesítés
- Készült ExpressRoute
- Virtuális hálózati átjárók
- Kiemelt Identitáskezelés
- Identitás-védelem
- Biztonság otthon

## <a name="role-based-access-control"></a>Szerepköralapú hozzáférés-vezérlés

Szerepköralapú hozzáférés-vezérlő (RBAC) Azure erőforrásokhoz tartozó külön access management biztosít. RBAC használ, adhat személyek csak munkájuk elvégzéséhez szükségük van hozzáférési összegét.  RBAC is biztosíthatja, hogy amikor személyek a szervezetben hagyja őket elveszíti hozzáférését erőforrásokat a felhőben.

tudj meg többet:

- [A RBAC az Active Directory csapatának blogja](http://i1.blogs.technet.com/b/ad/archive/2015/10/12/azure-rbac-is-ga.aspx)
- [Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md)

## <a name="antimalware"></a>Antimalware

Azure használhatja például a Microsoft, Symantec, Trend Micro, McAfee és Kaspersky főbb biztonsági szállítóktól antimalware szoftver védelmet a virtuális gépeken futó rosszindulatú fájlokat, adatokat és más kockázatok.

A Microsoft Antimalware felajánlja az azt jelenti, hogy egy antimalware ügynököt a PaaS szerepkörök és a virtuális gépeken futó. System Center Endpoint Protection alapul, ez a funkció életre igazolt helyszíni biztonsági technológia a felhőben.

A Trend- [Mély biztonsági](http://www.trendmicro.com/us/enterprise/cloud-solutions/deep-security/)mély integrációs is kínálunk™ és [SecureCloud](http://www.trendmicro.com/us/enterprise/cloud-solutions/secure-cloud/)™ az Azure platform-terméket. DeepSecurity vírusvédelmi megoldást, SecureCloud pedig egy titkosító megoldás. DeepSecurity fog szolgálni belül VMs-bővítmény modellt használja. A portál felhasználói felület és a PowerShell használata esetén megadhatja, hogy be van folyamatban is új VMs vagy meglévő VMs már telepített DeepSecurity használni.

Azure is támogatott Symantec végpont védelem (SZEPT). Portál programmal az ügyfelek belül egy virtuális SZEPT használni kíván megadásának lehetősége van. SZEPT egy teljesen új virtuális az Azure-portálon keresztül telepíthető, vagy egy meglévő virtuális PowerShell szolgáltatással telepíthető.

tudj meg többet:

- [Azure virtuális gépeken Antimalware megoldások telepítése](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
- [Az Azure Felhőszolgáltatások és a virtuális gépeken futó Microsoft Antimalware](../security/azure-security-antimalware.md)
- [Telepítse és állítsa be a Trend Micro-mély biztonsági kattintson egy Windows virtuális szolgáltatásként hogyan](../virtual-machines/virtual-machines-windows-classic-install-trend.md)
- [Telepítése és beállítása a Windows virtuális Symantec Endpoint Protection](../virtual-machines/virtual-machines-windows-classic-install-symantec.md)
- [Új Antimalware beállításainak Azure virtuális gépeken futó – McAfee Endpoint Protection védelme](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)

## <a name="multi-factor-authentication"></a>Többtényezős hitelesítés

Azure többtényezős hitelesítés (MFA), amelyek egynél több ellenőrzési módszer van szükség, és hozzáadja a kritikus második réteg biztonsági felhasználói bejelentkezések, és a tranzakciók hitelesítési módszer. MFA segít védelmét hozzáférés az adatokhoz, és az alkalmazások felhasználói igény szerint egy egyszerű bejelentkezési folyamat az értekezlet közben. Via ellenőrzési beállítások adattartomány erős hitelesítés kézbesíti – telefonhívás, szöveges üzenet vagy mobilalkalmazásban értesítési vagy ellenőrzési kódot és 3rd fél elfogadható tokenek.

tudj meg többet:

- [Többtényezős hitelesítés](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
- [Mi az Azure többtényezős hitelesítést?](../multi-factor-authentication/multi-factor-authentication.md)
- [Hogyan működik a Azure többtényezős hitelesítés](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="expressroute"></a>Készült ExpressRoute

Microsoft Azure készült ExpressRoute teszi lehetővé a helyszíni hálózatok kiterjeszti a Microsoft cloud könnyíteni a kapcsolat szolgáltatója saját személyes kapcsolaton keresztül. Készült ExpressRoute, a Microsoft felhőszolgáltatásokhoz, például a Microsoft Azure, Office 365-ben és CRM Online kapcsolatokat hozhat létre. Kapcsolat lehet egy bármely-a-bármely (IP-VPN) hálózat, egy Ethernet pontok közötti hálózati vagy egy kapcsolatot a helymegosztást létesítmény közvetítésével virtuális határokon-kapcsolatot. Készült ExpressRoute kapcsolatok megy nyilvános interneten keresztül. Ebben a csoportban adhatja készült ExpressRoute kapcsolatok további megbízhatóság, gyorsabb sebesség, alsó késések vagy magasabb szintű biztonságos, mint a szokásos kapcsolatok kínálatát az interneten keresztül.

tudj meg többet:

- [Technikai áttekintés készült ExpressRoute](../expressroute/expressroute-introduction.md)

## <a name="virtual-network-gateways"></a>Virtuális hálózati átjárók

Virtuális Magánhálózati átjárók, más néven Azure virtuális hálózati átjárók segítségével virtuális hálózatok és a helyszíni helyek közötti hálózati forgalmat. Azure (VNet VNet) belül több virtuális hálózatok között forgalmat is használják.  Virtuális Magánhálózati átjárók Azure és a infrastruktúra biztonságos határokon helyszíni összekapcsolását adja meg.

tudj meg többet:

- [Tudnivalók a virtuális Magánhálózati átjárók](../vpn-gateway/vpn-gateway-about-vpngateways.md)
- [Azure hálózati biztonsági – áttekintés](security-network-overview.md)

## <a name="privileged-identity-management"></a>Kiemelt Identitáskezelés

Előfordul, hogy a felhasználóknak kell az Azure erőforrások vagy más szoftver alkalmazások jogosultsággal rendelkező tevékenység végzését. Ez általában azt jelenti szervezetek betekintést állandó Yammerhez az Azure Active Directory (Azure Active Directory). Ez a felhőben tárolt erőforrások növekvő biztonsági kockázatot jelentenek azért, mert a szervezetek elég nem lehet figyelni a mit tesznek azoknak a felhasználóknak az hozzáférési jogosultsággal rendelkező.
Ezenkívül hozzáférési jogosultsággal rendelkező felhasználói fiókot sérül meg, ha, hogy egy megszegése hatással lehetnek a felhőben védelméért. Azure Active Directory jogosultsággal rendelkező Identitáskezelés segít a kockázat csökkentése jogosultságokkal kapta időpontját és a betekintést kap abba, használatát növelésével feloldásához.  

Kiemelt Identitáskezelés bemutatja a egy ideiglenes rendszergazdai szerepkör vagy a "csak az idő" rendszergazdai hozzáférés, amely a felhasználó, aki a kiosztott szerepkör az aktiválási folyamat befejezéséhez szükséges. Az aktiválási folyamat változásai annak a felhasználónak a hozzárendelés szerepkörbe Azure AD az aktív, egy adott időben inaktív időszak oszlopból, például 8 órát.

tudj meg többet:

- [Azure Active Directory jogosultsággal rendelkező Identitáskezelés](../active-directory/active-directory-privileged-identity-management-configure.md)
- [Azure Active Directory Yammerhez Identitáskezelés – első lépések](../active-directory/active-directory-privileged-identity-management-getting-started.md)

## <a name="identity-protection"></a>Identitás-védelem

Azure Active Directory (AD) identitás védelem gyanús bejelentkezési tevékenységek és a vállalkozás védelme érdekében potenciális biztonsági összevont nézetének biztosít. Identitás védelem észleli a felhasználóknak a gyanús tevékenységek és a védett (rendszergazda) identitásainak próbálkozásos támadások, például jelek alapján kiszivárogtatott hitelesítő adatait, és a bejelentkezési bővítmények ismeretlen helyről eszközök fertőzött.

Értesítések és ajánlott remediation megadásával identitást védelem segít valós idejű kockázatok csökkentésében. Felhasználói kockázat súlyosságát számítja ki, és beállíthatja a kockázat alapú házirendek automatikusan hozzáférni a jövőbeni veszélyek védelmét alkalmazás segítségével.

tudj meg többet:

- [Azure Active Directory identitás-védelem](../active-directory/active-directory-identityprotection.md)
- [Csatorna 9: Azure Active Directory és a Megjelenítés identitás: identitás védelem előzetes verzió](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="security-center"></a>Biztonság otthon

Azure biztonság otthon segítségével megakadályozhatja, hogy, észleli és veszélyek válaszolni, és itt növeli a betekintést kap abba, és szabályozni kell, az Azure erőforrások biztonsága. Integrált adatvédelem figyelése és a Csoportházirend kezelése az Azure-előfizetésekben biztosít, segít észleli, egyéb esetben lépjen észrevétlen veszélyek és dolgozik egy széles ökológiai biztonsági megoldásokkal együtt.

Biztonság otthon segítségével optimalizálásához, illetve az Azure erőforrások által biztonsága figyelni:

- Engedélyezése a vállalat igényeinek, és milyen típusú alkalmazásokat vagy magánjellegű az adatok megfelelően az Azure előfizetés erőforrások házirendek megadhatja az egyes előfizetések.
- Az Azure virtuális gépeken futó, a hálózathasználatra és az alkalmazások állapotának ellenőrzése.
- Rangsorolt biztonsági figyelmeztetések, többek között integrált partnermegoldások együtt a gyorsan vizsgálja meg a szükséges információkat, és arra vonatkozó javaslatokat támadások ismételt érkező riasztások listájával.

tudj meg többet:

- [Azure biztonság otthon – bevezetés](../security-center/security-center-intro.md)

<!--Image references-->
[1]: ./media/security-management-and-monitoring-overview/shared-responsibility.png
