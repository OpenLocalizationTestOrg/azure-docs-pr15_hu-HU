<properties
   pageTitle="Azure architektúra útmutató – a IaaS: Azure Active Directory kiterjesztése |} Microsoft Azure"
   description="Hogyan lehet egy biztonságos hibrid hálózat architektúrája, az Active Directory engedélyezési megvalósítása Azure-ban."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/19/2016"
   ms.author="telmos"/>

# <a name="extending-active-directory-directory-services-adds-to-azure"></a>Azure Active Directory szolgáltatásaihoz (ÖSSZEADJA) kiterjesztése

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Ez a cikk ismerteti az Active Directory (AD) környezet bővítése Azure [Active Directory tartományi szolgáltatások (AD DS)]használatával elosztott hitelesítési szolgáltatások nyújtása a gyakorlati tanácsok[active-directory-domain-services]. Ez a felépítés kiterjeszti az [Azure egy biztonságos hibrid hálózat architektúrája végrehajtási] cikkekben ismertetett[ implementing-a-secure-hybrid-network-architecture] és [a biztonságos hibrid hálózat architektúrája Azure-ban Internet-hozzáféréssel rendelkező végrehajtásához][implementing-a-secure-hybrid-network-architecture-with-internet-access].

> [AZURE.NOTE] Azure van két különböző telepítési modellek: [Erőforrás-kezelő] [ resource-manager-overview] és klasszikus. A hivatkozás architektúra erőforrás-kezelő Microsoft javasolja új telepítési használja.

Active Directory tartományi szolgáltatások segítségével identitások hitelesítést végezni. Ezek a identitások tartozhat felhasználók, számítógépek, alkalmazások és más erőforrások: a biztonsági tartomány részét képező. Is megbízhat a helyszíni Active Directory tartományi szolgáltatások, de a hibrid helyzetben hol található az alkalmazások elemei Azure-ban való replikáció ezt a funkciót, és az Active Directory tárházba a felhőbe hatékonyabbá tehető. Ezt a megközelítést csökkentheti az a késés okozta hitelesítési küldése, és a helyi kéri a felhőből futó helyszíni Active Directory tartományi szolgáltatások biztonsági másolatot. 

Kétféleképpen tárolni a címtárszolgáltatásaival Azure-ban:

1. [Azure Active Directory (AAD)] használható[ azure-active-directory] hozzon létre egy új Active Directory tartományi a felhőben, és kapcsolja az elemet egy helyszíni Active Directory tartományi. Kattintson a telepítő [Azure AD Connect] [ azure-ad-connect] a-tett való replikáció tartott identitások a a helyszíni tárat a felhőbe. Megjegyzendő, hogy a könyvtár (a felhőben) **nem** a helyszíni rendszer bővítménye inkább, amely tartalmazza az azonos identitások másolatot. Ezek a helyszíni bemásolja a felhőben, de a felhőben **nem hajtja végre** a módosításokat a azonosítási végrehajtott módosítások vissza a helyszíni tartomány kell replikált. Jelszó alaphelyzetbe állítása például kell végrehajtani a helyszíni és Azure AD Connect segítségével másolja át a módosítást a felhőben. Azt is vegye figyelembe, hogy ugyanazt a példányát AAD csatolható egynél több példány Active Directory tartományi szolgáltatások; Az egyes AD tárházba, amelyhez a csatolt identitások AAD tartalmaz.

    AAD akkor lehet hasznos olyan esetekben, ahol a helyszíni és Azure virtuális hálózat a felhőben erőforrások szolgáltatója nem kapcsolódnak közvetlenül VPN-csatorna vagy készült ExpressRoute áramkör használatával. Bár ezt a megoldást egyszerű, nem lehet rendszerek alkalmas hol összetevők sikerült áttelepítése az a – helyi és felhőbeli oszlopazonosító között, ezt a AAD konfigurálás igényel. Ezenkívül AAD számítógép-hitelesítés helyett felhasználói hitelesítéssel csak kezeli. Egyes alkalmazások és szolgáltatások, például SQL Server megkövetelheti számítógép-hitelesítés, amely esetben a megoldás értéke nem megfelelő.

2. A virtuális egy tartományvezérlőnek az Azure Active Directory tartományi szolgáltatások futtatott, a meglévő Active Directory-infrastruktúrát kiterjedő a helyszíni adatközponthoz telepítheti. Ez a módszer gyakrabban, ahol a helyszíni és Azure virtuális hálózat csatlakoztatott készült ExpressRoute, illetve virtuális Magánhálózati kapcsolat által felhasználási területei. Ez a megoldást is támogat a kétirányú replikációs, amely lehetővé teszi a módosítások elvégzése a felhőben, és a helyszíni, ahol nem leginkább megfelelő. Attól függően, hogy a biztonsági követelményeknek az Active Directory-telepítés a felhőben lehet:
    - a tárolt helyszíni azonos tartományban része
    - megosztott erdőn belül új tartomány
    - egy másik erdőben

<!-- we might want to add info on how to choose from the three options above -->

Ez a felépítés megoldás 2, használja az Azure és a helyszíni Active Directory tartományi szolgáltatások tartománynevek koncentrál.

Ez a felépítés esettel jellemző használata a következők:

- Hol munkaterhelésekből Futtatás részben a helyszíni hibrid alkalmazásokat és részben az Azure.

- Alkalmazások és szolgáltatások, hajtsa végre a hitelesítést az Active Directory használatával.

## <a name="architecture-diagram"></a>Architektúra diagramja

Az alábbi ábra kiemeli a fontos e (*kattintson a Nagyítás*) architektúra összetevőket. A szürkén jelenik meg elemeket kapcsolatos további tudnivalókért olvassa el a [végrehajtási egy biztonságos hibrid hálózat architektúrája Azure] [ implementing-a-secure-hybrid-network-architecture] és [a biztonságos hibrid hálózat architektúrája Azure-ban Internet-hozzáféréssel rendelkező végrehajtásához][implementing-a-secure-hybrid-network-architecture-with-internet-access]:

[! [0]][0]

- **A helyszíni hálózaton.** A helyszíni hálózaton magában foglalja a helyi Active Directory-kiszolgálók, amely végre hitelesítési és engedélyezési a helyszíni található összetevői.

- **Active Directory-kiszolgálók.** Ezek a címtár-szolgáltatások (AD DS) fut, mint a felhőben VMs végrehajtási tartomány vezérlők. Megadhatja, hogy az alábbi kiszolgálókon Azure virtuális hálózata futó összetevők hitelesítési.

- **Az Active Directory-alhálózat.** Az Active Directory tartományi szolgáltatások kiszolgálón lévő külön alhálózathoz van közzétéve. Az Active Directory tartományi szolgáltatások kiszolgálók védelme és szolgáltatja a forgalmat a váratlan források ellen tűzfalat NSG szabályokat.

- **Azure átjáró és az Active Directory-szinkronizálás.**. Az Azure átjáró a helyszíni hálózaton és az Azure VNet közötti kapcsolatot biztosít. Ez lehet a [virtuális Magánhálózati kapcsolat] [ azure-vpn-gateway] vagy [a készült Azure ExpressRoute][azure-expressroute]. Az Active Directory-kiszolgálók a felhőben, és a helyszíni közötti összes szinkronizációs kérelmekre az átjáró haladnia. Felhasználó által definiált útvonalak (UDRs) kezelni, amelyek közvetlenül át az AD-kiszolgáló a felhőben, és nem hatolhat át a meglévő hálózat virtuális készülékekre (NVAs) ebben az esetben használható szinkronizálási forgalmához útválasztás.

    UDRs és a NVAs beállításával kapcsolatos további tudnivalókért lásd: a [végrehajtási egy biztonságos hibrid hálózat architektúrája Azure][implementing-a-secure-hybrid-network-architecture].

## <a name="recommendations"></a>Javaslatok

Ez a szakasz összesíti Azure, Active Directory tartományi szolgáltatások végrehajtására javaslatok kiterjedő:

- Virtuális javaslatok.

- Hálózati javaslatok.

- Javaslatok biztonsági. 

- Az Active Directory-hely javaslatok.

- Az Active Directory FSMO elhelyezése javaslatok.

>[AZURE.NOTE] A dokumentum [telepítése a Windows Server Active Directory a Azure virtuális gépeken futó útmutató] [ ad-azure-guidelines] a következő pontok számos további részletes információkat tartalmaz.

### <a name="vm-recommendations"></a>Virtuális javaslatok

Kezelheti a várható forgalmat mennyiségű kellő erőforrások VMs hozhat létre. Használja az Active Directory tartományi szolgáltatások helyszíni kiindulási pontként szolgáltatója gépek méretét. Az erőforrás-kihasználtság; figyelése méretezze át a VMs, és méretezni, ha túl nagy. Az Active Directory tartományi szolgáltatások tartomány vezérlők méretezése kapcsolatos további tudnivalókért lásd: a [Kapacitás tervezés, az Active Directory tartományi szolgáltatások][capacity-planning-for-adds].

Hozzon létre külön virtuális adatok lemezen az Active Directory az adatbázist, naplók és SYSVOL tárolásához. Ne tároljon ezeket az elemeket az operációs rendszer ugyanazon a lemezen. Figyelje meg, hogy alapértelmezés szerint egy virtuális csatolt adatok lemez használata író gyorsítótárazás. Ezen az űrlapon gyorsítótárazásának azonban a követelményeknek, az Active Directory tartományi szolgáltatások ütközhetnek. Emiatt állítsa be a *Host gyorsítótár preferencia-sorrend* a adatok lemezen nincs értékre *állításával*. További tudnivalókért lásd: [a Windows Server AD DS adatbázis és SYSVOL elhelyezésének][adds-data-disks].

Telepítse az Active Directory tartományi szolgáltatások [elérhetősége](#Availability-considerations) okokból Azure virtuális hálózathoz tartomány vezérlők futtatott legalább két VMs.

### <a name="networking-recommendations"></a>Hálózati javaslatok

A hálózati kapcsolaton konfigurálása az Active Directory tartományi szolgáltatások statikus magánjellegű IP-címeket tároló VMs minden egyes. Ebben a konfigurációban DNS jobban támogat egyes az Active Directory VMs. További tudnivalókért lásd: [beállíthatja az Azure-portálon statikus magánjellegű IP-cím][set-a-static-ip-address].

> [AZURE.NOTE] Az Active Directory tartományi szolgáltatások VMs nyilvános IP-címek nem adnak. Lásd: a [biztonsági megfontolások] [ security-considerations] további információt.

Győződjön meg arról, hogy forgalom szabadon is flow az Active Directory-kiszolgálók a felhőben, és a helyszíni között:

- Adja hozzá a NSG szabályok, amelyek lehetővé teszik a bejövő forgalmat a helyszíni Active Directory az alhálózathoz. Az Active Directory tartományi szolgáltatások kihasználó portokon részletes tudnivalókért lásd: az [Active Directory és az Active Directory tartományi szolgáltatások portokon][ad-ds-ports].

- Ellenőrizze, hogy UDR táblák nem a forgalmat Active Directory tartományi Szolgáltatásokban használt ebben az esetben a NVAs keresztül. A saját környezetben, attól függően, hogy milyen NVAs használ érdemes a forgalom átirányítása.

### <a name="security-recommendations"></a>Javaslatok biztonsági

Az Active Directory tartományi szolgáltatások kiszolgálók hitelesítés kezelésének, ezért nagyon érzékeny elemek. Megakadályozhatja, hogy az Active Directory tartományi szolgáltatások kiszolgálók közvetlen módon kapcsolódik az internethez. Active Directory tartományi szolgáltatások kiszolgálók külön alhálózathoz, a saját tűzfallal helyezze. Győződjön meg arról, hogy a hitelesítési és engedélyezési és synchronzing kiszolgálók Active Directory tartományi szolgáltatások használatához szükséges portokat maradjon, de zárjon be minden más portokat. További tudnivalókért lásd: az [Active Directory és az Active Directory tartományi szolgáltatások portokon][ad-ds-ports]. A [megoldás-összetevők] [ solution-components] a dokumentumon belül szakaszból megtudhatja, hogy a NSG szabályokat, hogy a minta megoldást portok megnyitásához használja.

NSG szabályok létrehozása egyszerű tűzfalat is használhatja. Ha szüksége van átfogóbb védelmet alkalmazhat egy további biztonsági külső kiszolgálók körül alhálózat és NVAs, használatával leírtak szerint a dokumentum [egy biztonságos hibrid hálózat architektúrája Azure-ban Internet-hozzáféréssel rendelkező végrehajtási][implementing-a-secure-hybrid-network-architecture-with-internet-access].

BitLocker vagy a Azure merevlemez-titkosítás használatára a lemez szolgáltatója, az Active Directory tartományi szolgáltatások adatbázis titkosítása.

### <a name="active-directory-site-recommendations"></a>Az Active Directory-hely javaslatok

Az Active Directory tartományi szolgáltatások csoport együtt tartomány vezérlők gyors hivatkozás csatlakoztatott webhelyeken is használhatja. Active Directory tartományi szolgáltatások ugyanazon a helyen lévő tartomány vezérlők bizonyos címtár módosításaikat automatikusan, és a kis konfigurációja a replikáció kezeléséhez szükséges.

Azure és a helyszíni adatközponthoz közötti replikációs forgalom szabályozni, akkor célszerű az Azure által használt címterület használatára képviselik külön Active Directory tartományi szolgáltatások hely hozzáadása. Beállíthatja, hogy egy adott helyhivatkozás között a helyszíni Active Directory tartományi szolgáltatások webhelyek és a webhelyközi replikációs hatékonyabban szabályozhatja.

Webhely elkülönítésének különböző csoportházirend-objektumok (GPO) számítógépek csatlakozott az Azure-ban, és kihasználhatja az alkalmazásokat, amelyek a "webhely tartsa szem előtt", például a System Center Configuration Manager alkalmazásához is használhatja.

### <a name="active-directory-fsmo-placement-recommendations"></a>Az Active Directory FSMO elhelyezése javaslatok

Rugalmas fő művelet (FSMO) kiszolgálók olyan speciális tartomány vezérlők, reposnsible adatok konzisztencia végig az Active Directory tartományi szolgáltatásokban, az alább felsorolt különböző beállításokat.

- **Séma fő**. Ez a szintű szerepkört, amely megőrzi az Active Directory tartományi szolgáltatások által használt séma szerkezete.

- **A tartomány elnevezése diaminta**. Ez az információ a tartománynevek erdőben kezelő szintű szerepkörbe.

- **Elsődleges tartományvezérlőnek (elsődleges)**. Ez a tartományra vonatkozó szerepkörbe. Az elsődleges jelszó frissítéseket és a fiók zárolások kezeli. Szolgáltatáskérések hitelesítés esetén nem egyező jelszavak azt az egyeztetni más vezérlői által. Az elsődleges is kezeli a csoportházirend-frissítéseket, és a cél Adatközpont régebbi alkalmazások és néhány felügyeleti eszközök, amelyek az egyes írható hajtanak végre.

- **Relatív azonosító (RID) gombra**. RID segítségével egyedileg azonosító egy directory objektumait. A kiszolgáló feladata használatra azonosítók készlete tartomány szintű készlet létrehozása a tartományon belül minden Active Directory-kiszolgálók.

- **Infrastruktúra-szerepkört**. Egy tartományban található objektum hivatkozhat objektum egy másik tartományban. A tartomány szintű szerepkör feladata egy objektum biztonsági AZONOSÍTÓK és megkülönböztető név, a tartományok objektum-hivatkozások frissítését. Figyelje meg, hogy a szerepkör végrehajtási kiszolgáló is nem tudja-e használni kívánt Tartományain globális katalógus kiszolgáló.

További tudnivalókért lásd: a [az Active Directory FSMO szerepkörök Windows][AD-FSMO-roles-in-windows].

Ebben az esetben ajánlott elkerülheti a FSMO szerepkörök bevezetéshez a tartomány vezérlők Azure-ban. 

## <a name="availability-considerations"></a>Elérhetőség kapcsolatos szempontok

Hozzon létre egy elérhetőségét, az Active Directory-kiszolgálók beállítása. Győződjön meg arról, hogy nincsenek-e legalább két kiszolgálók megadása. Az Active Directory-kiszolgálók a felhőben kell tartományt vezérlők ugyanabban a tartományban. Ezzel engedélyezi az automatikus replikáció kiszolgálók között.

Célszerű kiszolgálók kijelölő [készenléti műveleti mesteralakzatok] szerint is[ standby-operations-masters] abban az esetben, ha FSMO szerepet eljáró kiszolgálóra való kapcsolódási sikertelen lesz.

## <a name="security-considerations"></a>Biztonsági megfontolások

Ha minden tartomány felügyeleti feladatok valószínűleg a helyszíni DCs kell elvégezni, fontolja meg, a felhőben DCs írásvédetté tétele. Csak egy írásvédett Adatközpont megtartja a felhasználó hitelesítő adatait (helyben hitelesítését elég) csak egy részhalmazát, és beállítható úgy, hogy csak bizonyos felhasználók gyorsítótár információi. Emiatt a csak olvasható Adatközpont sérül meg, ha csak a felhasználók gyorsítótárazott adatokat hatással, a részletek, a tartomány minden fiók nem pedig annak. További tudnivalókért lásd: az [Írásvédett tartomány vezérlők][read-only-dc].

Minimalizálása érdekében az egyes felhasználói fiókok, és megpróbálja betörési megakadályozására, kövesse az ajánlott eljárás a beállítás, és a felhasználói jelszavak fenntartása az Active Directory. További tudnivalókért lásd: [Gyakorlati tanácsok a jelszóházirendek kényszerítése][best_practices_ad_password_policy]. Is ügyeljen arra, hogy mely csoportok hozzárendelése felhasználók. Ha például ne a vállalati rendszergazdák csoport, a séma rendszergazdák csoport és a Domain Admins csoportjának tagjai hétköznapi felhasználók.

## <a name="scalability-considerations"></a>Méretezhetőség kapcsolatos szempontok

Active Directory tartomány vezérlők tartománynevek részét képező automatikusan méretezhető. Kérelmek egy tartományon belül az összes vezérlő vannak elosztva. Egy másik tartományvezérlőnek is felvehet, és azt a tartománnyal automatikusan szinkronizálja. Irányítsa át a vezérlők alapú forgalmat a tartományon belül egy külön terheléselosztó nem állítja be. Gondoskodhat arról, hogy minden tartomány vezérlők elegendő memória és kezelje a tartomány adatbázisának; tároló-erőforrások Ideális esetben minden tartományvezérlőnek VMs az azonos méretűre.

## <a name="management-considerations"></a>Adatkezelési kapcsolatos szempontok

Nem a másolást virtuális tartomány vezérlők helyett rendszeres biztonsági mentés végrehajtása, mert visszaállításáról eredményezhet következetlenségekre állapotban tartomány vezérlők között.

Állítsa le, és indítsa újra a helyett használja a Leállítás parancsot az Azure-portálon vendég operációs rendszerben futó a tartomány szerepkört Azure virtuális. A virtuális felszabadítása lehet, hogy az Azure Portal segítségével állítsa le a virtuális okoz. Ez a művelet alaphelyzetbe állítása a virtuális GenerationID, amely olyan, a tartományvezérlőnek a nemkívánatos. Amikor a virtuális GenerationID alaphelyzetbe állítása is alaphelyzetbe állítása az Active Directory tárházba, a invocationID, a rendszer elveti az RID készlet és SYSVOL be van jelölve, nem mérvadó.

## <a name="monitoring-considerations"></a>Megfigyeléssel kapcsolatos szempontok

Adatkapcsolat figyelésére és karbantartása: az Active Directory-kiszolgálók hálózaton problémákat okozhat például:

- A **bejelentkezési hiba**. Bejelentkezési hiba akkor fordulhat elő a tartomány vagy erdő egész, egy megbízhatósági kapcsolat vagy a név feloldása sikertelen, vagy ha a globális katalógus kiszolgáló nem tudja megállapítani, univerzális csoporttagság.

- **Fiók zárolása**. Felhasználó-és szolgáltatásfiók is frissítve zárolt állapotba kerülnek Ha az elsődleges tartományvezérlőnek nem érhető el, vagy a replikáció megszakad, több tartományt vezérlők között.

- A **tartományvezérlőnek hiba**. Ha a Ntds.dit fájlt tartalmazó meghajtóra elegendő szabad lemezterület fut, a tartományvezérlőnek leállíthatja a működését.

- **Alkalmazáshiba**. Üzleti, például a Microsoft Exchange vagy más levelezőprogram kritikus alkalmazásokat is sikertelen, ha a cím könyv lekérdezések át a directory sikertelenek lesznek.

- **Várttól eltérően működik a címtár-adatokat**. Replikációs egy hosszabb idejű távollétet sikertelen, ha az ismétlődő objektumok a címtárban hozhatja létre, és előfordulhat, hogy teljes körű diagnosztizálása és az idő kiküszöbölése érdekében.

- A **fiók létrehozása sikertelen**. A tartományvezérlőnek nem tudja felhasználó vagy a számítógép-fiókok létrehozása, ha azt kimeríti relatív azonosítók szállítási és főkiszolgálójának nem érhető el.

- **Biztonsági házirend hiba**. Ha a megosztott SYSVOL nem megfelelően bizonyos, a csoportházirend-objektumok és biztonsági házirendek nem megfelelően veszi ügyfeleknek.

Gondosan figyelheti az Active Directory-kiszolgálók, és hajlandó korrekciós lépéseket gyorsan. Hozzon létre egy tevékenység-ellenőrzőlistát megfelelő időközönként elvégzendő feladatok figyelése. Ha például a következő kritikus tevékenységek naponta sikerült ütemezése:

- Vizsgálja meg, és az értesítések tartomány vezérlők, által jelzett megoldása 

- Ellenőrizze, hogy minden tartomány vezérlők tudnak kommunikálni, hogy a replikáció működik.

- Győződjön meg arról, hogy a SYSVOL megosztott marad.

- Idő-szinkronizálási problémák hatósági.

- Ellenőrizze, hogy a fizikai meghajtókon AD által használt lemezterületet.

Egyéb mindennapos feladatok kevésbé gyakori tudta végrehajtani. Például, képes bizalmi kapcsolatok áttekintése és keressen heti elavult vagy hibás meghatalmazások, és győződjön meg róla, hogy minden tartomány vezérlők fut a azonos szervizcsomagok és meleg fix javítások havi használatával.

További tudnivalókért lásd: az [Active Directory figyelése][monitoring_ad]. Eszközök, például [Microsoft rendszerek Center] telepítheti[ microsoft_systems_center] a megfigyeléssel kiszolgálón (lásd: a [architektúra diagram][architecture]) érdekében ezeket a műveleteket. 

## <a name="solution-components"></a>Megoldás-összetevők

A megoldás e architektúra létrehoz egy biztonságos hibrid hálózati [végrehajtása egy biztonságos hibrid hálózat architektúrája Azure-ban] a dokumentumok ellenkezőjét előírt[ implementing-a-secure-hybrid-network-architecture] és [a biztonságos hibrid hálózat architektúrája Azure-ban Internet-hozzáféréssel rendelkező végrehajtásához][implementing-a-secure-hybrid-network-architecture-with-internet-access], de az alábbi elemek hozzáadása:

- Az Azure erőforráscsoport nevű *basename*- dns-rg, hol *basename* -e egy előtagot, adja meg, ha a megoldást üzembe helyezné.

- Két Azure VMs *basename*nevű-ad1-virtuális és *basename*-ad2-virtuális, a *basename*- dns-rg erőforráscsoport létrehozott. Ezek a VMs címtárszolgáltatásaival és a DNS-telepítette és beállította az Active Directory-kiszolgálók van konfigurálva.

- Egy NSG nevű *basename*-Active Directory-nsg *basename*- ntwk-rg Azure erőforrás csoportjának. Az erőforráscsoport alkotják a biztonságos hibrid hálózati infrastruktúra része, de az új NSG bővítménye, amely meghatározza a bejövő szabályok az Active Directory-kiszolgálók, az alábbi táblázatban látható módon:


    Prioritás|név|Forrás|Cél|Szolgáltatás|Művelet|
    --------|----|------|-----------|-------|------|
    170|vnet-port53|10.0.0.0/16|Bármely|Custom(ANY/53)|Engedélyezése|
    180|vnet-port88|10.0.0.0/16|Bármely|Custom(ANY/88)|Engedélyezése|
    190|vnet-port135|10.0.0.0/16|Bármely|Custom(ANY/135)|Engedélyezése|
    200|vnet – port137 9|10.0.0.0/16|Bármely|Custom(ANY/137-139)|Engedélyezése|
    210|vnet-port389|10.0.0.0/16|Bármely|Custom(ANY/389)|Engedélyezése|
    220|vnet-port464|10.0.0.0/16|Bármely|Custom(ANY/464)|Engedélyezése|
    230|vnet való rpc dinamikus|10.0.0.0/16|Bármely|Custom(ANY/49152-65535)|Engedélyezése|
    240|onprem-Active Directory-a-port53|192.168.0.0/24|Bármely|Custom(ANY/53)|Engedélyezése|
    250|onprem-Active Directory-a-port88|192.168.0.0/24|Bármely|Custom(ANY/88)|Engedélyezése|
    260|onprem-Active Directory-a-port135|192.168.0.0/24|Bármely|Custom(ANY/135)|Engedélyezése|
    270|onprem-Active Directory-a-port389|192.168.0.0/24|Bármely|Custom(ANY/389)|Engedélyezése|
    280|onprem-Active Directory-a-port464|192.168.0.0/24|Bármely|Custom(ANY/464)|Engedélyezése|
    290|kezelése rdp engedélyezése|10.0.0.128/25|Bármely|Custom(ANY/3389)|Engedélyezése|
    300|Átjáró engedélyezése|10.0.255.224/27|Bármely|Custom(ANY/any)|Engedélyezése|
    310|önálló engedélyezése|10.0.255.192/27|Bármely|Custom(ANY/any)|Engedélyezése|
    320|vnet-elutasítása|Bármely|Bármely|Custom(ANY/any)|Engedélyezése|

    Active Directory tartományi szolgáltatások portokat 53, 89, 135, 389 és 464 használ fogadja el a bejövő replikáció és a hitelesítési forgalmat. Az alábbi táblázatban a helyszíni tartományvezérlőnek van a cím terület 192.168.0.0/24 (a címterület változhat - e kitöltését paraméterként a sablonok a megoldás rendszerbe.

    A NSG is határozza meg, a következő kimenő biztonsági szabályokat, amelyek lehetővé teszik a szinkronizálás és engedélyezési forgalmat a helyszíni hálózaton történő vissza:

    Prioritás|név|Forrás|Cél|Szolgáltatás|Művelet|
    --------|----|------|-----------|-------|------|
    100|kimenő-port53|Bármely|192.168.0.0/24|Custom(ANY/53)|Engedélyezése|
    110|kimenő-port88|Bármely|192.168.0.0/24|Custom(ANY/88)|Engedélyezése|
    120-ra|kimenő-port135|Bármely|192.168.0.0/24|Custom(ANY/135)|Engedélyezése|
    130|kimenő-port389|Bármely|192.168.0.0/24|Custom(ANY/389)|Engedélyezése|
    140|kimenő-port445|Bármely|192.168.0.0/24|Custom(ANY/445)|Engedélyezése|
    150|kimenő-port464|Bármely|192.168.0.0/24|Custom(ANY/464)|Engedélyezése|
    160|kimenő rpc-dinamikus|Bármely|192.168.0.0/24|Custom(ANY/49152-65535)|Engedélyezése|

A parancsfájl ellátni a megoldást is végre az alábbi műveleteket:

- A *basename*ad hozzá-ad1-virtuális és *basename*-ad2-vezérlők tartományát a tartomány virtuális-kiszolgálókkal. Ezeket a kiszolgálókat az *Active Directory-felhasználók és -számítógépek* konzol a helyszíni tartományvezérlőnek tekintheti meg:

![[1]][1]

- Létrehoz egy új alhálózat (10.0.0.0/16) Azure-VNet-Active Directory-hely megnevezett tartományt AD-webhelyéhez. Ez a webhely tartalmaz a *basename*-ad1-virtuális és *basename*-ad2-virtuális kiszolgálókhoz. 

- IP-helyek közötti átviteli beállításokat, amelyek a replikáció közötti időtartamot a helyszíni webhely és a tartomány vezérlők konfigurálása a felhőben oszlopaihoz. Az alhálózathoz, webhelyek és az *Active Directory-helyek és a kiszolgálón* konzolban a helyszíni tartományvezérlőnek a átviteli beállítások beállításainak láthatja:

![[2]][2]

## <a name="deployment"></a>Telepítési

A minta megoldást a következő prerequsites foglalja magában:

- Már beállította a helyszíni tartomány, és az, hogy konfigurálva a DNS, és telepítve van a virtuális Magánhálózattal támogatási Útválasztás és távelérési szolgáltatások csatlakozni az Azure virtuális Magánhálózati átjáró.


- Az Azure CLI legújabb verziója van telepítve. [További információt az alábbi útmutatás][cli-install].

- Ha a Windows a megoldást is telepít, telepítenie kell a egy eszköz, amely tartalmaz egy bash rendszerhéj, például [GitHub asztali][github-desktop].

>[AZURE.NOTE] Ha nem Önnek van hozzáférése egy meglévő a helyszíni tartomány, egy tesztkörnyezetben a [onpremdeploy.sh] használatával hozhat létre[ onpremdeploy] parancsfájl bash. A parancsfájl a felhőben, amely a legalapvetőbb helyszíni telepítés hoz létre egy hálózati és virtuális. Szerkessze a parancsfájlt futtatása előtt, és adja meg a következő változók elején a fájlt a megadott:
>
> - **BASE_NAME**. A felhasználó által definiált előtag az erőforráscsoport és a parancsfájl által létrehozott virtuális. Ez a cikk **5 karakternél nem hosszabb** különben a program a parancsfájl megpróbálja egy virtuális érvénytelen nevű készítése és nem kell lennie.
> 
> - **ELŐFIZETÉS**. Az Azure előfizetés azonosítójával. Az erőforráscsoport létrejön az e suscription.
> 
> - **HELYÉT**. Az Azure helyet, ahová az erőforráscsoport, például *eastus* vagy *westus*létrehozásához.
> 
> - **ADMIN_USER_NAME**. A rendszergazdai fiók a virtuális a használni kívánt nevet.
> 
> - **ADMIN_PASSWORD**. A rendszergazdai fiók jelszava.

Hajtsa végre az alábbi lépéseket a minta megoldást létrehozásához:

1. Töltse le és szerkesztése a [azuredeploy.sh] [ azuredeploy] parancsfájl, és adja meg a következő paraméterek elején található a fájlt:

    - **BASE_NAME**. A felhasználó által definiált előtag az erőforrás csoportok és a parancsfájl által létrehozott VMs. Ezt az elemet, mielőtt **5 karakternél nem hosszabb**kell lennie.

    - **ELŐFIZETÉS**. Az Azure előfizetés azonosítójával.

    - **Operációs_rendszer**. Az operációs rendszer (*Windows* vagy *Linux rendszerhez*) használata a webes, üzleti és adat-hozzáférési szint VMs. Figyelje meg, hogy a parancsprogram által létrehozott összes Active Directory-kiszolgálón ettől a beállítástól függetlenül a Windows Server 2012, futnak.

    - A **tartománynév**. A helyszíni tartomány nevét. Figyelje meg, hogy ha használ a környezetben, a onpremdeploy.sh parancsfájl által létrehozott, ez lehet *contoso.com*.

    - **HELYÉT**. Az Azure helyet, ahová az erőforrás-csoportokat hozhat létre.

    - **ADMIN_USER_NAME**. A nevet, a különböző VMs a rendszergazdafiókjához.

    - **ADMIN_PASSWORD**. A rendszergazdai fiók jelszava.

    - **ON_PREMISES_PUBLIC_IP**. A helyszíni VPN gép nyilvános IP-címét.

    - **ON_PREMISES_ADDRESS_SPACE**. A helyszíni hálózaton belső címterület. A környezet hozta létre a onpremdeploy.sh parancsfájl használatakor 192.168.0.0/16 kell lennie.

    - **VPN_IPSEC_SHARED_KEY**. A IPSec megosztott kulcs a virtuális Magánhálózati kapcsolat a helyszíni hálózaton és az Azure virtuális Magánhálózati átjáró között.

    - **ON_PREMISES_DNS_SERVER_ADDRESS**. A helyszíni DNS-kiszolgáló IP-címét. Ha a hozta létre a onpremdeploy.sh parancsfájl környezetben használja, ez lehet 192.168.0.4

    - **ON_PREMISES_DNS_SUBNET_PREFIX** A cím a helyszíni alhálózat előtagját. A környezet hozta létre a onpremdeploy.sh parancsfájl használatakor 192.168.0.0/24 kell lennie.

    >[AZURE.NOTE] A parancsprogram SZERETNÉ menteni a források és az idő, nem hoz létre az üzleti vagy adatok access-meghatározási. Ha szüksége van ezeket az elemeket, akkor is vegye ki a megjegyzésjeleket a azuredeploy.sh parancsfájl szakaszt:
    >
    >
    > ```
    > #### # create biz tier
    > #### TEMPLATE_URI=${URI_BASE}/ARMBuildingBlocks/Templates/bb-ilb-backend-http-https.json
    > #### SUBNET_NAME_PREFIX=${DEPLOYED_BIZ_SUBNET_NAME_PREFIX}
    > #### ILB_IP_ADDRESS=${BIZ_ILB_IP_ADDRESS}
    > #### NUMBER_VMS=${BIZ_NUMBER_VMS}
    > #### 
    > #### RESOURCE_GROUP=${BASE_NAME}-${SUBNET_NAME_PREFIX}-tier-rg
    > #### VM_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VM_COMPUTER_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VNET_RESOURCE_GROUP=${NTWK_RESOURCE_GROUP}
    > #### VNET_NAME=${DEPLOYED_VNET_NAME}
    > #### SUBNET_NAME=${DEPLOYED_BIZ_SUBNET_NAME}
    > #### PARAMETERS="{\"baseName\":{\"value\":\"${BASE_NAME}\"},\"vnetResourceGroup\":{\"value\":\"${VNET_RESOURCE_GROUP}\"},\"vnetName\":{\"value\":\"${VNET_NAME}\"},\"subnetName\":{\"value\":\"${SUBNET_NAME}\"},\"adminUsername\":{\"value\":\"${ADMIN_USER_NAME}\"},\"adminPassword\":{\"value\":\"${ADMIN_PASSWORD}\"},\"subnetNamePrefix\":{\"value\":\"${SUBNET_NAME_PREFIX}\"},\"ilbIpAddress\":{\"value\":\"${ILB_IP_ADDRESS}\"},\"osType\":{\"value\":\"${OS_TYPE}\"},\"numberVMs\":{\"value\":${NUMBER_VMS}},\"vmNamePrefix\":{\"value\":\"${VM_NAME_PREFIX}\"},\"vmComputerNamePrefix\":{\"value\":\"${VM_COMPUTER_NAME_PREFIX}\"}}"
    > #### 
    > #### echo
    > #### echo
    > #### echo azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > ####      azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > #### echo
    > #### echo
    > #### echo azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ####      azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > #### 
    > #### # create db tier
    > #### TEMPLATE_URI=${URI_BASE}/ARMBuildingBlocks/Templates/bb-ilb-backend-http-https.json
    > #### SUBNET_NAME_PREFIX=${DEPLOYED_DB_SUBNET_NAME_PREFIX}
    > #### ILB_IP_ADDRESS=${DB_ILB_IP_ADDRESS}
    > #### NUMBER_VMS=${DB_NUMBER_VMS}
    > #### 
    > #### RESOURCE_GROUP=${BASE_NAME}-${SUBNET_NAME_PREFIX}-tier-rg
    > #### VM_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VM_COMPUTER_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VNET_RESOURCE_GROUP=${NTWK_RESOURCE_GROUP}
    > #### VNET_NAME=${DEPLOYED_VNET_NAME}
    > #### SUBNET_NAME=${DEPLOYED_DB_SUBNET_NAME}
    > #### PARAMETERS="{\"baseName\":{\"value\":\"${BASE_NAME}\"},\"vnetResourceGroup\":{\"value\":\"${VNET_RESOURCE_GROUP}\"},\"vnetName\":{\"value\":\"${VNET_NAME}\"},\"subnetName\":{\"value\":\"${SUBNET_NAME}\"},\"adminUsername\":{\"value\":\"${ADMIN_USER_NAME}\"},\"adminPassword\":{\"value\":\"${ADMIN_PASSWORD}\"},\"subnetNamePrefix\":{\"value\":\"${SUBNET_NAME_PREFIX}\"},\"ilbIpAddress\":{\"value\":\"${ILB_IP_ADDRESS}\"},\"osType\":{\"value\":\"${OS_TYPE}\"},\"numberVMs\":{\"value\":${NUMBER_VMS}},\"vmNamePrefix\":{\"value\":\"${VM_NAME_PREFIX}\"},\"vmComputerNamePrefix\":{\"value\":\"${VM_COMPUTER_NAME_PREFIX}\"}}"
    > #### 
    > #### echo
    > #### echo
    > #### echo azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > ####      azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > #### echo
    > #### echo
    > #### echo azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ####      azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ```

2. Nyisson meg egy bash rendszerhéj ablakot, és helyezze a azuredeploy.sh parancsfájlt tartalmazó mappát.

3. Jelentkezzen be az Azure-fiókjába. Írja be a bash felületén a következő parancsot:

    ```
    azure login
    ```

    Kövesse az utasításokat követve csatlakoztassa az Azure.

4. A parancs futtatása `./azuredeploy.sh`, és ezután várja meg, amíg a parancsfájl hoz létre a hálózati infrastruktúrát.

5. A parancssorba *Győződjön meg arról, hogy a VNet létrehoztak*az Azure portal segítségével ellenőrizze, hogy *basename*- ntwk-rg nevű erőforráscsoport létrehoztak, és elemek hasonlóak az alábbi képen látható, hogy tartalmazza:

    ![[3]][3]

    >[AZURE.NOTE] A példákat *basename* állította be a *felhőbe* a parancsfájl futtatásakor.

    Kattintson a *basename*- vnet VNet *alhálózat*kattintson, és ellenőrizze, hogy létrejött a alhálózat alább látható módon:

    ![[4]][4]

6. A parancssorba bash rendszerhéj ablakban lenyom egy billentyűt, és várja meg, amíg a webes réteg és terheléselosztó jönnek létre.

7. A parancssorba *Győződjön meg arról, hogy a webes réteg helyesen létre*az Azure portal segítségével ellenőrizze, hogy a webhely-réteg-rg *basename*nevű erőforráscsoport létrehoztak, és, hogy a hasonló alább látható elemek tartalmazza:

    ![[5]][5]

8. A parancssorba bash rendszerhéj ablakban lenyom egy billentyűt, és várja meg, amíg a NVAs jönnek létre.

9. A parancssorba *Győződjön meg arról, hogy a NVA helyesen létre*az Azure portal segítségével ellenőrizze, hogy egy erőforrás - kezelése-rg *basename*nevű csoportot, a következő tartalommal jött létre:

    ![[6]][6]

10. A parancssorba bash rendszerhéj ablakban lenyom egy billentyűt, és várja meg, amíg a jumpbox jön létre.

11. A parancssorba *Győződjön meg arról, hogy a jumpbox helyesen létre*az Azure portal segítségével ellenőrizze, hogy az alábbi elemek van hozzáadva a *basename*- kezelése-rg erőforráscsoport:

    - Az elérhetőség beállítása nevű *basename*- jb-szerint.

    - A virtuális *basename*- jb-virtuális nevű.

    - A hálózati kapcsolat nevű *basename*- jb-adaptert

12. A parancssorba bash rendszerhéj ablakban lenyom egy billentyűt, és várja meg, amíg az Azure virtuális Magánhálózati átjáró és a kapcsolat létrehozásakor. Figyelje meg, hogy ezt a lépést a legfeljebb 30 percig is eltarthat.

13. A parancssorba *Győződjön meg arról, hogy a virtuális Magánhálózati átjáró helyesen létre*az Azure portal segítségével ellenőrizze, hogy az alábbi elemek van hozzáadva a *basename*- ntwk-rg erőforráscsoport:

    - A helyi hálózati átjáró a helyszíni környezetbe lgw neve.
    
    - A virtuális hálózati átjáró neve *basename*- vpngw.

    - A névvel ellátott *basename*- vnet-vpnconn átjáró kapcsolat. Ne feledje, hogy a kapcsolat állapotának előfordulhat, hogy *nem csatlakozott* , ha még nem konfigurálta a kapcsolat; a helyszíni végére Ez a később megoldja.

14. A parancssorba bash rendszerhéj ablakban lenyom egy billentyűt, és várja meg, amíg a VMs és más forrásokat nyújt a DMZ jönnek létre.

15. A parancssorba *Győződjön meg arról, hogy a DMZ helyesen létre*az Azure portal segítségével ellenőrizze, hogy egy erőforrás -dmz-rg *basename*nevű csoportot, a következő tartalommal jött létre:

    ![[7]][7]

16. A parancssorba bash rendszerhéj ablakban nyomja le az a billentyűt. Jelenjen meg az alábbi útmutatásokat:

    ```text
    Manual Step...

    Please configure your on-premises network to connect to the Azure VNet

    Make sure that you can connect to the on-premises AD server from the Azure VMs
    ```

    Jelentkezzen be a helyszíni számítógépen, amely az Azure átjáró csatlakozik, és állítsa be megfelelően a kapcsolatot. Statikus útvonalak hozzáadása a helyszíni átjáró eszköz, amely arra utasítja az átjárót a VNet – 10.0.0.0/16 cím tartományba requestsfor. Ennek lépéseit a hogyan csatlakozik megfelelően változik. Lásd: a [végrehajtási egy hibrid hálózat architektúrája Azure és a helyszíni VPN] [ implementing-a-hybrid-network-architecture-with-vpn] további információt.

    Tartsa szem előtt, hogy talál az Azure virtuális Magánhálózati átjáró nyilvános IP-címét az Azure portál vizsgálja meg a *basename*- ntwk-rg erőforráscsoport az *basename*- vpngw átjáró használatával:

    ![[8]][8]

    Meghatározhatja, hogy a kapcsolat létrejött megfelelően megjeleníti a *basename*- vnet-vpnconn kapcsolat állapotát. Meg kell beállítani *csatlakoztatva*.

    Tesztelje a kapcsolatot, hogy a számítógépről a helyszíni hálózaton található nyissa meg a távoli asztali kapcsolat a jumpbox (10.0.0.254).

17. A parancssorba bash rendszerhéj ablakban nyomja le az a billentyűt. A következő kérdés *tetszőleges billentyűt a VNet beállítása a VNet, mutasson a helyszíni DNS frissítése*, lenyom egy billentyűt, és várja meg, amíg a DNS-beállításait a VNet frissülnek a azuredeploy.sh parancsfájl **ON_PREMISES_DNS_SERVER_ADDRESS** paraméterként megadott érték.

18. A megjelenő kérdésnél, *Győződjön meg arról, hogy a DNS-kiszolgáló beállítása a VNet a frissült*, az Azure portal segítségével vizsgálja meg a *basename*- vnet VNet *basename*- ntwk-rg erőforrás csoportjában a *DNS-kiszolgálók* beállítást. *Egyéni*DNS-meg kell, és az *elsődleges DNS-kiszolgáló* kell a helyszíni DNS-kiszolgáló címe:

    ![[9]][9]

19. A parancssorba *tetszőleges billentyűt az Active Directory-kiszolgálók az erőforrás-csoport létrehozásához* a bash rendszerhéj ablakban lenyom egy billentyűt, és várja meg, amíg az Active Directory-kiszolgálók tartsa a felhőben az erőforrás-csoport jön létre.

20. A parancssorba *tetszőleges billentyűt lenyomva létrehozása az Active Directory-kiszolgálók a VMs* bash rendszerhéj ablakban lenyom egy billentyűt, és várja meg a VMs létrehozott, és hozzáadja az erőforrás-csoporthoz.

21. *Tetszőleges billentyűt lenyomva Bekapcsolódás a helyszíni tartomány VMs* megjelenése után az Azure portált, és győződjön meg arról, hogy egy *basename*- dns-rg nevű csoportot létrehoztak, és, hogy a két VMS tartalmazza (*basename*-ad1-virtuális és *basename*-ad2-virtuális):

    ![[10]][10]

22. *Basename*- ntwk-rg erőforráscsoport ellenőrzése, hogy egy NSG létrehoztak nevű *basename*-Active Directory-nsg. Ez a NSG vizsgálja meg a bejövő és kimenő biztonsági szabályokat. Azokat meg kell egyezniük a táblázatban az [megoldás-összetevők] [ solution-components] szakaszban.

23. A parancssorba bash rendszerhéj ablakban lenyom egy billentyűt, és várja meg, amíg a VMs bekerülnek a helyszíni tartomány.

24. A a *Keresse fel a helyszíni Active Directory-kiszolgálót, ellenőrizze, hogy a számítógép felvette a tartományok* kérdésre, a helyi számítógéphez, és ellenőrizze, hogy mindkét VMs felvette a tartományt az *Active Directory-felhasználók és számítógépek* konzol használatával:

    ![[11]][11]

25. A parancssorba bash rendszerhéj ablakban lenyom egy billentyűt, és várja meg, amíg az Active Directory-replikációs webhely jön létre a tartomány.

26. A a *Keresse fel a helyszíni Active Directory-kiszolgálót, ellenőrizze, hogy a replikáció webhely létrehoztak* kérdés, az *Active Directory-helyek és szolgáltatások* konzol segítségével történő ellenőrizze, hogy *Azure-Vnet-Active Directory-hely* nevű replikációs webhely létrehozása sikeresen befejeződött, az IP-helyek közötti átviteli *AzureToOnpremLink*és a VNet hivatkozó alhálózat című hivatkozást együtt használható:

    ![[12]][12]

27. A parancssorba bash rendszerhéj ablakban lenyom egy billentyűt, és várja meg, amíg a parancsfájl címtárszolgáltatásaival és a DNS-telepíti az egyes az Active Directory VMs.

28. Amikor megjelenik a kérdés *, kérjük, jelentkezzen be a minden Azure AD-kiszolgálóhoz, ellenőrizze, hogy címtárszolgáltatásaival sikeresen beállította* , nyissa meg a távoli asztali kapcsolat egy helyszíni gépi a jumpbox (*basename*- jb virtuális), és nyissa meg másik távoli asztali kapcsolat-ból a jumpbox és az első Active Directory-kiszolgáló (*basename*-ad1-virtuális). Jelentkezzen be a **tartománynevét**, **ADMIN_USER_NAME**és a azuredeploy.sh parancsfájl megadott **ADMIN_PASSWORD** . A Kiszolgálókezelő használ, győződjön meg arról, hogy az Active Directory tartományi szolgáltatások és a DNS-szerepkörök van mindkét hozzáadva. Ismételje meg ezt a folyamatot a második Active Directory-kiszolgáló (*basename*-ad2-virtuális).

29. A parancssorba bash rendszerhéj ablakban nyomja le az a billentyűt. Amikor megjelenik az üzenet *tetszőleges billentyűt, mutasson a DNS-Rekordokat az Azure Azure VNet DNS-beállítások megadásához* , lenyom egy billentyűt, és frissítse a DNS-beállításait a VNet a parancsfájl.

30. Amikor az üzenet *Győződjön meg arról, hogy a beállítás lett VNet DNS frissítése hivatkozás az Azure virtuális DNS servers* jelenik meg, az Azure portál jelölőnégyzet a *DNS-kiszolgálók* beállítása a *basename*- vnet VNet *basename*- ntwk-rg erőforrás csoportjának használatával. Az elsődleges és másodlagos DNS-kiszolgálók most hivatkoznia kell az Active Directory két VMs:

    ![[13]][13]

31. Indítsa újra az Active Directory VMs mindegyike a továbblépés előtt. Ez a lépés nem szükséges ahhoz, hogy azok minden emelje fel a megfelelő DNS-beállítások az Azure. Várja meg mindkét VMs futtatja a továbblépés előtt.

32. A parancssorba bash rendszerhéj ablakban nyomja le az a billentyűt. *Tetszőleges billentyűt lenyomva alkalmazása az átjáró UDR az átjáró alhálózathoz (Ez lehet, hogy eltávolította)*, a következő sorba lenyom egy billentyűt, és a parancsfájl frissítheti az átjáró UDR.

33. Győződjön meg arról, hogy a parancsprogram befejeződik.

## <a name="next-steps"></a>Következő lépések

- További tudnivalók a gyakorlati tanácsok [ÖSSZEADJA az erőforrás erdőn létrehozásához] [ adds-resource-forest] Azure-ban.

- További tudnivalók a gyakorlati tanácsok [az ADFS-infrastruktúrát létrehozásához] [ adfs] Azure-ban.

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[adfs]: ./guidance-identity-adfs.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[adds-resource-forest]: ./guidance-identity-adds-resource-forest.md
[script]: #sample-solution-script
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-3-tier-vm.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-hybrid-network-architecture-with-vpn]: ./guidance-hybrid-network-vpn.md
[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[azure-active-directory]: ../active-directory-domain-services/active-directory-ds-overview.md
[azure-ad-connect]: ../active-directory/active-directory-aadconnect.md
[architecture]: #architecture_diagram
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[azure-vpn-gateway]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-vpngateways/
[azure-expressroute]: https://azure.microsoft.com/documentation/articles/expressroute-introduction/
[claims-aware applications]: https://msdn.microsoft.com/en-us/library/windows/desktop/bb736227(v=vs.85).aspx
[active-directory-federation-services-overview]: https://technet.microsoft.com/en-us/library/hh831502(v=ws.11).aspx
[capacity-planning-for-adds]: http://social.technet.microsoft.com/wiki/contents/articles/14355.capacity-planning-for-active-directory-domain-services.aspx
[ad-ds-ports]: https://technet.microsoft.com/library/dd772723(v=ws.11).aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048(v=ws.11).aspx
[powershell-ad]: https://technet.microsoft.com/en-us/library/ee617195.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[domain_and_forests]: https://technet.microsoft.com/library/cc759073(v=ws.10).aspx
[best_practices_ad_password_policy]: https://technet.microsoft.com/magazine/ff741764.aspx
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[microsoft_systems_center]: https://www.microsoft.com/server-cloud/products/system-center-2016/
[cli-install]: https://azure.microsoft.com/documentation/articles/xplat-cli-install
[github-desktop]: https://desktop.github.com/
[sssd-and-active-directory]: https://help.ubuntu.com/lts/serverguide/sssd-ad.html
[set-a-static-ip-address]: https://azure.microsoft.com/documentation/articles/virtual-networks-static-private-ip-arm-pportal/
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[adds-data-disks]: https://msdn.microsoft.com/library/azure/jj156090.aspx#BKMK_PlaceDB
[AD-FSMO-recommendations]: #active_directory_FSMO_placement_recommendations
[AD-FSMO-roles-in-windows]: https://support.microsoft.com/en-gb/kb/197132
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[transfer-FSMO-roles]: https://technet.microsoft.com/library/cc816946(v=ws.10).aspx
[view-fsmo-roles]: https://technet.microsoft.com/library/cc816893(v=ws.10).aspx
[read-only-dc]: https://technet.microsoft.com/library/cc732801(v=ws.10).aspx
[AD-sites-and-subnets]: https://blogs.technet.microsoft.com/canitpro/2015/03/03/step-by-step-setting-up-active-directory-sites-subnets-site-links/
[sites-overview]: https://technet.microsoft.com/library/cc782048(v=ws.10).aspx
[implementing-adfs]: ./guidance-iaas-ra-secure-vnet-adfs.md
[onpremdeploy]: https://github.com/mspnp/blueprints/blob/master/ARMBuildingBlocks/guidance-iaas-ra-ad-extension/onpremdeploy.sh
[azuredeploy]: https://github.com/mspnp/blueprints/blob/master/ARMBuildingBlocks/guidance-iaas-ra-ad-extension/azuredeploy.sh
[solution-components]: #solution_components

[0]: ./media/guidance-iaas-ra-secure-vnet-ad/figure1.png "Hibrid hálózat architektúrája, az Active Directoryval, biztonságos"
[1]: ./media/guidance-iaas-ra-secure-vnet-ad/figure2.png "Az Active Directory-felhasználók és számítógépek konzol listaelem kiszolgálók a két Azure VMs"
[2]: ./media/guidance-iaas-ra-secure-vnet-ad/figure3.png "Az Active Directory-helyek és szolgáltatások konzol beállításaival replikációs a webhelyet a felhőben"
[3]: ./media/guidance-iaas-ra-secure-vnet-ad/figure4.png "A basename-ntwk-rg erőforráscsoport tartalma"
[4]: ./media/guidance-iaas-ra-secure-vnet-ad/figure5.png "Az alhálózathoz a basename-vnet VNet a"
[5]: ./media/guidance-iaas-ra-secure-vnet-ad/figure6.png "A webes réteg eleme"
[6]: ./media/guidance-iaas-ra-secure-vnet-ad/figure7.png "A NVAs basename-kezelése-rg erőforráscsoport"
[7]: ./media/guidance-iaas-ra-secure-vnet-ad/figure8.png "Az erőforrások erőforráscsoport basename-dmz-rg"
[8]: ./media/guidance-iaas-ra-secure-vnet-ad/figure9.png "A nyilvános IP-cím az Azure virtuális Magánhálózati átjáró megkeresése"
[9]: ./media/guidance-iaas-ra-secure-vnet-ad/figure10.png "A DNS-kiszolgáló beállításait a * basename *-vnet VNet"
[10]: ./media/guidance-iaas-ra-secure-vnet-ad/figure11.png "A * basename *-dns-rg erőforráscsoport tartalmazó VMs AD-kiszolgáló"
[11]: ./media/guidance-iaas-ra-secure-vnet-ad/figure12.png "Az Active Directory-felhasználók és számítógépek konzol, az Active Directory-kiszolgáló VMs bejegyzésére tagként a tartomány"
[12]: ./media/guidance-iaas-ra-secure-vnet-ad/figure13.png "Az Active Directory-helyek és szolgáltatások konzol a replikáció webhelyet az Azure Active Directory-kiszolgálók megjelenítő"
[13]: ./media/guidance-iaas-ra-secure-vnet-ad/figure14.png "A DNS-kiszolgáló beállításait a basename-vnet VNet hivatkozó az Active Directory-kiszolgálók a felhőben"