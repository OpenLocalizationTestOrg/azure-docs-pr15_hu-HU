<properties
    pageTitle="Jelentkezzen be az analitikai adatok biztonsági |} Microsoft Azure"
    description="Megtudhatja, hogyan napló Analytics védi az adatait, és az adatok."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="banders"/>

# <a name="log-analytics-data-security"></a>Jelentkezzen be az analitikai adatvédelem

A Microsoft kötelességének tekinti, a személyes adatok védelme és az adatok védelme, lejátszásakor szoftverek és -szolgáltatások, amelyek segítséget nyújtanak az informatikai infrastruktúrát a szervezet kezelése. A rendszer ismeri fel, hogy mások számára az adatok bízza meg, amikor a meghatalmazás szigorú biztonsági szükséges. Microsoft nakhttp://go.microsoft.com/fwlink/?LinkId=311864 megfelelően igazodik szigorú megfelelőség és a biztonsági útmutató – a szolgáltatás működik-ból.

Biztonságossá tétele és az adatok védelme fontosságú felső a Microsoftnál. Kérjük, forduljon us kérdések, javaslatokkal, vagy problémákkal kapcsolatban a következő információkat, beleértve a saját biztonsági házirendek: [Azure által elérhető támogatási lehetőségek](http://azure.microsoft.com/support/options/)közül.

Ez a cikk ismerteti, adatainak összegyűjtött, feldolgozott, és a tevékenységek kezelése csomagja (MOBILE) napló Analytics védi. A webszolgáltatás csatlakozni, System Center Operations Manager műveleti adatokat szeretne gyűjteni, illetve adatok visszakeresése napló Analytics által használt Azure diagnosztika ügynökök is használhatja. A gyűjtött adatok küldhetők el az interneten keresztül tanúsítvány-alapú hitelesítés és az SSL-3-as az napló Analytics szolgáltatás üzemelteti, a Microsoft Azure-ban. Az adatok a ügynök tömöríthetők elküldés előtt.

A napló Analytics szolgáltatás felhőalapú adatok biztonságos kezeli az alábbi módszerekkel:

- adatok feladatkörök
- Adatmegőrzés
- fizikai biztonsági
- az esemény kezelése
- megfelelőség
- biztonsági szabványok tanúsítványok


## <a name="data-segregation"></a>Adatok feladatkörök

Felhasználói adatok legyen logikailag külön minden összetevő egész a MOBILE szolgáltatást. Az összes adat egy szervezet címkézett. A címkézés továbbra is fennáll, az adatok életciklus során, és ez a szolgáltatás egyes rétegben vannak érvényben. Ügyfelek van egy dedikált Azure blob, amely a hosszú távú adatokat tárolja.

## <a name="data-retention"></a>Adatmegőrzés

Indexelt keresési naplóadatok tárolja, és árak csomagja szerint tartja meg. További tudnivalókért olvassa el a [Napló Analytics árak](https://azure.microsoft.com/pricing/details/log-analytics/)című témakört.

A Microsoft 30 nap után a MOBILE workspace be van zárva törli az ügyfél adatait. A Microsoft is törli a Azure tárhely a fiókot, amely az adatokat tárolja. Felhasználói adatok eltávolítása után nem fizikai meghajtók elvész.

Az alábbi táblázat néhány a rendelkezésre álló megoldásokat MOBILE és a példák a típusú adatokat gyűjtenek.

| **Megoldás** | **Adattípusok** |
| --- | --- |
| Konfigurációs értékelése | Állapot adatai, konfigurációs adatok és metaadatok |
| Kapacitás tervezése | A Teljesítményadatok és metaadatok |
| Antimalware | Konfigurációs adatok és metaadatok |
| Rendszer frissítés értékelése | Metaadat-alapú és az állapot adatok |
| Log kezelése | Felhasználó által definiált eseménynaplók, a Windows-eseménynaplók és/vagy az IIS naplók |
| Változások követése | Szoftver készlet és a Windows-szolgáltatás metaadat-alapú |
| SQL és az Active Directory értékelése | WMI adatok, rendszerleíró adatokat, a Teljesítményadatok és az SQL Server dinamikus kezelése eredményeinek megtekintése |

A következő táblázat mutatja az adattípusok példákat:

| **Adattípus** | **Mezők** |
| --- | --- |
| A felhasználó | A felhasználó nevét, értesítés leírása, BaseManagedEntityId, probléma azonosítója, IsMonitorAlert, RuleId, ResolutionState, prioritás, súlyosságát, kategória, tulajdonosának, ResolvedBy, TimeRaised, TimeAdded, módosítás dátuma, LastModifiedBy, LastModifiedExceptRepeatCount, TimeResolved, TimeResolutionStateLastModified, TimeResolutionStateLastModifiedInDB, RepeatCount |
| Konfiguráció | CustomerID, AgentID, entityid beállítást, ManagedTypeID, ManagedTypePropertyID, a CurrentValue, ChangeDate |
| Esemény | EventId, EventOriginalID, BaseManagedEntityInternalId, RuleId, PublisherId, PublisherName, FullNumber, szám, kategória, ChannelLevel, LoggingComputer, EventData, EventParameters, TimeGenerated, TimeAdded <br>**Megjegyzés:** A Windows eseménynaplójának be egyéni mezőket tartalmazó rendszerbe eseményeket, MOBILE gyűjti össze őket. |
| Metaadatok | BaseManagedEntityId, ObjectStatus, szervezeti_egység, ActiveDirectoryObjectSid, PhysicalProcessors, hálózatnév, IP-cím, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-címet, NetbiosDomainName, LogicalProcessors, Dns_név, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime |
| Teljesítmény | Objektumnév, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded |
| Állam | StateChangeEventId, és a StateId, NewHealthState, OldHealthState, környezetben, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, módosítás dátuma, LastGreenAlertGenerated, DatabaseTimeModified |

## <a name="physical-security"></a>Fizikai biztonsági

A napló Analytics MOBILE szolgáltatásban a Microsofton van működtetett, és minden tevékenység van jelentkezve, és naplózható. A szolgáltatás Azure teljesen fut, és megfelel Azure általános mérnöki. A [Microsoft Azure biztonsági áttekintése](http://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf)a 18 lapon megtekintheti a Azure eszközök fizikai biztonsága olvashat. Fizikai hozzáférési jogosultsággal, és biztonságos területek bárki, aki a továbbiakban nem rendelkezik a MOBILE szolgáltatást, többek között az átadás és lemondási felelősséget módosítja egy üzleti napon belül. A globális fizikai infrastruktúrát használjuk, a [Microsoft-Adatközpontokkal](https://www.microsoft.com/en-us/server-cloud/cloud-os/global-datacenters.aspx)olvashat.

## <a name="incident-management"></a>Az esemény kezelése

MOBILE tartalmaz egy esemény kezelési folyamat, amely az összes Microsoft-szolgáltatás igazodjon. Összefoglalva, hogy:

- Használjon megosztott felelősség modellt, ahol biztonsági felelősség egy részét tartozik-e a Microsoft közben az ügyfélnek tartozik bizonyos része
- Azure védelmi események kezelése
  - Az első jelzést, a vizsgálat indításához a esemény feltárása
  - Mérje fel, hogy a hatás és esemény szerint egy Skype-hívás az esemény válasz csapatban dolgozó munkatárssal súlyosságát. Adatokon alapuló a felmérés is, illetve nem eredményezhet további a biztonsági válasz csapatnak kiterjesztés.
  - A műszaki vagy Törvényszéki vizsgálat lebonyolítása, beágyazott, kezelési és megoldás stratégiák meghatározása biztonsági válasz szakértőktől esemény diagnosztizálása. Ha a biztonsági csoport úgy véli, hogy ügyféladatok előfordulhat, hogy rendelkezik válnak kitéve jogellenes vagy jogosulatlan bemutató, az ügyfél esemény értesítési folyamat párhuzamos végrehajtását megkezdi a párhuzamos.  
  - Stabilizálásához, és helyreállítása az eseményt. A válasz az esemény csapatának hoz létre a helyreállítási terv csökkentésében a problémát. Válság biztonsági lépéseket a probléma által sújtott rendszerek quarantining például akkor fordulhat elő, azonnal és diagnosztika párhuzamosan. Hosszú távú megoldásokkal kapcsolatban is kell megtervezni a azonnali kockázat eltelte után előforduló.  
  - Zárja be az eseményt, és egy utólagos használata. A válasz az esemény csapatának létrehoz egy utólagos kiemelő tartalmazó szabályok, folyamatok és folyamatok, hogy az esemény egy előfordulásának módosítása irányuló szándékát, az esemény részleteit.
- A biztonsági események ügyfelek értesítése
  - A probléma által sújtott vevők, és bárki, aki hatással van, mint a lehető értesítés részletes megadásához a hatókör meghatározása
  - Hozzon létre értesítést arról, hogy az ügyfelek részletes elegendő információt, hogy nyomozásban végrehajtani a befejezési és bármely vállalt azok van a végfelhasználói közben az értesítési folyamat nem jogosulatlanul késleltetése felel meg a szükséges.
  - Erősítse meg, és az eseményt, szükség szerint deklarálhatnak.
  - Az esemény értesítjük túlzott késedelem nélkül és bármely jogi vagy szerződési lekötési ügyfelek értesítést. Biztonsági eseményekkel kapcsolatos értesítési érkeznek meg egy vagy több ügyfél rendszergazdák bármilyen eszközzel, Microsoft lehetőséget választja, beleértve a mailben.
- Csoportwebhely felkészüléshez és a tananyagok lebonyolítása
  - Biztonság és tájékoztatás képzés, amely segíti őket azonosításához és gyanús biztonsági hibát elvégzéséhez szükségesek a Microsofton.  
  - A Microsoft Azure szolgáltatás dolgoznak operátorok összeadás oktatás kötelezettségek az ügyfél adatait tároló bizalmas rendszereihez hozzáférésüket billentyűzetre van.
  - Microsofton biztonsági választ kap a szerepkörökhöz speciális oktatás


Minden olyan felhasználói adatok elvesztése esetén azt egy napon belül ügyfelek értesíteni kell. Felhasználói adatok elvesztését azonban MOBILE soha nem történt. Ezenkívül azt létrehozott adatok másolatait, és földrajzi oszlik.

Hogyan Microsoft reagál biztonsági kapcsolatos további tudnivalókért lásd: [Microsoft Azure biztonsági válasz a felhőben](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/1/Microsoft Azure Security Response in the cloud.pdf).

## <a name="compliance"></a>Megfelelőség

A MOBILE szoftver fejlesztés és szolgáltatás csapat adatok biztonsági és irányítási célokra program támogatja az üzleti követelmények, és nakhttp://go.microsoft.com/fwlink/?LinkId=311864 megfelelően igazodik a törvényi és szabályok a [Microsoft Azure az Adatvédelmi központ](https://azure.microsoft.com/support/trust-center/) és a [Microsoft adatvédelmi központ megfelelőség](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx)leírt módon. Hogyan MOBILE létrehozza a biztonsági követelményeknek, biztonsági funkciók azonosítja, kezeli, és figyeli a kockázatok is megkötésekről van. Nyereséggel azt lebonyolítása ismerkedést házirendeket, szabványok, eljárások és irányelveket.

Mindegyik MOBILE fejlesztési csapatban dolgozó munkatárssal hivatalos alkalmazás biztonsági oktatás kap. Belső fogjuk kiszámolni verzió ellenőrző rendszer szoftverfejlesztési számára. A vezérlő rendszerhez minden szoftver projekt védett.

A Microsoft tartalmaz, amely ellenőrzi, és a Microsoft szolgáltatások értékeli biztonság és megfelelőség csapatnak. A csapat alkotó adatok biztonsági szakemberek, azok nem a MOBILE kidolgozása mérnöki részlegek társítva. A biztonsági szakemberek saját felettesi lánca van, és a termékek és szolgáltatások annak érdekében, és a megfelelőségre független felmérések lebonyolítása.

A Microsoft igazgatótanácsi értesíti és éves jelentés adatai biztonságát kapcsolatos programok a Microsoft.

A MOBILE szoftver fejlesztés és szolgáltatás csapatának aktívan használata a Microsoft Legal és a megfelelőség csapatok és szerezheti be a tanúsítványok számos más üzleti partnereivel.

## <a name="security-standards-certifications"></a>Biztonsági szabványok tanúsítványok

Bejelentkezés az analitika MOBILE jelenleg megfelel az alábbi biztonsági követelményeknek:

- [ISO/IEC 27001 tanúsítással](http://www.iso.org/iso/home/standards/management-standards/iso27001.htm) és az [ISO/IEC 27018:2014](http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498) kompatibilis
- Fizetési kártya (PCI kompatibilis) adatok biztonsági szabványos (PCI DSS) a PCI biztonsági szabványok Tanács.
- A [szolgáltatás szervezeti vezérlők (SOC) 1 1 és 1-es típusú SOC 2](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2) kompatibilis
- A Windows közös műszaki feltétel
- Microsoft megbízható számítások hitelesítő
- Azure szolgáltatásként MOBILE használó összetevők Azure megfelelőségi követelmények igazodjon. Erről további a [Microsoft adatvédelmi központ megfelelőségi](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx).


## <a name="cloud-computing-security-data-flow"></a>Az a felhő biztonsági adatfolyam számítások
Az alábbi ábra az adatáramlást az egy felhőalapú biztonsági architektúrája látható vállalaton kívüli, és hogyan, biztonságos, mint helyezi át a napló Analytics szolgáltatás, végül Ön által látott a MOBILE portálon. Minden egyes lépés további információt a diagram követi.

![Adatgyűjtés MOBILE és a biztonsági képe](./media/log-analytics-security/log-analytics-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. regisztrálhat napló Analytics és adatgyűjtés

Adatok küldése a napló Analytics szervezete Windows ügynökök, Linux az Azure virtuális gépeken futó vagy MOBILE ügynökök rendszeren futó ügynökök beállítása. Ha Operations Manager ügynökök használ, majd használni a Fiókkonfiguráló varázslót a műveletek konzolban konfigurálása őket. (Ez lehet meg, a többi felhasználónként vagy a felhasználók egy csoportja) felhasználók hozzon létre egy vagy több MOBILE fiókokat (MOBILE munkaterületek), és regisztráljon ügynökök az alábbi fiókok egyikével:


- [Szervezeti azonosító](../active-directory/sign-up-organization.md)

- [Microsoft - fiókjából, Outlook, az Office Live, MSN](http://www.microsoft.com/account/default.aspx)

Egy MOBILE munkaterület hol adatok gyűjtött, összesíti, elemezni, de mutatják be. Munkaterület elsősorban a partíciót adatok eszközként, és az egyes munkaterületeken egyedi. Érdemes lehet például, hogy az egyik MOBILE munkaterület felügyelt termelési adatok és a másik munkaterület felügyelt tesztadatokat. Munkaterületek támpontot-rendszergazdai felhasználói hozzáférés szabályozása az adatokat. Egyes munkaterületeken is van társítva több felhasználói fiókot, és a felhasználói fiókokat több MOBILE munkaterület férhet hozzá. A munkaterületek adatközponthoz régió alapján hoz létre. Egyes munkaterületeken az egyéb adatközpontokkal a régióban elsősorban a MOBILE szolgáltatáselérhetőség van replikált.

Az Operations Managerhez Ha befejezte a Fiókkonfiguráló varázslót, minden futó Operations Manager kezelése csoport kapcsolatot létesít a napló Analytics szolgáltatással. Ezt követően az számítógépek hozzáadása varázsló választhatja ki, mely a számítógépek, a kezelés csoport engedélyezettek adatokat küld a szolgáltatásnak. Más ügynök típusnál minden biztonságosan a MOBILE szolgáltatáshoz csatlakozik.

Csatlakoztatott rendszerek és a napló Analytics szolgáltatás közötti összes kommunikáció titkosítva van.  A TLS (HTTPS) protokoll titkosításhoz.  A Microsoft SDL folyamat annak biztosítására, a legutóbbi tovább a titkosítási protokollok naprakészen napló Analytics záradékot követő.

Ügynök különböző típusú adatokat gyűjt a napló ábrázolja. Által gyűjtött adatokat a típusú érték használt megoldások típusú függ. Megtekintheti a [napló Analytics hozzáadása solutions a megoldástárból](log-analytics-add-solutions.md)adatgyűjtés összefoglalását. Ezenkívül webhelycsoport részletesen érhető el a legtöbb megoldásokhoz. A megoldás egy előre definiált nézetet, napló keresési lekérdezések, adatok gyűjtése szabályok és feldolgozása logika az első lépésekhez. Csak a rendszergazdák napló Analytics segítségével importálása a megoldást. A megoldás importálása után az Operations Manager-kezelési kiszolgálók (ha van), majd minden Ön által választott ügynökök kerül. Ezután a ügynökök összegyűjti az adatokat.

## <a name="2-send-data-from-agents"></a>2. adatok küldése a ügynökök beállítása

Diagramtípusokat ügynök regisztrációs kulccsal regisztrálja, és a biztonságos kapcsolat folyamatban van a agent és a tanúsítvány-alapú hitelesítés és az SSL használatával a 443-as port napló Analytics-szolgáltatás között. MOBILE titkos tárolót hozhat létre és kulcsok karbantartása használ. Titkos kulcsokat van forgatva minden 90 napon és Azure-ban van tárolva, és az Azure műveletek, akik szintén követik szigorú szabályozási és a megfelelőség eljárások kezeli.

Munkaterület regisztrálása a napló Analytics szolgáltatás Operations Manager, és egy biztonságos HTTPS-kapcsolat létrehozása a Operations Manager management server között.

Ügynökök beállítása a Windows Azure virtuális gépeken futó csak olvasható tároló kulcs segítségével olvassa el a diagnosztikai események Azure táblázatokban.

Bármely ügynök nem tud kapcsolatba lépni a szolgáltatásban valamilyen okból, ha a gyűjtött adatok helyben tárolt egy ideiglenes gyorsítótárba, és küldje el az adatok 2 órán 8 percenként próbálja a management server. Az operációs rendszer hitelesítőadat-tárolót védi az ügynök gyorsítótárban tárolt adatokat. A szolgáltatás 2 óra elteltével nem lehet feldolgozni azokat az adatokat, ha a ügynökök adatokat fog sorba. A várakozási sorban található megtelik, ha a MOBILE adattípusok teljesítményadatokat kezdődő leválasztása kezdődik. A ügynök várólista korlát egy beállításkulcs úgy módosíthatja is azt, ha szükséges. Gyűjtött adatok tömörítve, és elküldi a szolgáltatást, a helyszíni-adatbázisok kihagyásával, így nem hozzáadása bármely betöltés őket. Gyűjtött adatokat küldi el, akkor a program törli a gyorsítótárból.

Fentebb ismertetett a ügynökök származó adatokat a rendszer elküldi SSL Microsoft Azure-adatközpontokkal. Tetszés szerint készült ExpressRoute is használhatja az adatokat a biztonság fokozása érdekében. Készült ExpressRoute módja közvetlenül csatlakozhat Azure a meglévő WAN hálózatról, például egy több protokoll címke csomagváltási (MPLS) VPN Használatát a hálózat szolgáltató által biztosított. További tudnivalókért olvassa el a [készült ExpressRoute](https://azure.microsoft.com/services/expressroute/)című témakört.


## <a name="3-the-log-analytics-service-receives-and-processes-data"></a>3. a napló Analytics szolgáltatás fogadása és folyamatok adatok

A napló Analytics szolgáltatás biztosítja, hogy bejövő adatok megbízható forrásból származó ellenőrzi a tanúsítványok és Azure-hitelesítéssel az adatok integritását. A feldolgozatlan nyers adatokból van [Microsoft Azure-tárolóban](../storage/storage-introduction.md) lévő blob-ként ezt követően tárolni, és nem titkosítva van. Azonban minden tároló Azure blob tartozik egy jogosultságkészletet kulcsok, amely csak a felhasználó számára érhető el. A tárolt adatokhoz típusú érték, amelyeket importálni és adatokat szeretne gyűjteni használt megoldások típusú függ. Ezután a napló Analytics szolgáltatás dolgozza fel az Azure tároló blog nyers adatait.

## <a name="4-use-log-analytics-to-access-the-data"></a>4. Log Analytics eléréséhez használt adatok

Jelentkezzen napló Analytics a MOBILE portálon a szervezeti fiók vagy korábban állítsa be Microsoft-fiók használatával. Az összes között a MOBILE portál és a MOBILE napló Analytics adatforgalom biztonságos HTTPS csatornán keresztül. A MOBILE portál használata esetén a munkamenet-azonosító (webböngészőben) felhasználói ügyfélen jön létre, és adatok vannak tárolva helyi gyorsítótár mindaddig, amíg befejeződik a munkamenetet. Amikor befejeződött, a gyorsítótár törlődik. Ügyféloldali cookie-kat, amelyek nem tartalmaznak személyes azonosításra használható adatokat, automatikusan nem törlődnek. Munkamenet cookie-k HTTPOnly vannak megjelölve, és vannak-e csatlakoztatva. Egy előre meghatározott üresjárati idő után a MOBILE portál munkamenet megszakad.

A MOBILE portálon használja, akkor is adatok exportálása CSV-fájlba és az adatoknak keresési API-khoz is elérheti. Exportálás CSV korlátozódik / exportálás 50 000 sort és API adatok 5000 sorok keresése egy korlátozott hozzáférésű.

## <a name="next-steps"></a>Következő lépések

- A [napló Analytics – első lépések](log-analytics-get-started.md) további tudnivalók a napló Analytics és get felfelé és futó perc alatt.
