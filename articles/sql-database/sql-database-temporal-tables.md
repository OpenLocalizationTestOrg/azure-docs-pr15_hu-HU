<properties
   pageTitle="Első lépések – időbeli Azure SQL-adatbázisból származó táblázatot |} Microsoft Azure"
   description="Megtudhatja, hogy miként veheti használatba az Azure SQL-adatbázis időbeli tábláinak használatával."
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
   ms.workload="sql-database"
   ms.date="08/29/2016"
   ms.author="carlrab"/>

#<a name="getting-started-with-temporal-tables-in-azure-sql-database"></a>Azure SQL-adatbázisban időbeli táblázatok – első lépések

Időbeli táblázatok egy új programozhatóság az Azure SQL-adatbázissal, amely lehetővé teszi, hogy nyomon követése és elemezheti az adatokat, egyéni kódolási nélkül módosításainak teljes előzményeit. Időbeli táblák megtartása szorosan kapcsolódó idő környezetben, hogy a tárolt tények értelmezhetők adatok érvényesnek csak az adott időszakon belül. Ez a tulajdonság időbeli táblák időalapú hatékony elemzés és keresztüli háttérismeretek az adatokat a fejlesztés tesz lehetővé.

##<a name="temporal-scenario"></a>Időbeli eset

Ez a cikk bemutatja a lépéseket követve időbeli táblák csatlakozást az alkalmazás használatával. Tegyük fel, nyomon követheti a felhasználói tevékenység fejlesztése folyamatban van az alapoktól új webhelyen, vagy egy meglévő webhelyet, amelyet a felhasználó tevékenység analytics kiterjesztése a használni kívánt. Egyszerűsített példánkban azt feltételezik, hogy ideje alatt felkeresett weblapok számát mutató rögzítse és a webhely adatbázis fájlkiszolgálón található Azure SQL-adatbázis ellenőrizni kell, hogy a. A felhasználói tevékenység korábbi elemzésének célja első bemenetben webhely újratervezéséhez, és jobban élmény a látogatók a szükséges.

Az ebben az esetben az adatbázismodell rendkívül egyszerű – felhasználói tevékenység metrikus mezővel egy egész szám, **PageVisited**, jeleníti meg, és a felhasználói profil alapvető információ együtt rögzíthető. Ezenkívül időpontokat elemzéshez volna frissítse és tartsa egy sorozat, a sorok minden olyan felhasználóhoz, ahol minden sor jelöli a egy felhasználó egy adott időszakban megtekintett oldalak száma.

![Séma](./media/sql-database-temporal-tables/AzureTemporal1.png)

Szerencsére nem kell minden munkamennyiség helyezi el a tevékenység adatainak kezelése az alkalmazását. Időbeli táblázatokkal ezt a folyamatot automatizált - webhelyének megjelenését és több időt az adatelemzés magát kiemelése során teljes rugalmasság, biztosítva. Beállítást kell elvégeznie ahhoz, hogy **WebSiteInfo** táblázat van konfigurálva [időbeli rendszer verziószámmal](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0). A pontos eljárás való csatlakozást időbeli táblák ebben az esetben az alábbiakban ismertetjük.

##<a name="step-1-configure-tables-as-temporal"></a>Lépés: 1: Állítsa be táblázatok időbeli

Attól függően vannak új fejlesztés indítása vagy a meglévő alkalmazás frissítése fog időbeli táblázatok létrehozása vagy módosíthatja a meglévőket időbeli attribútumok hozzáadásával. Az általános esetben az igényektől lehet kombinálja a következő két lehetőség. Ezek [Az SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS), [Az SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) vagy bármely más Transact-SQL nyelvben fejlesztői eszköz segítségével művelet végrehajtása


> [AZURE.IMPORTANT] Javasoljuk, hogy mindig a legújabb verzióját használja Management Studio maradjon szinkronizálja a frissítéseket és a Microsoft Azure SQL-adatbázis. Az [SQL Server Management Studio frissítése](https://msdn.microsoft.com/library/mt238290.aspx).


###<a name="create-new-table"></a>Új tábla létrehozása

Helyi menü elem "Új rendszer verziószámmal táblázat" használata SSMS objektum Explorer nyissa meg a Lekérdezésszerkesztő sablon táblázat időbeli parancsfájl, és az "Adja meg értékeket a sablon paraméterek" (Ctrl + Shift + M) segítségével a sablon feltöltése:

![SSMSNewTable](./media/sql-database-temporal-tables/AzureTemporal2.png)

A SSDT választotta új elem hozzáadása az adatbázis projekt "időbeli (rendszer-verzióval)" táblasablon. Nyissa meg a táblát Tervező, és engedélyezze, hogy könnyen adja meg a táblázat elrendezése:

![SSDTNewTable](./media/sql-database-temporal-tables/AzureTemporal3.png)

Is időbeli tábla létrehozása közvetlenül a Transact-SQL-utasításait megadásával az alábbi példában látható módon. Ne feledje, hogy minden időbeli táblázat kötelező elemeit az időszak definícióját és a SYSTEM_VERSIONING záradék egy másik felhasználó táblázat, amely fog tárolni a sor korábbi verzióiban való hivatkozással:

````
CREATE TABLE WebsiteUserInfo 
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED 
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL 
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
````

Amikor a rendszer verziószámmal időbeli táblát hoz létre, a hozzá tartozó előzmények táblázat az alapértelmezett beállításokat tartalmazó automatikusan létrejön. Az alapértelmezett előzmények tábla tartalmazza az időszak oszlop (end kezdő) csoportosított B fa index létrehozása lap tömörítést engedélyezve van. Ez a beállítás, amelyben időbeli táblák használnak, különösen a [adatok Képletvizsgálat](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0)esetek többségét az optimális. 

Adott esetben azt célja, hogy időalapú trend elemzést végezhet egy hosszabb előzményeinek fölé, és a nagyobb adathalmazok, ezért a tárhely választás az előzmények táblázat csoportosított columnstore index létrehozása. A csoportosított columnstore nagyon hasznos a tömörítési és a teljesítmény analitikai lekérdezések biztosít. Időbeli táblázatai teljesen független szervezet által a jelenlegi és időbeli táblák állítsa be az indexek rugalmasan. 

**Megjegyzés**: Columnstore indexek csak érhetők el a támogatási szolgáltatás réteg.

A következő parancsfájl jeleníti meg, hogyan a csoportosított columnstore be előzmények táblázat alapértelmezett index módosítható:

````
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

Időbeli táblák közben gyermek csomópontjának jelenik meg az előzmények táblázatát jelennek meg az objektum Explorer, a könnyebb azonosítás, az adott ikon.

![AlterTable](./media/sql-database-temporal-tables/AzureTemporal4.png)

###<a name="alter-existing-table-to-temporal"></a>ALTER meglévő tábla időbeli

Vegyük terjed ki az alternatív alkalmazási példát, amelyben a WebsiteUserInfo táblázat már létezik, de nem készült megtartása módosítási előzményeit. Ebben az esetben egyszerűen bővítheti a meglévő tábla válhat időbeli, az alábbi példában látható módon:

````
ALTER TABLE WebsiteUserInfo 
ADD 
    ValidFrom datetime2 (0) GENERATED ALWAYS AS ROW START HIDDEN  
        constraint DF_ValidFrom DEFAULT DATEADD(SECOND, -1, SYSUTCDATETIME())
    , ValidTo datetime2 (0)  GENERATED ALWAYS AS ROW END HIDDEN   
        constraint DF_ValidTo DEFAULT '9999.12.31 23:59:59.99'
    , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo); 

ALTER TABLE WebsiteUserInfo  
SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
GO

CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

##<a name="step-2-run-your-workload-regularly"></a>Lépés: 2: A terhelést rendszeresen futtatása

A fő időbeli táblák előnye, hogy nem kell módosítása vagy a webhely oly módon, amely végezze el a változások követésének beállítása. Létrehozása után időbeli táblák átlátszó továbbra is fennáll verzióinak sorba minden alkalommal, amikor az adatok módosításokat hajt végre. 

Annak érdekében, hogy kihasználhatja az automatikus a változások követésének ebben az esetben, nézzük csak frissítés oszlop **PagesVisited** minden alkalommal, amikor a felhasználó befejeződik megszűnik munkamenet a webhelyen:

````
UPDATE WebsiteUserInfo  SET [PagesVisited] = 5 
WHERE [UserID] = 1;
````

Fontos figyelje meg, hogy a frissítő lekérdezés nem kell tudnia a pontos időt, a tényleges művelet történt, sem hogyan korábbi adatok megmaradnak a jövőbeli elemzéshez. Az SQL Azure-adatbázis mindkét szempont automatikusan kezeli. Az alábbi ábra szemlélteti, hogyan előzmények adatok létrehozása folyamatban van a minden módosítás.

![TemporalArchitecture](./media/sql-database-temporal-tables/AzureTemporal5.png)

##<a name="step-3-perform-historical-data-analysis"></a>Lépés 3: Végezze el a korábbi adatelemzés

Most már időbeli rendszer-Verziószámozás engedélyezve van, korábbi adatelemzés esetén távolabb, csak egy lekérdezésben. Ebben a cikkben néhány példa, hogy cím analysis esetei – ismerje meg az összes adatot, a [Vonatkozó SYSTEM_TIME](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) záradék bevezetett különféle lehetőségek lesz elérhető.

A felső 10 felhasználó egy órával korábbi kezdve felkeresett weblapok száma szerint rendezett megtekintéséhez a lekérdezés futtatása:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
SELECT TOP 10 * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME AS OF @hourAgo
ORDER BY PagesVisited DESC
````

Ezt a lekérdezést egy hónapja kezdve a napja, a webhely látogatás elemzéséhez egyszerűen módosítható, vagy bármely pontján múltbeli kíván.

Egyszerű statisztikai elemzések elvégezheti az előző napra, használja az alábbi példában:

````
DECLARE @twoDaysAgo datetime2 = DATEADD(DAY, -2, SYSUTCDATETIME());
DECLARE @aDayAgo datetime2 = DATEADD(DAY, -1, SYSUTCDATETIME());

SELECT UserID, SUM (PagesVisited) as TotalVisitedPages, AVG (PagesVisited) as AverageVisitedPages,
MAX (PagesVisited) AS MaxVisitedPages, MIN (PagesVisited) AS MinVisitedPages,
STDEV (PagesVisited) as StDevViistedPages
FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME BETWEEN @twoDaysAgo AND @aDayAgo
GROUP BY UserId
````

Keresés az egy adott felhasználó tevékenységeit ideje, használja a tárolt IN záradék:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
DECLARE @twoHoursAgo datetime2 = DATEADD(HOUR, -2, SYSUTCDATETIME());
SELECT * FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME CONTAINED IN (@twoHoursAgo, @hourAgo)
WHERE [UserID] = 1;
````

Ábra képi megjelenítés tudja megjeleníteni trendek és szokásai egy ötletes módon nagyon egyszerűen megegyezik különösen akkor hasznos, időbeli lekérdezések:

![TemporalGraph](./media/sql-database-temporal-tables/AzureTemporal6.png)

##<a name="evolving-table-schema"></a>Táblázat séma változó

A szokásos szüksége lesz a időbeli táblázat séma módosítása a használata közben is alkalmazások fejlesztéséhez. Az, hogy egyszerűen futtassa a normál ALTER TABLE utasítás és Azure SQL-adatbázis fog megfelelően módosítások átvitele az előzmények táblázat. A következő parancsfájl jeleníti meg, hogy miként adhat további attribútum nyomon követés céljából:

````
/*Add new column for tracking source IP address*/
ALTER TABLE dbo.WebsiteUserInfo 
ADD  [IPAddress] varchar(128) NOT NULL CONSTRAINT DF_Address DEFAULT 'N/A';
````

Hasonlóképpen oszlop megadása módosítása, amíg a terhelést működik:

````
/*Increase the length of name column*/
ALTER TABLE dbo.WebsiteUserInfo 
    ALTER COLUMN  UserName nvarchar(256) NOT NULL;
````

Végül egy oszlop, amely már nem szükséges eltávolíthatja.

````
/*Drop unnecessary column */
ALTER TABLE dbo.WebsiteUserInfo 
    DROP COLUMN TemporaryColumn; 
````
    
Másik lehetőségként legújabb [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) használatával módosíthatja a időbeli táblázat séma, mialatt csatlakozik az adatbázishoz (online üzemmódban), illetve az adatbázis projekt (kapcsolat nélküli üzemmódban) részeként.

##<a name="controlling-retention-of-historical-data"></a>Adatmegőrzési korábbi adatok szabályozása

Rendszer verziószámmal időbeli táblázatokkal az előzmények táblázat növelheti az adatbázis mérete, amely több, mint normál táblákat. Nagy- és egyre táblába előfordulhat, hogy mindkét miatt csak tárolási költségeket, valamint a előadás olyan adó időbeli lekérdezése a problémát. Az előzmények táblázatban lévő adatok kezelése az adatmegőrzési elkészítésének ily módon tervezése és kezelése az életciklus minden időbeli táblázat fontos eleme. Az Azure SQL-adatbázissal az alábbi módszerekkel korábbi adatok időbeli táblázatban kezelésére, ha van:

- [Táblázat szétválasztás](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_2)
- [Egyéni karbantartása parancsfájl](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_3)

##<a name="next-steps"></a>Következő lépések

Időbeli táblázatokban információ kivétele [MSDN dokumentációt](https://msdn.microsoft.com/library/dn935015.aspx).
Látogasson el a csatorna 9 [valós ügyfél időbeli implementációs sikeres szövegegység](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) és [live időbeli bemutató](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016)megtekintés.
