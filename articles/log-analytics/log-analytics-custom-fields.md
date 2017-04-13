<properties
   pageTitle="Az egyéni mezők napló Analytics |} Microsoft Azure"
   description="Az egyéni mezők a napló Analytics funkcióval hozhat létre saját kereshető mezőket összegyűjtött rekord tulajdonságainak hozzáadása MOBILE adatokból.  Ez a cikk ismerteti a folyamat típusú egyéni mező létrehozása és részletes ismertetését megtalálja a minta eseményhez."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="custom-fields-in-log-analytics"></a>Log Analytics az egyéni mezők

A napló Analytics **Egyéni mezők** szolgáltatása lehetővé teszi a MOBILE adattárban meglévő rekordok saját kereshető mezők hozzáadásával bővítik.  A program automatikusan kitölti az egyéni mezők más tulajdonságokat ugyanazt a rekordot a kiolvasott adatokból.

![Egyéni mezők – áttekintés](media/log-analytics-custom-fields/overview.png)

Ha például a minta az alábbi tartalmazza az esemény leírása megbúvó hasznos adatokat.  Ez az adatok kinyerése a be külön tulajdonságok számára elérhetővé teszi azt az olyan műveleteket, mint a rendezés és szűrés.

![Jelentkezzen be a Keresés gomb](media/log-analytics-custom-fields/sample-extract.png)

>[AZURE.NOTE] Kattintson az előnézetben a munkaterület 100 az egyéni mezők korlátozott áll.  Ha ez a funkció eléri általános elérhetősége kibontva jelennek meg ezt a korlátot.

## <a name="creating-a-custom-field"></a>Egyéni mező létrehozása

Egyéni mező létrehozásakor napló Analytics kell megértése mely értékére kitöltéséhez használandó adatokat  A Microsoft Research FlashExtract nevű technológiát használja, az adatok gyors azonosítása.  Nem igénylő, hogy explicit útmutatást, a napló Analytics kapcsolatban az adatok kinyerése a számukra nyújtott példák szeretne folyamatosan tanul.

A következő szakaszokban a lépéseket az egyéni mező létrehozása.  Ez a cikk alján található egy minta kivonása a áttekintése a következő.

> [!NOTE] Az egyéni mező van kitöltve, mivel a megadott feltételeknek megfelelő rekordok bekerülnek MOBILE adatok mentése, így csak a rekordok az egyéni mező létrehozása után gyűjtött fognak megjelenni.  Az egyéni mező nem adhatók hozzá után már az adatokat tároló rekordok.

### <a name="step-1--identify-records-that-will-have-the-custom-field"></a>Lépés: 1 – fog rendelkezni az egyéni mező rekordok azonosítása
Első lépésként azonosítani a rekordokat, hogy az egyéni mező jelenik meg.  Egy [szabványos napló keresés](log-analytics-log-searches.md) indítása, és válassza a használni kívánt a modellt, amely a napló Analytics tanuljon fog egy rekordot.  Amikor ad meg, hogy bontsa ki az adatokat az egyéni mező fogja, ahol ellenőrzése, és módosítsa a feltételeket a **Mező a varázsló** nyitja meg.

2. Nyissa meg a **Napló keresési** , és [történő beolvasásához a rekordok](log-analytics-log-searches.md) , amelyek az egyéni mezőt.
2. Jelölje ki egy bejegyzést, amelyekkel a napló Analytics használni kívánt adatok kitöltéséhez az egyéni mező kiolvasó modell.  Az adatok kinyerése a rekord, amelyet határozható meg, és a napló Analytics határozza meg az egyéni mező minden hasonló rekordhoz kitöltéséhez logika fogja használni ezt az információt.
3. Kattintson a balra lévő bármilyen szöveget tulajdonság a bejegyzés gombra, és válassza a **mezőjét kibontásához**.
4. **Mező kivonása varázsló nyitja meg**, és a rekord kijelölt jelenik meg a **Fő példa** oszlopban.  Az egyéni mező határozza meg azokat a rekordokat, amely a kijelölt tulajdonságok ugyanazokkal az értékekkel.  
5. Ha az érték nem pontosan milyen szeretne, jelölje ki a feltételek szűkítésével további mezőket.  A feltétel mező értékének megváltoztatásához megszakítása és válasszon egy másik bejegyzést, amelyet a feltételnek.

### <a name="step-2---perform-initial-extract"></a>Lépés: 2 – az első kivonat végrehajtása.
Miután azonosította, hogy a rekordok, amelyek az egyéni mező, meg kell határoznia a kinyerni kívánt adatokat.  Log Analytics ezen információk segítségével hasonló hasonló rekordokban azonosításához.  Ezt követően az lépésben fogja tudni az eredmények ellenőrzése, és további részletek az elemzés használata a napló elemzéséhez biztosítása.

1. Jelölje ki a minta rekord, amelyet szeretne feltölteni az egyéni mező szövegét.  Majd választhat egy párbeszédpanelen nevezze el a mezőt, és végezze el a kezdeti kivonat.  A karakterek ** \_CF** fog automatikusan hozzáfűzzön.
2. Kattintson a **kibontása** összegyűjtött rekordok elemzésének végrehajtásához.  
3. Az **összefoglaló** és a **Keresési eredmények** szakaszok a kivonat eredményének megjelenítése, így megvizsgálhatja pontosságát.  **Összefoglaló** jeleníti meg a feltételt, azonosítja a darab és a rekordok minden adatérték azonosította.  **Keresési eredmények** biztosít a feltételeknek megfelelő rekordok részletes listáját.


### <a name="step-3--verify-accuracy-of-the-extract-and-create-custom-field"></a>Lépés a 3 – a kivonat pontosságának ellenőrzése és egyéni mező létrehozása

A kezdeti kivonat elvégzése, miután napló Analytics jelennek meg az eredmények már gyűjtött adatok alapján.  Ha az eredményeket keresse meg a pontos majd létrehozhat az egyéni mező nem további munkahelyi.  Ha nem, majd finomíthatja az eredményeket, hogy a napló Analytics javíthatja a összefüggés.

2.  Ha a kezdeti kivonat az értékeket nem helyesek, majd kattintson a **Szerkesztés** ikonra a helytelen rekord melletti, és válassza a **Módosítás a kiemelés** érdekében módosítsa a kijelölést.
3.  A bejegyzés alatt a **Fő példa**a **További példákat** szakaszban másolja a program.  Beállíthatja, hogy a kiemelés itt megérteni a kijelölés kell végrehajtott napló Analytics érdekében.
4.  Kattintson a **bontsa ki** az új információkat használni a meglévő rekordok ki szeretné számítani.  Az eredmények nem az éppen módosított alapján a új üzletiintelligencia-rekordok módosíthatók.
5.  Továbbra is mindaddig, amíg az összes rekordot a kivonat megfelelően azonosítsa az adatok kitöltéséhez az új egyéni mező felvétele a korrekciók.
6. Ha elégedett az eredménnyel, kattintson a **Mentés kibontásához** .  Az egyéni mező most már van megadva, de azt nem lehet hozzáadni azokat a rekordokat még.
7.  Várja meg az új rekordok gyűjtött, és futtassa újra a napló keresés a megadott feltételeknek. Az új rekordokat kell tartalmaznia az egyéni mezőt.
8.  Az egyéni mező, mint bármely más rekord tulajdonság használja.  Adatok összesítése és a csoport vele, és még akkor is használhassa kapcsol új összefüggésekre.


## <a name="viewing-custom-fields"></a>Megtekintés az egyéni mezők
Az összes egyéni mezők listáját megtekintheti a kezelés csoportban a **Beállítások** csempére MOBILE irányítópult.  Jelölje ki az **adatokat** , majd az **egyéni mezők** a munkaterületen található összes egyéni mezők listáját.  

![Egyéni mezők](media/log-analytics-custom-fields/list.png)

## <a name="removing-a-custom-field"></a>Egyéni mező eltávolítása
Kétféleképpen egyéni mezőt szeretne eltávolítani.  Az első így **távolítsa el** az egyes mezők teljes listájának megtekintésekor, fent leírt módon.  A más módszer, hogy beolvashatja egy rekordot, és a gombra kattintva a mező bal oldalán.  A menü egy lehetőséget, ha el szeretné távolítani az egyéni mező lesz.

## <a name="sample-walkthrough"></a>Minta útmutató

Egyéni mező létrehozása a kész példa az alábbiakban a következő szakaszt.  Ebben a példában az első karaktertől a szolgáltatás neve a Windows-események jelölő állapota módosítása szolgáltatás.  Ez a számítógépen Windows rendszer bejelentkezés által szolgáltatáskezelő létrehozott események támaszkodik.  Kövesse az ebben a példában ha kell lennie [a rendszer Eseménynapló információk események összegyűjtési](log-analytics-data-sources-windows-events.md).

Azt adja meg a szolgáltatás kezelőjét 7036 számsort az esemény azonosítója, amely az esemény, amely jelzi a szolgáltatás elindítása és leállítása rendelkező összes eseményének térni az alábbi lekérdezés.

![Lekérdezés](media/log-analytics-custom-fields/query.png)

Akkor válassza a eseményhez azonosító 7036 számsort az egyik rekordot.

![Adatforrás-rekord](media/log-analytics-custom-fields/source-record.png)

Azt szeretné, hogy a szolgáltatás neve, amely a **RenderedDescription** tulajdonság jelenik meg, és válassza a tulajdonság melletti gombot.

![Bontsa ki a mezők](media/log-analytics-custom-fields/extract-fields.png)

A **Mező kivonása varázsló** nyitja meg, és az **Eseménynapló** és **EventID** mező ki van jelölve a **Fő példa** oszlopban.  Ez azt jelzi, hogy az egyéni mező határozza meg a rendszer naplófájlból 7036 számsort esemény azonosítójú eseményekre vonatkozóan.  Ez a elegendő, így azt nem kell minden más mező kijelölése parancsra.

![Fő példa](media/log-analytics-custom-fields/main-example.png)

Jelölje ki a nevét a szolgáltatás a **RenderedDescription** tulajdonságban azt, és használja a **szolgáltatás** azonosítja a szolgáltatás neve.  Az egyéni mező neve **Service_CF**.

![A cím mező](media/log-analytics-custom-fields/field-title.png)

Láthatja, hogy a szolgáltatás neve azonosítjuk megfelelően az egyes rekordok, de nem mások számára.   A **Keresési eredmények** mutatják, hogy a név, a **WMI Teljesítmény kártya** részét nem lett-e jelölve.  Az **összefoglaló** jeleníti meg, hogy a **DPRMA** szolgáltatással négy rekordot helytelenül tartalmaznak egy további szót, és a két rekordot **Modulok telepítője** helyett **A Windows Installer-modulok**azonosította.  

![Keresési eredmények](media/log-analytics-custom-fields/search-results-01.png)

Azt indítsa el a **WMI Teljesítmény kártya** rekordot.  Akkor kattintson a Szerkesztés ikonra, majd a **Módosítás ezt kiemelendő**.  

![Kiemelés módosítása](media/log-analytics-custom-fields/modify-highlight.png)

A kiemelés szerepeltetni az **WMI** , és futtassa a kivonat növelik azt.  

![További példa](media/log-analytics-custom-fields/additional-example-01.png)

Azt láthatja, hogy kijavította a bejegyzéseket **WMI Teljesítmény** kártya, és napló Analytics is ezeket az információkat a rekordok javítása a **Windows Installer-modul**.  Azt láthatja az **összefoglaló** szakaszában, hogy az adott **DPMRA** van még mindig nem azonosított megfelelően.

![Keresési eredmények](media/log-analytics-custom-fields/search-results-02.png)

Azt a DPMRA szolgáltatással rekordhoz görgetés és ugyanezt az eljárást segítségével javítása, a rekordot.

![További példa](media/log-analytics-custom-fields/additional-example-02.png)

 A kibontás futtatásakor azt láthatja, hogy az összes az eredmények pontosak most.

![Keresési eredmények](media/log-analytics-custom-fields/search-results-03.png)

Azt láthatja, hogy **Service_CF** hoz létre, de még nem adja hozzá azokat a rekordokat.

![Kezdeti száma](media/log-analytics-custom-fields/initial-count.png)

Egy kis időt, így új eltelte után események begyűjtési azt is, hogy, hogy, hogy a **Service_CF** mező most már hozzáadott a rekordok megfelelő a feltétel.

![Végleges eredménye](media/log-analytics-custom-fields/final-results.png)

Most már az egyéni mező, mint bármely más rekord tulajdonság használhatja azt.  Ezt mutatják be, hogy hozzunk létre egy lekérdezést, amely a csoportok, az új **Service_CF** mező alapján vizsgálja meg, hogy mely szolgáltatások legyenek a legaktívabb.

![Csoportosítási szempont lekérdezés](media/log-analytics-custom-fields/query-group.png)

## <a name="next-steps"></a>Következő lépések

- Egyéni mezők feltétel lekérdezések összeállításához megismerheti a [napló keresések](log-analytics-log-searches.md) .
- Figyelje meg elemezni, egyéni mezők használatával [egyéni naplófájlok](log-analytics-data-sources-custom-logs.md) .