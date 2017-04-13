<properties
    pageTitle="Adatok importálása az Azure keresési indexelő használatával az Azure-portálon |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás"
    description="Használatával az Azure keresési importálása varázsló az Azure-portálon bejárási adatok Azure Blob tároló tábla stroage, SQL-adatbázis és SQL Server Azure VMs."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="Azure Portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="heidist"/>

# <a name="import-data-to-azure-search-using-the-portal"></a>Adatok importálása az Azure-keresés a portálon

Az Azure portál- **Adatok importálása** varázsló az Azure keresési irányítópulton adatok betöltésének index be. 

  ![Adatok importálása a parancssávon][1]

A varázsló belső állítja be, és elindítja a *Indexelő*automatizálása az indexelő folyamat néhány lépést: 

- Az aktuális Azure előfizetés külső adatforrás csatlakoztatása
- Időpontjához az index séma alapján a forrás adatstruktúra
- Az adatforrás beolvasása sorhalmazt alapján dokumentumok létrehozása
- Dokumentumok feltöltése a keresési szolgáltatás az indexet

Ez a munkafolyamat mintaadatok használata DocumentDB tekinthetők meg. Látogasson el az [Ismerkedés az Azure keresés az Azure-portálon](search-get-started-portal.md) utasításokat.

## <a name="data-sources-supported-by-the-import-data-wizard"></a>Adatok importálása varázsló által támogatott adatforrások

Adatok importálása varázsló támogatja a következő adatforrásokhoz: 

- Azure SQL-adatbázis
- Az SQL Server-Azure virtuális relációs adatok
- Azure DocumentDB
- Azure Blob-tárolóhoz (az előzetes verzió)
- Azure táblatárolóhoz (az előzetes verzió)

Egy sík adatkészlet egy szükséges beviteli. Csak egy táblát, adatbázis nézete vagy azzal egyenértékű adatstruktúra importálhatja. A varázsló futtatása előtt kell létrehoznia a adatstruktúra.

Ne feledje, hogy az indexelő néhány továbbra is az előnézeti, ami azt jelenti, hogy az indexelő definition mögött az API előzetes verzióját. További információk és hivatkozásokat a [Indexelő áttekintése](search-indexer-overview.md) című témakörben találhat.

## <a name="connect-to-your-data"></a>Az adatok csatlakoztatása

1. Jelentkezzen be az [Azure-portál](https://portal.azure.com) és -nyitott szolgáltatási irányítópult. A metszéspont sávon a meglévő szolgáltatások megjelenítéséhez a jelenlegi előfizetését a **keresési szolgáltatás** kattinthat. 

2. Kattintson az **Adatok importálása** a dia nyitott az adatok importálása lap parancssávon.  

3. Kattintson a **Csatlakozás az adatok** adjon meg egy indexkiszolgáló által használt adatforrás-definíciót. A varázsló adatforrások belüli-előfizetés esetén általában észleli és olvasni a kapcsolat adatait, kis méretre általános beállításokat.

| | |
|--------|------------|
|**Létező adatforrás** | Ha már van definiálva a keresési szolgáltatás indexelő, választhat egy meglévő adatforrás-definíciót egy másik importáláshoz.|
|**Azure SQL-adatbázis** | Szolgáltatás nevét és egy adatbázis-olvasási engedéllyel rendelkező felhasználó hitelesítő adatait, és az adatbázis nevét a lapon, vagy egy ADO.NET kapcsolati karakterlánc keresztül adható meg. Válassza a megtekintéséhez vagy tulajdonságainak testreszabása kapcsolat-karakterlánc lehetőséget. <br/><br/>Tábla vagy nézet, ahol a sorhalmazon meg kell adni a lapon. Ez a beállítás jelenik meg, miután a kapcsolat létrejött, a legördülő lista biztosítva, így fel, hogy a kijelölés.|
|**SQL Server Azure virtuális** | Adja meg a szolgáltatás teljesen minősített neve, a felhasználói azonosító, és a jelszót és az adatbázis, a kapcsolati karakterlánc. Szeretne használni ehhez az adatforráshoz, korábban telepíteni kell egy tanúsítványt az, hogy titkosítja a kapcsolatot a helyi tárolóba. <br/><br/>Tábla vagy nézet, ahol a sorhalmazon meg kell adni a lapon. Ez a beállítás jelenik meg, miután a kapcsolat létrejött, a legördülő lista biztosítva, így fel, hogy a kijelölés.
|**DocumentDB** |Követelmények szerepeltetni a fiókot, az adatbázis és a webhelycsoport. A webhelycsoport összes dokumentumok szerepelni fog a tárgymutató. Lekérdezés összeolvasztása, vagy a sorhalmazon szűréséhez vagy módosított dokumentumokat az ezt követő adatfrissítést feltárása határozhatja meg.|
|**Azure Blob-tárolóhoz** | Követelmények a tárterület-fiók és a tároló tartalmazzák. Tetszés szerint blob nevek hajtsa végre a csoportosítás célokra virtuális elnevezni, ha megadhatja a nevét a virtuális könyvtár részét tárolóban mappaként. További információt talál [Az indexelés Blobtárolóhoz (előzetes verzió)](search-howto-indexing-azure-blob-storage.md) . |
|**Azure Táblatárolója** | Követelmények a tárterület-fiók és a táblázatok nevét tartalmazza. Tetszés szerint megadhatja a történő beolvasásához a táblák egy részét. További információt talál [Az indexelés Táblatároló (előzetes verzió)](search-howto-indexing-azure-tables.md) . |

## <a name="customize-target-index"></a>Cél index testreszabása

Index létrehozása előzetes általában az adatkészlet a következtetett. Hozzáadása, szerkesztése és törlése a mezők a séma befejezéséhez. Ezenkívül állítsa be a jellemzőket határozza meg a következő keresési viselkedése a mező szintjén.

1. **Testreszabás cél index**adja meg a név és a **kulcs** egyértelműen azonosítják az egyes dokumentum. A kulcs egy karakterlánc kell lennie. Ha a mezőértékek tartalmaz szóközöket vagy a szaggatott vonal feltétlenül speciális beállítások megadása a következő karakterek közül ellenőrzésének mellőzése **az adatimportálás** .

2. Tekintse át, és a többi mező módosítása. A mező név és típus általában kitölti meg. Módosíthatja az adattípusát.

3. Az egyes mezők indexet attribútumok beállítása:

 - Beolvasható adja eredményül a mező a keresési eredmények között.
 - Szűrhető lehetővé teszi, hogy a szűrőkifejezések a hivatkozni kívánt mezőt.
 - Rendezhető lehetővé teszi, hogy a mező rendezésének használható.
 - Facetable lehetővé teszi, hogy a síklapos navigációs mezőt.
 - Kereshető lehetővé teszi, hogy a teljes szöveges keresés.
  
4. Ha meg szeretné adni a nyelv analyzer mező szintre, kattintson a **Analyzer** fülre. Csak nyelvi gázelemzők adott időben lehet megadni. Egy egyéni analyzer vagy kulcsszó, mintát és így tovább, például egy nem nyelvű analyzer segítségével kód van szükség.

 - Kattintson a **kereshető** kijelöli a teljes szöveges keresés a mezőre, és engedélyezze a Analyzer legördülő listában.
 - Válassza ki a kívánt analyzer. Lásd: [dokumentumok több nyelven index létrehozása](search-language-support.md) további információt.

5. Kattintson a kijelölt mezők javaslatokat lekérdezési javaslatok engedélyezéséhez a **Suggester** .


## <a name="import-your-data"></a>Az adatok importálása

1. Az **adatok importálása**nevezze el az indexelő. Visszahívása, hogy az adatok importálása varázsló szorzata indexkiszolgáló. Később szeretne megtekinteni és szerkeszteni, ha Ön fogja jelölje ki a portálról, hanem a varázsló újrafuttatásával. 

2. Adja meg az ütemtervet, amelyen alapul, amelyben a szolgáltatás már kiépítve területi időzónát.

3. Küszöbértékek megadhatja, hogy az indexelés továbbra is megszakad-e egy dokumentumot a Speciális beállítások megadása. Továbbá megadhatja, hogy **kulcs** mezőt, amely tartalmazhat szóközt, és a ferde engedélyezve vannak-e.  

## <a name="edit-an-existing-indexer"></a>Meglévő indexkiszolgáló szerkesztése

A szolgáltatás irányítópulton kattintson duplán egy listában megtalálja az előfizetés által létrehozott összes indexelő diaelrendezés az indexelő csempére. Kattintson duplán az egyik az indexelő, szerkesztheti vagy törölheti azt. Az index cseréje másik meglévővel, adatforrás módosítása és indexelés során hiba küszöbértékek beállításainak megadása.

## <a name="edit-an-existing-index"></a>Egy meglévő indexet szerkesztése

Azure Keresés index szerkezeti frissítések igényel egy újra kell építenie tárgymutató, amely tartalmazza az index törlését, az index újbóli létrehozása és adatok újbóli betöltése Szerkezeti frissítései az adattípus módosítása és átnevezése vagy töröl egy mezőt.

Egy újraépítő nem igénylő szerkesztéseket olyan pontozási profilok suggesters módosítása vagy módosítása a nyelvi gázelemzők módosítása új mezőt. [Index frissítése](https://msdn.microsoft.com/library/azure/dn800964.aspx) talál további információt.

## <a name="next-step"></a>Következő lépés

Tekintse át ezeket a hivatkozásokat, ha többet szeretne tudni az indexelő:

- [Az indexelési Azure SQL-adatbázis](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [Az indexelési DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Az indexelési Blob-tárolóhoz (előzetes verzió)](search-howto-indexing-azure-blob-storage.md)
- [Az indexelési Táblatároló (előzetes verzió)](search-howto-indexing-azure-tables.md)



<!--Image references-->
[1]: ./media/search-import-data-portal/search-import-data-command.png

