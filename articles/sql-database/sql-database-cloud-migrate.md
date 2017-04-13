<properties
   pageTitle="Az SQL Server-adatbázis áttelepítése SQL-adatbázishoz |} Microsoft Azure"
   description="Megtudhatja, hogyan kell tudni a helyszíni SQL Server adatbázis áttelepítése az Azure SQL-adatbázis a felhőben. Kompatibilitás az adatbázis áttelepítése előtt ellenőrizze a adatbázis áttelepítési eszközök segítségével."
   keywords="adatbázis áttelepítése, sql server-adatbázis áttelepítése, áttelepítési Adatbáziseszközök, az adatbázis áttelepítése, sql-adatbázis áttelepítése"
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
   ms.workload="sqldb-migrate"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="sql-server-database-migration-to-sql-database-in-the-cloud"></a>Az SQL Server-adatbázis áttelepítési a felhőben SQL-adatbázishoz

Ebben a cikkben megismerheti, hogy miként telepítheti át egy helyszíni SQL Server 2005 vagy újabb adatbázis Azure SQL-adatbázishoz való. Az adatbázis áttelepítése folyamathoz áttelepítése a séma és az adatok az SQL Server-adatbázisból az SQL-adatbázissal az aktuális környezetben. A sikeres, az a meglévő adatbázis először meg kell felelnie a kompatibilitási teszten. Az [SQL-adatbázis V12](sql-database-v12-whats-new.md), néhány hátralévő kompatibilitási problémák merülnek fel, eltérő kiszolgálói szintű és idegen adatbázis-műveletek kapcsolatos problémát. Adatbázisok és alkalmazások támaszkodó [részben vagy nem támogatott funkciók](sql-database-transact-sql-information.md) néhány újratervezéséhez e kompatibilitási problémák megoldásához, mielőtt telepíthető át az SQL Server-adatbázis szükséges.

Áttelepítendő, az alábbi táblázat a lépéseket, hogy:

- **Próba a kompatibilitási**: [Az SQL-adatbázis V12](sql-database-v12-whats-new.md)adatbázis kompatibilitásának ellenőrzése. 
- **Fix kapcsolatos kompatibilitási problémák, ha vannak ilyenek**: Ha nem sikerül egy adatérvényesítési, ki kell javítani az adatérvényesítési hibák.  
- **Az áttelepítés végrehajtása** Miután az adatbázis kompatibilis, egy vagy több módszer is használhatja az áttelepítés végrehajtásához. 

Az SQL Server elvégezheti az alábbi műveleteket minden egyes számos módszert kínál. Ez a cikk áttekintést nyújt a rendelkezésre álló módszerek az egyes tevékenységek. A következő ábra bemutatja a lépéseket, és a módszereket.

  ![VSSSDT áttelepítési diagram](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)
  
 > [AZURE.NOTE] Microsoft Access, Sybase, MySQL Oracle és DB2 Azure SQL-adatbázishoz, SQL Server-adatbázis áttelepítése olvassa el [Az SQL Server áttelepítési Segéd](http://blogs.msdn.com/b/ssma/).

## <a name="database-migration-tools-test-sql-server-database-compatibility-with-sql-database"></a>Adatbázis-áttelepítőeszköz SQL-adatbázis SQL Server-adatbázis kompatibilitásának tesztelése

Ha tesztelni szeretné az SQL-adatbázis kompatibilitási problémák az adatbázis-áttelepítés megkezdése előtt, használja az alábbi lehetőségek közül:

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Advisor frissítése](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

- [Az SQL Server Data Tools for Visual Studio ("SSDT")](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): SSDT használja a legutóbbi kompatibilis szabályokat feltárása SQL-adatbázis V12 kompatibilitási problémák. Kompatibilitási problémák észlel, ha észlelt problémák oldhatja meg közvetlenül a ezzel az eszközzel. Ez a módszer a javasolt mód tesztelése és az SQL-adatbázis V12 kompatibilitási problémák megoldása. 
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md): SqlPackage, amelyek a kompatibilitási problémák, és létrehoz egy jelentést, amely tartalmazza a talált kompatibilitási problémák vizsgálatok parancssori segédprogramot. Ez az eszköz használata, ellenőrizze, hogy a legutóbbi kompatibilitási szabályok használatára legújabb verzióját használja. Ha hibát észlel, egy másik eszközt kell használnia – talált kompatibilitási problémák megoldásához SSDT ajánlott.  
- [Az SQL Server Management Studio az exportálás adatok réteg varázsló](sql-database-cloud-migrate-determine-compatibility-ssms.md): Ezzel a varázslóval észleli, és hibát jelez a képernyőre. Ha hibát észlel, továbbra is, és töltse ki az áttelepítési SQL-adatbázishoz. Ha hibát észlel, egy másik eszközt kell használnia – talált kompatibilitási problémák megoldásához SSDT ajánlott.
- [A Microsoft SQL Server 2016 frissítése Advisor előzetes verzió](http://www.microsoft.com/download/details.aspx?id=48119): az önálló eszköz, amely jelenleg előzetes verzióban, észleli és SQL-adatbázis V12 kompatibilitási problémák jelentést készít. Ez az eszköz még nem rendelkezik a legutóbbi kompatibilis szabályokat. Ha nincs hibák továbbra is, és töltse ki az áttelepítési SQL-adatbázishoz. Ha hibát észlel, egy másik eszközt kell használnia – talált kompatibilitási problémák megoldásához SSDT ajánlott. 
- [Az SQL Azure-áttelepítés varázsló ("SAMW")](sql-database-cloud-migrate-fix-compatibility-issues.md): SAMW Azure SQL-adatbázis V12 kompatibilitási problémák feltárása az Azure SQL-adatbázis V11 kompatibilis szabályokat használó codeplex eszköz. Kompatibilitási problémák észlel, ha problémák elhárításához kijavíthatók közvetlenül az eszközben. Ez az eszköz tapasztalhatja kompatibilitási problémák, amelyek nem kell meg kell oldódnia. Az első Azure SQL-adatbázis áttelepítési támogatási eszköz érhető el lett és a aktívan támogatja az SQL Server-Közösség. Ez az eszköz is, az áttelepítés belül az eszközt, magát a végezhetik el. 

## <a name="fix-database-migration-compatibility-issues"></a>Adatbázis áttelepítése kapcsolatos kompatibilitási problémák megoldása

Kompatibilitási problémák észlel, kell javítani az SQL Server-adatbázis áttelepítése a továbblépés előtt. Vannak olyan számos különböző típusú előforduló, attól függően, hogy mindkét kapcsolatos kompatibilitási problémák a forrásadatbázis és összetettsége az áttelepíteni kívánt adatbázist az SQL Server verziójában. Az SQL Server régebbi verzióit van kompatibilitási problémát észlelt. Használja az alábbi források internetes keresést célzott használja a keresőt választási lehetőségek, mellett:

- [Azure SQL-adatbázisban nem támogatott SQL Server-adatbázis szolgáltatások](sql-database-transact-sql-information.md)
- [Az SQL Server 2016-ban megszűnt adatbázis motor szolgáltatásai](https://msdn.microsoft.com/library/ms144262%28v=sql.130%29)
- [Az SQL Server 2014-es megszűnt adatbázis motor szolgáltatásai](https://msdn.microsoft.com/library/ms144262%28v=sql.120%29)
- [SQL Server 2012 megszűnt adatbázis motor szolgáltatásai](https://msdn.microsoft.com/library/ms144262%28v=sql.110%29)
- [Megszűnt adatbázis motor funkciók az SQL Server 2008 R2 rendszerben](https://msdn.microsoft.com/library/ms144262%28v=sql.105%29)
- [SQL Server 2005 megszűnt adatbázis motor szolgáltatásai](https://msdn.microsoft.com/library/ms144262%28v=sql.90%29)

Nemcsak a keresése az interneten, és használja az alábbi források, [MSDN SQL Server közösségi fórumok](https://social.msdn.microsoft.com/Forums/sqlserver/home?category=sqlserver) és [StackOverflow](http://stackoverflow.com/)használhatja.

A következő adatbázis áttelepítési eszközök segítségével az észlelt problémák megoldása:

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

- [Az SQL Server Data Tools for Visual Studio ("SSDT")](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): SSDT használatához az adatbázis sémája programba importált SQL Server Data Tools for Visual Studio "SSDT"), és hozza létre a projekt SQL-adatbázis V12 telepítés. Majd az összes talált kompatibilitási problémák megoldása az SSDT. Amikor elkészült, szinkronizálás a módosítások vissza a forrásadatbázis (vagy a forrásadatbázis másolatát. SSDT jelenleg az ajánlott módszer, tesztelése és az SQL-adatbázis V12 kompatibilitási problémák megoldása. [Segédlet használatával SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)hivatkozásra.
- [SQL Server Management Studio ("SSMS")](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)használata: SSMS használja, akkor hajtsa végre a Transact-SQL-parancsok a másik eszközzel észlelt hibáinak elhárításához. Ez a módszer elsősorban a haladó felhasználók módosítása közvetlenül a forrás-adatbázisban az adatbázissémát szolgál. 
- [SQL Azure](sql-database-cloud-migrate-fix-compatibility-issues.md)-áttelepítés varázsló ("SAMW"): SAMW használatához hoz létre a Transact-SQL nyelvben parancsfájl az adatforrás-adatbázisból. A varázsló a parancsfájlt, amikor csak lehetséges, hogy a séma az SQL-adatbázis V12 kompatibilis, alakítja. Amikor elkészült, az SQL-adatbázis V12 végrehajtani a parancsfájl SAMW lehet csatlakozni. Ez az eszköz is elemzi a nyomkövetési fájlok kapcsolatos kompatibilitási problémák határozza meg. A parancsprogram csak séma használata hozhatók létre, vagy BCP formátumban adatokat tartalmazhat.

## <a name="migrate-a-compatible-sql-server-database-to-sql-database"></a>Kompatibilis SQL Server-adatbázis áttelepítése az SQL-adatbázis

Kompatibilis SQL Server-adatbázis áttelepítése, a Microsoft számos különböző forgatókönyvekben áttelepítési módszerek biztosít. A kiválasztott módszerrel attól függ, hogy a hibatűrést legrövidebb leállás, méretének és összetettsége feleljen az SQL Server-adatbázishoz, és a kapcsolatot a Microsoft Azure felhőbe.  

> [AZURE.SELECTOR]
- [SSMS áttelepítése varázsló](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [BACPAC fájl exportálása](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Importálás BACPAC fájlból](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Tranzakció alapú replikációs](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

Válassza ki az áttelepítési mód, kérje az első kérdéshez, ha Ön is engedheti meg magának az áttelepítés során gyártási ki az adatbázisban állapotba. Az adatbázis-következetlenségek és az adatbázis esetleges sérülése okozhat a áttelepítése az adatbázis, miközben aktív tranzakciók fordul elő. Nincsenek leválasztása számos módszert egy adatbázist, az [adatbázis-pillanatfelvétel](https://msdn.microsoft.com/library/ms175876.aspx)létrehozásának Ügyfélcsatlakozás letiltása.

Minimális legrövidebb leállás áttelepítéséhez használja [az SQL Server tranzakció replikációs](sql-database-cloud-migrate-compatible-using-transactional-replication.md) , ha az adatbázis megfelel az tranzakció alapú replikáció. Ha akkor is biztosít néhány legrövidebb leállás vagy újabb áttelepítésre gyártási adatbázis próba áttelepítés hajt végre, vegye figyelembe az alábbi módszerek egyikét:

- [SSMS áttelepítése varázsló](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md): kicsi, közepes adatbázisokhoz, a kompatibilis SQL Server 2005 vagy újabb adatbázis áttelepítése az egyszerűen, a [Microsoft Azure-adatbázis varázsló adatbázis terjesztése](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) futó SQL Server Management Studio eszközben.
- [Exportálás fájlba BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) , majd az [Importálás fájlból BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md): Ha (nincs kapcsolat, kis sávszélességű vagy időtúllépés problémák) kapcsolódási problémáit, és közepes nagy adatbázisok, használja a [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) fájl. Ezzel a módszerrel exportálhatja az SQL Server-sémafájl és az adatok BACPAC fájlba. Kattintson a BACPAC fájl importálását SQL-adatbázisokkal az Exportálás réteg alkalmazás varázsló az SQL Server Management Studio vagy a [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) parancssori segédprogramot.
- BACPAC és BCP együttes használata: használjon egy [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) fájl és [BCP](https://msdn.microsoft.com/library/ms162802.aspx) sokkal nagyobb adatbázisok nagyobb parallelization számára növeli a teljesítmény elérése érdekében jóllehet nagyobb összetettsége. Ezzel a módszerrel a séma és át az adatokat külön-külön.
 - [A séma csak a BACPAC fájl exportálása](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md).
 - [A csak a BACPAC fájlból séma importálása](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) SQL-adatbázisba.
 - Az adatok kinyerése strukturálatlan fájl, majd a [párhuzamos betöltés](https://technet.microsoft.com/library/dd425070.aspx) ezeket a fájlokat az Azure SQL-adatbázis használata [BCP](https://msdn.microsoft.com/library/ms162802.aspx) .

     ![Az SQL Server áttelepítése az adatbázis - SQL-adatbázis áttelepítése a felhőben.](./media/sql-database-cloud-migrate/01SSMSDiagram_new.png)

## <a name="next-steps"></a>Következő lépések

- [A Microsoft SQL Server 2016 frissítési Advisor előzetes verzió](http://www.microsoft.com/download/details.aspx?id=48119)
- [SSDT legújabb verziója](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studio legújabb verziója](https://msdn.microsoft.com/library/mt238290.aspx)

##<a name="additional-resources"></a>További források

- [SQL-adatbázis V12](sql-database-v12-whats-new.md)
[Transact-SQL nyelvben részben vagy nem támogatott funkciók](sql-database-transact-sql-information.md)
- [Az SQL Server áttelepítési Segéd használata SQL Server-adatbázis áttelepítése](http://blogs.msdn.com/b/ssma/)
