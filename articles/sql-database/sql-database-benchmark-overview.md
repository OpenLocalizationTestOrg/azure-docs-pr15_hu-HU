<properties
    pageTitle="Azure SQL-adatbázis javasolt – áttekintés"
    description="Ez a témakör használni a Azure SQL-adatbázis teljesítményének Azure SQL-adatbázis javasolt."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="06/21/2016"
    ms.author="carlrab" />

# <a name="azure-sql-database-benchmark-overview"></a>Azure SQL-adatbázis javasolt – áttekintés

## <a name="overview"></a>– Áttekintés
Microsoft Azure SQL-adatbázis kínál három [szolgáltatási rétegek](sql-database-service-tiers.md) többszintű teljesítményét. Minden egyes teljesítményszint növekvő erőforrások vagy a "power" egyre nagyobb átviteli továbbít biztosít.

Fontos, hogy tudja számszerűsítése: hogyan a az egyes teljesítményszint növekvő hatványon fordítja megnövelt adatbázis teljesítményét. Végezze el a Microsoft Azure SQL adatbázis javasolt (ASDB) van fejlesztett. A viszonyítási alap található összes OLTP munkaterhelésekből az alapműveletek a kettő vegyesen él. Az adatbázisok minden teljesítményszint futó elért átviteli mérjük azt.

Az erőforrások és a power minden szolgáltatás réteg és a teljesítmény szint kifejezhető [Adatbázis-tranzakción egységek (DTUs)](sql-database-technical-overview.md#understand-dtus). DTUs kínál a Processzor, a memóriahasználat, a kombinált mértéke alapján teljesítményszint relatív kapacitása ahhoz és és olvasási díjak minden teljesítményszint által kínált. A DTU értékelése az adatbázis cérnázó megfelel az adatbázis power cérnázó. A viszonyítási alap lehetővé teszi, hogy az egyes teljesítményszint nyújtotta gyakorló tényleges adatbázis-művelet közben a méretezés az adatbázis mérete, a felhasználók és az erőforrások az adatbázis megadott arányában tranzakció díjak száma szerint növekvő teljesítmény adatbázis teljesítményre gyakorolt hatásának us.

MAPI / órás, a tranzakciók perc segítségével szabványos szolgáltatás réteg és a támogatási szolgáltatás réteg tranzakciók másodpercenként, egyszerűbbé teszi a teljesítmény gyorsan kapcsolódik az egyes szolgáltatási réteg alkalmazás követelményeinek potenciális használatával alkalmazásával az egyszerű szolgáltatási réteg a kapacitásának kifejező.

## <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Javasolt eredmények használatával történik a valós adatbázis teljesítménye
Ha meg szeretné érteni, hogy ASDB, az összes szintek, például az képviselő és tájékoztató csak fontos. A tranzakciók díjak érhető el a javasolt alkalmazással nem lesznek megegyeznek, előfordulhat, hogy más alkalmazásokkal valósítható meg. A viszonyítási alap tartalmazó táblázatok és adattípusok adattartomány séma futtassanak típusai eltérő tranzakció gyűjteménye magába foglalja. Miközben a viszonyítási alap él az azonos alapműveletek közös összes OLTP munkaterhelésekből, nem jelent bármilyen adatbázis vagy alkalmazás adott osztály. A viszonyítási alap az a célja, hogy adja meg a várható felfelé vagy lefelé méretezésekor közötti teljesítményszint adatbázisok relatív teljesítménye lehetővé teszi útmutatóját. A valóságban adatbázisok különböző méretű és összetettsége, munkaterhelésekből különböző keverékei találkozik, és különböző módokon válaszol. Például IO igénylő alkalmazás hamarabb lehet találati IO küszöbérték, vagy egy Processzor-igényes alkalmazás hamarabb lehet találati Processzor korlátai. Nem biztos, hogy bármilyen adott adatbázist ugyanúgy, mint a viszonyítási alap betöltés növelésével fog méretezése nem.

A viszonyítási alap és annak módszertan részletesebben az alábbi témakörben olvashat.

## <a name="benchmark-summary"></a>Javasolt összegzése
ASDB méri kombinálja a egyszerű adatbázis-műveletek az online tranzakció (OLTP) munkaterhelésekből feldolgozása leggyakrabban előforduló teljesítményét. A viszonyítási alap lett tervezve a felhőben szem előtt, az adatbázissémát, az adatok statisztikai sokaság számítások és tranzakciók kell a leggyakrabban használt OLTP munkaterhelésekből alapelemek szélesebb körben képviselő verziójához.

## <a name="schema"></a>Séma
A séma célja, hogy elég különböző és összetettsége műveletek széles köre támogatása. A viszonyítási alap fut, hat táblák áll egy adatbázisban. A táblák három kategóriába sorolhatók: rögzített méretű, méretezése és növekedését. Két rögzített méretű táblát; három méretezési tábla; és egy növekvő táblából. A sorok állandó rögzített méretű táblák van. Méretezési egy hivatkozás, az adatbázisok teljesítményének arányos, de nem változtatja meg a viszonyítási alap során rendelkeznek. A növekvő táblázat van méretű, például egy méretezési táblázat első betöltése, de a hivatkozás módosításokat a viszonyítási alap futtatása a sorok beszúrt és törlése során.

A séma tartalmaz kombinálja a adattípusokat, például az egész szám, karakter és dátum/idő. A séma tartalmaz, az elsődleges és másodlagos kulcsok, de nem minden idegen kulcsok – Ez azt jelenti, hogy nincsenek nincs a hivatkozási integritás kényszerek táblák között.

Adatok generációs program az adatokat a kezdeti adatbázist hoz létre. Az egész és numerikus adatokat a különböző stratégiák jön létre. Egyes esetekben az értékek eloszlása véletlenszerűen vonatkozóan. Egyéb esetben értékhalmaz van véletlenszerűen a(z) annak érdekében, hogy egy adott terjesztési karbantartása. Szövegmezők szavak reálisan megjelenésű adatok megadására a súlyozott listájából jönnek létre.

Az adatbázis melynek a mérete megegyezik egy "skála tényező." alapján A nagyítási érték (KB rövidítése) határozza meg, hogy a hivatkozás a méretezés és táblák növő. Szakaszban leírt módon alatt a felhasználók és Pacing, az adatbázis mérete felhasználószám, maximális teljesítmény összes méretezni egymással arányos.

## <a name="transactions"></a>Tranzakciók
A terhelést a kilenc tranzakció típus, áll, az alábbi táblázatban látható módon. Jelölje ki az adatbázis motor és a rendszer a hardver és a többi tranzakciók kontrasztos rendszer jellemzők meghatározott csoportját készült tranzakciók. Ezt a megközelítést megkönnyíti a más összetevőket általános teljesítményére hatásának. A "Olvasás nehéz" tranzakció például nagyszámú lemezről olvasási műveletek hoz létre.

| Tranzakció típusa | Leírás |
|---|---|
| Olvassa el a Lite | JELÖLJE KI; a memóriában; csak olvasható |
| Olvasás közepes | JELÖLJE KI; többnyire a memóriában; csak olvasható |
| Olvasás nehéz | JELÖLJE KI; nem főleg a memóriában; csak olvasható |
| A frissítés Lite | FRISSÍTÉS; a memóriában; írási és olvasási |
| Vastag frissítése | FRISSÍTÉS; nem főleg a memóriában; írási és olvasási |
| A Beszúrás Lite | BESZÚRÁS; a memóriában; írási és olvasási |
| Vastag beszúrása | BESZÚRÁS; nem főleg a memóriában; írási és olvasási |
| Törlése | TÖRLÉS; kombinálja a memóriában és nem a memóriában; írási és olvasási |
| Processzor nehéz | JELÖLJE KI; a memóriában; az általában viszonylag nagy Processzor terhelés; csak olvasható |

## <a name="workload-mix"></a>Mix terhelést
Tranzakciók véletlenszerűen kiválasztott az alábbi általános kombinálja a súlyozott eloszlás. Az általános mix körülbelül 2:1 olvasási/írási arány tartalmaz.

| Tranzakció típusa | Kombinálja a % |
|---|---|
| Olvassa el a Lite | 35-tel |
| Olvasás közepes | 20 |
| Olvasás nehéz | 5 |
| A frissítés Lite | 20 |
| Vastag frissítése | 3 |
| A Beszúrás Lite | 3 |
| Vastag beszúrása | 2 |
| Törlése | 2 |
| Processzor nehéz | 10 |

## <a name="users-and-pacing"></a>Felhasználók és pacing
A javasolt terhelést hajtja egy eszköz, amely a tranzakciók elküldi a vezérlő viselkedése egyidejű felhasználóját szám hasonlóan kapcsolatokat halmazának keresztül. Habár minden kapcsolatok és tranzakciók generált gépi, az egyszerűség hivatkozunk ezek a kapcsolatok felhasználóként"." Minden felhasználó a többi felhasználó függetlenül működik, de az összes felhasználó végezze el az azonos ciklus lépést alább látható módon:

1. Egy adatbázis-kapcsolatot létrehozni.
2. Ismételje, amíg a való kilépéshez adatbitjei:
    - Jelölje ki a tranzakciók (véletlenszerűen súlyozott eloszlása).
    - A kijelölt tranzakció végrehajtása, és a válaszidő mérjük.
    - Várja meg pacing késleltetést.
3. Zárja be az adatbázis-kapcsolatot.
4. Kilépés parancsot.

Véletlenszerűen van-e jelölve a pacing késése (lépés: 2c), de egy eloszláshoz, amelynek 1,0 második átlaga. Ily módon minden olyan felhasználóhoz, átlagosan miatt másodpercenként legfeljebb egy tranzakció.

## <a name="scaling-rules"></a>Méretezési szabályok
A felhasználóknak a száma az adatbázis mérete (skála varianciaanalízis egységekben) határozza meg. Egyetlen felhasználó minden öt skála varianciaanalízis egységek van. A pacing késleltetés miatt egyetlen felhasználó hozhat létre, másodpercenként legfeljebb egy tranzakció átlagosan.

Ha például egy skála varianciaanalízis 500 (KB = 500) adatbázis lesz 100 felhasználók és a maximális sebesség TP 100-k érhet el. Nagyobb TP-k meghajtóra ráta további felhasználók és a nagyobb adatbázis van szükség.

Az alábbi táblázatban látható a ténylegesen fenntartható szolgáltatás réteg és a teljesítmény szintenként felhasználók számát.

| Szolgáltatási réteg (teljesítményszint) | Felhasználók | Az adatbázis mérete |
|---|---|---|
| Egyszerű | 5 | 720 MB |
| A standard (S0) | 10 | 1 GB |
| A standard (S1) | 20 | 2.1 GB |
| A standard (S2) | 50 | 7.1 GB |
| Prémium (P1) | 100 | 14 GB |
| Prémium (P2) | 200 | 28 GB |
| Prémium (P6/P3) | 800 | 114 GB |

## <a name="measurement-duration"></a>A mérési időtartam
Egy érvényes javasolt futtatásához szükséges legalább egy órával a munkafolyamat állandó mérési időtartama.

## <a name="metrics"></a>Mértékek
A fő mérési módja miatt a viszonyítási alap hitelesítettnek átviteli és választ.

- Átviteli a viszonyítási alap az alapvető teljesítmény méri felel meg. Átviteli jelenteni az egységnyi-a-idő alatt, diagramtípusokat tranzakció számlálás tranzakciók.
- Válaszidő azt méri, teljesítmény előreláthatóság. A válasz időkorlát szolgáltatás a szolgáltatást a szigorúbb válasz idő követelmény alább látható módon magasabb kategóriái osztálya művelettől.

| A szolgáltatás osztály  | Átviteli mérték | Válasz idő követelmény |
|---|---|---|
| Támogatás | MAPI / szekundum | a 0,5 másodperc 95 PERCENTILIS |
| Normál | A tranzakciók percenként | a 1,0 másodperc 90 PERCENTILIS |
| Egyszerű | MAPI / óra | a 2.0-s másodperc 80 PERCENTILIS |

## <a name="conclusion"></a>Elfogadásáról
Az Azure SQL-adatbázis javasolt méri Azure SQL-adatbázis fut végig az elérhető szolgáltatási rétegek és teljesítményszint köre relatív teljesítményét. A viszonyítási alap kombinálja a egyszerű adatbázis-műveletek az online tranzakció (OLTP) munkaterhelésekből feldolgozása leggyakrabban előforduló él. Tényleges teljesítmény mérése, a viszonyítási alap Ez a témakör a milyen következményekkel alkalmasabb értékelése átviteli módosítása a teljesítményszint, mint az erőforrások, például a Processzor sebességét, memóriaméret és IOPS szintenként által biztosított csak felsorolásával lehetőség. A jövőben is a viszonyítási alap kiszélesítése hatókörét, és bontsa ki a megadott adatok alapkoncepciójára folytatjuk.

## <a name="resources"></a>Erőforrások
[SQL-adatbázis – bevezetés](sql-database-technical-overview.md)

[Szolgáltatási rétegek és a teljesítmény szintek](sql-database-service-tiers.md)

[Útmutató a teljesítmény egyetlen adatbázisok](sql-database-performance-guidance.md)
