<properties
   pageTitle="Áttelepítése: Adatok raktári áttelepítési segédprogram |} Microsoft Azure"
   description="SQL-adatraktár áttelepítése."
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
   ms.date="08/08/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# <a name="data-warehouse-migration-utility-preview"></a>Adatok raktári áttelepítési segédprogram (előzetes verzió)

> [AZURE.SELECTOR]
- [Töltse le az áttelepítési segédprogram][]

Az adatok raktári áttelepítési eszköz olyan eszköz, hogy séma és az adatok az SQL Server és Azure SQL-adatbázis az Azure SQL-adatraktár készült. Séma áttelepítés során az eszköz automatikusan leképezi a megfelelő séma forrásból származó célhelyre. A séma áttelepítését követően az eszközöket az automatikusan generált parancsfájlok adatok áthelyezése lehetőséget nyújt a.

Séma és az adatok áttelepítési mellett ez az eszköz lépve készítése a kompatibilitás-jelentéseket, amelyeket a forrás- és példányai, amelyek megakadályozná egyesíti áttelepítési között kompatibilitási problémák összegzés lehetőséget.

## <a name="get-started"></a>Első lépések
Telepítési feltételeként szüksége lesz az BCP parancssori segédprogram áttelepítési parancsfájlokat és a kompatibilitás-jelentés megtekintése az Office futtatására. A végrehajtható fájl, amely letöltődik indítása után kéri lesz az eszköz telepítése előtt, fogadja el a szokásos EULA.

Ezeken kívül az áttelepítési Utiliy futtatásához szüksége lesz az engedélyek követően az adatbázist, amely az áttelepítendő keresi:-adatbázis létrehozása, bármilyen adatbázis módosítása vagy bármely NÉZETDEFINÍCIÓ.

### <a name="launching-the-tool-and-connecting"></a>Az eszköz indítása és csatlakozás
Az Asztal ikon, amely akkor jelenik meg a bejegyzés telepítés parancsára kattintva nyissa meg az eszköz. Az eszköz megnyitásakor kérni fogja a egy kapcsolatlétesítési lapot, ahol megadhatja a forrás- és más rendeltetési helyet az áttelepítési eszköz. Ekkor támogatjuk SQL Server és Azure SQL-adatbázissal, források és SQL adatraktár célként. Ha bejelöli ezt követően a rendszer kéri kiszolgálóhoz való csatlakozáshoz a forrás jobb kitöltése a kiszolgáló nevét, és hitelesítése, majd kattintson a "Csatlakozás" parancsra.

Miután hitelesítése, az eszköz jelennek meg az adatbázisokra, amelyek szerepelnek a kiszolgáló, amelyhez csatlakozik a listáját. Jelölje ki az áttelepíteni kívánt adatbázist, és ezután kattintson a "Áttelepítése a kijelölt" megkezdheti az áttelepítési.

## <a name="migration-report"></a>Áttelepítési jelentés
Az eszköz ellenőrzése adatbázis kompatibilitási kijelölése készítése a kért áttelepítése az adatbázis összes objektum előtti összegzése jelentés. Az SQL Server-funkciókat, amely nem szerepel a SQL adatraktár részét szélesebb listája az [áttelepítési dokumentáció][]találhatók. Miután a jelentést hoz létre fogja mentheti, és nyissa meg a jelentést az Excel programban.

Ne feledje, hogy az áttelepítési séma, mint "Object" megváltoztatja ahhoz, hogy azonnal adatainak áttelepítési állapotával, hogy a legtöbb problémák létrehozásakor. Olvassa el a módosításokat annak érdekében, amelyeket nem szeretne további módosításokat a séma alkalmazása előtt.

## <a name="migrate-schema"></a>Séma áttelepítése

A csatlakozás után áttelepítése séma kiválasztása hoz létre a kijelölt táblák séma áttelepítési parancsfájl. A parancsprogram-portok a térképek nem kompatibilis adatokat a táblázat szerkezetének kompatibilis a további űrlapok fájltípusokat, és hoz létre a hitelesítő és -sémafájlok, ha ez jelzi az áttelepítési beállításokat a felhasználó által. Ez a kód futtatását is lehetővé teszi a célként megadott SQL adatraktár példány ellen, egy fájlba, a vágólapra másolt vagy akár szerkesztett egysoros előtt további műveleteket végezhet.  

Feljegyzett fölött, mint amikor áttelepítése sémájának áttekintése az áttelepítés változik, amely az eszköz összes dia megnézéséhez annak érdekében, hogy, hogy megértette őket.  

## <a name="migrate-data"></a>Adatok áttelepítése

Az adatok áttelepítésének parancsra kattintva is készíthet BCP parancsprogramok helyezi át az adatok első strukturálatlan fájlokat a kiszolgálón, majd az SQL adatraktár veszi. Javasoljuk, hogy ezt a folyamatot, az adatokat, és újrapróbálkozások kis lépésekben áthelyezése nem beépített és hibák történhetnek, ha a hálózati kapcsolat mérvű. Annak érdekében, hogy ezt futtatni, meg kell a BCP parancssori segédprogram telepítve van, és kell már jött létre az adatok a sémában.

Miután kitöltötte a fenti paramétereknél egyszerűen kattintson a Futtatás áttelepítési, és két csomagok csoportja a megadott helyen jön létre. Az exportált fájlt annak érdekében, hogy az áttelepítési forrásból származó adatok exportálása strukturálatlan fájlokat, és az importálási fájl végre annak érdekében, hogy az adatok importálása az SQL adatraktár.

## <a name="next-steps"></a>Következő lépések
Most, hogy bizonyos adatok áttelepítését ellenőrizze, hogy miként [fejlesztését][].

<!--Image references-->

<!--Article references-->
[áttelepítési dokumentáció]: sql-data-warehouse-overview-migrate.md
[kidolgozása]: sql-data-warehouse-overview-develop.md

<!--Other Web references--> 
[Töltse le az áttelepítési segédprogram]: https://migrhoststorage.blob.core.windows.net/sqldwsample/DataWarehouseMigrationUtility.zip
