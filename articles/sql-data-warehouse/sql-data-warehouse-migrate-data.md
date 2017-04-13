<properties
   pageTitle="Az adatok áttelepítése SQL adatraktár |} Microsoft Azure"
   description="Tippek az adatok áttelepítését Azure SQL-adatraktár megoldások fejlesztésével."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="migrate-your-data"></a>Az adatok áttelepítése
Az SQL adatraktár a különböző eszközök helyezhető különböző forrásokból származó adatok.  ADF másolás, SSIS és bcp az összes használható a cél elérését. Azonban adatok nő összegként célszerű átgondolni megszakítása le azokat a lépéseket, az adatok áttelepítési folyamatot. Ez számára biztosítja, a teljesítményt, és címtárfrissítések annak biztosítására, a zökkenőmentes adatok áttelepítésének lépésein optimalizálása lehetőséget.

Ez a cikk első azt ismerteti, hogy a ADF másolás, SSIS és bcp az egyszerű áttelepítési jelenik meg. Majd vizsgálja meg kissé mélyebb a hogyan az áttelepítés optimalizálhatók.

## <a name="azure-data-factory-adf-copy"></a>Azure adatok gyári (ADF) másolása
[ADF másolása][] az [Azure Data Factory][]része. Az adatok exportálására strukturálatlan fájlok helyi tároló távoli strukturálatlan fájlok tárolt Azure blob-tárolóhoz vagy közvetlenül egy SQL adatraktár tartózkodó ADF másolás is használhatja.

Ha egyszerű fájlok elindítja az adatok, akkor először terhelést kezdeményezése előtt átvitele tároló Azure blob azt az SQL adatraktár. Miután az adatátvitel az Azure blob-tárolóhoz megadhatja [ADF másolás][] nyomni az adatokat az SQL adatraktár újra felhasználhatja.

PolyBase is biztosít az adatok betöltésének egy nagy teljesítményű lehetőséget. Jelent azonban, hogy két eszközeivel egy helyett. Ha szükséges, a legjobb teljesítmény elérése érdekében PolyBase használja. Ha azt szeretné, hogy egy egyszerű eszközt élmény (és az adatok nem tömeges) ADF választ a kérdésére.

> [AZURE.NOTE] PolyBase szükséges, az adatfájlok UTF-8 lesz. Ez a ADF másolás alapértelmezett kódolást, hogy semmi nem módosíthatja. Ez a nem módosíthatja az alapértelmezett működés ADF példány csak egy emlékeztetőt.

Központi keresztül az alábbi cikkben néhány nagyszerű [ADF minták][].

## <a name="integration-services"></a>Integrációs szolgáltatások ##
Integration Services (SSIS) egy olyan hatékony és rugalmas átalakítása kibontása és terhelés (ETL) eszköz, amely támogatja az összetett munkafolyamatok, átalakítási és több adat betöltése beállítások. Az SSIS segítségével egyszerűen adatátviteli az Azure vagy szélesebb áttelepítés részeként.

> [AZURE.NOTE] SSIS UTF-8 exportálhatja a bájt sorrend be van jelölve a fájl nélkül. Beállításához ezt előbb használja a származtatott oszlop összetevő szeretné alakítani a UTF-8 65001 kódlapot használ az adatfolyam karakter adatait. Az oszlopok konvertálás után írni az adatokat a strukturálatlan fájlhoz cél kártya biztosítva, hogy 65001-es is van kiválasztva a kódlapot a fájlt.

Az SSIS SQL adatraktár csatlakozik, ahogyan azt egy SQL Server deployment kíván csatlakozni. Azonban a kapcsolatokat kell egy ADO.NET kapcsolatkezelő használ. Meg kell is gondoskodik konfigurálása a "használata tömeges beszúrás esetén érhető el" átviteli maximalizálása beállítást. Olvassa el a [ADO.NET cél kártya][] cikk, ha többet szeretne tudni a tulajdonság

> [AZURE.NOTE] Kapcsolódás OLEDB használatával Azure SQL-adatraktár nem támogatott.

Ezenkívül van mindig a lehetséges, hogy a csomag miatt előfordulhat, hogy nem szabályozásának vagy hálózati hibát. Tervező csomagokat, azokat is folytatható helyén hiba nélkül ismétlése használata előtt a hibát végeznie.

További információt az [SSIS-dokumentáció][]verzióhoz.

## <a name="bcp"></a>BCP
BCP egy strukturálatlan, egybesimított fájlhoz adatok importálása és exportálása készült parancssori segédprogramot. Néhány átalakítás során adatok exportálása történhet. Egyszerű átalakítások végrehajtásához lekérdezés használata a jelölje ki az adatok és átalakítása. Exportálása után a strukturálatlan fájl ezután tölthetők közvetlenül a cél az adatraktár SQL-adatbázisban.

> [AZURE.NOTE] Érdemes gyakran célszerű az beágyazására az átalakítás során adatok exportálása a forrás rendszeren egy nézetben. Ez az biztosítja, hogy a logika megőrződnek, és a folyamat megismételhető.

Bcp előnyei vannak:

- Egyszerű. BCP parancsait egyszerű összeállítása és végrehajtása
- Másik ismételt betöltése a folyamat. Egyszer exportált a betöltés lehet tetszőleges számú alkalommal végrehajtása

Bcp korlátozások a következők:

- BCP működik-e a strukturálatlan fájl tabulált. Nem működik a fájlokat, például az XML- vagy JSON
- BCP UTF-8 exportálása nem támogatja. Ez megakadályozhatja a PolyBase exportált bcp adatok használata
- Adatok átalakítása funkciók csak az Exportálás szakasz korlátozták, ezért egyszerűen jellegű
- BCP nem volt módosítani kell robusztus betöltésekor adatokat az interneten keresztül. Hálózati instabilitást egy betöltési hiba jelenhet meg.
- a címtárra előtt a terhelést a céladatbázisban található séma támaszkodik BCP

További tudnivalókért lásd: az [adatok az SQL adatraktár betöltése használata bcp][].

## <a name="optimizing-data-migration"></a>Adatok áttelepítése optimalizálása
Egy SQLDW adatok áttelepítési folyamatot hatékony bonthatók három különálló lépésből áll:

1. Az adatforrás-adatok exportálása
2. Azure adatok továbbítása
3. A céladatbázisban SQLDW betöltése

Minden egyes lépés egyenként optimalizálhatók robusztus, újra másik és rugalmassá áttelepítési folyamatot, amely a teljesítmény minden lépésnél maximalizálja létrehozni.

## <a name="optimizing-data-load"></a>Optimalizálás adatok betöltése
Fordított sorrendben egy pillanatra; ezek rögzíthetők adatok betöltése leggyorsabb módja PolyBase található. Optimalizálás PolyBase betöltés folyamat helyez el Előfeltételek az előző lépéseket, célszerű lehet áttekinteni a előzetes. Ezek a következők:

1. Az adatfájlok kódolást
2. Az adatfájlok formázása
3. Adatok fájlok helye

### <a name="encoding"></a>Kódolás
PolyBase szükséges adatfájlokat UTF-8 kódolású lesz. Ez azt jelenti, hogy az adatok exportálásakor követnie kell ez alól. Ha az adatok csak tartalmaz egyszerű ASCII-karakterek (nem extended ASCII) majd ezeket a térkép közvetlenül a UTF-8 standard, és nem kell aggódnia amiatt, hogy túl nagy a kódolást. Jó helyen jár Ha az adatok speciális karaktereket tartalmaz, például umlautokat, ékezetes vagy szimbólumok vagy az adatok nem latin betűs nyelvet támogat majd akkor annak érdekében, hogy a Exportálás fájlok megfelelően UTF-8 kódolású.

> [AZURE.NOTE] adatok exportálása UTF-8 BCP nem támogatja. Ezért a azt javasoljuk, hogy az adatok exportálása Integration Services vagy a Másolás ADF használata. Érdemes mutató, hogy a UTF-8 bájtos sorrendben jel (AJ) nincs szükség az adatok fájlban.

Kódolt UTF-16 használata fájlok írandó újra ***előzetes*** az adatátvitel lesz szüksége.

### <a name="format-of-data-files"></a>Az adatfájlok formázása
PolyBase csak egy rögzített sor Befejezés \n vagy újsor jelenik meg. Az adatfájlok meg kell felelnie a szabványos. Nincsenek korlátozások a karaktersorozat vagy oszlop végpontok.

Be kell a fájl minden oszlop meghatározása a külső tábla PolyBase részeként. Győződjön meg arról, hogy az összes exportált oszlop szükség, és hogy a típusok megfelelnek a szükséges szabványokat.

Nézze meg újra a [áttelepítése a séma]-cikk részletesen a támogatott adattípusok.

### <a name="location-of-data-files"></a>Adatok fájlok helye
SQL adatraktár PolyBase használja az Azure Blob-tárolóhoz adatok kizárólag betöltéséhez. Ennek az adatokat kell először átadott blob-tárolóhoz be.

## <a name="optimizing-data-transfer"></a>Optimalizálás adatátvitel
A legkisebb részei adatok áttelepítése egyik az adatok átvitele Azure. Nemcsak lehet hálózati sávszélesség problémát, de is hálózati megbízhatóság jelentősen hátráltathatják a előrehaladását. Adatok áttelepítése az Azure alapértelmezés szerint az az interneten keresztül az átadás hibák előforduló bizonnyal ésszerűen valószínűleg így. A hibák azonban szükség lehet újraküldött, teljesen vagy részben az adatokat.

Szerencsére sebességének és a folyamat címtárfrissítések javítható számos lehetőség áll rendelkezésére:

### <a name="expressroute"></a>[Készült ExpressRoute][]
Érdemes kipróbálnia [készült ExpressRoute][] növelése érdekében a továbbított. [Készült ExpressRoute][] biztosít létrehozott személyes kapcsolattal Azure, a kapcsolat nem megy nyilvános interneten keresztül. Ez a semmiképpen kötelező lépés. Azonban azt hatékonyabbá tehetők átviteli, amikor az Azure adatok küldése a korábbi helyszíni vagy helymegosztást eszközt.

A [készült ExpressRoute][] előnyei vannak:

1. Nagyobb megbízhatóság
2. Gyorsabb hálózati
3. Alsó hálózati késés
4. nagyobb hálózati biztonság

Akkor előnyös esetek; számú, [készült ExpressRoute][] nemcsak az áttelepítést.

Kíváncsi? További információt és árak, kérjük, látogasson el a [készült ExpressRoute dokumentációt][].

### <a name="azure-import-and-export-service"></a>Azure importálás és exportálás szolgáltatás
Az Azure importálása és exportálása szolgáltatás a következő tömeges (TB ++) adatok továbbítása az Azure-nagy (GB ++) készült adatok átadás áll. Az adatok lemezre írása és a szállítási őket az Azure adatközpontja csapattól kell beszerezni. A lemez tartalmáról majd töltődik Azure BLOB-tároló az Ön nevében.

Magas szintű nézetének az importálás exportálási folyamat során a következőképpen történik:

1. Állítsa be az Azure Blob-tárolóhoz tároló az adatok fogadásához
2. Az adatok exportálása a helyi tárolóba
2. Másolja az adatokat 3,5 hüvelykes SATA II/III merevlemez-meghajtók a eszközzel [Azure importálás/exportálás]
3. Hozzon létre egy importálás az Azure importálás és a napló fájlokat a [Azure importálás/exportálás eszköz] készített biztosító exportálása szolgáltatás segítségével
4. A kijelölt Azure adatközpont kiszállítása a lemezt
5. Az adatok átkerül az Azure Blob-tárolóhoz tároló
6. Adatok betöltése SQLDW PolyBase használatával

### <a name="azcopy-utility"></a>[AZCopy][] segédprogram
A [AZCopy][] segédprogram kiváló eszköz a lekérése az Azure BLOB-tároló programba átvitt adatok. Azt a nagyon nagy (GB ++) adatátvitel kis (MB ++) szolgál. [AZCopy] van is kialakítva adja meg a jó rugalmassá átvitt adatok átvitele Azure, ezért ha a kiváló választás az adatok továbbítása lépés. Egyszer átvitt betöltheti az adatokat PolyBase SQL adatraktár be. AZCopy is részévé az SSIS-csomagok egy "Végrehajtása folyamat" tevékenységet használ.

AZCopy használatához először töltse le és telepítse azt. Van egy [gyártási verzió][] és az [előzetes verziójú][] érhető el.

Töltse fel a fájlt a fájlrendszerből szüksége lesz egy olyanra, mint az alábbi parancsot:

```
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt
```

A folyamat magas szintű összefoglaló lehet:

1. Állítsa be a kívánt Azure tároló blob-tárolóban az adatok fogadásához
2. Az adatok exportálása a helyi tárolóba
3. Adatok ábrázolása a Azure Blob-tárolóhoz tároló AZCopy
4. Adatok betöltése SQL adatraktár PolyBase használatával

Teljes dokumentáció: [AZCopy][].

## <a name="optimizing-data-export"></a>Optimalizálás adatok exportálása
Kívül biztosítva, hogy az Exportálás megfelel-e a követelményeknek PolyBase határozza meg is kérhet optimalizálhatja az Exportálás az adatok a folyamat további javítása érdekében.

> [AZURE.NOTE] PolyBase megköveteli a adatok UTF-8 formátumban valószínű bcp fogja használni a végezze el az adatok exportálása. BCP UTF-8 kimenetre adatfájlok nem támogatja. SSIS vagy ADF másolása alkalmasak sokkal jobban adatok exportálása az ilyen típusú hajt végre.

### <a name="data-compression"></a>Adatok tömörítés
PolyBase tömörített gzip adatok olvashatja. Ha tudja, akkor az adatok gzip fájlokat, a hálózaton keresztül éppen tolni adatok mennyiségét fog összezárása.

### <a name="multiple-files"></a>Több fájl
Nagy táblázatok több fájlokba feldarabolása nem csak exportálás sebesség növelésének, szintén gondoskodhat az átadás re-startability, és az általános kezelhetőség egyszer az Azure blob-tárolóban lévő adatok. Sok szép funkciói PolyBase egyik, hogy fog egy egy mappába a fájlok olvasása és tekinti meg egy táblában. Éppen ezért célszerű elkülönítése minden táblázatában a fájlokat a saját mappába való felvételéhez.

PolyBase támogatja az úgynevezett "rekurzív mappa átviteli" is. Ez a funkció használatával további növelése az exportált adatok javítható az adatok kezelése a szervezetben.

Többet szeretne tudni a PolyBase adatok betöltése, lásd: az [Adatok az SQL adatraktár betöltése használata PolyBase][].


## <a name="next-steps"></a>Következő lépések
Áttelepítési bővebben: [áthelyezése SQL adatraktár a megoldást][].
Fejlesztési vonatkozó további tippek [fejlesztési áttekintése][]című témakörben találhat.

<!--Image references-->

<!--Article references-->
[AZCopy]: ../storage/storage-use-azcopy.md
[ADF másolása]: ../data-factory/data-factory-data-movement-activities.md 
[ADF minták]: ../data-factory/data-factory-samples.md
[ADF Copy examples]: ../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md
[fejlesztési – áttekintés]: sql-data-warehouse-overview-develop.md
[A megoldás áttelepítése SQL adatraktár]: sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Adatok betöltése SQL adatraktár bcp használatával]: sql-data-warehouse-load-with-bcp.md
[Adatok betöltése SQL adatraktár PolyBase használatával]: sql-data-warehouse-get-started-load-with-polybase.md


<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory]: http://azure.microsoft.com/services/data-factory/
[Készült ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[Dokumentáció készült ExpressRoute]: http://azure.microsoft.com/documentation/services/expressroute/

[gyártási verzió]: http://aka.ms/downloadazcopy/
[előzetes verzió]: http://aka.ms/downloadazcopypr/
[ADO.NET cél kártya]: https://msdn.microsoft.com/library/bb934041.aspx
[Az SSIS-dokumentáció]: https://msdn.microsoft.com/library/ms141026.aspx
