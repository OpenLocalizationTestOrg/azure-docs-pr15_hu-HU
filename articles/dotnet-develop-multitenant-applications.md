<properties
    pageTitle="Több elem bérlői webhelyen alkalmazás mintát |} Microsoft Azure"
    description="Építészeti tekintse át és tervezés mintázatok, amelyek a több elem bérlői webalkalmazás megvalósítását a Azure talál."
    services=""
    documentationCenter=".net"
    authors="wadepickett" 
    manager="wpickett"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/05/2015"
    ms.author="wpickett"/>

# <a name="multitenant-applications-in-azure"></a>Multitenant alkalmazások Azure-ban

Egy multitenant alkalmazás, amely lehetővé teszi, hogy a különböző felhasználók vagy "-ös bérlői webhelyek," megtekintéséhez az alkalmazás, mintha volt a saját megosztott erőforrás. Házirendkezelő multitenant alkalmazás tipikus példa egyike, amelyben az alkalmazás minden felhasználónak merülnek fel a felhasználói felület testreszabását, de a egyébként is ugyanazon egyszerű üzleti követelményeknek. Nagy multitenant alkalmazások példák az Office 365, az Outlook.com és az visualstudio.com.

Az alkalmazás szolgáltató perspektívából multitenancy előnyei főleg összekapcsolása működési és a költség hatékonyságot. Az alkalmazás több verzióját is igényeinek bérlők/vevőknek, lehetővé teszi a rendszer összesítés felügyeleti feladatát, például figyelése, teljesítményhangolás, szoftver karbantartási és adatok biztonsági mentést.

A következő biztosít a legjelentősebb célok és a szolgáltató perspektívából követelmények listáját.

- **Létesítése**: meg kell jelenítenie hozhatók létre új bérlők alkalmazásához.  Nagyszámú bérlők multitenant alkalmazások általában szükség automatizálása ezt a folyamatot, mivel az önkiszolgáló kiépítési.
- **Karbantartási követelmények**: meg kell jelenítenie a alkalmazás frissítése és más karbantartási műveleteket végezhet, több bérlők használata közben.
- **Figyelés**: meg kell jelenítenie a Lync-mindig alkalmazás bármely kapcsolatos problémák azonosítása és elhárítása őket. Ide tartoznak a minden bérlői hogyan használja az alkalmazás figyelése.

Egy megfelelően végrehajtott multitenant alkalmazás a következő előnyöket nyújtja a felhasználók számára.

- **Elkülönítési**: egyes bérlők tevékenységének nem érintik a többi bérlők által az alkalmazás használatát. Bérlők egymás adatok nem tudja elérni. Megjelenik a bérlő, mintha az alkalmazás kizárólagos használata rendelkeznek.
- **Elérhetőség**: egyes bérlők szeretné elérhetővé szeretné tenni folyamatosan, akár egy SLA meghatározott garanciákkal az alkalmazás. Más bérlők tevékenységének ismét nem érinti a az alkalmazás elérhetőségét.
- **Méretezhetőség**: az alkalmazás az egyes bérlők igényeknek méretezze át. A jelenléti adatok és más bérlők végrehajtott műveletek az alkalmazás teljesítményének nem érinti.
- **Költségek**: költségei kisebb, mint egy dedikált, egyetlen-bérlői alkalmazást futtató, mivel több bérleti lehetővé teszi, hogy az erőforrások megosztása.
- **Testreszabhatóságát**. Az azt jelenti, hogy az alkalmazás különböző módokon például hozzáadása vagy eltávolítása a szolgáltatások, színek és emblémákat vagy még a saját kód vagy parancsprogram hozzáadása egy egyéni bérlő testreszabását.

Rövid sok dolgot, figyelembe kell vennie egy erősen méretezhető szolgáltatás nyújtásához, miközben Számos is célok és számos multitenant alkalmazás közös követelményeknek. Néhány nem lehet vonatkozó különböző forgatókönyvekben, és egyéni célokat és követelmények fontosságát függvényében eltérőek, minden esetben. Az multitenant alkalmazás szolgáltatóként is választania kell célok és követelményeknek, mint a bérlők célok és követelményeknek, jövedelmezőségi, Számlázás, több szolgáltatás szintek, kiépítési, karbantartási követelmények figyelése, és automatizálási.

További tervezési szempontjait multitenant alkalmazás kapcsolatos további tudnivalókért olvassa el az [Azure a több elem bérlői alkalmazást üzemeltető][]című témakört. Közös adatok architektúra mintázatok több bérlői szoftverek,-a-szolgáltatási (szoftver) adatbázis-alkalmazásokat a további tudnivalókért lásd [Tervezés mintázatok az Azure SQL-adatbázis több bérlői szoftver alkalmazásokhoz](./sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md). 

Azure itt sok olyan szolgáltatást, amelyek lehetővé teszik a ütközött multitenant a rendszer tervezésekor a termékkulccsal kapcsolatos problémák megoldása érdekében.

**Elkülönítési**

- A szakasz webhely bérlők által Host élőfejet vagy az SSL-kapcsolati anélkül
- A szakasz webhely bérlők által a lekérdezés paraméterei
- Webszolgáltatások dolgozó szerepkörök
    - Dolgozó szerepkörök. amely a szokásos folyamat az alkalmazás a kódmentes adatokat.
    - A szokásos használni kívánt az alkalmazások frontend Web szerepköröket.

**Tárhely**

Adatok kezelése, például Azure SQL-adatbázis vagy az Azure tároló szolgáltatásokat, például a táblázat szolgáltatás, amely magában foglalja a tárolására szolgáló nagy mennyiségű adatot strukturálatlan services és az Blob-szolgáltatás, amely nagy mennyiségű strukturálatlan szöveg tárolásához szolgáltatásokat nyújt a vagy a bináris adatokat, például a video-, hang- és képeket.

- Megfelelő SQL-adatbázis adatainak védelme Multitenant per-bérlői SQL Server bejelentkezést.
- Azure táblák az alkalmazás erőforrások megadásával tároló hozzáférési házirend segítségével állíthatja be az engedélyek anélkül, hogy új URL-cím látták el átengedése aláírások erőforrás hiba.
- Az alkalmazás erőforrások Azure sorban várakozó Azure sorban várakozó meghajtó feldolgozás nevében bérlők gyakran használják, de is felhasználhatók terjesztheti kiépítési vagy kezelésre szükséges munkamennyiség.
- Szolgáltatás Bus sorban várakozó alkalmazás erőforrások, amely verembe küldi munkára a megosztott szolgáltatást, akkor egyetlen várólista hol minden bérlői feladó csak rendelkezik engedélyekkel (mint ACS által kibocsátott követelések származás) nyomni, hogy a sorba, miközben a csak a címzett, a szolgáltatásból engedélye arra, hogy a sorból lekérje több bérlők az adatok.


**Kapcsolat és biztonsági szolgáltatások**

- Azure Service Bus, egy üzenetben infrastruktúra, amelyek lehetővé teszik alkalmazások üzenetváltásra továbbfejlesztett skála módszerektől összekapcsolt módon és tűrőképessége között helyezkedik el.

**Hálózati szolgáltatások**

Azure több hálózati szolgáltatásokat hitelesítés támogatására és javítása az szolgáltatott alkalmazás kezelhetőséget biztosít. Az alábbi szolgáltatások közé tartoznak az alábbiak:

- Azure virtuális hálózati lehetővé teszi, hogy meg kiépítése és Azure virtuális magánhálózat (VPN) kezelésére, valamint biztonságosan hivatkozás: ezen a helyszíni informatikai infrastruktúrát.
- Virtuális hálózati forgalmának engedélyezésére kezelője egyensúly bejövő forgalom betöltése több szolgáltatott Azure szolgáltatásban, hogy azok operációs rendszert használ, ugyanazt a adatközponthoz egy vagy több a világ különböző adatközpontokkal teszi lehetővé.
- Azure Active Directory (Azure Active Directory), amelybe a felhőben alkalmazások kezelése és az access vezérlő Identitáskezelés modern, a többi-alapú szolgáltatás. Azure AD az Azure Active Directory egyszerűvé a hitelesítése és engedélyezése a felhasználók a webalkalmazások és szolgáltatások eléréséhez szükséges hitelesítési és engedélyt kell figyelembe venni, a kód kívül funkciók téve alkalmazás erőforrások használja.
- Azure Service Bus tartalmaz egy biztonságos üzenetküldés és adatok továbbításához működése elosztott, és a hibrid alkalmazások, például az Azure közötti kommunikáció alkalmazások és a helyszíni alkalmazások és szolgáltatások anélkül, hogy bonyolult tűzfal és a biztonsági infrastruktúra. Az alkalmazás erőforrások szolgáltatás Bus továbbítási használatával a szolgáltatásokat, a végpontok vannak közzétéve tartozhat a bérlő (például a rendszer, például a helyszíni-Ön kívül üzemeltetett), vagy lehetnek a szolgáltatások külön meg a bérlői kiépítve (azért, mert a bizalmas, a bérlői webhely-specifikus adatok utazik át őket).



**Erőforrások kiépítése**

Azure többféle módon rendelkezést új bérlők az alkalmazás. Nagyszámú bérlők multitenant alkalmazások általában szükség automatizálása ezt a folyamatot, mivel az önkiszolgáló kiépítési.

- Dolgozó szerepkörök lehetővé teszi a rendelkezés és bérlőnként megszüntetése kiépítése (például amikor új bérlő jelek felfelé vagy megszakít) erőforrások, összegyűjtése a mértékek, mérési használata és kezelése egy bizonyos ütemterv követő skála vagy a fő teljesítménymutatók küszöbértékek átlépése válaszként. A azonos szerepkör is felhasználhatók leküldéses frissítések és frissítések a megoldáshoz.
- Azure BLOB használt számítási hozhatók létre, vagy előre inicializált védelme a számítási közben container hozzáférési házirendek nyújtó új bérlők tároló-erőforrások csomagok virtuális képek és más erőforrások: a szolgáltatás.
- SQL-adatbázis-erőforrások kiépítési bérlő beállításai a következők:

    -   A parancsfájlok DDL vagy beágyazott erőforrásként összeállítások belül
    -   Az SQL Server 2008 R2 DAC csomagok programozás útján rendszerbe.
    -   Adatbázis fő hivatkozás másolása
    -   Adatbázis importálása és exportálása használatával hozhatók létre új adatbázisok fájlból.



<!--links-->

[A több elem bérlői alkalmazás Azure szolgáltatónál]: http://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: http://msdn.microsoft.com/library/windowsazure/hh689716
