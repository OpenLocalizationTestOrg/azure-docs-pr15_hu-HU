<properties
   pageTitle="Biztonsági ajánlások az Azure biztonság otthon kezelése |} Microsoft Azure"
   description="A dokumentum végigvezeti hogyan javaslatok az Azure biztonság otthon segítik az Azure erőforrások védelme és biztonsági házirendek megfelelően maradjon."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/25/2016"
   ms.author="terrylan"/>

# <a name="managing-security-recommendations-in-azure-security-center"></a>Biztonsági ajánlások az Azure biztonság otthon kezelése

A dokumentum végigvezeti az Azure biztonság otthon javaslatok használata az Azure erőforrások védelméhez.

> [AZURE.NOTE] A dokumentum egy példa telepítési használatával vezet be a szolgáltatást.  Ez a cikk részletesen ismerteti nem.

## <a name="what-are-security-recommendations"></a>Mik azok a javaslatok biztonsági?
Biztonság otthon rendszeres elemzi a biztonsági állapot Azure erőforrásait. Ha a biztonság otthon azonosítja a potenciális biztonsági rés, javaslatok hoz létre. A javaslatok végigvezeti Önt a folyamaton, konfigurálása a szükséges vezérlőelemeket.

## <a name="implementing-security-recommendations"></a>Javaslatok biztonsági végrehajtása

### <a name="set-recommendations"></a>Javaslatok beállítása

A [Beállítás biztonsági házirendek az Azure biztonság otthon](security-center-policies.md), megtudhatja, hogy miként:

- Eszközbiztonsági házirendek beállítása.
- Kapcsolja be a adatgyűjtés.
- Válassza a mely javaslatok megjelenítéséhez a Yammer biztonsági házirendje részeként.

Aktuális házirend javaslatok központ körül a frissítéseket a rendszer, eredeti szabályokat, antimalware programok alhálózat és az hálózati kapcsolatok, a SQL-adatbázis naplózás, a SQL-adatbázis átlátszó adatok titkosítás és a webes alkalmazás tűzfalak [hálózati biztonsági csoportokat](../virtual-network/virtual-networks-nsg.md) .  [Eszközbiztonsági házirendek beállítása](security-center-policies.md) az egyes ajánlási beállítások leírását tartalmazza.

### <a name="monitor-recommendations"></a>Monitor javaslatok
Biztonsági házirendek beállítása, után a biztonság otthon elemzi az erőforrások azonosítása potenciális biztonsági biztonsági állapotát. A **Biztonság otthon** lap a **javaslatok** csempe jelzi, hogy a biztonság otthon jelölt javaslatok száma.

![Javaslatok csempe][1]

Minden egyes ajánlási részleteinek megtekintéséhez:

1. Kattintson a **javaslatok csempe** a **Biztonság otthon** lap. A **javaslatok** lap megnyitása

Táblázatos formátumban, ahol minden sora egy adott ajánlási felel meg a javaslatok jelennek meg. Ez a tábla oszlopainak a következők:

- **Leírás**: ismerteti, hogy az ajánlást, és mit kell még hátra foglalkozik.
- **Erőforrás**: Megjeleníti az erőforrásokat, amelyre a javaslat vonatkozik.
- **Állapot**: az aktuális állapotát az ajánlási ismerteti:
    - **Nyitott**: A ajánlási még nem javított.
    - **Folyamatban**: ajánlása az aktuálisan alkalmazott forrásokat, és ilyenkor nem szükséges, Ön által.
    - **Feloldva**: az ajánlási már elvégzett (ebben az esetben a sor fog kell szürkén jelenik meg).
- **SÚLYOSSÁGÁT**: ismerteti, hogy az adott ajánlási súlyosságát:
    - **Magas**: biztonsági értelmes erőforrás (például egy alkalmazást, egy virtuális vagy hálózati biztonsági csoport) van, és beavatkozást igénylő ügyekben.
    - **Közepes**: rés, és nem kritikus, illetve további lépésekre szükség, akkor kiküszöbölése érdekében vagy a folyamat befejezéséhez.
    - **Alacsony**: olyan rés, amely kell foglalkozni, de nem azonnali figyelmet igénylő. (Alapértelmezés szerint alacsony javaslatok nem bemutatni, de szűrést végezhet alacsony javaslatok, ha meg szeretné jeleníteni őket.)

A hivatkozások segít megérteni a rendelkezésre álló javaslatok, és mindegyik fog mire alkalmazhat, ha az alábbi táblázatban használják.

> [AZURE.NOTE] Megtudhatja, hogy a [hagyományos és erőforrás-kezelő telepítési modellek](../azure-classic-rm.md) Azure erőforrások érdemes.

|Javaslat|Leírás|
|-----|-----|
|[Engedélyezése előfizetésben adatgyűjtés](security-center-enable-data-collection.md)|Javasolja, hogy bekapcsolja az egyes az előfizetések és az összes virtuális gépeken futó (VMs) a biztonsági házirendben adatgyűjtés az előfizetések.|
|[Operációs rendszer biztonsági ismételt](security-center-remediate-os-vulnerabilities.md)|Ajánlja igazíthatja a az operációs rendszer konfigurációk ajánlott konfiguráció szabályokkal, például nem engedélyezik a jelszavak szeretné menteni.|
|[Rendszer-frissítések telepítése](security-center-apply-system-updates.md)|Javasolja, hogy beállítaná hiányzó rendszer biztonsági és a fontos frissítések VMs.|
|[Indítsa újra a rendszer frissítés után](security-center-apply-system-updates.md#reboot-after-system-updates)|Javasolja, indítsa újra a virtuális alkalmazásának rendszerben frissítéseket a folyamat befejezéséhez.|
|[A webes alkalmazás tűzfal hozzáadása](security-center-add-web-application-firewall.md)|Javasolja, hogy a webes alkalmazás tűzfalat (WAF) webes végpontok rendszerbe. A biztonság otthon több webalkalmazások megvédheti ezeket az alkalmazásokat, a meglévő WAF környezetekben való hozzáadásával. (Az erőforrás-kezelő telepítési modell használatával létrehozott) WAF készülékek kell telepíthetők külön virtuális hálózathoz. (A klasszikus telepítési modell használatával létrehozott) WAF készülékek korlátozva hálózati biztonsági csoport használata. Ez a támogatás a jövőben kiterjesztik egy teljesen testre szabott példányhoz egy WAF készülék (klasszikus). Biztonság otthon fog javasoljuk, hogy a segíti a webalkalmazások VMs és alkalmazás szolgáltatási környezetben (SKB) kiválasztásával támadások elleni védelmét a WAF kiépítése. ASE kapcsolatos további információért lásd: [Alkalmazás szolgáltatási környezetben dokumentációt](../app-service/app-service-app-service-environments-readme.md). |
|[Alkalmazás védelmének véglegesítése](security-center-add-web-application-firewall.md#finalize-application-protection)|Egy WAF konfigurációja befejezéséhez forgalom WAF készülékre kell átirányítva. Ennek ellenére követő befejezi a szükséges az oldalbeállítások módosításához.|
|[A következő generációs tűzfal hozzáadása](security-center-add-next-generation-firewall.md)|Javasolja, hogy felvette a következő generációs tűzfal (NGFW) Microsoft partnertől a biztonsági védelem növelése érdekében.|
|[Útvonal forgalom az NGFW csak](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only)|Javasolja úgy beállítani, hogy hálózati biztonsági csoport (NSG) szabályok kényszerítése bejövő forgalmat a virtuális a NGFW keresztül.|
|[Az Endpoint Protection telepítése](security-center-install-endpoint-protection.md)|Javasolja, hogy a VMs (csak a Windows VMs) antimalware programok kiépítése.|
|[Az Endpoint Protection állapot riasztások feloldása](security-center-resolve-endpoint-protection-health-alerts.md)|Javasolja, hogy a endpoint protection hibák megoldásához.|
|[Hálózati biztonsági csoportok alhálózat vagy virtuális gépeken futó engedélyezése](security-center-enable-network-security-groups.md)|Alhálózat vagy VMs NSGs engedélyezését javasolja.|
|[Szemben lévő végpont interneten keresztül hozzáférés korlátozása](security-center-restrict-access-through-internet-facing-endpoints.md)|Azt a bejövő szabályok beállítása NSGs javasolja.|
|[Naplózás az SQL server engedélyezése](security-center-enable-auditing-on-sql-servers.md)|Javasolja, kapcsolja be a naplózás az Azure SQL-kiszolgálók (Azure SQL szolgáltatás csak; nem tartalmazza a virtuális gépeken futó SQL).|
|[Adatbázis SQL a naplózás engedélyezése](security-center-enable-auditing-on-sql-databases.md)|Javasolja, kapcsolja be a naplózás az Azure SQL-adatbázisok (Azure SQL szolgáltatás csak; nem tartalmazza a virtuális gépeken futó SQL).|
|[Áttetsző titkosítás engedélyezéséhez a SQL-adatbázis](security-center-enable-transparent-data-encryption.md)|(Csak az Azure SQL szolgáltatás) SQL-adatbázisok titkosítása engedélyezését javasolja.|
|[Virtuális ügynök engedélyezése](security-center-enable-vm-agent.md)|Lehetővé teszi, hogy mely VMs megkövetelése a virtuális Agent. A virtuális Agent kell telepíteni kell VMs kiépítése javítás eredeti a beolvasás és antimalware programok végzett vizsgálatot. A virtuális Agent alapértelmezés szerint a a Microsoft Azure piactéren lévő telepített VMs telepítve van. A [virtuális ügynök és bővítmények – 2 rész](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) a cikkben információt a virtuális Agent telepítési.|
| [Lemezen titkosítási alkalmazása](security-center-apply-disk-encryption.md) |Javasolja, hogy titkosítsa-e a virtuális merevlemezeken Azure lemez titkosítással (Windows és Linux VMs). Az operációs rendszer és az adatok kötet a virtuális a titkosítási ajánlott.|
|[Adja meg a biztonsági kapcsolattartási adatai](security-center-provide-security-contact-details.md) | Ajánlja, hogy Ön megadja-e biztonsági kapcsolatfelvételi információk az egyes az előfizetések. Partneradatok egy e-mail cím és telefonszám megadása. Az adatok lesz, ha biztonsági csoportunk megállapítja, hogy az erőforrások hordoznak kapcsolatot. |
| [Operációs rendszer frissítése](security-center-update-os-version.md) | Javasolja, hogy az operációs rendszeren verziók frissítése a Felhőbeli szolgáltatás elérhető legújabb verzióját az operációs rendszer család számára.  Felhőbeli szolgáltatásokkal kapcsolatos további információért olvassa el a a [Cloud Services – áttekintés](../cloud-services/cloud-services-choose-me.md)című témakört. |
| [Nincs telepítve a biztonsági értékelése](security-center-vulnerability-assessment-recommendations.md) | Javasolja, rés értékelési megoldás telepítése a virtuális. |
| [Biztonsági ismételt](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Lásd: a biztonsági értékelési telepítve van a virtuális a megoldás által talált rendszer és az alkalmazások biztonsági teszi lehetővé. |

Szűrés, és zárja be a javaslatok.

1. Kattintson a **Szűrés** gombra a **javaslatok** lap. A **szűrő** lap megnyílik, és akkor válassza a súlyosságát és állapot értéket meg szeretne jeleníteni.

    ![Javaslatok szűrése][2]

2. Ha úgy dönt, hogy ajánlást, akkor nem használható, zárja be az ajánlási, és ezután azt szűrve a nézetből. Kétféleképpen ajánlást bezárásához. Egy úgy, hogy a jobb gombbal egy elemet, és válassza a **Nem emlékeztet újra**. A másik pedig, vigye az egérmutatót egy elem fölé, kattintson a jobbra látható három pontra, és válassza a **Nem emlékeztet újra**. Elbocsátott javaslatok, kattintson a **szűrő**, és ezután válassza az **Dismissed**tekinthet meg.

    ![Zárja be az ajánlási][3]

### <a name="apply-recommendations"></a>Javaslatok alkalmazása
Ellenőrzése után minden javaslatok, döntse el, amely először el kell alkalmazni. Azt javasoljuk, hogy a besorolása használja, mely javaslatok ki szeretné számítani a fő paraméter először kell alkalmazni.

A fenti javaslatok táblázatban jelölje ki a ajánlást, és azt, hogy miként alkalmazhat egy ajánlási példaként végigvezetik.

## <a name="see-also"></a>Lásd még:
A dokumentum biztonsági ajánlások a biztonság otthon hozták be. Többet szeretne tudni a biztonság otthon, olvassa el az alábbiakat:

- [Az Azure biztonság otthon biztonsági házirendek beállítása](security-center-policies.md) – a Azure előfizetések és erőforrás-csoportok biztonsági házirendek konfigurálásának ismertetése.
- [Az Azure biztonság otthon figyelése, biztonsági állapot](security-center-monitoring.md) – útmutató a Lync-állapota az Azure erőforrások.
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md) – megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések válaszolni.
- [Az Azure biztonság otthon partnermegoldások figyelése](security-center-partner-solutions.md) – megtudhatja, hogy miként figyelheti a partnerek megoldásaival állapotának.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – gyakori kérdések a szolgáltatással a keresés.
- [Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – Azure biztonság és megfelelőség kapcsolatos blogbejegyzéseket találja.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
[3]: ./media/security-center-recommendations/dismiss-recommendations.png
