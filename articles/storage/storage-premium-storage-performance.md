<properties
    pageTitle="Azure prémium tárterület: Tervezése a teljesítmény elérése érdekében |} Microsoft Azure"
    description="Nagy teljesítményű alkalmazások Azure prémium tárolót használó tervezéséről. Prémium tároló nagy teljesítményű, alacsony-késés lemezt támogatása a Azure virtuális gépeken futó futó lehet/O-igényes munkaterhelésekből kínál."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn" />

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="aungoo"/>

# <a name="azure-premium-storage-design-for-high-performance"></a>Azure prémium tárterület: Nagy teljesítményű megtervezése

## <a name="overview"></a>– Áttekintés  
Ez a cikk Azure prémium tárolót használó nagy teljesítményű alkalmazások létrehozásába nyújt útmutatást. A megjelenő utasításokat a dokumentumban, beleszámítva a teljesítmény gyakorlati tanácsok az alkalmazás által használt technológiák alkalmazandó is használhatja. Az útmutatásokat ábrázolására, e dokumentumban példaként prémium tároló futó SQL Server használta azt.

Miközben azt helyzetekben teljesítmény ebben a cikkben a tárhely réteg, szüksége lesz az alkalmazási réteg optimalizálása. Ha például Azure prémium tárterület a SharePoint-Farm üzemeltet, az SQL Server példát tartalmaz az Ez a cikk a optimalizálhatja az adatbázis-kiszolgáló is használhatja. Ezenkívül optimalizálhatja a SharePoint-Farm webkiszolgáló és alkalmazáskiszolgáló számára beolvasása a legtöbb teljesítményét.

Ez a cikk segít a következő alkalmazás teljesítményének Azure prémium tárolón optimalizálására kapcsolatos gyakori kérdésekre választ

-   Mérje le az alkalmazás teljesítményének hogyan?  
-   Miért van, nem jelennek meg várható nagy teljesítményű?  
-   Milyen tényezőket befolyásolhatja az alkalmazás teljesítményének prémium tárolón?  
-   Hogyan hajtsa végre ezeket tényezők befolyásolják az alkalmazás prémium tárolón teljesítményét?  
-   Hogyan tudja optimalizálni a IOPS, a sávszélesség és a késés?  

Azt módon biztosítva van a következő irányelvek kifejezetten prémium tároló mert prémium tárolón futó feladatok erősen bizalmas teljesítményét. Példák nyújtott szükség. Is alkalmazhat néhány ezek útmutatások a szabványos tároló lemezre IaaS VMs futó alkalmazások.

Mielőtt elkezdené, ha Ön új prémium tárolóhoz, akkor először olvassa el a [prémium tároló: Azure virtuális gép Munkaterhelésekből nagy teljesítményű tárterület](storage-premium-storage.md) cikk és [Azure prémium tároló méretezhetőség és a teljesítmény célokat](storage-scalability-targets.md#premium-storage-accounts).

## <a name="application-performance-indicators"></a>Alkalmazás eredmények mutatói  
Azt mérje fel, hogy az alkalmazás végrehajtásához jól vagy nem használja a teljesítmény kijelzők, mint hogy milyen gyorsan az alkalmazás a felhasználó kérésére, hogy mennyi adatot, az alkalmazások által feldolgozott egy kérelemre feldolgozása, hány kéri az alkalmazás feldolgozás az egy adott időszakra vonatkozóan, hogy mennyi ideig tartott a felhasználó rendelkezik, hogy csak azok a kérelem elküldése után kiváltása. A következő teljesítménymutatók technikai használt, IOPS, a átviteli vagy a sávszélesség és a késés.

Ebben a részben fog témákat ölelik fel a közös teljesítménymutatók prémium tároló környezetében. Az alábbi szakaszban gyűjtése alkalmazás követelményeknek, megtanulhatja, hogyan felmérni, ezek teljesítménymutatók az alkalmazás. Alkalmazás teljesítményének optimalizálása a megtanulhatja, azokkal a tényezőkkel, ezek teljesítménymutatók és javaslatok történő optimalizálásához őket érintő.

## <a name="iops"></a>IOPS  
IOPS szám, hogy az alkalmazás a tárhely lemezre küld egy második kérelmek. Bemeneti és kimeneti művelet olvasható vagy írása, egymás után következő véletlen. OLTP-alkalmazásokban, például egy online kiskereskedelem webhelyet kell azonnal sok egyidejű felhasználói összehívások. A felhasználói kérések beszúrása és frissítése intenzív adatbázis-tranzakciók, amely az alkalmazás gyorsan fel kell dolgoznia. Ezért OLTP alkalmazásoknak nagyon magas IOPS. Az ilyen alkalmazások kezelheti a kis- és véletlen IO kérések milliónyi. Ha például egy alkalmazást, az alkalmazás infrastruktúra való optimalizálásáról IOPS kell tervezésekor. *Alkalmazás teljesítményének optimalizálása*későbbi szakaszában ölelik részletesen a magas IOPS megszerezni figyelembe kell vennie tényezőket.

Ha csatol prémium tároló lemezen a magas méretarányra virtuális, Azure rendelkezések IOPS garantált száma szerint a merevlemez specifikációja meg. Ha például P30 lemezen kiépítése 5000 IOPS. Minden magas skála virtuális méretét egy adott IOPS korlátozást azt fenntartása is tartalmaz. Például a szokásos GS5 virtuális tartalmaz 80,000 IOPS korlátozza.

## <a name="throughput"></a>Átviteli  
Átviteli vagy sávszélességre az alkalmazás a tárhely lemezre küld a megadott időszak adatmennyiség. Ha az alkalmazás a nagy IO egység méretű bemeneti és kimeneti műveleteket hajt végre, nagy átviteli van szükség. Raktári alkalmazások általában a hiba-ellenőrzési intenzív műveleteket, amelyek az adatok nagyobb részeinek egyszerre és gyakran a tömeges műveletek elvégzéséhez. Más szóval ezek az alkalmazások szükség magasabb kapacitása. Ha például egy alkalmazást, az infrastruktúra való optimalizálásáról átviteli kell tervezésekor. A következő szakaszban ölelik részletesen a tényezőket kell optimalizálhatja a cél.

Ha csatol prémium tároló lemezen magas skálával virtuális, Azure rendelkezések átviteli, hogy a lemez specifikációja szerint. P30 lemezen például 200 MB kiépítése egy második lemezen kapacitása. Minden magas skála virtuális mérete is rendelkezik, akkor is fenntartása adott átviteli korlátozást. Például a szokásos GS5 virtuális maximálisan 2000 MB másodpercenként tartalmaz.

Van egy átviteli és IOPS, ahogy az alábbi képletet közötti kapcsolat.

![](media/storage-premium-storage-performance/image1.png)

Ezért fontos az optimális teljesítmény és IOPS értékek, az alkalmazáshoz szükséges. Próbálja ki valamelyik optimalizálása, a másik is kap hatással van. Későbbi szakaszában *Alkalmazás teljesítményének optimalizálása*fog ölelik fel további részleteket a IOPS és az átvitel optimalizálása.

## <a name="latency"></a>Időtartam  
Időtartama az idő, amíg az alkalmazások egyetlen kérelem, küldje el a tárhely lemezre küldhet és fogadhat a választ, az ügyfél számára. A kritikus méri, az alkalmazások teljesítmény IOPS és az átvitel mellett. Prémium tároló lemezen késleltetése az idő, amíg egy kérelmet adatait beolvasásához, és közölje újra az alkalmazást. Prémium tároló egységes alacsony késések biztosít. Ha engedélyezi a csak olvasható host prémium tároló lemezen gyorsítótárazás, sok alsó olvasási késés elérheti. Fog ölelik lemezre gyorsítótárazás későbbi részében részletesebben *optimalizálása alkalmazás*teljesítményére.

Az alkalmazás újabb IOPS és kapacitása optimalizálása, amikor az alkalmazás a késés befolyásolja. Után az alkalmazás teljesítményének javítása, mindig kiértékelésének eredménye a váratlan nagy késést viselkedés elkerülése érdekében az alkalmazás a késés.

## <a name="gather-application-performance-requirements"></a>Alkalmazás teljesítményének követelmények gyűjtése  
A nagy teljesítményű alkalmazások Azure prémium tároló futó tervezésének első lépése van, az alkalmazás teljesítmény előírásainak megértéséhez. Miután gyűjtse össze a teljesítmény követelményeknek, optimalizálhatja az alkalmazás, a legtöbb optimális teljesítmény eléréséhez.

Az előző szakaszban azt magyarázza a közös teljesítménymutatók, IOPS, a teljesítmény és a késés. Meg kell adnia, hogy ezek teljesítménymutatók amelyeket fontos, hogy az alkalmazás a kívánt felhasználói élményt. Ha például magas IOPS témákban tranzakcióinak milliónyi feldolgozása egy második OLTP alkalmazások. Mivel nagy átviteli fontos adatraktár alkalmazások egy második feldolgozás nagy mennyiségű adatot. Nagyon kis késés elengedhetetlen valós idejű alkalmazásokban, például live video-adatfolyam webhelyekre.

Ezután mérje le az alkalmazás egész élettartama maximális teljesítmény követelményeinek. A minta ellenőrzőlista alatti használata egy kezdő. A maximális teljesítményére normál alatt, a csúcs és munkaidőn kívüli terhelést időszakok rekord. Az összes munkaterhelésekből szint követelményei azonosításával kiderítheti, hogy az alkalmazás általános teljesítmény követelménye fogja. Például a normál terhelését egy e-kereskedelmi webhely lesz a tranzakciókat, a legtöbb nap során szolgál. A csúcs terhelést a webhely lesz a tranzakciók téli ünnepek vagy speciális eladási események során szolgál. A csúcs terhelést általában a tapasztaltabb korlátozott ideig, de az alkalmazás, ha át kívánja méretezni két vagy több időpontok rendes működését is szükséges. Ismerje meg az 50 PERCENTILIS, 90 PERCENTILIS és 99 PERCENTILIS követelményeknek. Ezzel az elemzéssel kiszűrése a teljesítmény követelmények bármely kiugró, és a erőfeszítéseket koncentráljon optimalizálás a megfelelő értékeket.

**Alkalmazás teljesítményének követelmények ellenőrzőlistája**

| **Teljesítménnyel kapcsolatos követelmények** | **50 százalékos rangsor** | **PERCENTILIS 90** | **99 PERCENTILIS** |
|---|---|---|---|
| Max. MAPI / szekundum | | | |
| % Olvasási műveletek            | | | |
| % Írási műveletek           | | | |
| % Véletlen műveletek          | | | |
| % Egymást követő műveletek      | | | |
| IO kérés mérete              | | | |
| Átlagos átvitt           | | | |
| Max. Átviteli              | | | |
| Min. Időtartam                 | | | |
| Átlagosan              | | | |
| Max. PROCESSZOR                     | | | |
| Átlagos Processzor                  | | | |
| Max. A memória                  | | | |
| Átlagos memória               | | | |
| A várakozási z                  | | | |

>**Fontos megjegyzés:**  
>Ezeket a számokat az alkalmazás várható jövőbeli értéknövekedésével alapján méretezés figyelembe. Célszerű az megtervezése a NÖV időszakokra, mert módosítása a infrastruktúra később a teljesítmény javítására nehezebben lehet érdemes.

Ha egy meglévő alkalmazást, és át szeretné helyezni a prémium tárolóhoz, először összeállítása a meglévő alkalmazást feladatlistáját felett. Ezután az alkalmazás prémium tárolón egy prototípusának összeállítása, és az *Alkalmazás teljesítményének optimalizálása* a dokumentum egy újabb szakaszban ismertetett irányelvek alapján alkalmazás tervezése. A következő szakasz ismerteti a gyűjtése a teljesítmény mérések használható eszközöket.

A meglévő kérelmezése a prototípusának hasonló készített ellenőrzőlistákat. Benchmarking eszközeivel hasonlóan a feladatok, és a prototípusának alkalmazás teljesítményének mérjük. Nézze meg a szakasz [Benchmarking](#benchmarking) további információt. Hajtsa végre, így eldöntheti, hogy prémium tároló egyezik-e, vagy művelet túllépje az alkalmazás teljesítményének igényeknek megfelelően alakíthatja. A gyártási alkalmazás majd alkalmazhat megegyező irányelvek.

### <a name="counters-to-measure-application-performance-requirements"></a>Számláló mérésére alkalmazás teljesítményre vonatkozó követelmények  
Az alkalmazás teljesítmény előírásainak mérésére legegyszerűbben, a kiszolgáló az operációs rendszer által biztosított teljesítményét figyelve eszközeivel. Linux PerfMon a Windows és iostat használható. Ezek az eszközök minden mérték a fentebb ismertetett megfelelő számláló rögzítése lehetőséget. Számláló értékeket kell rögzítése, amikor az alkalmazás fut, a normál, a csúcs és munkaidőn kívüli munkaterhelésekből.

A teljesítményszámlálók processzor, a memóriahasználat, és minden egyes logikai és fizikai lemezzel a kiszolgáló érhetők el. Egy virtuális a prémium tároló lemez használatakor a fizikai lemez számláló az egyes prémium tároló lemez, illetve logikai lemez számláló minden a prémium tároló lemezen létrehozott kötet. Az alkalmazás terhelést szolgáltató lemezt értékének kell menteni. Ha a fizikai, logikai és lemez között egy az egyhez megfeleltetés hivatkozhat fizikai lemez számláló; a logikai lemez számláló más módon hivatkozhat. Linux a iostat parancs Processzor- és kihasználtsági jelentést hoz létre. A lemez kihasználtsági jelentéshez a fizikai eszközök vagy partíciót egy statisztikai adatokkal szolgál. Ha külön lemezen adatok és a napló adatbázis-kiszolgálóval rendelkezik, a adatgyűjtés mindkét számára. Táblázat alatti lemezt, a processzor és a memóriában számláló ismerteti:

| A számláló | Leírás | PerfMon | Iostat |
|---|---|---|---|
| **IOPS vagy a tranzakciók / szekundum** | A tároló lemezre másodpercenként kiadott I/O kérések száma. | Olvasás/mp <br> Lemezre írása/sec | TP-k <br> r/s <br> w/s |
| **Lemezen olvasása és írása** | %-át olvasása és írása a lemezen végrehajtott műveletek. | % Lemez olvasási idő <br> % Lemez írási idő | r/s <br> w/s |
| **Átviteli** | Olvasni, vagy a lemezre másodpercenként adatok mennyiségét. | Olvassa el a bájt/sec lemez <br> Lemezen írási bájt/sec | kB_read/s <br> kB_wrtn/s |
| **Időtartam** | Teljes időt vesz igénybe a lemez IO kérést. | Átlagos lemezre sec/olvasott <br> Átlagos sec/írási | kerülve <br> svctm |
| **IO-méret** | A méret i/o-problémák a tárhely lemezre kéri. | Átlagos bájt/olvasott <br> Átlagos bájt/írási | avgrq-kódot sz-re |
| **A várakozási z** | Függőben lévő i/o-szám elolvashatja az űrlap vagy a tárhely lemezre várakozó kéri. | Várólista jelenlegi hossza | avgqu-kódot sz-re |
| **Max. A memória** | A memória gördülékenyen alkalmazás futtatásához szükséges | % Lekötött előjegyzett | Vmstat használata |
| **Max. PROCESSZOR** | Összeg gördülékenyen alkalmazás futtatásához szükséges Processzor | Processzor kihasználtsága (%) | % segédprogram |

További információ a [iostat](http://linuxcommand.org/man_pages/iostat1.html) és [PerfMon](https://msdn.microsoft.com/library/aa645516.aspx)megismerése


## <a name="optimizing-application-performance"></a>Alkalmazás teljesítményének optimalizálása  
Prémium tárolón futó alkalmazás teljesítményének befolyásoló főbb tényezők olyan jellegű a IO kérések virtuális méretét, a lemez méretét, a lemezt, gyorsítótár-lemezre, a többszálas végrehajtás és a várólista z száma. Megadhatja, hogy ezek közül néhány tényező és a rendszer által biztosított belül. A legtöbb alkalmazások előfordulhat, hogy nem ad IO méretének és várólista mélység megváltoztatására közvetlenül egy lehetőséget. Például SQL Server használatakor nem választhatja ki a IO méretét és várólista mélységét. Az SQL Server úgy dönt, hogy az optimális IO méretét és várólista mélység értékeket a legtöbb teljesítmény eléréséhez. Fontos tényezők mindkét fajta hatása az alkalmazás teljesítményének megértéséhez, hogy akkor is kiépítése teljesítmény igényeknek megfelelő erőforrásokat.

Ebben a szakaszban olvassa el az Ön által létrehozott, azonosíthatja, hogy mennyire kell alkalmazás teljesítményének alkalmazás követelmények ellenőrzőlista. Amely alapján, fogja kiderítheti, hogy milyen tényezőket ebből a szakaszból kell finomhangolása. Az egyes tényező hatása az alkalmazás teljesítményének tanúsítsa, az alkalmazás telepítésekor összehasonlítási eszközök futtatható. Keresse meg a [Benchmarking](#Benchmarking) szakasz végén található ez a cikk a lépéseket a Windows és Linux VMs közös összehasonlítási eszközök futtatásához.

### <a name="optimizing-iops-throughput-and-latency-at-a-glance"></a>Optimalizálás a IOPS, a teljesítmény és a késés áttekintése  
Az alábbi táblázat összefoglalja a összes teljesítmény tényezői és a lépéseket követve optimalizálása IOPS, a teljesítmény és a késés. Ez az összefoglaló követő szakaszok leírja minden tényező rengeteg mélységét.

| | **IOPS** | **Átviteli** | **Időtartam** |
|---|---|---|---|
| **Példa eset** | Vállalati OLTP alkalmazás igénylő egy második ráta nagyon magas tranzakciók.                                                                                                                                 | Vállalati adattárolási alkalmazás feldolgozás nagy mennyiségű adatot. | Közelében valós idejű alkalmazások igénylő felhasználói kérések, például online játékok azonnali visszajelzést. |
| Teljesítmény tényezői  | | | |
| **IO-méret** | Kisebb IO méretű együttesen magasabb IOPS.                                                                                                                                                                           | Nagyobb IO méret együttesen magasabb kapacitása. | |
| **Virtuális mérete** | A virtuális méretet, amely felajánlja a nagyobb, mint az alkalmazás követelmény IOPS használja. Lásd: virtuális méretű, és itt IOPS határai. | A virtuális méretet használata a nagyobb, mint az alkalmazás követelmény kapacitása korlát. Virtuális méretét és a teljesítmény határai itt talál. | A virtuális méretét, hogy ajánlatok méretezni nagyobb, mint az alkalmazás követelmény korlátai használja. Lásd: virtuális méretű, és itt határai. |
| **Lemez mérete** | A lemez méretét, amely felajánlja a nagyobb, mint az alkalmazás követelmény IOPS használja. Lásd: a lemez méretű, és itt IOPS határai. | A lemez méretet használata a nagyobb, mint az alkalmazás követelmény átviteli korlát. Lásd: a lemez méretű, és itt átviteli határai. | Használja a lemez méretét, hogy a ajánlatok nagyobb, mint az alkalmazás követelmény korlátai méretezni. Lásd: a lemez méretű, és itt határai. |
| **Virtuális és a lemez skála korlátai** | A kiválasztott virtuális mérete legfeljebb IOPS prémium tároló lemezt csatolva által vezérelt teljes IOPS nagyobbnak kell lennie. | A kiválasztott virtuális mérete legfeljebb átviteli prémium tároló lemezt csatolva által vezérelt teljes átviteli nagyobbnak kell lennie. | A kiválasztott virtuális méretű skála korlátai csatolt prémium tároló lemezt a teljes méret korlátai nagyobbnak kell lennie. |
| **Lemezen gyorsítótárazás** | Csak olvasható gyorsítótár engedélyezése prémium tároló lemezen olvasható nehéz műveletekkel magasabb olvasható IOPS eléréséhez. | | Csak olvasható gyorsítótár engedélyezése késések nagyon kicsi olvasható megszerezni kész nehéz műveletekkel prémium tároló lemezen. |
| **Lemezcsíkozás** | Több lemezre és a velük együtt kombinált magasabb IOPS és az átvitel határérték első paritásos. Ne feledje, hogy a kombinált korlát egy virtuális kell nagyobb, mint a kombinált korlátai csatolt prémium lemezre. | |
| **Csíkok mérete** | Kisebb csíkok, OLTP alkalmazásokban jelzése véletlen kis IO minta mérete. 64KB csíkok méretének például SQL Server OLTP alkalmazás használatával. | Nagyobb csíkok, adatraktár alkalmazásokban jelzése egymás után következő nagy IO minta mérete. 256KB csíkok méret például SQL Server Data raktári alkalmazás használatával. | |
| **Többszálas** | A leküldéses kérelmek nagyobb szám, amely nagyobb IOPS és az átvitel vezet prémium tárolóhoz többszálas használata. Például SQL Server állítsa az SQL Server további processzorok foglaláshoz nagy MAXDOP érték. | |
| **A várakozási z** | Nagyobb várólista mélység együttesen magasabb IOPS. | Nagyobb várólista mélység együttesen magasabb kapacitása. | Kisebb várólista mélység együttesen alsó késések. |

## <a name="nature-of-io-requests"></a>Természeti IO kérés  
Felkérés IO az alkalmazás fog hajt végre műveletet bemeneti és kimeneti időegység. Azonosító IO kérelmeket, véletlen vagy valamilyen sorrendben követik egymást, jellegét írási és olvasási, kicsi vagy nagy, megállapíthatja, az alkalmazás teljesítmény előírásainak. Nagyon fontos IO kérelmeket, hogy a jobb döntéseket, az alkalmazás infrastruktúra tervezésekor jellegét megértéséhez.

IO mérete a fontosabb tényezők közül. IO mérete, a bemeneti és kimeneti művelet kérelmet, az alkalmazás által létrehozott méretét. IO méretét a teljesítményéről különösen a IOPS és sávszélességet az alkalmazás elérni kívánt jelentős hatással van. A következő képlet a közötti kapcsolat IOPS, IO méretét és a sávszélesség/kapacitása.  
    ![](media/storage-premium-storage-performance/image1.png)

Egyes alkalmazások IO méretük megváltoztathatja, miközben az egyes alkalmazások nem teszi lehetővé. Például SQL Server határozza meg, hogy az optimális IO méret magát, és nem biztosítson felhasználók módosítása, hogy minden olyan belül. Azonban, az Oracle biztosít nevű paraméter [DB\_BLOKKOLÁSA\_méret](https://docs.oracle.com/cd/B19306_01/server.102/b14211/iodesign.htm#i28815) használatával adhatja meg, amelyek adatbázis I/O kérelem méretének.

Ha egy alkalmazást, amely nem engedélyezi a IO méretének módosítása, használja, akkor ez a cikk kövesse az útmutatásokat optimalizálásához KPI-k, amely a legjobban megfelelő az alkalmazást. Ha például

-   OLTP alkalmazás a kis- és véletlen IO kérések milliónyi hoz létre. Kezelheti az ilyen típusú IO kérelmeket, az alkalmazás infrastruktúra magasabb IOPS megszerezni kell tervezésekor.  
-   Egy adattárolási alkalmazás nagy- és egymás után következő IO kérések hoz létre. Ilyen típusú IO kérések kezelendő tervezheti kell az alkalmazás infrastruktúra nagyobb sávszélességre vagy átvitel.

Ha egy alkalmazást, amely lehetővé teszi a IO méretének módosítása, használja a kívül más teljesítmény irányelveihez IO méretének Ez a legjobb megoldás használata

-   Kisebb IO méretű magasabb IOPS eléréséhez. Ha például egy OLTP alkalmazás 8 KB.  
-   IO méretének nagyobb sávszélességre/teljesítmény eléréséhez. Ha például 1024 KB raktári alkalmazáshoz.

Íme egy példa a hogyan akkor számíthatja ki a IOPS és a teljesítmény és sávszélesség az alkalmazás. Fontolja meg az alkalmazás P30 lemezen. A maximális IOPS és a teljesítmény és sávszélesség P30 lemezen érhet el az 5000 IOPS és 200 MB másodpercenként a kurzor. Most az alkalmazáshoz szükséges a maximális IOPS a P30 lemezről, és használhat például 8 KB IO kisebb méretű, az eredményül kapott sávszélesség is eljuthassanak esetén 40 MB másodpercenként. Jó helyen jár, ha az alkalmazás P30 lemezről maximális átviteli/sávszélességre van szükség, és használhatja például 1024 KB IO nagyobb méretű, az eredményül kapott IOPS lesz kisebb, 200 IOPS. Ezért finomhangolása IO méretét, úgy, hogy azt megkövetelt mindkét az alkalmazás IOPS és a teljesítmény és sávszélesség. A következő táblázat összefoglalja a különböző IO méretű és a megfelelő IOPS és átviteli P30 lemez.

| **Alkalmazás követelmény** | **I/O mérete** | **IOPS** | **Átviteli/sávszélesség** |
|-----------------------------|--------------|----------|--------------------------|
| Max IOPS                    | 8 KB         | 5000    | 40 MB / szekundum         |
| Max-átviteli              | 1024 KB      | 200      | 200 MB / szekundum        |
| Max átviteli + magas IOPS  | 64 KB        | 3,200    | 200 MB / szekundum        |
| Max IOPS + nagy teljesítmény  | 32 KB        | 5000    | Másodpercenként 160 MB        |

IOPS és sávszélesség nagyobb, mint a maximális érték egyetlen prémium tároló lemezen, használja több prémium lemezre, amelyben együtt. Ha például csíkok két P30 lemez másodpercenként a 10 000 IOPS kombinált IOPS vagy a Kombinált átviteli 400 MB-os eléréséhez. A következő szakaszban leírtak kell használnia, amely támogatja a kombinált virtuális méretet a lemez IOPS és az átvitel.

>**Megjegyzés:**  
>IOPS vagy a többi szintén növeli az átviteli növelésekor győződjön meg arról, hogy a nem gombra kattint teljesítmény vagy a lemez vagy a virtuális IOPS korlátozások, amikor a növekvő vagy az egyik.

Az alkalmazás teljesítményének IO méret gyakorolt tanúsítsa, a virtuális és tartalmazó lemezen futtathatja a összehasonlítási eszközöket. Hozzon létre több próba fut, és minden futtatásakor más IO méretű hatása megjelenítéséhez. Keresse meg a [Benchmarking](#Benchmarking) szakasz végén található ez a cikk további információt.

## <a name="high-scale-vm-sizes"></a>Magas skála virtuális méretek  
Az alkalmazások megtervezése indításakor az első tennivalók egyik, válassza ki a virtuális tárolni az alkalmazás. Prémium tároló megtalálható futtatását is lehetővé teszi, hogy újabb számítási power és magas helyi lemezen I/O teljesítményt igénylő alkalmazások magas skála virtuális betűméretet. Ezek VMs adnak a merevlemezre gyorsabb processzor magasabb memória-core arány és egy Solid-State meghajtó (SSD). Magas skála VMs prémium tároló támogató példák a Tartományi, DSv2 és GS sorozat VMs.

A különféle magmintákat, a memóriahasználat, a operációs rendszer és a ideiglenes lemez méretét Processzor különböző méretű magas skála VMs érhetők el. Minden virtuális méretét is rendelkezik, amely a virtuális csatolhat adatok lemezre maximális száma. Ezért a választott virtuális mérete mennyi feldolgozása, memóriát, hatással vannak, és tárolókapacitású érhető el az alkalmazás. Is hatással van a számítási és tárolási költség. Alatti mindegyike a legnagyobb virtuális méret DS sorozat, DSv2 sorozat és GS sorozat jellemzői:

| Virtuális mérete | Processzormagok | A memória | Virtuális lemezre méretek | Max. adatok lemez | Gyorsítótár méretét | IOPS | Sávszélesség-gyorsítótár IO korlátai |
|---|---|---|---|---|---|---|---|
| Standard_DS14 | 16 | 112 GB | OPERÁCIÓS RENDSZER = 1023 GB <br> Helyi SSD = 224 GB | 32 | 576 GB | 50 000 IOPS <br> 512 MB / szekundum | 4000 IOPS és másodpercenként 33 MB |
| Standard_GS5 | 32 | 448 GB | OPERÁCIÓS RENDSZER = 1023 GB <br> Helyi SSD = 896 GB | 64 | 4224 GB | 80,000 IOPS <br> 2000 MB / szekundum | 5000 IOPS és 50 MB / szekundum |

Ha szeretné megtekinteni az összes rendelkezésre álló Azure virtuális méretű listáját, [Windows virtuális méretű](../virtual-machines/virtual-machines-windows-sizes.md) vagy [Linux virtuális méretű](../virtual-machines/virtual-machines-linux-sizes.md)vonatkoznak. Válassza a virtuális méretét, amelyek eleget és átméretezheti a kívánt alkalmazás teljesítményének követelményeknek. Mindezeken kívül követő fontos dolgot, virtuális méretű kiválasztásakor figyelembe venni.


*Méretezés korlátai*  
A maximális IOPS és lemez egy virtuális jutó határértékeket különféle, egymástól független. Győződjön meg arról, hogy az alkalmazás IOPS adja a virtuális, valamint a prémium lemez csatolva keretén belül. Egyéb esetben alkalmazás teljesítményének szabályozásának fog tapasztalni.

Példaként tegyük fel egy alkalmazás követelmény 4000 IOPS legfeljebb. Cél, létrehozhatnak egy DS1 virtuális P30 lemez. A P30 lemez tartana legfeljebb 5 000 IOPS. Azonban a DS1 virtuális a 3,200 IOPS korlátozódik. Ennek az alkalmazás teljesítményének fog kell korlátozza a virtuális határérték 3,200 IOPS, és csökkent teljesítményét lesz. Ennek elkerülése érdekében, válassza a egy virtuális és a lemez méretét, hogy a rendszer mindkét alkalmazás követelményeknek.

*A művelet költségét*  
Sok esetben célszerű lehet, hogy a teljes költsége prémium tárolót használó művelet kisebb, mint normál tároló használatával.

Például érdemes megvizsgálni a 16 000 IOPS igénylő kérelmet. A teljesítmény elérése érdekében szüksége lesz egy szabványos\_adhat a maximális IOPS 16000 32 szabványos tároló 1TB lemezre használata a D14 Azure IaaS virtuális. Legfeljebb 500 IOPS minden 1TB szabványos tároló lemezre érhet el. A virtuális havonta becsült költsége $1,570 lesz. 32 szabványos tároló lemezre havi költségének $1,638 lesz. A becsült havi teljes költsége $3,208 lesz.

Jó helyen jár akkor is prémium tárolón ugyanazt a kérelmet, ha szüksége lesz virtuális kisebb méretű és prémium kevesebb tárterület lemezt, így csökken a teljes költséget. Szabványos\_DS13 virtuális is követelménynek 16 000 IOPS négy P30 lemezt használ. A DS13 virtuális egy maximális IOPS 25,600, és minden egyes P30 lemez rendelkezik egy maximális IOPS az 5000. Ez a beállítás összesített, érhet el 5000 x 4 = 20 000 IOPS. A virtuális havonta becsült költsége $1,003 lesz. Négy P30 prémium tároló lemezt havi költségének $544.34 lesz. A becsült havi teljes költsége $1,544 lesz.

A következő táblázat összefoglalja az ebben az esetben a költség bontása Standard és a prémium tárolásához.

| | **Normál** | **Támogatás** |
|---|---|---|
| **Virtuális költségét / hó** | $1,570.58 (normál\_D14)   | $1,003.66 (normál\_DS13) |
| **Havonta lemezt a költség** | $1,638.40 (32 x 1 TB lemez) | $544.34 (4 x P30 lemez) |
| **Teljes költsége / hó**  | $3,208.98 | $1,544.34 |

*Linux Distros*  

Azure prémium tárolására kap VMs teljesítményt azonos szintű fut a Windows és Linux rendszerhez. A Linux distros sok változatok támogatjuk, és láthatja, hogy a teljes listáját [Itt](../virtual-machines/virtual-machines-linux-endorsed-distros.md). Fontos tudni, hogy különböző distros jobban alkalmasak a különböző típusú munkaterhelésekből. Ekkor megjelenik a teljesítmény attól függően, hogy fut, a terhelést a distro különböző szintjeit. Tesztelje a Linux distros az alkalmazással, és válassza ki azt, amely a legjobban.


Fut Linux prémium adathordozós, jelölje be a nagy teljesítményű ahhoz szükséges illesztőprogramok legújabb frissítéseket.

## <a name="premium-storage-disk-sizes"></a>Prémium tárolási lemez méretek  
Azure prémium tároló jelenleg három lemezre méretű kínál. Minden egyes lemez méretét IOPS, a sávszélesség és a tárterület-os különböző méretű érvényes. Válassza a prémium tárhely méretét attól függően, hogy az alkalmazás követelmények és a magas skála virtuális méretét. Az alábbi táblázatban látható, a három lemez méretű és azok a funkciók.

| **Lemezen típusa**       | **P10**           | **P20**           | **P30**           |
|---------------------|-------------------|-------------------|-------------------|
| Lemez mérete           | 128 orrot           | 512 orrot           | 1024 orrot (1 TB)   |
| Egy lemezen IOPS       | 500               | 2300              | 5000              |
| Egy lemezen átviteli | Másodpercenként 100 MB | 150 MB / szekundum | 200 MB / szekundum |

Kiválaszthatja, hogy a lemezen attól függ, hogy hány lemezt a választott méretét. Egyetlen P30 lemez vagy több P10 lemezre kihasználva felel meg az alkalmazás követelménynek. A választási lehetőségek, ha az alább felsorolt fiókkal kapcsolatos szempontok figyelembe.

*Méretezés korlátai (IOPS és átviteli)*  
Az egyes prémium lemez méret IOPS és az átvitel korlátozások a virtuális skála korlátai különböző és független származik. Győződjön meg arról, hogy a teljes IOPS és a lemezről átviteli skála korlátok a választott virtuális méretét.

Ha például egy alkalmazás követelmény legfeljebb 250 MB-os/sec átviteli, és használja a DS4 virtuális egyetlen P30 lemezen. A DS4 virtuális adhat legfeljebb 256 MB/sec kapacitása. Egyetlen P30 lemez azonban a átviteli legfeljebb 200 MB/sec rendelkezik. Ennek az alkalmazás korlátozott 200 MB/sec miatt a lemez által a kell. Ezt a korlátot, hogy kiépítése egynél több adat lemezt a virtuális.

>**Megjegyzés:**  
>A gyorsítótár, melyet olvasás nem szerepelnek a lemez IOPS és a teljesítmény, ezért nem tárgy lemez számával. Gyorsítótár-os saját külön IOPS és az átvitel érvényes egy virtuális.
>
>Az eredetileg a olvasása és írása mindegyike 60MB/sec és 40MB/sec rendre. Az idő múlásával a gyorsítótár bemelegedett, és további és annak az olvasás szolgál a gyorsítótárból. Ezután magasabb írási kapacitása elérheti a lemezről.

*A lemez száma*  
A lemez szüksége lesz alkalmazás követelmények felmérése révén számának meghatározására. Minden virtuális méretét szintén korlátozott a lemezt, amely a virtuális csatolhat számára. Ez általában kétszer magmintákat számát. Győződjön meg arról, hogy a virtuális méretét a választott támogatniuk kell a szükséges lemez számát.

Ne feledje, hogy a prémium tárhely, hogy nagyobb teljesítmény funkciók szabványos tároló lemezre képest. Ezért ha az alkalmazás az Azure IaaS virtuális prémium tárolóhoz szabványos tároló használatával, valószínűleg szüksége lesz az alkalmazás a ugyanazon vagy nagyobb teljesítmény elérése érdekében kevesebb prémium lemezt.

## <a name="disk-caching"></a>Lemezen gyorsítótárazás  
Magas skála VMs, amely az Azure prémium tároló kihasználhatja a több szálon gyorsítótárazási technológiát BlobCache nevű van. BlobCache gyorsítótárazás a virtuális gép RAM és a helyi SSD kombinációját használja. Ez a gyorsítótár prémium tároló állandó lemez és a virtuális helyi lemez érhető el. Alapértelmezés szerint a gyorsítótár beállítás értéke olvasási/írási OS lemez és a csak olvasható az adatok lemezt prémium tárterület is. A lemez gyorsítótárazás engedélyezve van a prémium tároló lemezen, a nagy skála VMs nagyon magas szintű teljesítményét, amelyekre az alapul szolgáló lemez teljesítménye érhet el.

>[AZURE.WARNING] A gyorsítótár beállításának Azure-lemez leválasztja, és újra csatolja a do not use. Ha az operációs rendszer lemezen, a virtuális újraindítása. Állítsa le az alkalmazások és szolgáltatások, előfordulhat, hogy ez előtt a lemez gyorsítótár beállításának zavarok érinti.

BlobCache működésével kapcsolatos további információért olvassa el a belső [Azure prémium tároló](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/) blogbejegyzést.

Fontos a megfelelő beállítása lemezt a gyorsítótár engedélyezése. E kell gyorsítótárazása lemez prémium lemezen, vagy nem függ a terhelést mintázatot, hogy a lemez kezelése lesz. Az alábbi táblázatban látható az alapértelmezett gyorsítótár beállításainak-OS és adatok.

| **Lemezen típusa** | **Alapértelmezett gyorsítótár beállítása** |
|---|---|
| Operációs rendszer lemez | ReadWrite |
| Adatok lemez | Nincs lehetőség |

Az alábbiakban az ajánlott gyorsítótár beállításainak adatok lemezt,

| **A lemez gyorsítótárazása** | **Mikor célszerű használni ezt a beállítást a ajánlási** |
|---|---|
| Nincs lehetőség | Állítsa be a host-gyorsítótár nincs csak olvasásra és az írási-nehéz lemezt. |
| Csak olvasható | Állítsa be host-gyorsítótár írásvédett, csak olvasásra és írásra. |
| ReadWrite | Csak akkor, ha az alkalmazás megfelelően kezeli a gyorsítótárban tárolt adatokat ír állandó lemezre szükség esetén állítsa be a host-gyorsítótár ReadWrite. |

*Csak olvasható*  
Csak olvasható lemez prémium tároló adatai gyorsítótárazás beállításával kis olvasható késés elérése, és a nagyon magas olvasható IOPS és az átvitel beszerzése az alkalmazást. Ez a két oka lehet annak, határidő

1.  Olvasás gyorsítótárból, amely a virtuális memória és a helyi SSD elvégzett, sokkal gyorsabb olvasása, amelyben be van kapcsolva az Azure blob-tárolóhoz adatok lemezről.  
2.  Prémium tároló nem számolja meg az olvasás felszolgált gyorsítótárát, a lemez IOPS felé és a teljesítmény. Az alkalmazás ezért tudja újabb teljes IOPS és a teljesítmény elérése érdekében.

*ReadWrite*  
Alapértelmezés szerint a OS lemezt ReadWrite gyorsítótárazás engedélyezve van. A legutóbb bővítettük gyorsítótárazás adatok, valamint a lemez ReadWrite támogatása. Gyorsítótár ReadWrite használja, ha az adatok írásához gyorsítótárból állandó lemezre megfelelő úgy kell rendelkeznie. Például SQL Server kezeli a gyorsítótárban tárolt adatokat írása állandó tároló lemezt a saját. Olyan alkalmazás, amely nem kezeli a szükséges adatokat pályától tartós ReadWrite gyorsítótár használata vezethet adatvesztést, ha a virtuális összeomlik.

Példaként alkalmazhat, így ezek irányelvek hajtsa végre az alábbi lépéseket a prémium tároló futó SQL Server

1.  Állítsa be a "Csak olvasható" gyorsítótár adatfájlok szolgáltatója prémium tároló lemezen.  
    egy.  A gyors felolvassa alsó gyorsítótárból az SQL Server lekérdezés idő óta adatlapok sokkal gyorsabb beolvasható a gyorsítótárból összehasonlítva közvetlenül az adatokat a lemezt.  
    b.  Olvasás szolgáló gyorsítótárból, azt jelenti, hogy van rendelkezésre álló prémium adatok lemezről további kapacitása. Az SQL Server is használja a további átviteli felé beolvasása közben a további lapok és egyéb műveletek, például a biztonsági mentés és visszaállítás terhelések köteg és index újraépíti.  
2.  Állítsa be a naplófájlok szolgáltatója prémium tároló lemezen "Nincs" gyorsítótár.  
    egy.  Naplófájlok elsősorban írási-nehéz műveletekre van. Ezért ezeket nem kihasználni a csak olvasható gyorsítótárból.

## <a name="disk-striping"></a>Lemezcsíkozás  
Ha több prémium tároló állandó lemezt a csatolt virtuális magas skálával lemezt a IOPs sávszélesség és tárolókapacitású összesítéséhez együtt is amelyben.

A Windows rendszeren is használhatja tárhelyek csíkok lemezre együtt. Meg kell adnia egy oszlop minden egyes lemez egy készletben. Ellenkező esetben csíkos mennyiségi teljesítménye lehet kisebb, mint a várható, a forgalom az lemezre páratlan terjesztési miatt.

Fontos: Kiszolgálókezelő felhasználói felület használatával, beállíthatja, hogy legfeljebb 8 csíkos kötet oszlopok száma. 8-nál több lemezt csatolni, amikor a PowerShell használatával a kötet létrehozása. A PowerShell használata esetén beállíthatja, hogy az oszlopok száma a lemez száma megegyezik. Ha 16 lemezen van egy egyetlen csíkok halmaz; például Adja meg az *Új-VirtualDisk* PowerShell-parancsmag *NumberOfColumns* paramétere 16 oszlopot.


Linux a segédprogrammal MDADM csíkok lemezre együtt. A lépések részletes leírását a Linux csíkozást lemezen [Szoftver RAID konfigurálása Linux](../virtual-machines/virtual-machines-linux-configure-raid.md)hivatkozhat.


*Csíkok mérete*  
Egy fontos konfiguráció lemezcsíkozás csíkok mérete. Csíkok méretének vagy blokk méretét az a legkisebb adattömb alkalmazása a sávos kötet is választ. Konfigurálja méretének csíkok alkalmazás és a kérelem mintát típusától függ. Ha úgy dönt, hogy a megfelelő csíkok mérete, vezethet IO átrendeződését, amely az alkalmazás csökkentett teljesítményét.

Ha például ha az alkalmazás által létrehozott IO felkérés nagyobb, mint a lemez csíkok méretét, a tároló rendszerben írja azt csíkok egység közötti együttműködést kívánja lehetővé egynél több lemezen. Ha az adatok eléréséhez időt, a kérelem befejezéséhez egynél több csíkok egységek különböző próbálják lesz. Az göngyölt effektus ezen viselkedési képest jelentős teljesítménybeli csökkenés vezethet. Azonban ha a IO kérés mérete kisebb, mint csíkok mérete, és véletlen jellegű esetén, a IO kérések előfordulhat, hogy adja össze a ugyanazon a lemezen egy szűk okozza, és végül elrontása IO teljesítményét.


A terhelési típusától függően a alkalmazást futtató, válassza ki a megfelelő csíkok méretét. Véletlen kis IO kérések csíkok kisebb méretű használja. Mivel nagy egymás után következő IO kérések csíkok nagyobb méretű használja. Megtudhatja, hogy a csíkok méret javaslatok az alkalmazás prémium tárolón futtatása. Az SQL Server állítsa be az OLTP munkaterhelésekből 64KB és az adatok raktári munkaterhelésekből 256KB csíkok méretét. Olvassa el az [SQL Server Azure VMs ajánlott eljárások a teljesítmény](../virtual-machines/virtual-machines-windows-sql-performance.md#disks-and-performance-considerations) további információt.


>**Megjegyzés:**  
>Akkor is paritásos közös 32 prémium tároló DS sorozat virtuális és a 64 prémium tároló lemezt a virtuális GS sorozat legfeljebb.

## <a name="multi-threading"></a>Több szálon futó műveletvégzés  
Azure prémium tároló platform nagymértékben párhuzamos lehetővé teszi. Emiatt a több szálon történő alkalmazások éri el sokkal nagyobb teljesítményű, mint egy egyetlen összefűzött alkalmazást. A több szálon történő alkalmazások felfelé feladatainak kettéválaszthatja-e több szálak keresztül, és végrehajtása során hatékonyságát növeli a virtuális és a lemez erőforrások maximális felhasználásával.

Például ha az alkalmazás a két szálak használatáról virtuális egyetlen alapszintű fut, a Processzor válthat a két szál hatékonyság eléréséhez. Több szálon befejezéséhez IO lemezen várakozik, amíg a Processzor más szálhoz tartozó üzenet lehet váltani. Ezzel a módszerrel két szálak végezheti el több mint egy szál tenné. Ha a virtuális core egynél több van, így további csökken futó ideje mivel minden alapvető tevékenységek hajthat végre párhuzamosan.

Nem lehet vásárolt alkalmazás hajtja végre egyetlen megjelenítési módjának megváltoztatása szálon futó műveletvégzés vagy a több szálon futó műveletvégzés. Ha például SQL Server is alkalmas többszörös-Processzor és a több elem core kezelése. Jó helyen jár SQL Server úgy dönt, hogy milyen feltételek szerint azt az egy vagy több feldolgozásá lekérdezést hoz fog kihasználhatja. Képes lekérdezések futtatása és az indexek használatával több szálon futó műveletvégzés összeállítása. Egy lekérdezés, amely magában foglalja a csatlakozás a nagy táblázatok, és visszatér a felhasználói adatok rendezése SQL Server fogják használni több beszélgetésekben. A felhasználó azonban nem tudja ellenőrizni, hogy SQL Server végrehajtja a lekérdezés egyetlen szálon vagy több beszélgetésekben.

Nincsenek-beállításokkal módosíthatja a befolyásolhatja a több szálon futó műveletvégzés vagy párhuzamos alkalmazás feldolgozása. Például SQL Server esetén célszerű a legnagyobb mértékben a párhuzamos konfiguráció. Ez a beállítás nevű MAXDOP, lehetővé teszi, hogy állítsa be az SQL Server használható, ha a párhuzamos processzorok maximális száma a feldolgozása. Az egyes lekérdezések vagy index műveletek MAXDOP is beállíthatja. Ez akkor hasznos, ha a teljesítmény kritikus alkalmazáshoz a rendszer erőforrásainak egyenleg szeretne.

Tegyük fel, hogy az alkalmazást az SQL Server végrehajtja a nagy Lekérdezéstervező művelet és egy index egy időben. Tegyük fel, tudassa velünk, hogy az index művelet legyen a nagy Lekérdezéstervező összehasonlítva további performant megy végbe. Ebben az esetben az index művelet lehet nagyobb, mint a MAXDOP értéket a lekérdezés MAXDOP értékének beállíthatja. Ezzel a módszerrel az SQL Server több processzort szeretne, akkor is kihasználhatja az index művelet összehasonlítja azt a nagy Lekérdezéstervező is jelöl ki processzorok számának tartalmaz. Ne feledje, hogy az egyes használt SQL Server szálak száma nem szabályozható. Megadhatja, hogy az éppen kijelölt processzorok maximális száma több szálon futó műveletvégzés.

További tudnivalók [A párhuzamos fok](https://technet.microsoft.com/library/ms188611.aspx) SQL Server-alapú. Megtudhatja, hogy ilyen beállításokat, amelyek befolyásolják több szálon futó műveletvégzés az alkalmazás és a konfigurációk az optimális teljesítmény érdekében.

## <a name="queue-depth"></a>A várakozási z  
A várólista z vagy a sor hossza vagy a várólista mérete szerepel a rendszer a függőben lévő IO kérelmek számát. Várólista mélység értékének meghatározza, hogy az alkalmazás is egy vonalba, a tárhely lemez meg fog feldolgozása hány IO műveleteket. Ez a cikk ti IOPS, a teljesítmény és a késés azt tárgyalja összes három alkalmazás teljesítménymutatók hatással van.

Magassági és a több szálon futó műveletvégzés várólista szorosan kapcsolódó. A várakozási mélység érték azt jelzi, hogy mennyit több szálon futó műveletvégzés lehet valósítható meg az alkalmazást. Ha a várólista mélység túl nagy, alkalmazás is további műveletek végrehajtása egyidejűleg, ez azt jelenti, még több szálon futó műveletvégzés. Ha a várólista mélység kicsi, akkor is, ha a több szálon történő alkalmazása, nem lesz elég egyidejű végrehajtása a elrendezve kérések.

Általában kész alkalmazások nem engedélyezi a várólista-mély, mert ha beállított helytelenül tegye jó-nél több kárt. Alkalmazások az optimális teljesítmény eléréséhez várólista mélység jobb értéke lesz. Azonban fontos megérteni a fogalom, úgy, hogy a teljesítménnyel kapcsolatos problémák elhárítása az alkalmazást. Várólista mélység hatásai figyelheti a rendszer összehasonlítási eszközök futtatásával is.

Egyes alkalmazások adja meg a várólista mélység befolyásoló beállításait. Ha például MAXDOP (párhuzamos legnagyobb fokú) beállítás az előző szakaszban leírtak SQL Server. MAXDOP módja a várólista mélység befolyásoló és a több szálon futó műveletvégzés, bár nem változik közvetlenül a SQL Server várakozási sorban található mélység értékét.

*Magas várólista z*  
Magas várólista mélységét sorok felfelé a lemezen a további műveletek. A lemez tudja, hogy a következő kérelem időszakokra a várakozási sorban található. Emiatt a lemez időszakokra tevékenységek ütemezése és feldolgozni azokat az optimális sorrendben. Az alkalmazás több kérést küld a lemez, mivel a lemez dolgozhat további párhuzamos IOs. Végül az alkalmazás újabb IOPS eléréséhez tudja. Mivel az alkalmazás feldolgozó több kérést, az alkalmazás a teljes kapacitásának is megnöveli.

Általában az alkalmazások érhet el maximális sebesség, 8 – 16 + egy csatolt lemezen nyitott IOs. Várólista mélységét az egyik, ha alkalmazás nem van terjesztése elég az IOs rendszer, és azt kisebb mennyiségű dolgozza fel egy adott időszakban. Más szóval kisebb kapacitása.

Például SQL Server-alapú "4" lekérdezés-MAXDOP értékének beállítása tájékoztatja SQL Server, hogy négy magmintákat használhatja a lekérdezés végrehajtása. Mi a legjobb várólista mélység érték és a lekérdezés-végrehajtási fúrásmintáit számát az SQL Server határozza meg.

*Ha a z optimális várakozási sora*  
Nagyon magas várólista mélység érték van a hátrányai is. Ha várólista mélység érték túl nagy, az alkalmazás megpróbálja nagyon magas IOPS elindításához. Kivéve, ha az alkalmazás rendelkezik a megfelelő kiépített IOPS állandó lemezt, ez az alkalmazás késések negatív hatással. A következő képlet a közötti kapcsolat IOPS, időtartam és várólista mélységét.  
    ![](media/storage-premium-storage-performance/image6.png)

Várólista mélység nem konfigurálja a legmagasabb értékre van, de az optimális érték, amely elég IOPS alkalmazásához tartana késések megtartásával. Például ha az alkalmazás késés ezredmásodperc 1 lesz, az 5000 IOPS eléréséhez szükséges várólista mélység is, QD = 5000 x 0,001 = 5.

*Sávos mennyiségi várólista mélységét*  
Egy csíkos kötet úgy, hogy minden lemez rendelkezik csúcs várólista mélységét egyenként karbantartása elég magas várólista mélységét. Például érdemes megvizsgálni egy alkalmazást, amely a 2 várólista mélységét verembe küldi, és a csíkok 4 lemezt szerepelnek. A két IO kérések két lemez ugrik, és a hátralévő két lemez üresjárati. A várakozási mélység emiatt beállítása, hogy az összes lemez zsúfoltnak lenniük. Az alábbi képlet bemutatja, hogyan csíkos kötet várólista mélységét határozza meg.  
    ![](media/storage-premium-storage-performance/image7.png)

## <a name="throttling"></a>Szabályozása  
Azure prémium tároló rendelkezések megadott számú IOPS és átviteli attól függően, hogy a virtuális méretét és kiválaszthatja, hogy a lemez méretű. Az alkalmazás megpróbálja IOPS vagy átviteli meghajtó feletti ezek a korlátok: Mi a virtuális vagy lemez képes kezelni, bármikor prémium tároló szabályozása fog azt. Ez az alkalmazás csökkent teljesítményét formájában jegyzékfájlok:. Ez lehet magasabb időtartama jelenti, átviteli alsó vagy IOPS alsó. Prémium tároló nem szabályozása, ha az alkalmazás teljesen leállhat által haladja meg a Mi az erőforrások képesek elérése. Igen teljesítményét érintő hibák miatt szabályozásának elkerüléséhez mindig kiépítése kellő erőforrások az alkalmazás. Figyelembe venni, mi azt tárgyalja a virtuális méretét és a fenti lemez méretű szakaszban. A legjobb módja, milyen erőforrások, meg kell tárolni az alkalmazás megállapítása mérés.

## <a name="benchmarking"></a>Mérés  
A folyamaton, amelyek az alkalmazás a különböző munkaterhelésekből, és az alkalmazás teljesítményének minden terhelés mérésére mérés. Egy korábbi szakaszban leírt lépésekkel összegyűjtötte az alkalmazás teljesítményének követelményeknek. Az alkalmazás szolgáltatója a VMs összehasonlítási eszközök futtatásával meghatározhatja a teljesítményszint, hogy az alkalmazás prémium adathordozós érhet el. Ebben a részben elvégezheti a szükséges, a szabványos DS14 virtuális Azure prémium tároló lemezt kiépítve mérés példák.

Azt használt közös összehasonlítási eszközök Iometer és FIO, a Windows és Linux rendre. Ezeket az eszközöket, több szálak, amelyek egy gyártási, például a terhelést elindítanak, és a rendszer teljesítményét mérjük. Az eszközök segítségével is beállítható paraméterek például blokkokból álló méretét és várólista mélység, amely általában nem módosítható az alkalmazás. Ezzel kap a legnagyobb teljesítmény meghajtóra magas szintű alkalmazás munkaterhelésekből különböző típusú prémium lemezen kiépítve virtuális nagyobb rugalmasság érdekében. További információ a összehasonlítási eszközeiről, keresse fel [Iometer](http://www.iometer.org/) és [FIO](http://freecode.com/projects/fio).

Hajtsa végre az alábbi példákat, hozzon létre egy szokásos DS14 virtuális és 11 prémium tároló lemezre csatolása a virtuális. 11 lemezt állítsa be a 10 lemezt, "Nincs" gyorsítótárazás állomáswebhelyén, és be NoCacheWrites nevű kötet paritásos őket. Állítsa be a "Írásvédett" mint hátralévő gyorsítótárazás host, és a lemez CacheReads nevű kötet létrehozása. E beállítás használatával, fogja láthatja a maximális olvasási és írási egy szabványos DS14 virtuális teljesítményét. A lépések részletes leírását a DS14 virtuális létrehozása a prémium lemezre lépjen [létrehozása](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk)és az adatok virtuális gép lemezen prémium tárterület-fiók használatával.

*Lépett fel a gyorsítótár*  
A lemez gyorsítótárazás csak olvasható állomáswebhelyén tudják lemez határértéknél magasabb IOPS ad. Úgy juthat az állomás gyorsítótár a maximális olvasási teljesítmény, először meg kell meleg felfelé a gyorsítótár a lemez. Ezzel biztosíthatja, hogy az olvasás IOs melyik összehasonlítási eszköz fog meghajtó CacheReads kötet ténylegesen találatok a gyorsítótár és nem a lemez közvetlenül. A gyorsítótár találatok eredményt az egyetlen gyorsítótárból további IOPS lemez engedélyezve van.

>**Fontos:**  
>Futtatása, mérés, minden alkalommal, amikor a virtuális újraindítása után előtt meleg kell a gyorsítótár be.

#### <a name="iometer"></a>Iometer   
[Töltse le a Iometer eszközt](http://sourceforge.net/projects/iometer/files/iometer-stable/2006-07-27/iometer-2006.07.27.win32.i386-setup.exe/download) a virtuális meg.

*Tesztfájl*  
Iometer a hangerőt, amelyen a összehasonlítási vizsgálat fog futni tárolt próba fájlt használ. Olvasás meghajtók és a ír a próba-fájl a lemez mérésére IOPS és az átvitel. Iometer hoz létre a próba-fájl, ha nincs megadva, egy. A CacheReads és NoCacheWrites kötet iobw.tst nevű 200GB próba-fájl létrehozása.

*Az Access alkalmazásra vonatkozó technikai adatok*  
Az alkalmazásra vonatkozó technikai adatok kérése IO méretét, % olvasási/írási, % véletlen/egymás után következő állíthatók be a "A hozzáférési jellemzői" lap a Iometer. Hozzon létre egy access-specifikációja az alábbiakban ismertetett jelenik meg. Az access specifikációi létrehozása, és a "Mentés" egy megfelelő nevet például – RandomWrites\_8K, RandomReads\_8K. A próba forgatókönyv fut, jelölje ki a megfelelő specifikációja.

Példa access specifikációi maximális írása IOPS forgatókönyvet alább,  
    ![](media/storage-premium-storage-performance/image8.png)

*Maximális IOPS próba alkalmazásra vonatkozó technikai adatok*  
Maximális IOPs bemutatják, használja a kérelem mérete kisebb lesz. Véletlen írása és olvasása jellemzői létrehozása és használata: 8K kérelem méretét.

| Az Access meghatározása | Kérés mérete | Véletlen % | Olvasás % |
|----------------------|--------------|----------|--------|
| RandomWrites\_8K     | 8K           | 100      | 0      |
| RandomReads\_8K      | 8K           | 100      | 100    |

*Maximális sebesség tesztelése alkalmazásra vonatkozó technikai adatok*  
Maximális sebesség bemutatják, hogy a nagyobb kérelem méret használata. 64 ezer kérelem méret használata, és hozza létre a véletlen írása és olvasása jellemzői.

| Az Access meghatározása | Kérés mérete | Véletlen % | Olvasás % |
|----------------------|--------------|----------|--------|
| RandomWrites\_64K    | 64 EZER          | 100      | 0      |
| RandomReads\_64K     | 64 EZER          | 100      | 100    |

*A Iometer teszt futtatása*  
Hajtsa végre az alábbi lépéseket követve meleg gyorsítótár felfelé

1.  Hozzon létre két access specifikációi alább látható értékekkel

  	| név              | Kérés mérete | Véletlen % | Olvasás % |
  	|-------------------|--------------|----------|--------|
  	| RandomWrites\_1MB | 1MB          | 100      | 0      |
  	| RandomReads\_1MB  | 1MB          | 100      | 100    |

2.  Futtassa a következő paraméterek gyorsítótár lemez inicializálhatóként a Iometer vizsgálat. Három dolgozó szálak használata a cél kötet, és egy 128 várólista mélységét. Az időtartamot "Futási idő" a próba 2hrs a "Telepítés tesztelése" lapon.

  	| Eset              | Cél mennyiségi | név              | Időtartam |
  	|-----------------------|---------------|-------------------|----------|
  	| Gyorsítótár lemez inicializálni | CacheReads    | RandomWrites\_1MB | 2hrs     |

3.  Futtassa a Iometer próba lépett fel a következő paraméterek gyorsítótár lemez. Három dolgozó szálak használata a cél kötet, és egy 128 várólista mélységét. Az időtartamot "Futási idő" a próba 2hrs a "Telepítés tesztelése" lapon.

  	| Eset           | Cél mennyiségi | név             | Időtartam |
  	|--------------------|---------------|------------------|----------|
  	| Meleg gyorsítótár lemezre mentése | CacheReads    | RandomReads\_1MB | 2hrs     |

Gyorsítótár után lemez bemelegíteni, folytassa az alább felsorolt próba jelenik meg. A Iometer vizsgálat futtatásához használja **minden** cél kötet legalább három dolgozó beszélgetésekben. Minden egyes dolgozó szálhoz jelölje be a cél mennyiségi, várólista mélység beállítása, és válassza a mentett próba jellemzői egyikét, ahogy az alábbi táblázatban látható, a megfelelő tesztelési forgatókönyv futtatásához. Ezek a vizsgálatok futtatása a táblázat is mutatja várt eredményeket IOPS és kapacitása. Ha minden esetben 8KB kis IO méretének és a magas várólista mélységét 128 használják.

| Teszt eset      | Cél mennyiségi | név              | Eredmény       |
|--------------------|---------------|-------------------|--------------|
| Max. Olvasási IOPS     | CacheReads    | RandomWrites\_8K  | 50 000 IOPS  |
| Max. IOPS írása    | NoCacheWrites | RandomReads\_8K   | 64 000 IOPS  |
| Max. Kombinált IOPS | CacheReads    | RandomWrites\_8K  | 100 000 IOPS |
|                    | NoCacheWrites | RandomReads\_8K   |              |
| Max. Olvassa el a MB/sec   | CacheReads    | RandomWrites\_64K | 524 MB/sec   |
| Max. Írás MB / (mp)  | NoCacheWrites | RandomReads\_64K  | 524 MB/sec   |
| Kombinált MB/sec    | CacheReads    | RandomWrites\_64K | 1000 MB/sec  |
|                    | NoCacheWrites | RandomReads\_64K  |              |

Az alábbiakban a Iometer a képernyőképek a vizsgálati eredmények kombinált IOPS és a teljesítmény forgatókönyvek.

*Kombinált olvasása és írása maximális IOPS*  
![](media/storage-premium-storage-performance/image9.png)

*Kombinált olvasása és írása maximális sebesség*  
![](media/storage-premium-storage-performance/image10.png)

### <a name="fio"></a>FIO  
FIO javasolt tárolóhoz a Linux VMs a használt eszköz. Jelölje ki a különböző IO méretű, egymás után következő rugalmasság vagy véletlen Olvasás és a ír. Azt indít dolgozó szálak vagy a folyamatok végrehajtásához a megadott bemeneti és kimeneti műveletet. Megadhatja, hogy milyen típusú bemeneti és kimeneti műveletet minden dolgozó szál kell elvégeznie, feladat-fájlok használata. Egy feladat fájl / forgatókönyvet az alábbi példában a illusztrált létrehozott. Módosíthatja a feladat fájlokban prémium tároló futó másik munkaterhelésekből összehasonlító alkalmazásra vonatkozó technikai adatok. A példákban **Ubuntu**futó szabványos DS 14 virtuális használja azt. Elejére a [Benchmarking szakaszban](#Benchmarking) leírtak és szeretetteli azonos beállításaitól használja fel a gyorsítótár összehasonlítási vizsgálatok futtatása előtt.

Mielőtt indításához [FIO töltse le](https://github.com/axboe/fio) és telepítse a virtuális gépen.

A következő parancsot a Ubuntu,

        apt-get install fio

Fogjuk használni négy dolgozó szálak írási műveletek vezetéséhez és négy dolgozó beszélgetésekben a lemezen vezetői olvasási műveletekhez. Az írás dolgozók forgalmat a "nocache" kötet, amelynek 10 lemezt gyorsítótár "Nincs" értékre van állítva a fog kell ösztönzése. Az olvasási dolgozók forgalmat a "readcache" kötet, amelynek való gyorsítótár "Csak olvasható" 1 lemezre fog kell ösztönzése.

*Maximális írási IOPS*  
A feladat fájl maximális írása IOPS megszerezni a következő jellemzői készíthet. Nevezze el a "fiowrite.ini".

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[writer1]
rw=randwrite
directory=/mnt/nocache
[writer2]
rw=randwrite
directory=/mnt/nocache
[writer3]
rw=randwrite
directory=/mnt/nocache
[writer4]
rw=randwrite
directory=/mnt/nocache
```

Megjegyzés: a követés legfontosabb dolog, amit a tervezési útmutató az előző szakaszokban ismertetett megfeleljenek. Ezek a jellemzői lényegesek maximális IOPS, meghajtóra  
-   256 magas várólista mélységét.  
-   Egy kis blokk mérete 8KB.  
-   Több szálak véletlen írások hajt végre.

A következő parancsot a 30 másodperc FIO próba elindításához  

    sudo fio --runtime 30 fiowrite.ini

A próba fut, miközben látja számát írása IOPS a virtuális és prémium lemezt tartja fogja. Ahogy az alábbi minta, a DS14 virtuális elkötelezett a maximális írási IOPS legfeljebb 50 000 IOPS.  
    ![](media/storage-premium-storage-performance/image11.png)

*Maximális olvasható IOPS*  
A projekt-fájl létrehozása a maximális olvasási IOPS megszerezni a következő beállításokat. Nevezze el a "fioread.ini".

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache
```

Megjegyzés: a követés legfontosabb dolog, amit a tervezési útmutató az előző szakaszokban ismertetett megfeleljenek. Ezek a jellemzői lényegesek maximális IOPS, meghajtóra

-   256 magas várólista mélységét.  
-   Egy kis blokk mérete 8KB.  
-   Több szálak véletlen írások hajt végre.

A következő parancsot a 30 másodperc FIO próba elindításához

    sudo fio --runtime 30 fioread.ini

A próba fut, miközben fogja látja beolvasott IOPS a virtuális száma és prémium lemezt tartja meg. Ahogy az alábbi minta, a DS14 virtuális elkötelezett legfeljebb 64 000 olvasható IOPS. Ez a lemez és a gyorsítótár teljesítményét.  
    ![](media/storage-premium-storage-performance/image12.png)

*Maximális olvasási és írási IOPS*  
A feladat fájl létrehozása a következőre jellemzői, hogy a legnagyobb kombinált olvasási és írási IOPS. Nevezze el a "fioreadwrite.ini".

```
[global]
size=30g
direct=1
iodepth=128
ioengine=libaio
bs=4k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache

[writer1]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer2]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer3]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer4]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
```

Megjegyzés: a követés legfontosabb dolog, amit a tervezési útmutató az előző szakaszokban ismertetett megfeleljenek. Ezek a jellemzői lényegesek maximális IOPS, meghajtóra

-   Egy nagy várólista mélységét 128.  
-   A 4KB kis blokk méretet.  
-   Több szálak elvégzéséhez véletlen beolvassa és.

A következő parancsot a 30 másodperc FIO próba elindításához

    sudo fio --runtime 30 fioreadwrite.ini

A vizsgálat fut, amíg tekintheti meg a kombinált olvasható számát, és írja be a virtuális IOPS lesz, és prémium lemezt tartja meg. Ahogy az alábbi minta, a DS14 virtuális elkötelezett legfeljebb 100 000 kombinált olvasható IOPS írni. Ez a lemez és a gyorsítótár teljesítményét.  
    ![](media/storage-premium-storage-performance/image13.png)

*Maximális Kombinált átviteli*  
A maximális megszerezni kombinált olvasási és írási kapacitása, a nagyobb méretű blokkokból álló méretét és a nagyméretű sorban z használata több szálak olvasása és írása elvégzéséhez. A továbbfejlesztett fájlblokkolás méretet 64 KB és 128 várólista mélységét is használhatja.

## <a name="next-steps"></a>Következő lépések  

További információ az Azure prémium tároló:

- [Prémium tárterület: Azure virtuális gép Munkaterhelésekből nagy teljesítményű tárterület](storage-premium-storage.md)  

Az SQL Server-felhasználóknak, olvassa el a cikkek teljesítmény gyakorlati tanácsok az SQL Server:

- [Azure virtuális gépeken futó SQL Server ajánlott eljárások a teljesítmény](../virtual-machines/virtual-machines-windows-sql-performance.md)
- [Azure prémium tároló biztosít az SQL Server Azure virtuális a legmagasabb teljesítmény](http://blogs.technet.com/b/dataplatforminsider/archive/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm.aspx) 
