<properties
    pageTitle="Válassza a Termékváltozatot vagy réteg árak Azure keresés |} Microsoft Azure"
    description="Azure keresés a következő termékváltozatok kiépítése: szabad, Basic és a normál, ahol Standard érhető el különböző erőforrás-beállításokat, és a kapacitás szintek."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="10/24/2016"
    ms.author="heidist"/>

# <a name="choose-a-sku-or-pricing-tier-for-azure-search"></a>Válassza a Termékváltozatot vagy réteg árak az Azure-keresés

A [szolgáltatás kiépítve](search-create-service-portal.md) egy adott árak réteg vagy Termékváltozat Azure keresés. A választható lehetőségek **ingyenes**, **egyszerű**vagy **szabványos**, ahol **szabványos** érhető el beállításokat és a kapacitás. 

Ez a cikk célja, válassza az egy réteg nyújt segítséget. Ha az egy réteg kapacitás kell túl halk azzal kikapcsolja, szüksége lesz a magasabb réteg az új szolgáltatás kiépítése, és töltse be újra az indexek. Nincs helybeni egy Termékváltozat a a másikra egyazon szolgáltatás nem. 

> [AZURE.NOTE] Után az egy réteg és a [keresési szolgáltatás kiépítése](search-create-service-portal.md)lehetőséget választja, megnövelheti a replika, és partíciót számolja meg a szolgáltatás belül. Olvassa el [a lekérdezés és az indexelő munkaterhelésekből skála erőforrásszintek](search-capacity-planning.md).

## <a name="how-to-approach-a-pricing-tier-decision"></a>Hogyan megközelítés árak réteg döntés

Azure keresés a réteg beosztását, nem szolgáltatások elérhetősége határozza meg. Általánosságban elmondható szolgáltatások érhetők el a Minden réteg előzetes verzió szolgáltatásainak. Kivétel rendszer S3 HD indexelő nem támogatja.

> [AZURE.TIP] Javasoljuk, hogy mindig (egy előfizetésenként lejárat nélkül) **ingyenes** szolgáltatás kiépítése, hogy az erőforrásszintek egyszerűsített projektekhez elérhető. Tesztelés és értékelést; az **ingyenes** szolgáltatás segítségével a gyártási vagy nagyobb próba-munkaterhelésekből **egyszerű** vagy **szabványos** réteg második számlázható szolgáltatás hozzon létre.

Beosztását, és a szolgáltatást futtató költségeinek nyissa meg az aktuális kéz. Az információk ebben a cikkben könnyebben eldöntheti, amely Termékváltozat kézbesíti a megfelelő egyenleg, de bármely akkor hasznos lehet, szükség legalább durva becslést ad a következő:

- Szám és elkészítendő indexek mérete
- Szám és a méret a dokumentumok feltöltése
- Néhány arról lekérdezés kötet, értelmez lekérdezések egy második (QPS)

Szám és a méret fontosak, mert nehezen korlátozott indexek vagy a szolgáltatás a dokumentumok számát, vagy a szolgáltatás által használt erőforrások (tárhely vagy kópiák) keresztül elérésekor maximális számával. A tényleges a szolgáltatásbeli korlát attól másolatot kap használt: erőforrások vagy objektumot.

Az aktuális becslése az alábbi lépéseket kell egyszerűbbé:

- **Lépés: 1** Tekintse át az alábbi, ha többet szeretne tudni a rendelkezésre álló beállítások Termékváltozat leírások.
- **Lépés: 2** Előzetes döntés megérkeznek az alábbi kérdésekre.
- **3 lépés** Ön döntése véglegesítéséhez tároló kemény korlátairól megtekintésével és árak.

## <a name="sku-descriptions"></a>Termékváltozat ismertetése

Az alábbi táblázat az egyes szint leírását tartalmazza. 

Réteg|Elsődleges felhasználási területei
----|-----------------
**Ingyenes**|Ingyenes, kiértékelés, a vizsgálat vagy a kis munkaterhelésekből használt megosztott szolgáltatás. Más előfizetők megosztja, mert a lekérdezési teljesítmény és indexelés alapján ki más használja a szolgáltatás változik. Kis kapacitása (50 MB-os és 10 000 felfelé 3 indexek dokumentumok egyes).
**Egyszerű**|A hardveres munkaterhelésekből kis termelési. Nagyon érhető el. Legfeljebb 3 replikák és 1 partition (2 GB) kapacitása.
**S1**|Szabványos 1 támogatja a partíciók (12) és kópiák (12), a hardveres közepes gyártási munkaterhelésekből használható rugalmas kombinációi. Akkor is lefoglalhat partíciók és a 36 számlázható keresési egységek maximális száma által támogatott összes lehetséges kombinációinak kópiájába. Ez az engedélyszint a partíciók 25 GB pedig QPS másodpercenként körülbelül 15 lekérdezések.
**S2**|Szabványos 2 használja ugyanazt a 36 keresési mértékegységet S1, nagyobb méretű partíciót és kópiák nagyobb gyártási munkaterhelésekből fut. Ez az engedélyszint a partíciók 100 GB pedig QPS másodpercenként körülbelül 60 lekérdezések.
**S3**|Szabványos 3 fut arányosan nagyobb gyártási munkaterhelésekből magasabb vége rendszeren maximum 12 partíciót vagy 12 kópiák konfigurációit a 36 keresési egységek. Ez az engedélyszint a partíciók 200 GB pedig QPS másodpercenként több mint 60 lekérdezések. 
**HD-MINŐSÉGŰ S3**|Szabványos 3 magas sűrűség kisebb indexek nagyszámú készült. 200 GB legfeljebb 3 partíciók, is. QPS másodpercenként több mint 60 lekérdezések. 

> [AZURE.NOTE] Replika és partíciót méretkorlát ki egységként keresés (36 egység maximális érték szolgáltatás), amely díjat hatékony az alsó_határ névértékű a Mi a maximális azt jelenti, mint a számlát kapni a vannak. Például a maximális 12 kópiák használatához tartozhat legfeljebb 3 partíciót (12 * 3 = 36 egységek). Hasonlóképpen maximális partíciót használatához csökkentése kópiák 3. A megengedett kombinációk diagram [skála erőforrásszintek lekérdezés és Azure keresése az indexelő munkaterhelésekből](search-capacity-planning.md) talál.

## <a name="review-limits-per-tier"></a>Tekintse át az egy réteg korlátai

A következő diagramon része a [Szolgáltatás-korlátozások az Azure keresési](search-limits-quotas-capacity.md)essen. A nagy valószínűséggel hatással lehet a Termékváltozat döntés tényezők jeleníti meg. Is hivatkozhat a diagramot, ha az alábbi kérdésekre áttekintése.

Erőforrás|Ingyenes|Egyszerű|S1|S2|S3 |S3 HD
---|---|---|---|----|---|----
Szolgáltatásiszint-szerződés (SLA)|<sup>1</sup> |igen |igen  |igen |igen  |igen 
Index korlátai|3|5|50|200|200|1000 <sup>2</sup>
Dokumentum korlátai|10 000 összesen|1 millió szolgáltatás|partition 15 millió |partition 60 millió|partition millió 120-ra |index 1 millió
Maximális partíciót|A #HIÁNYZIK |1 |12  |12 |12|3 <sup>2</sup>
Partition mérete|A teljes 50 MB|2 GB / szolgáltatás|Partition / 25 GB |Partition (legfeljebb 1.2-es TB szolgáltatás használati) / 100 GB|200 GB / partíciót (legfeljebb 2.4 TB per service)|200 GB (legfeljebb 600 GB / service)
Maximális kópiák|A #HIÁNYZIK |3 |12 |12 |12|12
Lekérdezések / szekundum|A #HIÁNYZIK|~ replika / 3|~ 15 per replika|~ egy replika 60|> egy replika 60|> egy replika 60

<sup>1</sup> ingyenes, a betekintő termékváltozatok nem jár az SLA. SLA akkor lép életbe, amikor Termékváltozatot általánosan elérhetővé válik.

<sup>2</sup> S3 és HD-minőségű S3 is szeretne biztonsági másolatot azonos nagy kapacitású infrastruktúra szerint, de a maximális érték egyenként eléri a különböző módokon. S3 célként nagyon nagy indexek kisebb számot. Például a maximális érték-e erőforrás kötött (az egyes szolgáltatásokhoz 2.4 TB). S3 HD nagyon kicsi indexek nagyszámú célként. Az indexek 1000 S3 HD eléri a korlátok index kényszerek formájában. Ha Ön az S3 HD ügyfele, akinek van szüksége a 1000-nél több indexek, lépjen kapcsolatba a Microsoft Support kapcsolatos tudnivalókat a folytatáshoz.

## <a name="eliminate-skus-that-dont-meet-requirements"></a>Termékváltozatban, amely nem felel meg a követelményeknek megszüntetése 

A következő kérdések segíthetnek a megfelelő Termékváltozat döntés a terhelést a érkeznek meg.

1. **Szolgáltatási szerződés SZOLGÁLTATÁSISZINT-** követelményeik vannak? Ha szűkíteni szeretné a Termékváltozat döntés egyszerű vagy nem előzetes szabványos.
2. **Hány indexek** végezze el szükség? A legfontosabb változók faktoring Termékváltozat döntés az egyik az egyes Termékváltozat által támogatott indexek száma. Tárgymutató-támogatás az alsó árak rétegek jelentősen különböző szinteken van. Indexek vonatkozó követelmények lehet egy elsődleges determinánsa Termékváltozat döntés.
3. **Hány dokumentumok** töltődik be minden index? A szám és a dokumentumok mérete határozza meg az esetleges az index méretét. Feltételezve, hogy a is becsüljük meg az index tervezett méretét, összehasonlíthatja ellen Termékváltozat, a megadott méretű index tárolásához szükséges partíciók száma meghosszabbított partíciót méretet telefonszámát. 
4. **Mi az a várt lekérdezés betöltés**? Miután tárhelyre alatt érteni, fontolja meg a lekérdezés munkaterhelésekből. S2 és mindkét S3 termékváltozatok kínálnak közelében egyenértékű átviteli, de SLA követelmények fog szabály minden termékváltozatban előzetes. 
5. Ha tervezi, hogy a S2 vagy S3 réteg, állapítsa meg, hogy szükség van-e [Indexelő](search-indexer-overview.md). Indexelő még nem érhetők el a S3 HD réteg számára. Alternatív megközelítés, használata leküldéses modell index frissítésekhez, ahol írhat alkalmazás kód egy adathalmaz leküldéses az index.

A legtöbb ügyfelek szabály is vagy kicsinyítés egy adott Termékváltozat azok a fenti kérdések megválaszolása alapján. Ha még nem arról, hogy melyik Termékváltozat válassza, az MSDN webhelyen vagy StackOverflow fórumok kérdéseket tehet közzé, vagy Azure ügyfélszolgálatát további útmutatásért.

## <a name="decision-validation-does-the-sku-offer-sufficient-storage-and-qps"></a>Adatérvényesítési határozattal: a Termékváltozat elegendő tárolására és QPS ajánlja fel?

Utolsó lépésként látogatnia [árak, oldal](https://azure.microsoft.com/pricing/details/search/) és a [szolgáltatás és a tárgymutató szakaszok szolgáltatás korlátok](search-limits-quotas-capacity.md) ellenőrizze az előfizetést és a szolgáltatás határértékek a becsült. 

Ha ár vagy tároló követelményei kívül esik, érdemes lehet (például) refactor több kisebb szolgáltatások munkaterhelésének. A további alapszinten, sikerült újratervezéséhez kisebb lesz az indexek vagy lekérdezések hatékonyabbá teheti a szűrők használatával.

> [AZURE.NOTE] Tárhelyre túlságosan felfújt lehet, ha a dokumentumok felesleges adatokat tartalmaznak. Dokumentumok ideális esetben csak a kereshető adatok vagy a metaadatok tartalmaznak. Bináris adatokat nem kereshető és kell tárolni külön-külön (talán az Azure táblázat vagy blob-tároló) a hivatkozás URL-CÍMÉT a külső adatok tárolására tárgymutató mezővel. Egy adott dokumentumot maximális mérete 16 MB (vagy ha kevesebb egy összehívásban több dokumentum feltöltése tömeges). [Szolgáltatás-korlátozások az Azure keresési](search-limits-quotas-capacity.md) további információt talál.

## <a name="next-step"></a>Következő lépés

Ha már elsajátította, amely Termékváltozat a jobbra igazítás, folytassa a ezeket a lépéseket:

- [Hozzon létre egy keresési szolgáltatás a portálon](search-create-service-portal.md)
- [A partíciók és kópiák, ha át kívánja méretezni a szolgáltatás a felosztás módosítása](search-capacity-planning.md)

