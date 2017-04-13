<properties
    pageTitle="Azure tároló replikációs |} Microsoft Azure"
    description="A Microsoft Azure tároló fiók adatainak tartóssági és magas elérhetősége van replikált. Replikációs beállításai helyileg felesleges tároló (LRS), zóna felesleges tároló (ZRS), geo felesleges (GRS), és olvasásra geo felesleges tárolására (TS-GRS) tartalmazza."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="tamram"/>

# <a name="azure-storage-replication"></a>Azure tároló replikációs

A Microsoft Azure tároló fiók adatainak mindig replikált, tartóssági és elérhetőséget biztosítása érdekében. A replikáció másolja az adatokat, akár az azonos adatközpont, akár egy második adatközpontja, attól függően, hogy melyik replikációs lehetőséget választja. A replikáció védi az adatokat, és megőrzi az alkalmazás up – időbeosztás megelőzve tranziens hardver hibák. Ha az adatok egy második adatközpontja van replikált, amely is megakadályozza, hogy az adatok egy elsődleges hely Katasztrofális hiba.

Replikációs biztosítja, hogy a tárterület-fiók megfelel-e a [Szolgáltatói szerződést (SLA) tárolására](https://azure.microsoft.com/support/legal/sla/storage/) is hibák szemben. Lásd: az Azure tároló információt SLA garantálja tartóssági és elérhetősége. 

Amikor létrehoz egy tárterület-fiókkal, a következő replikációs lehetőségek közül választhat:  

- [Helyi meghajtóra felesleges tároló (LRS)](#locally-redundant-storage)
- [Zóna felesleges tároló (ZRS)](#zone-redundant-storage)
- [GEO felesleges tároló (GRS)](#geo-redundant-storage)
- [Olvasás-elérés geo felesleges tároló (TS-GRS)](#read-access-geo-redundant-storage)

Olvasási-hozzáférés geo felesleges tároló (TS-GRS) az alapértelmezett beállítás esetén, hozzon létre egy új tárterület-fiókot.

A következő táblázat LRS, ZRS, GRS és TS-GRS, közötti különbségek gyors áttekintés közben a további szakaszok az a cím különböző típusú replikációs részletesebben.

| Replikációs stratégia                                                               | LRS | ZRS | GRS | TS-GRS |
|:----------------------------------------------------------------------------------|:---|:---|:---|:------|
| Adatok van replikált több adatközpontokkal keresztül.                                     | nem  | igen | igen | igen    |
| Adatok és a másodlagos helyről egyaránt az elsődleges helytől is olvashatók. | nem  | nem  | nem  | igen    |
| Adatok karbantartása külön csomóponton példányainak száma.                             | 3   | 3   | 6   | 6      |

Lásd: [Azure tároló árak](https://azure.microsoft.com/pricing/details/storage/) árinformációkat a különböző redundancia beállításokat.

>[AZURE.NOTE] Prémium tároló támogatja a felesleges csak helyileg tárolását (LRS). Prémium tároló kapcsolatos további tudnivalókért lásd [prémium tároló: Azure virtuális gép Munkaterhelésekből nagy teljesítményű tárterület](storage-premium-storage.md).

## <a name="locally-redundant-storage"></a>Helyi meghajtóra felesleges tárhely

Helyi meghajtóra felesleges tároló (LRS) replikálja háromszor belül egy tároló időosztás egységet a régió, amelyben a tárterület-fiók létrehozása egy adatközpontban üzemelteti, amely az adatok. Írási kérelem sikeresen függvény csak egyszer az összes három másolatnál íródott. Ezek a három replikák tárolnak külön hibafa tartományok és a frissítési tartományok belül egy tároló időosztás egységet. 

A tároló időosztás egységet állványon tároló csomópontok gyűjteménye. Hibafa tartományt (FD) csomópontot, amely hiba fizikai időegység jelenítik meg lehet tekinteni az azonos fizikai állvány tartozó csomópontok beállításcsoport. Egy frissítési (UD) tartománya egy csoportja közös frissített során a szolgáltatásfrissítés (bevezetési) csomópontot. A három replikák belül egy tároló időosztás egységet annak érdekében, hogy adatokat érhető el, ha a hardver hiba egyetlen állvány hatással van, vagy csomópontok során a bevezetés frissítésekor UDs és FDs helyezkednek el. 

LRS a legalacsonyabb költség lehetőséget, és legalább tartóssági és egyéb lehetőségek összehasonlítása nyújt. Adatközponthoz szintű katasztrófa (fire, amelyet stb) esetén minden három kópiák valószínűleg az elveszett vagy helyrehozhatatlan. A kockázat csökkentése érdekében Geo felesleges tároló (GRS) ajánlott legtöbb alkalmazással.

Helyi meghajtóra felesleges tároló továbbra is célszerű bizonyos esetekben: 

- Azure tároló replikációs beállításai legnagyobb maximális sávszélessége biztosít.

- Ha az alkalmazás, amely egyszerűen dokumentumoldalakra adatokat tárolja, akkor LRS az választhatnak.

- Egyes alkalmazások korlátozva adatreplikálás csak egy irányítási adatszolgáltatási miatt ország belül. Egy másik országban lehet párosított terület régió párban információt, lásd: [Azure régiók](https://azure.microsoft.com/regions/) .

## <a name="zone-redundant-storage"></a>Zóna felesleges tárhely

Zóna felesleges tároló (ZRS) az adatok aszinkron replikálja belül három kópiák LRS, így biztosítva LRS-nál magasabb tartóssági hasonló tárolásáról mellett egy vagy két régióban adatközpontokkal keresztül. ZRS tárolt adatok tartós, akkor is, ha az elsődleges adatközponthoz helyrehozhatatlan, vagy nem érhető el.
Azon ügyfelek, akik ZRS használni kívánt tartsa szem előtt kell lennie, amely: 

- ZRS lehetőség csak a blokk BLOB az általános célú tároló fiókok, és csak a tárhely szolgáltatás verziókkal 2014-02-14 támogatott és újabb verziók. 

- Mivel a aszinkron replikációs késleltetést, helyi katasztrófa esetén lehetőség, hogy a módosításokat, amelyeket van még nem lett replikált a másodlagos elvesznek az adatok nem állíthatók át az elsődleges.

- A replika nem feltétlenül vehető igénybe mindaddig, amíg a Microsoft a másodlagos áttérni kezdeményez.

- ZRS fiókok később LRS vagy GRS nem alakítható. Hasonlóan egy meglévő LRS vagy GRS fiókból nem alakítható ZRS-fiókjába.

- ZRS fiókok mértékek vagy a naplózási szolgáltatás nincs. 

## <a name="geo-redundant-storage"></a>GEO felesleges tárhely

GEO felesleges tároló (GRS) replikálja az adatok egy másodlagos terület, amely több száz mérföld az elsődleges régió hagyta. Ha a tárterület-fiók GRS engedélyezve van, az adatok még akkor is, ha egy teljes területi üzemszünetek vagy az elsődleges régió nem helyreállítható katasztrófa tartós.

Az engedélyezett GRS tároló fiókot frissítés először elkötelezte magát az elsődleges terület, ahol azt van replikált háromszor. Kattintson a frissítés van replikált aszinkron a másodlagos területére, ahol, akkor is replikált háromszor. 

Az elsődleges és másodlagos régiók GRS kópiák kezelése külön hibafa tartományok, és frissítése a tárhely időosztás egységet LRS leírtak tartományokat.

Szempontok

- Aszinkron replikációs késleltetést foglal magában, mivel területi katasztrófa esetén lehetőség, hogy a módosításokat, amelyeket van nem került még bele a másodlagos régió elvesznek az adatokat nem lehet visszaállítani, az elsődleges területről.

- A replika nem érhető el, kivéve, ha a Microsoft a másodlagos régió áttérni kezdeményez.

- Ha egy alkalmazást, olvassa el a másodlagos területről a felhasználó engedélyeznie kell a TS-GRS szeretné. 

Amikor létrehoz egy tárterület-fiókkal, jelölje ki az elsődleges régió, a fiókhoz. A másodlagos terület az elsődleges régió alapján lesz meghatározva, és nem módosíthatók. A következő táblázat mutatja az elsődleges és másodlagos régió párosítása.

| Elsődleges             | Másodlagos           |
|---------------------|---------------------|
| A központi Észak-amerikai    | A központi Dél-Amerikai Egyesült Államok    |
| A központi Dél-Amerikai Egyesült Államok    | A központi Észak-amerikai    |
| Kelet-Amerikai Egyesült Államok             | Nyugati Amerikai Egyesült Államok             |
| Nyugati Amerikai Egyesült Államok             | Kelet-Amerikai Egyesült Államok             |
| USA keleti 2           | A központi Amerikai Egyesült Államok          |
| A központi Amerikai Egyesült Államok          | USA keleti 2           |
| Észak-Európa        | Nyugati Európa         |
| Nyugati Európa         | Észak-Európa        |
| Dél-kelet-ázsiai     | Kelet-ázsiai           |
| Kelet-ázsiai           | Dél-kelet-ázsiai     |
| Kelet-kínai          | Észak-kínai         |
| Észak-kínai         | Kelet-kínai          |
| Japán keleti          | Japán nyugati          |
| Japán nyugati          | Japán keleti          |
| Brazília Dél        | A központi Dél-Amerikai Egyesült Államok    |
| Ausztrália keleti      | Ausztrália Könyvesbolt is. |
| Ausztrália Könyvesbolt is. | Ausztrália keleti      |
| India Dél         | India központi       |
| India központi       | India Dél         |
| Amerikai Egyesült Államok Gov Iowa         | Amerikai Egyesült Államok Gov Virginia     |
| Amerikai Egyesült Államok Gov Virginia     | Amerikai Egyesült Államok Gov Iowa         |
| Közép Kanadán      | Kanada-keleti          |
| Kanada-keleti         | Közép Kanadán      |
| Egyesült Királyság nyugati             | Egyesült Királyság Dél            |
| Egyesült Királyság Dél            | Egyesült Királyság nyugati             |
| Németország központi     | Németország élelmiszerbolt   |
| Németország élelmiszerbolt   | Németország központi     |
| Nyugati USA-beli 2           | Közép-nyugati USA-beli     |
| Közép-nyugati USA-beli     | Nyugati USA-beli 2           |

Azure által támogatott régiók információt olvassa el az [Azure régiók](https://azure.microsoft.com/regions/)című témakört.
 
## <a name="read-access-geo-redundant-storage"></a>Olvasás-elérés geo felesleges tárhely

Olvasás-elérés geo felesleges tároló (TS-GRS) teljes méretűre nagyítása elérhetőségét a tárhely fiók, hogy csak olvasható hozzáférhetővé tenné másodlagos hely, mellett a replikáció GRS által megadott két sorterület át az adatokat. 

Ha csak olvasási hozzáférést az adatok, a másodlagos régióban engedélyezésekor egy másodlagos végpontot, az elsődleges végpontot tároló fiókjának mellett a az adatok érhető el. A másodlagos végpont hasonlít az elsődleges végpontot, de a utótag `–secondary` a fiók nevére. Ha például a Blob-szolgáltatás az elsődleges végpont `myaccount.blob.core.windows.net`, majd a másodlagos végpont `myaccount-secondary.blob.core.windows.net`. A hívóbetűk tároló fiókjának megegyeznek az elsődleges és másodlagos végpontok.

Szempontok

- A alkalmazásnak melyik azt az interaktív használata, TS-GRS használatakor végpont kezeléséhez. 

- TS-GRS célokra magas rendelkezésre állás. Méretezhetőség útmutatást olvassa el a [teljesítménybeli ellenőrzőlistája](storage-performance-checklist.md).

## <a name="next-steps"></a>Következő lépések

- [Azure tároló árak](https://azure.microsoft.com/pricing/details/storage/)
- [Azure tároló fiókokról](storage-create-storage-account.md)
- [Azure tároló méretezhetőség és a teljesítmény cél](storage-scalability-targets.md)
- [Microsoft Azure tároló redundancia beállítások és olvasási hozzáférést Geo felesleges tárhely](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)  
- [SOSP papír - Azure tároló: Egy könnyen hozzáférhető felhőalapú Tárhelyszolgáltatáshoz az erős konzisztencia](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)  
