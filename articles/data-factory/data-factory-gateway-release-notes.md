<properties 
    pageTitle="Kibocsátási megjegyzések az adatkezelési átjáró |} Azure Data Factory" 
    description="Adatok az adatkezelési átjáró címe kibocsátási megjegyzések" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="spelluru"/>

# <a name="release-notes-for-data-management-gateway"></a>Kibocsátási megjegyzések az adatkezelési átjáró
A problémáit, modern adatok integrálása az egyik zökkenőmentes adatok áthelyezése és a helyszíni cloud onnan. Adatok gyári ellenőrzi az együttműködés zökkenőmentes az adatkezelési átjáró, amely ügynökkel, hogy a helyszíni hibrid adat mozgását ahhoz, hogy telepíthető-e.

Részletes információt talál az adatkezelési átjáró és használatához az alábbi cikkekben talál: 

- [Az adatkezelési átjáró](data-factory-data-management-gateway.md)
- [Adatok áthelyezése fiókok között a helyszíni és a felhő Azure Data Factory használatával](data-factory-move-data-between-onprem-and-cloud.md) 

## <a name="current-version-2260721"></a>Jelenlegi verziójával (2.2.6072.1)

- Beállítás az átjáró segítségével az átjáró konfigurációkezelőjének HTTP-proxy támogat. Ha beállítva, Azure Blob, az Azure-táblából, a Azure adatok tó és a dokumentum DB HTTP-proxyn keresztül érhetők el.
- Az adatokat a/Azure Blob, tó Azure adattár, másolásakor TextFormat kezelésének támogatja az élőfej helyszíni fájlrendszerben, és a helyszíni Fájlrendszerhez.
- Az adatok másolása a Hozzáfűzés Blob és lap Blob együtt a már támogatott blokk Blob támogatja.
- Vezet be egy új átjáró állapota **Online (korlátozott)**, amely jelzi, hogy működik-e az átjáró a főbb funkciók másolása varázsló interaktív műveletet támogatása kivételével.
- Hatékonyabbá tehető az átjáró regisztrációs regisztrációs kulccsal megbízhatóságát.

## <a name="earlier-versions"></a>Korábbi verziók

## <a name="2160401"></a>2.1.6040.1

- DB2 illesztőprogram most az átjáró telepítőcsomagját szerepel. Nem kell külön telepítenie. 
- DB2 illesztőprogram z/OS-támogatja DB2-e (mint / 400) együtt a már támogatott platformok (Linux Unix és Windows). 
- Forrás- vagy cél DocumentDB használata a helyszíni adatok tárolja támogatja
- Adatok másolása a kezdete/vége hideg/gyorsbillentyűk használatát támogatja blob-tároló együtt a már támogatott általános célú tárterület-fiókot. 
- Lehetővé teszi, hogy a helyszíni SQL Server távoli bejelentkezés jogosultságokkal rendelkező átjáró keresztül történő csatlakozást.  

## <a name="2060131"></a>2.0.6013.1

- Kézi telepítésekor az átjárók által használandó nyelvet/kulturális is választhat.
- Ha az átjáró nem működik a várt módon működik, megadhatja az utóbbi hét napban átjáró naplók küldése a Microsoftnak a probléma hibaelhárításának megkönnyítése érdekében. Ha nem csatlakozik átjáró a felhőbeli szolgáltatástól, megadhatja, mentéséhez és az átjáró naplók archiválni.  
- Továbbfejlesztett felhasználói kezelőfelület az átjáró konfigurációkezelőjének:
    - Átjáró állapota további láthatóvá tenni a Kezdőlap lapon.
    - Átrendezett és egyszerűsített vezérlők.
- Adatok egy tárhelyről [kód ingyenes másolás előzetes eszköz](data-factory-copy-data-wizard-tutorial.md)használatával másolhatja. Kapcsolatos további tudnivalók: [Előkészített másolása](data-factory-copy-activity-performance.md#staged-copy) funkció általános. 
- Azure gépi tanulási az adatkezelési átjáró bejövő adatok adatok közvetlenül egy helyszíni SQL Server-adatbázishoz is használhatja.
- Teljesítménybeli fejlesztések
    - Javítja a séma/előzetes verzió az SQL Server ellen megtekintése a kód ingyenes másolás előzetes eszközben.



## <a name="11259531"></a>1.12.5953.1
- Hibajavítás

## <a name="11159181"></a>1.11.5918.1

- Az átjáró eseménynapló maximális hossza lett növekedett 1 MB-40 MB.
- A figyelmeztetés párbeszédpanel jelenik meg abban az esetben, ha a számítógép újraindítása átjáró automatikus frissítés során van szükség. Megadhatja a jobb oldali majd vagy később újra. 
- Abban az esetben, ha az automatikus frissítés sikertelen lesz, az átjáró installer próbálkozások automatikus frissítése háromszor a maximum mezőben.
- Teljesítménybeli fejlesztések
    - Nagy táblázatok a helyszíni kiszolgálóról kód ingyenes másolása az esetben a teljesítmény javítása.
- Hibajavítás

## <a name="11058921"></a>1.10.5892.1

- Teljesítménybeli fejlesztések
- Hibajavítás

## <a name="1958652"></a>1.9.5865.2

- Nulla érintéses automatikus frissítés videofunkcióinak
- Tudnivalók az átjáró új tálca ikonja
- Az ügyféltől "Frissítés most" lehetősége
- Azt jelenti, hogy a frissítési ütemezés időpont beállítása
- Az automatikus frissítés ki-be kapcsoló választás PowerShell-parancsprogramot
- JSON formátum támogatása  
- Teljesítménybeli fejlesztések
- Hibajavítás

## <a name="1858221"></a>1.8.5822.1

- Hibaelhárítási élmény javítására
- Teljesítménybeli fejlesztések
- Hibajavítás

### <a name="1757951"></a>1.7.5795.1

- Teljesítménybeli fejlesztések
- Hibajavítás

### <a name="1757641"></a>1.7.5764.1

- Teljesítménybeli fejlesztések
- Hibajavítás

### <a name="1657351"></a>1.6.5735.1

- Támogatás a helyszíni adatforrás/gyűjtő Fájlrendszerhez
- Teljesítménybeli fejlesztések
- Hibajavítás

### <a name="1656961"></a>1.6.5696.1

- Teljesítménybeli fejlesztések
- Hibajavítás

### <a name="1656761"></a>1.6.5676.1

- Diagnosztikai eszközök Configuration Manager támogatása
- Táblázat oszlopainak Azure Data Factory táblázatos adatforrások támogatása
- Azure Data Factory SQL DW támogatása
- Azure Data Factory Reclusive BlobSource és FileSource támogatása
- BlobSink és FileSink bináris példányt az Azure Data Factory CopyBehavior – MergeFiles PreserveHierarchy és FlattenHierarchy támogatása
- Támogatja az Azure Data Factory jelentési másolás tevékenység
- Támogatási forrás kapcsolódási adatérvényesítési Azure Data Factory
- Hibajavítás


### <a name="1656721"></a>1.6.5672.1

- Azure Data Factory táblanév ODBC adatforrás támogatása
- Teljesítménybeli fejlesztések
- Hibajavítás

### <a name="1656581"></a>1.6.5658.1

- Az Azure Data Factory gyűjtése támogatási fájl
- Bináris másolása megőrzése hierarchia Azure Data Factory támogatása
- Azure Data Factory másolás tevékenység idempotencia támogatása
- Hibajavítás

### <a name="1656401"></a>1.6.5640.1

- 3 további adatforrások támogatása Azure Data Factory (ODBC, OData Fájlrendszerhez.)
- Azure Data Factory idézőjel a csv-elemző támogatás
- Tömörítés támogatását (BZip2)
- Hibajavítás

### <a name="1556121"></a>1.5.5612.1

- Öt relációs adatbázisok támogatása Azure Data Factory (MySQL, PostgreSQL, DB2, Teradata és Sybase)
- Tömörítés támogatását (Gzip és Deflate)
- Teljesítménybeli fejlesztések
- Hibajavítás


### <a name="1455491"></a>1.4.5549.1

- Az Oracle adatok forrása támogatásának Azure Data Factory hozzáadása
- Teljesítménybeli fejlesztések
- Hibajavítás

### <a name="1454921"></a>1.4.5492.1

- Az egyesített bináris, amely támogatja a Microsoft Azure Data Factory és az Office 365 Power BI szolgáltatásokat
- A konfiguráció felhasználói felület és a regisztrációs folyamat pontosítása
- Azure Data Factory – Azure be- és kilépési támogatja az SQL Server-adatforrásból

### <a name="1253031"></a>1.2.5303.1

-   Több időt vesz igénybe adatforrás-kapcsolatok támogatásához időtúllépése hiba megoldása. 
    
### <a name="1155268"></a>1.1.5526.8

- .NET-keretrendszer 4.5.1 szükséges után kaphatnak a telepítés során.

### <a name="1051442"></a>1.0.5144.2

- Nincs módosítás Azure Data Factory esetek befolyásolja. 
