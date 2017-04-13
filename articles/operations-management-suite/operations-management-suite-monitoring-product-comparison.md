<properties 
   pageTitle="A Microsoft-termékek összehasonlítása figyelése |} Microsoft Azure"
   description="A Microsoft műveletek Management csomagja (MOBILE), a Microsoft cloud-alapú, amelyek segítségével kezelése és védelme a helyszíni és felhőbeli infrastruktúra management levelezőprogramból.  Ez a cikk a MOBILE szereplő más szolgáltatások azonosítja, és a hivatkozásokra kattintva részletes tartalmuk."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="microsoft-monitoring-product-comparison"></a>A Microsoft felügyeleti termékek összehasonlítása

Ebben a cikkben architektúráját, hogyan figyelik a források, és hogyan végre azokat a gyűjtött adatok elemzése a logika System Center műveletek Manager (SCOM) és a napló Analytics műveletek Management csomagja (MOBILE) összehasonlítása.  Ez a ad a különbségeket és a relatív erősségek alapvető értelmezését.  

## <a name="basic-architecture"></a>Egyszerű architektúra
### <a name="system-center-operations-manager"></a>System Center Operations Manager

Minden SCOM összetevő az Adatközpont van telepítve.  [Ügynök telepítve van](http://technet.microsoft.com/library/hh551142.aspx) a Windows és Linux gépek SCOM kezeli.  Kommunikáció a SCOM adatbázis és az adatok raktári a [Kezelési kiszolgálók](https://technet.microsoft.com/library/hh301922.aspx) ügynökök csatlakozni.  A tartomány hitelesítése management Lync-kiszolgálókhoz használja arra, ügynökök.  Megbízható tartomány megbízható kívüli azokat végezze el a hitelesítés, vagy az [Átjáró-kiszolgáló](https://technet.microsoft.com/library/hh212823.aspx).

SCOM két SQL-adatbázisait, az üzemeltetési adatainak és egy másik, a jelentéskészítő és adatok elemzése támogatási adatraktár szükséges.  Egy [Jelentéskészítő kiszolgálót](https://technet.microsoft.com/library/hh298611.aspx) SQL Reporting Services jelentheti a adatraktár származó adatokat a fut. 

SCOM figyelheti a felhőben az erőforrások kezelése csomagok segítségével például [Azure](https://www.microsoft.com/download/details.aspx?id=38414), az [Office 365-ben](https://www.microsoft.com/download/details.aspx?id=43708)és a [AWS](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/AWSManagementPack.html)termékek.  Ezek management csomagok használni egy vagy több helyi ügynökök proxyk felfedezése felhő erőforrások és a futó munkafolyamatok felmérni, a teljesítmény és elérhetőségét.  A proxykiszolgáló ügynökök is használják a [monitor hálózati eszközök](https://technet.microsoft.com/library/hh212935.aspx) és más erőforrások: a külső.

A műveletek konzol a Windows-alkalmazás, amely valamelyik management kiszolgáló csatlakozik, és lehetővé teszi, hogy a rendszergazda megtekintése és gyűjtött adatok elemzése és a SCOM környezet beállításához.  A webes konzol bármely IIS-kiszolgálón tárolhatja, és lehetővé teszi az adatelemzés böngészőn keresztül.

![SCOM architektúra](media/operations-management-suite-monitoring-product-comparison/scom-architecture.png)

### <a name="log-analytics"></a>Log Analytics

A legtöbb MOBILE összetevői Azure a felhőben, telepítése, és kezelheti a minimális költség és felügyeleti munkamennyiség.  Az összes, a naplófájl Analytics által gyűjtött adatokat a MOBILE tárházba vannak tárolva.

Log Analytics három forrásokból származó adatokat gyűjt:

- Fizikai és virtuális gépeken futó operációs rendszert futtató Windows és a [Microsoft figyelése ügynök (MMA)](https://technet.microsoft.com/library/mt484108.aspx) vagy Linux rendszerhez és a [Tevékenységek kezelése csomagja Agent Linux](https://technet.microsoft.com/library/mt622052.aspx).  Lehet, hogy ezek gépeken futó helyszíni, vagy az Azure vagy egy másik a virtuális gépeken futó felhőalapú.
- Azure tároló fiók Azure dolgozó szerepkörrel, a webes szerepkör vagy a virtuális gép által gyűjtött [Azure diagnosztika](../cloud-services/cloud-services-dotnet-diagnostics.md) adatokkal.
- A [kapcsolat SCOM felügyeleti csoporthoz](https://technet.microsoft.com/library/mt484104.aspx).  Ebben a konfigurációban a ügynökök SCOM management-kiszolgálók, amelyek az adatok előadása a SCOM adatbázishoz, ahol majd eljuttatni a MOBILE adatokat tároló kommunikálni.
A rendszergazdák gyűjtött adatok elemzése és napló Analytics konfigurálása a MOBILE portáljába, amely üzemelteti Azure-ban, és egy tetszőleges böngészővel webböngészőn keresztül elérhetők.  A szokásos platformokon az adatok eléréséhez mobilalkalmazások érhetők el.

![Log Analytics-architektúra](media/operations-management-suite-monitoring-product-comparison/log-analytics-architecture.png)

### <a name="integrating-scom-and-log-analytics"></a>SCOM és a napló Analytics integrációja

SCOM napló elemzéséhez adatforrásként segítségével is élvezheti mindkét terméket hibrid környezet figyelése funkcióját.  Beállíthatja, hogy a meglévő SCOM ügynökök MOBILE, továbbra is management csomagok lebonyolítása SCOM kívül végzendőként műveletek konzolon.  
Adatok egy csatlakoztatott SCOM kezelése csoport érkeznek napló Analytics négy módszerek valamelyikével:

- Események és a teljesítményadatok gyűjti össze a agent és SCOM kézbesítve.  Kezelési kiszolgálók SCOM a napló Analytics majd kézbesítése az adatokat.
- Bizonyos eseményekhez, például az IIS naplók és biztonsági események továbbra is a ügynök közvetlenül az napló Analytics kézbesítése.
- Néhány megoldás külön szoftver kézbesítése a agent, vagy a szoftver további adatokat szeretne gyűjteni telepítését igényli.  Ezeket az adatokat a szokásos küld közvetlenül napló Analytics.
- Néhány megoldás adatokat gyűjti közvetlenül a SCOM kezelési-kiszolgálók, amelyek nem a agent származik.  Például az [értesítési projektmenedzsment megoldás](https://technet.microsoft.com/library/mt484092.aspx) gyűjti össze a riasztások SCOM után hozott létre.

## <a name="monitoring-logic"></a>Logika figyelése
SCOM és a napló Analytics ügynökök gyűjtött hasonló adatokkal végzett munkához, de van hogyan azok definiálása és megvalósítása a saját logika vonatkozó adatok gyűjtése és hogyan azok elemezheti az adatokat gyűjtenek alapvető különbség.

### <a name="operations-manager"></a>Operations Manager
Logika figyelése SCOM alkalmazása [management csomagok](https://technet.microsoft.com/library/hh457558.aspx) logika a Lync, összetevők felfedezése tartalmazó azokat a elemeket, valamint az elemezni kívánt adatokat gyűjt a vonatkozó mérési.  Nyomon követési adatok lehet mindössze egy eseményt vagy teljesítmény számláló gyűjteni, vagy kihasználva a komplex logikai parancsfájl szerepelni fog.  A különféle [Microsoft és harmadik fél alkalmazásában](http://go.microsoft.com/fwlink/?LinkId=82105) nemcsak a hardver és a hálózati eszközök, amely tartalmazza a teljes figyelése Management csomagok érhetők el.  Azt is megteheti, hogy a [Szerző saját management csomagok](http://aka.ms/mpauthor) egyéni alkalmazások.

Adatkezelési csomagok több munkafolyamatok, hogy az egyes elvégzi néhány distinct felügyeleti függvény, például a teljesítmény számláló mintavételi, a szolgáltatás állapotának ellenőrzése vagy a parancsfájl futtatása tartalmazzák.  Munkafolyamat egyes független fut, és saját eredménye például adatbázis azt ír, és hogy készítése jelzést határozza meg. 

Munkafolyamat, például a gyakoriság futnak, a küszöbértékét, amelyekben ezek érdemes megfontolni a hiba részleteit, és a figyelmeztetés azok készítése súlyosságát felülbírálhatja.  Egyéni munkafolyamatok hozzáadásával is adhat további funkciókat.

![Felülíró](media/operations-management-suite-monitoring-product-comparison/scom-overrides.png)

Felügyeleti csomag az Operations Manager-adatbázis telepített és automatikusan elosztott anyagokkal management-kiszolgálók.  Minden egyes ügynök automatikusan management csomagok letöltése, és töltse be a címzettek telepítették alkalmazásoknak megfelelő munkafolyamatok.  A agent által gyűjtött adatokat vissza az a SCOM adatbázis és az adatok raktári való beillesztését a management server kézbesítve.  A műveletek konzol megtekintéséhez és elemzéséhez Ez az egyéni nézetek, irányítópultok és jelentések, a felügyeleti csomag szereplő adatoknak teszi lehetővé.

Az eloszlás management csomagok mutatja be a következő diagramon.

![Felügyeleti csomag továbbításához](media/operations-management-suite-monitoring-product-comparison/scom-mpflow.png)

### <a name="log-analytics"></a>Log Analytics
#### <a name="event-and-performance-collection"></a>Esemény és a teljesítmény gyűjtemény
Log Analytics események és a teljesítmény számláló ügynök rendszerek forrásokhoz, például a Windows eseménynaplójának IIS naplók és Syslog használatával gyűjti össze.  Amelynek adatok összegyűjtött a napló Analytics-portálon keresztül, és kattintson a Log lekérdezések a gyűjtött adatok elemzéséhez létrehozása feltételeket adhat meg.  Szabványos feltételcsoport van megadva, amikor hoz létre a MOBILE munkaterület, és megadhatja, hogy az adott alkalmazások további adatokat. 

![Eseménynaplók definiálásáról a napló Analytics](media/operations-management-suite-monitoring-product-comparison/log-analytics-definedata.png)

Miközben SCOM munkafolyamatai számos részletes általában határozza meg az adott feltételeknek megfelelő adatok és a művelet, amelyet a választ kell elvégezni, napló Analytics foglalja magában: adatgyűjtés általánosabb feltételei  Log lekérdezések és megoldások nyújt további célzott elemzése és feltétele eljáró meghatározott adatokat a felhőben van a összegyűjtése után.

#### <a name="solutions"></a>Megoldások
Megoldások a adatgyűjtési és -elemzés további logika szükséges.  Megoldások gyűjtemény használatával adhat hozzá MOBILE-előfizetéséhez a megoldást is választhat.

![Megoldástár](media/operations-management-suite-monitoring-product-comparison/log-analytics-solutiongallery.png)

Megoldások elsősorban futtassa a felhőben, események és a MOBILE tárházba összegyűjtött teljesítmény számláló elemzése kezeléséről.  Előfordulhat, hogy is meghatározzák további adatok összegyűjtve, amely a napló lekérdezések, illetve a megoldást a MOBILE irányítópulton további felhasználói felülete elemezheti. 

A [változások követése megoldás](https://technet.microsoft.com/library/mt484099.aspx) például konfigurációs módosítások ügynök rendszeren észleli, és eseményeket, amelyek több grafikus nézetek elemezheti MOBILE tárházba észleli módosítások ír.  A napló lekérdezések gyűjti össze a megoldást a részletes adatok megjelenítése az összesített nézetben is részletezhetők.


Miközben választhat, hogy melyik megoldások hozzáadása az előfizetéshez, az azt jelenti, hogy a saját megoldások létrehozása jelenleg nem rendelkeznek.  Választhat a események és a teljesítmény számláló összegyűjtése és saját napló lekérdezések alapuló egyéni nézeteket hozhat létre.

Az alábbi ábra a megfigyeléssel logika a napló Analytics összesített.

![Log Analytics megoldás továbbításához](media/operations-management-suite-monitoring-product-comparison/log-analytics-solution-flow.png)

## <a name="health-monitoring"></a>Állapot ellenőrzése
### <a name="operations-manager"></a>Operations Manager
SCOM az alkalmazás különböző összetevői modell és is megadhatja a valós idejű állapot minden egyes.  Így nem csak a nézet talált hibákat, és a teljesítmény időbeli, hanem az adott időpontban egy alkalmazást vagy a rendszer a tényleges állapot és az egyes összetevői érvényesítéséhez.  Megérti, hogy az alkalmazás elérhető időszakok, mert az állapot motor a SCOM támogatja szolgáltatás rendelkezést SZOLGÁLTATÁSISZINT-, amelynek elemezni, és jelentse a rendelkezésre álló időbeli alkalmazás is.

Az alábbi nézetben például SQL-adatbázis motorok SCOM felügyeli valós idejű állapotának jeleníti meg.  Az adatbázisok adatbázis-kezelők egyik állapotának jelenik meg az alsó felében a nézetet.

![Állapot megtekintése](media/operations-management-suite-monitoring-product-comparison/scom-state-view.png)

Az állapot Explorer az adatbázis-kezelők egyik lásd alább a monitorok, amely alapján határozza meg az általános állapotát.  Ezek a monitorok az SQL-felügyeleti csomag az meghatározását, valamint futtassanak SQL-adatbázis összes motorok SCOM által talált.

![Állapot explorer](media/operations-management-suite-monitoring-product-comparison/scom-health-explorer.png)

Több rendszeren összetevők elosztott alkalmazás állapotának mérésére kombinálhatók.  Ez az üzletági alkalmazások több elosztott összetevőket tartalmazó különösen hasznos lehet.  Az alkalmazás elérhetőségét a modell, amely minden összetevő állapota méri, hogy az összesítő hozhat létre.

Az Active Directory példája egy felügyeleti csomag elosztott összetevői elemzése modell tartalmazza.  A minta az alábbi ábrán a teljes környezet és erdők, a tartományok és a tartomány vezérlők közötti kapcsolat állapota.  Minden összetevő összetevők kiválasztása és a több monitor használatával a fenti SQL példához hasonló tartalmazza.

![SCOM diagram nézetben](media/operations-management-suite-monitoring-product-comparison/scom-diagram-view.png)

### <a name="log-analytics"></a>Log Analytics
MOBILE nem tartalmazza az egy közös motor modell alkalmazások, illetve a valós idejű állapot mérjük.  Egyéni megoldások értékelheti általános állapotának adott szolgáltatások gyűjtött adatok alapján, és a valós idejű elemzés elvégzéséhez Agent egyéni logika telepítheti őket.  Megoldások futtassa a felhőben, az Access-szel a MOBILE tárházba, mert azok mélyebb elemzés, mint a szokásos végzi management csomagok gyakran lehet nyújtani. 

Például az [Active Directory felmérése és SQL értékelési megoldások](https://technet.microsoft.com/library/mt484102.aspx) gyűjtött adatok elemzése, és adja meg értékelést a környezeti különböző tényezők.  Javaslatok javítása az elérhetőség és a teljesítmény a környezet végrehajtott tartalmazza.

![Active Directory értékelési megoldás](media/operations-management-suite-monitoring-product-comparison/log-analytics-ad-assessment.png)

## <a name="data-analysis"></a>Adatok elemzése
SCOM és a napló Analytics minden adja meg a szolgáltatások gyűjtött adatok elemzése céljából.  SCOM a műveletek konzol a különféle formátumok és a jelentések adatainak a adatraktár táblázatos formában kapcsolatos legfrissebb adatok elemzéséhez használható nézetek és irányítópultok tartalmaz.  Log Analytics egy teljes napló lekérdezési nyelv és felületet biztosít a MOBILE adattárban adatok elemzése.  SCOM napló elemzéséhez használja adatforrásként, amikor a tárházba a SCOM által gyűjtött, így a napló Analytics eszközök mindkét rendszerekből elemzéséhez használható adatokat tartalmazza.

### <a name="operations-manager"></a>Operations Manager

#### <a name="views"></a>Nézetek
A műveletek konzolban nézetek SCOM által gyűjtött különböző adattípusú megtekintése az események, figyelmeztetések és állapot adatokat, a szokásos táblázatos különböző formátumokban, és a teljesítményadatok vonaldiagramok teszi lehetővé.  Nézetek végezze el a minimális elemzés vagy az adatok összevonására vonatkozóan, de lehetővé teszi, hogy az adott feltételek szerint szűrheti. 

![Nézetek](media/operations-management-suite-monitoring-product-comparison/scom-views.png)

Adatkezelési csomagok általában az alkalmazás vagy a rendszer, amely figyeli támogató több nézet nyújt.  Ez a különböző objektumokat a felügyeleti csomag találja, riasztási észlelt problémák, és nézeteket teljesítmény számláló az állapot nézetek tartalmazhat.

Nézetek különösen alkalmasak a környezet, ideértve a megnyitott riasztások aktuális állapotát és a megfigyelt rendszerek és objektumok állapotának elemzéséhez használható.  Felhatolás részletes eseményt vagy egy adott típusú támogatása érdekében a kiváltóok diagnosztizálása teljesítményadatokat le. Hasonlóképpen a teljesítmény és az alkalmazások felmérése az aktuális állapota különböző összetevői állapotának tekinthet meg.

#### <a name="dashboards"></a>Az irányítópultok
A műveletek konzolban irányítópultok elsősorban ugyanezekkel az adatokkal működik, mint a nézetek azonban több testre szabható, és sokoldalúbb megjelenítések tartalmazhatnak.  Egy sor olyan szabványos irányítópultok érhetők el, hogy egyszerűen testreszabhatja a saját céljából.  Egy PowerShell listáját megjelenítő vezérlővel PowerShell lekérdezés által visszaadott adatok megjelenítő is használhatja.

![Irányítópult](media/operations-management-suite-monitoring-product-comparison/scom-dashboard.png)

A fejlesztők adja hozzá egyéni összetevőket őket a felügyeleti csomagok szerepeltetni irányítópultok lehetősége van.  Ezek is lehet erősen speciális egy adott alkalmazásra, például az alább látható módon SQL-felügyeleti csomag az irányítópult.  Az irányítópult is használható sablonként egyéni verzióira vonatkozóan.

![SQL-irányítópult](media/operations-management-suite-monitoring-product-comparison/scom-sql-dashboard.png)

#### <a name="reports"></a>Jelentések
Jelentések a SCOM a adatraktár táblázatos formában adatok elemzését.  Is nyomtatni, és más fájlformátumokban, többek között PDF-fájlt, a CSV és a Word automatikus kézbesítési ütemezett.  Jelentések, különösen alkalmasak, a hosszú távú trendek elemzése az adatraktár adatainak használata.

Adatkezelési csomagok általában fog szolgálni egyéni jelentések egy adott alkalmazást.  Kijelölhet is testre szabhatja a saját alkalmazások vagy alkalmi elemzését általános valamelyikére-tárból.

Az alábbiakban az Active Directory felügyeleti csomag által gyűjtött adatokat tartalmazó minta Projektteljesítmény-jelentés.

![Jelentés](media/operations-management-suite-monitoring-product-comparison/scom-report.png)

### <a name="log-analytics"></a>Log Analytics
Log Analytics analízist nélkül hozhat létre egyéni nézetek vagy jelentések több alkalmazásokból származó adatok között használó [lekérdezési nyelv](https://technet.microsoft.com/library/mt484120.aspx) van.  MOBILE alkalmazása a felhőben, mert a teljesítmény lekérdezések és adatok elemzése nem vonatkoznak a hardver korlátozásait, és a is gyors elemzése többek között a rekordok milliónyi lekérdezések. 

Log Analytics lekérdezések is egyéb funkciók alapján.  Mentse a lekérdezést, annak eredményei exportálása az Excelbe, vagy már automatikusan rendszeres időközönként futtassa és értesítés létrehozása, ha az eredményeket az adott feltételeknek megfelelő.  

![Log lekérdezés továbbításához](media/operations-management-suite-monitoring-product-comparison/log-analytics-query-flow.png)

Az alábbi képen a napló Analytics lekérdezés.  Ebben a példában a "lépések" a neve az összes eseményének vissza és esemény szerint csoportosított azonosítóval.  A felhasználói egyszerűen biztosít a lekérdezést, és a napló Analytics dinamikusan hoz létre a felhasználói felület hajtsa végre az elemzés.  Minden elem kijelölése a listában, az események részletes adatokat ad vissza.

![Log lekérdezés](media/operations-management-suite-monitoring-product-comparison/log-analytics-query.png)

Kívül nyújtó alkalmi elemzés, napló Analytics lekérdezések menthetők későbbi felhasználás céljából, is megjelenik a [MOBILE irányítópult](http://technet.microsoft.com/library/mt484090.aspx) az alábbi példában látható módon.

![MOBILE irányítópult](media/operations-management-suite-monitoring-product-comparison/log-analytics-dashboard.png)

## <a name="next-steps"></a>Következő lépések

- Telepítse [a System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh205987.aspx).
- Regisztráció [Napló](https://azure.microsoft.com/documentation/services/log-analytics)ábrázolja.  
