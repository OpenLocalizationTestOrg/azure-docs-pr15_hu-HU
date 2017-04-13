<properties
   pageTitle="Azure SQL-adatbázis Azure esettanulmány - GEP |} Microsoft Azure"
   description="Megismerheti, hogyan GEP használja a SQL-adatbázis eléréséhez több globális ügyfelek, és a nagyobb hatékonyságot elérése"
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

# <a name="azure-gives-gep-global-reach-and-greater-efficiency"></a>Azure ad-GEP globális vannak, és a nagyobb hatékonyságot

![GEP embléma](./media/sql-database-implementation-gep/geplogo.png)

GEP szoftverek és -szolgáltatások, amelyek lehetővé teszik a maximális azok a vállalkozások műveletek, a stratégiák és a pénzügyi előadások gyakorolt hatásának a világon beszerzési vezető biztosítja. Kívül tanácsadási és kezelt szolgáltatások a vállalat felajánlja INTELLIGENS GEP®, egy felhőalapú, átfogó beszerzési szoftver platform szerint. Azonban GEP merült fel megoldások támogatására, például a saját GEP által INTELLIGENS helyszíni adatközpontokkal próbál korlátozások: a befektetés szükség lenne nagy ahhoz, és más országokban szabályozói követelmények bocsátotta volna a szükséges befektetések további továbbra is tűnhet. A felhőben való áthelyezésével GEP informatikai erőforrásokat, felszabadított lehetővé teszi, hogy kevesebb informatikai műveletekkel kapcsolatos és a fókusz a érték új források fejlesztésének felelősséget ügyfelei végig a földgömb további aggódjon.

## <a name="expanding-services-and-growth-by-using-azure"></a>Kibontás-szolgáltatások és a NÖV Azure használatával

INTELLIGENS GEP vevők közkedvelt a platform szolgáltatások és egyszerű használat; felhasználók kezelhetik a folyamatok bárhonnan, bármikor, bármilyen eszközről – laptopon, táblagépen vagy telefonon. Microsoft Azure való áthelyezésével GEP tudja a gyors növekedés és a swayből, bontsa ki az új piacokon volt. Megfelelően GEP's igazgató of Technology Dhananjay Nagalkar, "a Microsoft Azure van a lejátszani, szerepet GEP a sikeres azáltal, hogy gyorsan méretezése a szolgáltatások agility us és, mert a regionális adatközpont esetén, hogy segítse a globális ügyfeleink szabályozó igényeinek."

## <a name="the-limitations-of-a-do-it-yourself-datacenter"></a>Egy önálló adatközponthoz korlátozások

A 2013-ban GEP vezető felismerte, hogy azok szükség méretezhetőség és a teljesítmény biztosítja, ahogy azok növekedett az ügyfél alap. Nagalkar magyarázata, "felel meg, hogy igény szerint a saját meglévő adatközpontokkal, hogy azt kellett volna bontsa ki a infrastruktúra és informatikai erőforrások jelentősen. A befektetés és az adott időszakon belül lett volna hatalmas." A helyszíni fizikai és virtuális gépeken futó teljes körű konfigurációs, management, méretezés, biztonsági mentése és szükséges, amely a GEP tiltják költség volna sebesség javítása. Felhőalapú megoldások, azonban, felajánlja a egyszerűség és kényelmesebbé kiemelése több be fejlesztési helyett kezelése nagy GEP beállított – és növő – informatikai műveletek. Nagalkar tudomása volt, hogy GEP csökkentheti a infrastruktúra vásárlásával, beállítása és kezelése a terhelést a felhőbe áttelepítésével.

GEP szükség szabályozási korlátok, amelyek kívül néhány globális piacokon tartani, hogy úgy is. Számos GEP a potenciális Európai vevők szabályozási megfelelőségről lenne szükség problémákat a helyi földrajzi régióban tárolt adatok. De nem volna több adatközpontokkal kiépítésekor GEP számára praktikus. "Kiterjedt infrastruktúra-befektetések és az informatikai elvégzett munkára költségeket jelentős hatást margók, akinek előtérbe" Nagalkar megfelelően. "Eredményt adja, hogy ténylegesen kellett kapcsolja be a nem vagyok a gépnél potenciális ügyfelek, akik a helyben tárolt adatok szükséges."

Nagalkar tudomása volt, hogy egy felhőalapú megoldás lehet, hogy a kérdésre adott válasz a kapcsolatos dilemma megoldása is. Ha egy felhőalapú szolgáltatóhoz vagy globális jelenlétéhez GEP sikerült, azt is jobban megfeleljen az ügyfelei szabályozási és a teljesítmény van szüksége a vevők fizikai helyekre közelebbi adatokat tárol.

## <a name="finding-smooth-skies-in-the-cloud"></a>Zökkenőmentes skies keresése a felhőben

Nagalkar és csapata megismerkedett több felhő lehetőség, de a legtöbb infrastruktúra-mint-a-szolgáltatás (IaaS) – megoldások számára történő beállítása és kezelése a szolgáltatás informatikai erőforrások erősen fektetni GEP igényelt volna alapján. Az Azure platform-mint-a-szolgáltatás van kapcsolva, sokkal jobban kell (PaaS) megoldás illeszkedik.

"Az Azure-GEP nem kell kezelni az adatbázis-kezelés, virtuális gépi konfigurációs, javítása vagy más infrastruktúra teendőkről" jelzett Nagalkar. "Ehelyett azt is koncentráljon az erőforrások azt mire legjobb: a szoftver, amely valódi biztosítja az eredmények ügyfeleink írni beszerzési szakértelmet feljebb helyezése." 

Az Áthelyezés ide: Azure valójában engedélyezte a GEP az informatikai dolgozók zsugorítása közben nagyobb funkció egyidejű engedélyezése az ügyfeleknek.

Azure adatközpontokkal kihasználva végig a földgömb, GEP is egyszerűen kiterjesztése a gyermekektől Európa és Ázsia. Ezeket a globális adatközpontokkal engedélyezése GEP agility méretezése és csökkenti a késés, a szabályozó megfelel a helyben tárolt adatok ügyfél igényeinek.

## <a name="smart-by-gep-architecture"></a>INTELLIGENS által GEP architektúra

GEP által az alapoktól kezdve GEP INTELLIGENS épül Azure. A kritikus motivációját GEP volt a nagyobb méretezhetőség legrövidebb leállás, kisebb, és a csökkentett karbantartási költségek GEP sikerült az Azure SQL-adatbázis összehasonlítva milyen GEP fellépő sikerült elérése a helyszíni. Azonban helyez át a felhőben, miután GEP talált új fejlesztés lehetőségek gyors prototípusának elkészítéséhez és a lean mérnöki jobban válaszolni akkor ügyfél igényeinek, például a felhőben. Azure-ban elkészítésének engedélyezése a nem vagyok a gépnél tenni a szoftverrel, némi problémát, hogy a fejlesztők helyszíni fellépő esetleges licencelési GEP. Az alapvető az INTELLIGENS GEP által Azure SQL-adatbázis, bár GEP sok Azure szolgáltatás használata könnyebben és gyorsabban továbbra is az INTELLIGENS által GEP javítása.

![INTELLIGENS által GEP architektúra](./media/sql-database-implementation-gep/figure1.png) ábra 1. INTELLIGENS által GEP architektúra

## <a name="structured-data"></a>Strukturált adatok

Az INTELLIGENS GEP alkalmazás, a szívecskés, vannak az Azure SQL-adatbázis-példányok, hogy power a vállalati beszerzési-kezelési megoldás. GEP visszafejtett INTELLIGENS GEP szerint, amikor a architektúra, egyet, amelyek lehetővé teszik a vállalat adatok védelme a legmagasabb szintű eléréséhez, és a szabályozói követelmények betartását tökéletes mint látta Azure SQL-adatbázis. GEP teszi Azure SQL-adatbázis által kínált, beleértve a több rétegek adatvédelem használata:

- A titkosított elemre átlátszóvá adatok titkosítással a többi adatot.
- Azure SQL-adatbázis integrálása az Azure Active Directory webhelycsoportgazdákra hitelesítés.
- Egy megfelelő az adatok részhalmazát sor szintű biztonság való hozzáférés korlátozása.
- Házirendek valós idejű adatok maszkolás.
- Nyomon követés adatbázis Azure SQL-adatbázis Képletvizsgálat keresztül.

> "Ábrázolásakor alábbi beállítások közül fő kód változtatás nélkül, és a teljesítmény elérése érdekében minimális hatással" said Nagalkar.

Azure SQL-adatbázis használatával GEP automatikusan megkapja nagyobb katasztrófaelhárító funkciók, mint azt is van gazdaságilag visszafejtett helyszíni az Azure SQL-adatbázis beépített hibatűrést funkciók miatt. GEP különböző földrajzi régióban, Azure SQL-adatbázisban, több aktív, olvasható és online másodlagos kópiák (mindig a elérhetősége csoportok) kapcsolódik ahhoz aktív Geo-replikációs szolgáltatását használja magas rendelkezésre állás párban űrlap. INTELLIGENS által GEP replikálása adatok különböző régiók azt jelenti, hogy, hogy az esetében a régió szintű katasztrófa GEP is egyszerűen helyreállítása ügyféladatok minimális helyreállítási pontos cél (Készletben) és helyreállítási idejű cél (RTO).

Minden egyes INTELLIGENS GEP ügyfél által a két Azure SQL-adatbázis példánya van: egy online tranzakció feldolgozás (OLTP), és elemzéshez (például ügyfél töltött és elemzési jelentés). Azure SQL-adatbázis rugalmas adatbázis készletek egyszerűen kezelheti az adatbázisok igények járhat adatbázis-erőforrás globálisan kezelendő ezer GEP engedélyezése. Rugalmas készletek GEP annak érdekében, hogy ügyfél adatbázisok méretezheti szükség szerint túlságosan kiépítési vagy a – kiépítési, miközben is lehetővé teszi a vezérlő költségekkel GEP nélkül lehetőséget nyújtanak. Továbbá mivel ez PaaS szolgáltatás, GEP minden új Azure SQL-adatbázis szolgáltatásai automatikus verziófrissítése kap.

## <a name="unstructured-and-semi-structured-data"></a>Félig strukturált és strukturálatlan adatokat

Néhány INTELLIGENS GEP felhasználói adatok azonban kevésbé szigorúan strukturált tárolás van szüksége. Az ilyen típusú adat GEP Azure Blob tárolására, Azure Táblatárolóhoz és Azure vgx.dll gyorsítótár alkalmaz. Azure Blob-tárolóhoz minden melléklet, amely szerint GEP felhasználók feltöltése az alkalmazásba INTELLIGENS otthont ad. Is van szükség szerint GEP üzletek statikus tartalommá, például a stíluslapok (CSS) és a JavaScript-fájlok INTELLIGENS.

Azure Táblatárolóhoz, GEP diagramalakzatok GEP tárol nem ügyfélkapcsolati adatok, például az INTELLIGENS GEP naplóadatok, amelyet hatékony korlátlan költség hatékony tárhely és a gyors lekérés időpontok anélkül, hogy állítsa be az adatok séma aggódnia. A fő gyorsítótár Azure vgx.dll gyorsítótár GEP használja.

## <a name="authentication-and-routing"></a>Hitelesítés és továbbítása

Azure Access vezérlő szolgáltatást (ACS) GEP felhasználók: a szoftver-bA bejelentkezve lehetőségek számos különböző az INTELLIGENS biztosít. Azure ACS is kapcsolt minden identitásszolgáltatójával, mely hitelesítési adatok biztonsági állítás Markup Language (SAML), például az Active Directory tartományi szolgáltatások, Ping identitás, OneLogin vagy SiteMinder használatával. Ezzel az elemzéssel egyszeri bejelentkezés (SSO) végrehajtására GEP ügyfeleknek anélkül, hogy felhasználói hitelesítő adatok és az ügyfél jelszóházirendek fenntartása.

Miután bejelentkezett, az ügyfelek a megfelelő vállalati erőforrások hozzáféréssel rendelkeznek az INTELLIGENS GEP szerint. GEP Azure forgalom Manager irányítja és ügyfelei mobileszközök és a böngésző-munkamenetet érkező terhelést kérések használja.

## <a name="other-azure-services"></a>Egyéb Azure szolgáltatás

GEP alkalmaz, hogy az INTELLIGENS más Azure szolgáltatás egy szám által ügyfélnek válaszol GEP van szüksége. GEP host alkalmazás-bemutatót, és a biztonságos logikájának szolgáltatások Azure felhőszolgáltatások (webes és a dolgozó szerepkörök) segítségével. Felhőszolgáltatások lehetővé teszik a kezelését kódként (IAC) és az új INTELLIGENS GEP alkalmazások egy részét, meg kell adnia a helyszíni adatközpontokkal időpontot, a fejlesztők számára – nélkül bármely bevonása az informatikai. GEP fejlesztők használhatják a felhőszolgáltatások átmeneti tárolására új verziókban tesztelése nélkül érintő az aktuális éles üzemi környezetben. Miután vizsgálni, GEP Azure felhőszolgáltatások virtuális felcserélése funkciók áthelyezése a előkészítési kódját a termelési tárolóhely belül a percet, így csökken a telepítési legrövidebb leállás használja.

Alsó alkalmazás késés, GEP használja az Azure tartalom kézbesítési hálózati (CDN) helyezze a felhasználók közelébe peremhálózat-kiszolgálói (például CSS- és JavaScript-fájlok) Azure Blob-tárolóban lévő statikus tartalommá. GEP felhasználások Azure Service Bus támogatja az alkalmazás-architektúra mintázatok kezdve a közzététel-előfizetés a részben parancs lekérdezés válaszol feladatkörök (CQRS), és réteges architektúra laza kapcsoló és aszinkron kommunikáció. GEP Azure Media Services használatával javíthatja az ügyfél-támogatási szolgáltatás. GEP található, hogy azt egyszerűen feltehet felhasználói-támogatás videók Azure Media Services. Ezeket a videókat választ a felhasználó gyakori kérdésekre, ezzel megkönnyíti növelheti INTELLIGENS GEP felhasználói elégedettséget véve néhány GEP's ügyfélszolgálati munkatársak elhagyja a támogatási betöltés közben.

Elérhető több ezer naponta generálja a GEP által INTELLIGENS tranzakció alapú e-mailek küldéséhez a vállalkozás segítségével a SendGrid .NET API integrálása az Azure. GEP fejlesztők, ez az egyszerű tegye – az Azure SendGrid bővítmény a Microsoft Azure piactéren elérhető jobbra. GEP fejlesztők INTELLIGENS GEP által sikerült beállítani a Microsoft Visual Studio; SendGrid NuGet csomag jobbra használatával GEP informatikai figyelheti a szoftver SendGrid e-mail forgalom közvetlenül az Azure.

Végül a GEP által INTELLIGENS Azure virtuális gépeken futó használ – az Azure IaaS szolgáltatás – host-alkalmazások és szolgáltatások nem szereplő értelemben műszaki tervezés, szoftver-mint-a-szolgáltatás (szoftver) vagy PaaS megoldások lecserélése idején. Ha például a GEP integrációs API-szolgáltatások virtuális gépeken futó üzleti vállalati verzió (B2B) integrációja (az ügyfelek a helyszíni vállalati erőforrás tervezési ERP) rendszerek például SAP, az Oracle, PeopleSoft, JD Edwards, Microsoft Dynamics csoportházirend vagy Lawson és az ügyfél szoftver megoldások hatékony Exchange beszerzési dokumentumokat, például a számlák tárolja.

> "A Microsoft Azure felhőben GEP által INTELLIGENS épület teljesen eltávolította kell a helyszíni IT, nem csak a GEP, hanem műveletekhez ügyfelei beszerzési." 

> – Dhananjay Nagalkar, igazgató technológia megoldások

## <a name="expand-customer-satisfaction-without-expanding-it"></a>Bontsa ki a felhasználói elégedettséget kibontása nélkül informatikai

A helyszíni adatközpontokkal áttérés az Azure és a GEP által INTELLIGENS létrehozása az Azure platform, óta GEP növekedett méretezhetőség és rugalmasság anélkül, hogy bontsa ki a infrastruktúra vagy az informatikai szakemberek. A vállalat valójában négy évnél nem adott informatikai erőforrások. Azure kényelmes PaaS mintája csökkentheti a Címkegyártó és a műveletek management kiadások GEP engedélyezte. Emiatt GEP lett-e képes fókusz erőforrások szoftverfejlesztési; és a felhőben elkészítésének lehetővé teszi a GEP fejlesztők gyorsan tesztelje az új ötleteket anélkül, hogy töltött idő koordinálása az informatikai vagy aggódnia helyszíni licencelési feltételek. Azure SQL-adatbázis jobban gondoskodhat arról, hogy ügyfelei mindig kivételes szolgáltatás és a teljesítmény GEP segítségével.
 
## <a name="more-information"></a>További információ

- GEP kezdőlap: [GEP](http://www.gep.com)
- Intelligens által GEP: [GEP az intelligens](http://www.smartbygep.com)

##<a name="gep-contributors"></a>GEP munkatársak

- Huzaifa Matawala, társítása igazgató – Architect, GEP
- Sathyan Narasingh, tervezés-kezelőben GEP
- Deepa Velukutty, adatbázis Architect GEP
