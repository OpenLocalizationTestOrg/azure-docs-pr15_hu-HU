<properties
   pageTitle="Adatok raktári terhelés"
   description="SQL adatok raktári rugalmasság lehetővé teszi a nagyobb, a kisebb, vagy mutasson a számítási power csúszó skálával adatok raktári egységek (DWUs). Ez a cikk ismerteti, hogy az adatok raktári mértékek, és hogyan DWUs vonatkoznak. "
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
   ms.date="07/25/2016"
   ms.author="barbkess;mausher;jrj;sonyama"/>


# <a name="data-warehouse-workload"></a>Adatok raktári terhelés
Adatok raktári terhelési a műveleteket, amelyek egy adatraktár elleni transpire hivatkozik. Az adatok raktári terhelés lefedi a raktári az adatok betöltése, elemzése és a adatraktár jelentések, a adatraktár lévő adatok kezelése és adatok exportálása a adatraktár a teljes folyamatát. A magassági és az összetevők szélessége gyakran történik a lejárat szintjének a adatraktár.


## <a name="new-to-data-warehousing"></a>Új adatok raktározással?
Egy adatraktár betöltött egy vagy több adatforrásból, és használható például jelentése és adatok elemzése üzletiintelligencia-feladatok elvégzéséhez adatokat gyűjteménye.

Adatraktárakhoz jellemzi lekérdezést, amely nagyobb számok a sorok, nagy cellatartományok adatok beolvasása és esetleg a viszonylag nagy eredményeket elemzési és jelentéskészítési célra. Adatraktárakhoz is jellemzi viszonylag nagy adatok terhelések és a kis tranzakció szintű beszúrása és frissítések/törlése.

- Egy adatraktár leghatékonyabb, amikor az adatok tárolása oly módon, hogy a lekérdezéseket, nagy számú sort és nagy cellatartományok adatok beolvasása kell optimalizálja. A következő típusú végzett vizsgálatot akkor működik a legjobban, ha az adatokat tárolja, és szeretne keresni, oszlopok helyett sorok szerint.

>[AZURE.NOTE] Oszlop tárterületet használ, a memóriában columnstore index nyújt a legfeljebb 10 x tömörítés nyereség és a lekérdezési teljesítmény nyereség x 100 bináris fák hagyományos fölé jelentése és analytics lekérdezések. A standard tárolásához és a beolvasás egy adatraktár nagy adatainak columnstore indexek tekinti azt.

- Egy adatraktár (OLTP) feldolgozása online tranzakció optimalizálja a rendszer mint eltérő követelményei vannak. A OLTP rendszer számos beszúrása, frissítése és törlése a műveletek. Ezek a műveletek válassza ki azt a táblázat sorait. Táblázat kér legjobb végre, ha az adatok tárolása sorról sorra módon. Az adatok rendezhetők és gyorsan szeretne keresni az osztás és meghódítása bináris fa vagy btree keresési nevű megközelítés.


## <a name="data-loading"></a>Adatok betöltése
Adatok betöltése az adatok raktári terhelés nagy része. Vállalkozások általában van rájuk a csoporttal, amikor ügyfelektől üzleti tranzakciók állapotváltozásait OLTP rendszer elfoglalt. Rendszeres időközönként gyakran éjjel a karbantartási időszakokban, a tranzakciók vagy áthelyezése a adatraktár másolt. Miután az adatok az a adatraktár, munkatársai elemzést végezhet, és az üzleti döntéseket az adatok.

- Régebben betöltésének folyamatát meghívott ETL venni, az átalakítás és a betöltés. Adatok általában így egységes más adatokkal, a adatraktár az átalakítandó van szüksége. Korábban vállalkozások átalakítások végrehajtásához használt dedikált ETL kiszolgálókhoz. Most az ilyen gyors nagymértékben párhuzamos feldolgozási is adatok első betöltése SQL adatraktár, és hajtsa végre az átalakítás. Ez a folyamat nevezik kivonat betöltése és átalakítása (ELT), és az adatok raktári terhelés új szabvány egyre.

> [AZURE.NOTE] Az SQL Server 2016, most végezheti el a analytics-OLTP táblán valós időben. Ez nem váltja fel egy adatraktár tárolására és adatok elemzése van szükség, de azt kínál elemzést végezhet a lapra.

### <a name="reporting-and-analysis-queries"></a>Jelentési és elemzési lekérdezések
Jelentési és elemzési lekérdezések gyakran kategóriába be kicsi, közepes és több feltétel alapján nagy, de a idő alapján általában. A legtöbb adatok raktáron, belüli van egy gyors futó hosszan futó lekérdezések és a vegyes terhelést. Minden esetben fontos határozza meg a mix és a gyakoriság meghatározása (óránkénti, naponta, hó végi, negyedév végi, és így tovább). Ha meg szeretné érteni, hogy a lekérdezés vegyes terhelést párosított feldolgozási, fontos a megfelelő kapacitással egy adatraktár megtervezése érdeklődő.

- Kapacitás tervezés lehet egy vegyes lekérdezés terhelést bonyolult feladat, különösen akkor, ha egy hosszú átvezetési idő kapacitás hozzáadása a adatraktár van szüksége. SQL adatraktár eltávolítja a sürgős kapacitás tervezés, mivel a nagyobb és a számítási kapacitás zsugorítása, bármikor, és mivel tárhely és a számítási kapacitás független mérete.

### <a name="data-management"></a>Adatok kezelése
Adatok kezelése fontos, különösen akkor, ha tudja, hogy akkor lehet, hogy fogyni lemezterületet a közeljövőben. Adatraktárakhoz általában az adatok osztása értelmes tartományok, partíciót egy táblázatban, amelyeket tárolni. Az SQL Server-alapú összes termék adhatja meg a partíciók és kijelentkezés a táblázat áthelyezése. Váltás lehetővé teszi, hogy ezt a partíciót, régebbi-adatok áthelyezése kevésbé drága tárhely, és elérhető a legfrissebb adatok tartsa a online tárhely.

- Indexek Columnstore particionált táblák támogatja. Az indexek columnstore particionált táblák adatkezelés és az archiválás használják. Táblázatok tárolt--soronként nagyobb szerepkörbe partíciót a lekérdezési teljesítmény rendelkezik.  

- PolyBase fontos szerepet tölt be adatok kezelése. Ha PolyBase használ, a Hadoop vagy Azure blob-tárolóhoz régebbi adatok archiválása.  Ez számos lehetőség biztosít, mivel az adatok még online.  Adatok beolvasása Hadoop tovább tarthat, de a lekérés idő útján előfordulhat, hogy lényegesebbnek a tárhely költséget.

### <a name="exporting-data"></a>Adatok exportálása
Jelentések és elemzéshez elérhetővé adatok egy úgy, hogy adatokat küld a adatraktár a kiszolgálók dedikált futtatásához a jelentések és elemzés. Ezek a kiszolgálókat adatok marts nevezik. Például tudott előre a jelentés adatai feldolgozása, és majd exportálása azt a adatraktár szeretné körben elérhetővé tenni a vevők és a munkatársai számára világszerte sok kiszolgálón.

- Jelentések létrehozása az egyes éjszakai sikerült elhelyezte csak olvasható a napi adatai pillanatképét jelentéskészítő kiszolgálókon. Ezzel a módszerrel nagyobb sávszélességre ügyfeleknek közben a számítási erőforrásigények a adatraktár a csökkentése. A biztonsági méretarány az adatok marts teszi lehetővé a felhasználók, akik rendelkeznek hozzáféréssel a adatraktár számának csökkentése érdekében.
- Elemzéséhez is vagy a adatraktár analysis kockában összeállítása és futtatni az elemzést szemben a adatraktár vagy előre adatfeldolgozás és elemzéséhez további elemzés kiszolgálóra exportálhatja.

## <a name="next-steps"></a>Következő lépések
Most egy kicsit tudni SQL adatraktár, ismerkedjen meg gyorsan a [Hozzon létre egy SQL adatraktár][] , és [Töltse be a mintaadatokat][].

<!--Image references-->

<!--Article references-->
[Példaadatok betöltése]: ./sql-data-warehouse-load-sample-databases.md
[Hozzon létre egy SQL adatraktár]: ./sql-data-warehouse-get-started-provision.md

<!--MSDN references-->

<!--Other web references-->
