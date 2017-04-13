<properties
   pageTitle="Azure SQL-adatbázis Azure esettanulmány - Snelstart |} Microsoft Azure"
   description="Megismerheti, hogyan SnelStart használja a vállalati verzió szolgáltatásai és 1000 új Azure SQL-adatbázisait havonta mértéke gyorsan kibontott SQL-adatbázis"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/08/2016"
   ms.author="carlrab"/>

# <a name="with-azure-snelstart-has-rapidly-expanded-its-business-services-at-a-rate-of-1000-new-azure-sql-databases-per-month"></a>Az Azure-SnelStart gyorsan bővített vállalati verzió szolgáltatásai és 1000 új Azure SQL-adatbázisait havonta mértéke

![SnelStartLogo](./media/sql-database-implementation-snelstart/snelstartlogo.png)

Hollandia SnelStart teszi népszerű pénzügyi - és üzleti-kezelési szoftvert kis - és középvállalatok (SMB). 55,000 ügyfeleinek 110 alkalmazottak, beleértve az informatikai szakemberek 35 a munkatársai által vannak kiszolgált. Áthelyezésével asztali program a szoftver-mint-a-szolgáltatás (szoftver) kínáló Azure-ra épülő, az SnelStart lett beépített szolgáltatások, a legtöbb automatizálása jól ismert környezetéből a C#, és a teljesítmény és méretezhetőség optimalizálása által sem felett - vagy a – kiépítési sikertörté rugalmas adatbázis készletek kezelése. Azure körét SnelStart a helyszíni és felhőbeli közötti áthelyezése vevők rugalmasság.

> [AZURE.VIDEO azure-sql-database-case-study-snelstart]

##<a name="why-snelstart-extended-services-from-the-desktop-to-the-cloud"></a>Miért SnelStart bővített szolgáltatások az asztalról a felhőbe

> "Azure használata azt jelenti, hogy gyorsabban szoftver olvasása, gyorsan vevői igények léphessenek, és megoldások méretezni, amikor igények növelése."

> – Henry már, szoftver Architect

SnelStart futtatta sikeres szoftver üzleti az év hagyományos fejlesztési modell segítségével: kód csomagolása, a kiszállítása, és ismételje meg. Az idő múlásával a változás tempóban növekedett közé tartoznak gyorsabban és gyorsabbá. Szabályok gyakran változó, és ügyfelek könnyebben módszert, amellyel pénzügyi rekordok folyamat, és azok könyvelők és kormányzati ezekhez a változtatásokhoz nyilvántartása együttműködéshez szükséges.

"Szállítási szoftver vevőknek költséges és volt korlátozó," Henry lett, szoftver architect megfelelően. "Gyártási, a csomagolást és a szállítási költségek korlátozott gyakoriságának azt sikerült engedje fel az szoftvert. Csomag frissítéseinek periodikusan szállítóleveleket kellett azt, de, hogy végrehajtotta nehezen ügyfelei igényeknek megfelelően módosítását. Azt is soha nem biztos lehet abban, hogy ügyfeleink a termék a legújabb verzióra frissíteni. Emiatt azt kellett támogatja a többféle verzióját, kezdeményezhet nagyon nehezen, valamint a támogatási feladata."

Már veszi fel, "azt megy végbe, ha szeretne egy gyorsított a program és a fontos frissítések lépést, hogy sikerült gyorsabb feltárásáról, és hozzon létre új szolgáltatást ügyfeleink. Azt is szeretett volna további folyamatok automatizálása ügyfelei vállalati verzió – felügyeleti igényeit egyszerűsítése érdekében lehetőséget."

A megoldás SnelStart, szolgáltatásai meghosszabbítani a felhőalapú szoftver szolgáltató valamit volt. Az SQL Azure-adatbázis platform segíteni SnelStart út a fő informatikai terhelést, amely az infrastruktúra-mint-a-szolgáltatás (IaaS) megoldás igényelt volna nélkül.

A felhő modell lehetővé teszi a SnelStart hibajavítás, és gyorsan, adjon meg új funkciók ügyfelek erre szolgáló letöltéséhez és a szoftver frissítése nélkül is. A következők szerint lett "használata Azure cloud services lehetővé teszi, hogy velünk a gyorsan változó követelmények külső féltől származó, használni. Bízza meg az új verzió szállítandó ügyfelek ezer, akkor módosíthatja az asztali alkalmazás, a harmadik fél által igényelt új formátumokban küldött információk."

##<a name="a-modern-company-with-traditional-roots"></a>A hagyományos gyök modern vállalata

SnelStart való 1964, amikor a képet is elhelyezhet indítja el a vállalat musical instrument részeinek gyártó készítési kor szerény gyök a modern, Agilis, magas technológiájú üzleti. A szoftver SnelStart üzleti szívecskés valójában lépések során a száma, korlátok között maradjon a személyi számítógép 1980-as években, a zúzása. A vállalat szükséges jobb megoldás, mint a könyvelési az idő, a szoftvert, így a saját kezébe ügyekben tartott. A vállalat 1982, milyen lyncnek válna SnelStart könyvelési szoftver alapját hozott létre. Az elejétől a szoftver az egyszerűség és a sebesség lett admired – annyira, hogy a vállalat ahányat megváltozott a fókusz, és a szoftver vállalata reinvented magát.

1995 SnelStart fejlesztésének első könyvelés alkalmazásának Windows. Az alkalmazás, a Microsoft Visual Basic 1.0 és a Microsoft Access-adatbázisok épül segíteni a nagyobb az ügyfél alap 5000-nél több felhasználó számára.

Ma SnelStart egy sor üzleti (üzleti) és a vállalati verzió – felügyeleti alkalmazás holland SMB és önálló dinamikus alkalmazásindítójukban fő szolgáltatóként. Carlo Kuip, szoftverépítő, felirat látható, mint "célunk a szükséges a vállalati verzió – felügyeleti szolgáltatások ügyfeleink 100 %-os automatizálási."

##<a name="optimizing-performance-and-cost-with-elastic-pools"></a>A teljesítmény és a költség rugalmas készletek optimalizálása

SnelStart egy nagyméretű korai alkalmazó a rugalmas adatbázis készletek volt. Rugalmas készletek segítségével a vállalat korlátozza a költségek és teljesítménnyel kapcsolatos követelmények hatékonyabb kezeléséhez. A következők szerint lett "rugalmas adatbázis készletek használatával azt tudja optimalizálni a teljesítmény nélkül túlságosan kiépítési veszi ügyfeleinek, igényeinek megfelelően. Ha volt kiépítése csúcs betöltés alapján, igazán költséges lenne. Ehelyett a vezérlőt, amellyel több között az erőforrások megosztása alacsony-használat adatbázisok lehetővé teszi, hogy jól hajt végre, és a tényleges költség megoldás létrehozása us. "

##<a name="azure-sql-databases-help-containerize-data-for-isolation-and-security"></a>Azure SQL-adatbázisait súgó containerize elkülönítési és biztonsági adatok 

Azure SQL-adatbázis lehetővé teszi, hogy egyszerűen és könnyen áthelyezése az ügyfelek a helyszíni adatok vállalati verzió – felügyeleti Azure SnelStart. Az SQL Azure-adatbázist, amelybe elkülönítési, oszlopazonosító jobboldali hitelesítés, engedélyezését és könnyen biztonsági mentési és visszaállítási funkciók kényelmes tároló. Adatbázisok a vállalati verzió felügyeleti jól illeszkedik fogalmi modell szükséges. Szerint Carlo Kuip, szoftverépítő, "a tároló oszlopazonosító elemeire kulcsfontosságú üzleti bizalmas adatokat tartalmaznak, és ezeket az elemeket egy elszigetelt adatbázis tárolási követi azokat is védeni. Hogy kezelése az adatbázis szintű engedélyt, és még automatikus kezelését és skála meg az adatbázisok adatbázis-rendszergazdák (DBAs) személyzeti anélkül."

Azure SQL-adatraktár is játszik szerepet az SnelStart biztonsági és kezelése a szövegegység segítenek a vállalat gyűjtése telemetriai adatokat, például behatolási észlelése, a felhasználók naplózása és a kapcsolatot.

##<a name="azure-removes-overhead-so-that-developers-can-spend-more-time-delivering-value"></a>Azure felsőbb szünteti meg, hogy a fejlesztők számára is időt több érték előadásához 

Az Azure platform modell infrastruktúra terhelést eltávolítja, és SnelStart automatizálhatja a telepítések C# management tárak használata engedélyezve van. Ahogy Kuip állapotok, "azt tudott közben egyidejű növekvő méretezhetőség sebességétől és katasztrófa helyreállítási lehetőségeket az ügyfelek számára az aktuális műveleteket egy kis oktatói növekedéséhez. A shift billentyű szűkítheti az új szolgáltatások és funkciók csak a meglévő-kódot az új szabályoknak nyilvántartása vagy adó kódok frissítése helyett erőforrásokat felszabadított szolgáltatások fejlesztési." Automatikus hozzáadása a "hozzáadásához management automatizálása, és használja a szoftver kínáló képesek használhat az előadáshoz több értéket az ügyfelek számára anélkül, hogy a nagy befektetések végezze el az oktatói műveleti." Például Azure és rugalmas adatbázis készletek használatával SnelStart volt adhat hozzá új további funkcióiról, például megbízhatóbb ügyféladatok integráció a bankok, számos új számlázási szolgáltatások, a kisvállalati háttér ellenőrzések és az e-mail szolgáltatásokból.

> "Csak az első néhány hónap 2016, az azt kibontott az Azure SQL-adatbázis telepítések körülbelül 5,500 több, mint 12,000, és hogy jelenleg ad hozzá havonta körülbelül 1000 adatbázisok."

> – Henry már, szoftver Architect

Adatbázis-kezelés további automatizált rugalmas feladatok funkcióval. Kuip állapotok, mint "nagyra erősen értékeljük adatbázisokat a egy SQL-adatbázis [kiszolgáló] példánya automatikus felderítését." Adatkezelési műveletek végrehajtása végig a dinamikusan növekvő ügyfélkör nélkül további terhelést SnelStart lehetővé teszi.

SnelStart is van megtervezése az API-val, amely végpontként ügynök ügyfél adatait, és a szoftver külső partnerek készült alkalmazások között. Kuip állapotok, mint "Ez az API lehetővé teszi más gyártók funkciókat felvenni a szoftver, például az adatbevitel számlák és más dokumentumok kiküszöbölésével." Ügyfelek többé nem lesz a számlák kézzel beírja a kisvállalati könyvelési szoftver; a SnelStart szoftver fog exchange közvetlenül a szükséges információkat. Ez lehetővé teszi a felhasználóknak a vállalati verzió – felügyeleti feladatok, amely a digitális átalakítási a iparágban van felmerülő ökológiai csatlakozni szeretne.  

##<a name="how-azure-services-enable-saas-for-snelstart"></a>Hogyan Azure szolgáltatások SnelStart szoftver engedélyezése

Azure használatával SnelStart szolgálhat felelősséget ügyfelei és azok könyvelők további zökkenőmentes a felhőben. Például ügyfelek és a könyvelők megosztásához információk elérése közvetlenül a SnelStart's ügyfélprogram API, Azure is. Kuip állapotok "újrafelhasználható szolgáltatásokra az ügyfél elérésű web Apps alkalmazások érhetők el, és adja meg a közös infrastruktúra és funkciók és külső szoftver ügyfeleink által használt vállalati verzió felügyeleti ügyfeleknek való hozzáférés engedélyezése. Többek között termék-konfigurációs funkciók kezeléséről, tűzfalszabályokat kezeléséhez és például biztonsági másolatok hosszabb ideig futó folyamatok kezelése."

> Célunk a szükséges a vállalati verzió – felügyeleti szolgáltatások ügyfeleink 100 %-os automatizálási." 

> – Carlo Kuip, szoftverépítő

Ezeken kívül SnelStart webszolgáltatásokhoz ügyfelek és a könyvelők egyszerűen hozzáférés az adatokhoz Azure SQL-adatbázis rugalmas készletek engedélyezése. A szoftver adatbázis rugalmasság és Azure erőforrás-kezelő kapcsolódik ahhoz a modellben minden Azure környezetben kiegészítése méretezhetőség funkciói SnelStart nyújtó szolgáltatás. A végrehajtás teljesen automatizált C# management tárak használata.

![SnelStart architektúra](./media/sql-database-implementation-snelstart/figure1.png)

Ábra 1. Június 2016, kezdve SnelStart még több, mint 11 000 adatbázisokban és az 50-nél több rugalmas adatbázis-készletek
 
##<a name="simplicity-from-the-cloud"></a>A felhőből egyszerűség

A kurzor áthelyezése egy felhőalapú Azure megoldást, SnelStart óta tudja gyors ügyfél NÖV kínálja innovatív funkciók és szolgáltatások támogatása. A következők szerint lett "Azure, az azt tartana közelében folyamatos frissítések ügyfeleink, a műveletek oktatói kibontása nélkül. Azt be az összes többi nagyszerű Azure szolgáltatás – például méretezhetőség és katasztrófa helyreállítási – a kapcsolt. "

> "Ha azt szerepeltek ténylegesen fölé Redmond... Hívás kezdeményezése kapott egy adott problémáról hívnia, Hollandia, a vissza developer A rendszer eljuthassanak Microsoft használhat az előadáshoz módosítás azok üzemi környezetben 48 órában a probléma megoldására."

> – Henry már, szoftver Architect

SnelStart figyelemreméltónak tartja, az általuk fejlesztett a Microsoft Azure SQL-adatbázis csoporttal ki erős partnerség is. Mint Kuip állapota "vitafórumok felkínálunk funkciókat és technológia, ez olyan használatához mindkét oldalon értékeljük."
Azonnali SnelStart a célja, hogy megtartása növő elégedetten ügyfelének alap. Mivel állapotok "Nélkül műszaki és erőforrás korlátozások, mint egy külső volt, nincs korlátozva mennyivel azt is nagymértékű."


## <a name="more-information"></a>További információ

- Azure-adatbázis rugalmas készletek kapcsolatos további információért olvassa el a [Rugalmas adatbázis készletek](sql-database-elastic-pool.md)című témakört.

- További tudnivalók a webes és dolgozó szerepeket, a [dolgozó szerepkörök](../fundamentals-introduction-to-azure.md#compute)című témakör tartalmaz. 

- Azure SQL-adatraktár kapcsolatos további tudnivalókért lásd: az [SQL adatraktár](https://azure.microsoft.com/documentation/services/sql-data-warehouse/)

- SnelStart kapcsolatos további információért olvassa el a [SnelStart](http://www.snelstart.nl)című témakört.


