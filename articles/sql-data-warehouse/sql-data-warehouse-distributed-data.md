<properties
   pageTitle="Normális eloszlású adatokat, és a táblázat beállításai a nagymértékben párhuzamos feldolgozása (MPP) rendszerekhez SQL adatraktár és a párhuzamos adatraktár elosztott |} Microsoft Azure"
   description="Megtudhatja, hogyan van meghatározva az adatok nagymértékben párhuzamos feldolgozása (MPP) és a táblákat az Azure SQL-adatraktár és a párhuzamos adatraktár terjesztése lehetőségeit."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/10/2016"
   ms.author="barbkess"/>


# <a name="distributed-data-and-distributed-tables-for-massively-parallel-processing-mpp"></a>Az elosztott adatok és elosztott táblázatok esetében nagymértékben párhuzamos feldolgozása (MPP)

Megtudhatja, hogyan a felhasználói adatok az Azure SQL-adatraktár és a párhuzamos adatraktár, amelyek a Microsoft nagymértékben párhuzamos feldolgozása (MPP) rendszerek van meghatározva. A adatraktár elosztott adatok hatékony használatához tervezése segít a lekérdezésfeldolgozást előnyökkel jár a MPP-architektúra eléréséhez. Adatbázis-tervezés néhány választási beállíthatja, hogy a lekérdezési teljesítmény javítására jelentős hatást.  

>[AZURE.NOTE] Azure SQL-adatraktár és a párhuzamos adatraktár használja az azonos nagymértékben párhuzamos feldolgozása MPP-Tervező, de a néhány különbségek az alapul szolgáló platform miatt. SQL adatraktár platformot megegyezik egy futtat a Azure Service (PaaS). A párhuzamos adatraktár futtatja a Analytics Platform rendszer (Apps alkalmazások) amely Windows Server rendszeren futó helyszíni készülék.

## <a name="what-is-distributed-data"></a>Mit nevezünk elosztott adatok?

Az SQL adatraktár és a párhuzamos adatraktár elosztott adatok megtörtént a MPP rendszerben egynél több helyen tárolt felhasználói adatok hivatkozik. Egyes helyekre működik, mint egy független tárhely és a egység futó lekérdezések az adatokat a részéhez. Az elosztott adata alapvető futó lekérdezések párhuzamosan magas lekérdezési teljesítmény elérése érdekében.

Adatok terjesztheti, a adatraktár rendel egy felhasználó tábla minden egyes sorához egy megosztott helyen.  Táblázatok kivonat-eloszlás módszer vagy ciklikus módszer oszthat meg. A terjesztési módszert a CREATE TABLE utasítás van megadva. 

## <a name="hash-distributed-tables"></a>Táblázatok kivonat elosztott
  
Az alábbi ábra szemlélteti, hogyan az (tábla nem osztott) teljes kivonat elosztott táblázatként tárolja kap. Mérvadó függvény minden egyes sor egy terjesztési tagja rendelt azonosítószámot. A tábla definícióját egyik oszlop van kijelölve a terjesztési oszlopával azonosította. A kivonat függvény a minden egyes sorára hozzárendelése egy terjesztési használja a terjesztési oszlop értékét.

Dolgot teljesítmény egy terjesztési oszlopok kiválasztásának például megkülönböztethetőség, adatok ferdeség és a típusú lekérdezések futtatása a rendszer.
  
![Elosztott táblázat] (media/sql-data-warehouse-distributed-data/hash-distributed-table.png "Elosztott táblázat")  
  
-   Minden egyes sor egy terjesztési tartozik.  
  
-   A mérvadó ujjlenyomatot rendel egy terjesztési minden egyes sorára.  
  
-   Tábla sorainak egy terjesztési változik, ahogy a különböző méretű táblák.

## <a name="round-robin-distributed-tables"></a>Ciklikus elosztott táblák

Ciklikus elosztott táblázat hozzárendelésével minden egyes sorára terjesztési egymás után következő módon osztja meg a sorok. Adatok betöltése ciklikus táblába gyors, de a lekérdezési teljesítmény lassabban fog.  Ciklikus táblázat általában csatlakozás szükséges ahhoz, hogy a lekérdezés egy a pontos, időt vesz igénybe az eredményt adó a sorok reshuffling.

## <a name="distributed-storage-locations-are-called-distributions"></a>Az elosztott tárolási helye úgynevezett terjesztését

Minden olyan helyen, elosztott egy terjesztési neve. A lekérdezés futtatásakor párhuzamosan, minden egyes terjesztési SQL-lekérdezés elvégzi az adatokat a részéhez. SQL-adatraktár futtatja a lekérdezést SQL-adatbázis használja. A párhuzamos adatraktár SQL Server használatával futtatja a lekérdezést. A megosztott semmi architektúra tervezés alapvető méretezési párhuzamos számítások eléréséhez.

### <a name="can-i-view-the-distributions"></a>A felosztott megtekintése

Minden egyes terjesztési terjesztési Azonosítóra van, és SQL adatraktár és a párhuzamos adatraktár vonatkoznak rendszer nézetben látható. A lekérdezési teljesítmény és egyéb problémák elhárítása az eloszlás azonosító is használhatja. A rendszer nézetek listáját olvassa el a a [MPP rendszerek nézet](sql-data-warehouse-reference-tsql-statements.md)című témakört.

## <a name="difference-between-a-distribution-and-a-compute-node"></a>Egy eloszlás és a számítási csomópont közötti különbség

Egy eloszlás elosztott adatok tárolására és a párhuzamos lekérdezéseket az alapegység szolgál. Felosztás számítási csomópontok vannak csoportosítva. Az egyes számítási csomópontok egy vagy több felosztás nyomon követi.  

-   A hardveres architektúra és méretezési lehetőségek központi összetevője számítási csomópontok elemzési Platform rendszer használja. Mindig használ nyolc terjesztését számítási csomópontot, egy, a diagram kivonat elosztott táblák látható módon. Számítási csomópontok számának, ezért a felosztás, számát a készülék vásárolhat számítási csomópontok számának határozza meg. Ha például ha nyolc számítási csomópontok vásárol, kap 64 terjesztését (8 számítási x 8 terjesztését/csomópont csomópontok). 

-   SQL-adatraktár 60 terjesztését adott számú és a számítási csomópontok rugalmas számos tartalmaz. A számítási csomópontok alkalmazza az Azure számítások és a tárhely forrásokból. Számítási csomópontok számának kódmentes szolgáltatás terhelését, és megadhatja a adatraktár számítógépes kapacitás (DWUs) szerint módosíthatja. Számítási csomópontok számának megváltozásakor terjesztését egyes számítási csomópontok számának is módosítja. 

### <a name="can-i-view-the-compute-nodes"></a>A számítási csomópontok megtekintése

Az egyes számítási csomópontok csomópont Azonosítóra van, és SQL adatraktár és a párhuzamos adatraktár vonatkoznak rendszer nézetben látható.  A rendszer nézetek sys.pdw_nodes kezdődő node_id oszlopban keres megjelenítheti a számítási csomópontot. A rendszer nézetek listáját olvassa el a a [MPP rendszerek nézet](sql-data-warehouse-reference-tsql-statements.md)című témakört.

## <a name="Replicated"></a>Táblázatok replikált a párhuzamos adatraktár 
  
Vonatkozik: párhuzamos adatraktár

Nemcsak a elosztott táblákat használ, a párhuzamos adatraktár kínálja fel való replikáció táblákat. Egy táblázat minden egyes számítási csomóponton egészében tárolt *táblázat replikált* . Táblázat replikálása eltávolítja a sorok között számítási csomópontok átadása illesztés vagy összesítési táblázat használata előtt kell. Replikált táblák csak akkor valósítható kis táblázatokkal miatt a extra tárhely a tárolásához, a teljes táblázat minden egyes számítási csomóponton szükséges.  
  
Az alábbi ábra mutatja, amely az egyes számítási csomóponton replikált táblázat. A replikált tábla között a számítási csomóponthoz rendelt összes lemez vannak tárolva. A lemez stratégia írhatósága SQL Server használatával történik.  
  
![Replikált táblázat] (media/sql-data-warehouse-distributed-data/replicated-table.png "Replikált táblázat") 
  
## <a name="next-steps"></a>Következő lépések
  
Az elosztott táblák hatékony használatához olvassa el [Distributing táblákat az SQL adatraktár](sql-data-warehouse-tables-distribute.md)  
  



  
