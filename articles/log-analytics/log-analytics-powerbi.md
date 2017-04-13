<properties
   pageTitle="Log Analytics adatainak exportálása a Power BI |} Microsoft Azure"
   description="A Power BI szolgáltatás, amelybe széles tárházát, és jelentéseket különféle adathalmazok elemzése a Microsoft felhőalapú üzleti analytics szolgáltatás.  Log Analytics folyamatosan adatok exportálhatók a MOBILE tárat a Power BI úgy is kihasználhatja a képi megjelenítések és elemzőeszközök együtt.  Ez a cikk ismerteti a lekérdezések beállítása a Power bi rendszeres időközönként automatikusan exportált napló Analytics."
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

# <a name="export-log-analytics-data-to-power-bi"></a>Log Analytics adatainak exportálása a Power BI

[A Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) szolgáltatás, amelybe széles tárházát, és jelentéseket különféle adathalmazok elemzése a Microsoft felhőalapú üzleti analytics szolgáltatás.  Log Analytics automatikusan adatok exportálhatók a MOBILE tárat a Power BI úgy is kihasználhatja a képi megjelenítések és elemzőeszközök együtt.

A napló Analytics konfigurálásakor a Power BI hoz létre, amely az eredmények exportálása a Power BI megfelelő adatkészleteket napló lekérdezések.  A lekérdezés és exportálás továbbra is automatikusan fut, amely csak az adatkészlet naprakészen tartása a legújabb adatokkal napló Analytics által gyűjtött az ütemezés szerint.

![A Power BI elemzést napló](media/log-analytics-powerbi/overview.png)

## <a name="power-bi-schedules"></a>A Power BI ütemtervek

*A Power BI ütemezés* , amely a MOBILE tárházba adatok exportálása a Power BI és az ütemezett, az határozza meg, milyen gyakran fut az adatkészlet naprakészen tartása a keresés megfelelő adatkészletet napló keresés tartalmazza.

Az adatkészlet mezőinek tulajdonságait a rekordok, a naplófájl keresés által visszaadott fognak egyezni.  Ha a keresés különböző típusú rekordot visszaadja az adatkészlet fogja tartalmazni összes része rekord különböző a tulajdonságot.  

> [AZURE.NOTE] Célszerű a napló keresési lekérdezést, amely visszaadja a nyers adatokból, nem pedig a bármely összevonására vonatkozóan, például a [Viszonyítási pont](log-analytics-search-reference.md#measure)parancsaival elvégzéséhez használja a legjobb megoldás.  Végezhet összesítési és a számítások a Power BI nyers adatokból.

## <a name="connecting-oms-workspace-to-power-bi"></a>Munkaterület MOBILE csatlakoztatása a Power BI

Mielőtt napló Analytics a Power bi exportálni, csatolnia kell a MOBILE munkaterület a Power BI-fiókjába, az alábbi eljárást követve.  

1. A MOBILE konzolban kattintson a **Beállítások** csempére.
2. Válassza a **fiókok**.
3. A **Munkaterület-adatok** csoportban kattintson a **Csatlakozás a Power BI-fiók**elemre.
4. Írja be a Power BI-fiók hitelesítő adatait.

## <a name="create-a-power-bi-schedule"></a>A Power BI tevékenységeinek tervezése

Hozzon létre egy Power BI ütemtervet minden adatkészlet, az alábbi eljárást követve.

1. A MOBILE konzolban kattintson a **Log keresési** csempére.
2. Írjon be egy új lekérdezést, vagy válassza ki a mentett keresés, amely visszaadja a **Power**bi exportálni kívánt adatokat.  
3. A **Power BI** gombra a **Power BI** -párbeszédpanel megnyitásához a lap tetején.
4. Adja meg az alábbi táblázat az adatokat, és kattintson a **Mentés**gombra.

| A tulajdonság | Leírás |
|:--|:--|
| név | Az ütemezés azonosítása, amikor a Power BI ütemezések listáját neve. |
| Mentett keresés | A napló keresés futtatásához.  Jelölje ki az aktuális lekérdezést, vagy a legördülő listából válassza ki a korábban mentett keresési. |
| Ütemterv | Hogy milyen időközönként futtassa a mentett keresést és a Power BI adatkészlet exportálása.  Az érték 15 percet, és a 24 óra között kell lennie. |
| Adatkészlet neve | A Power BI az adatkészlet neve.  Fog azt jön létre, ha még nem létezik, és ha létezik frissíteni. |

## <a name="viewing-and-removing-power-bi-schedules"></a>A Power BI ütemezések megtekintése és eltávolítása

A meglévő Power BI ütemtervek a következő eljárással listájának megtekintéséhez.

1. A MOBILE konzolban kattintson a **Beállítások** csempére.
2. Jelölje ki a **Power BI**.

Az ütemezés részleteinek megoldásában, mellett az ütemtervet tartalmaz a múlt héten futó hányszor és az utolsó szinkronizálás állapotának jelenik meg.  Ha a szinkronizálás talált hibákat, is a hivatkozásra kattintott a hiba részleteit tartalmazó rekordok napló keresés futtatásához.

Ütemezés az a **oszlop eltávolítása**az **X** gombra kattintva eltávolíthatja.  Válassza **ki a**letilthatja az ütemezést.  Ütemezett módosításához távolítsa el azt, és hozza létre újból a beállításait.

![A Power BI ütemtervek](media/log-analytics-powerbi/schedules.png)

## <a name="sample-walkthrough"></a>Minta útmutató
Példa a Power BI ütemezett létrehozásáról és használatáról a az adatkészlet egyszerű jelentés létrehozása az alábbiakban a következő szakaszt.  Ebben a példában a számítógépek összes teljesítményadatokat exportálja a Power BI, és akkor jön létre az vonaldiagramon processzor kihasználtsági megjelenítéséhez.

### <a name="create-log-search"></a>Keresés a napló létrehozása
Azt a napló kereshet az Office.com adatokat szeretne küldeni az adatkészlet szeretnénk létrehozásával kell kezdenie.  Ebben a példában az összes teljesítményadatokat *srv*kezdődő nevű számítógépek visszaadó lekérdezés használjuk.  

![A Power BI ütemtervek](media/log-analytics-powerbi/walkthrough-query.png)

### <a name="create-power-bi-search"></a>Power BI keresés létrehozása
Akkor kattintson a **Power BI** gombra a Power BI-párbeszédpanel megnyitásához, és adja meg a szükséges információkat.  A keresés futtatásához óránként egyszer, és hozzon létre egy adatkészlet neve a *Contoso Perf*szeretnénk.  Mivel már a keresést, nyissa meg, amely hoz létre az adatok szeretnénk, és azt hagyja meg az alapértelmezett *használata aktuális keresési* lekérdezés az **Mentett keresés**.

![A Power BI-keresés](media/log-analytics-powerbi/walkthrough-schedule.png)

### <a name="verify-power-bi-search"></a>Ellenőrizze a Power BI-keresés
Annak ellenőrzéséhez, hogy azt az ütemezés helyesen, a MOBILE irányítópult a Power BI keresések listában a **Beállítások** csempére megtekintheti azt.  Hogy Várjon néhány percet, és frissítheti a nézetet, mindaddig, amíg az azt jelenti, hogy a szinkronizálás futtatása.

![A Power BI-keresés](media/log-analytics-powerbi/walkthrough-schedules.png)

### <a name="verify-the-dataset-in-power-bi"></a>Ellenőrizze az adatkészlet Power BI szolgáltatásban
Hogy jelentkezzen be a fiókjába, a [powerbi.microsoft.com](http://powerbi.microsoft.com/) , és görgetéssel keresse meg a **adatkészleteket** , a bal oldali ablaktábla alján.  A képen látható, hogy a *Contoso Perf* adatkészlet már szerepel, jelezve, hogy az exportálás sikeresen lefut.

![A Power BI adatkészlet](media/log-analytics-powerbi/walkthrough-datasets.png)

### <a name="create-report-based-on-dataset"></a>Adatkészlet alapján jelentés létrehozása
Jelölje ki a **Contoso Perf** adatkészlet azt, és kattintson **eredmények** a **mezők** ablaktáblában megtekintheti azokat a mezőket a adatkészlet részét képező jobb oldalán.  Mutató minden számítógépen processzor kihasználtsági vonaldiagram létrehozása, akkor hajtsa végre a következő műveleteket.

1. Jelölje ki a vonal diagram megjelenítést.
2. Húzza az **Objektumnév** **szintű** jelentésszűrőhöz, és jelölje be a **processzor**.
3. Húzza a **CounterName** **szintű** jelentésszűrőhöz, és jelölje be a **% processzor**.
4. Húzza a **ellenértéknek** **értékeket**.
5. Húzza a **számítógép** **Jelmagyarázat**.
6. Húzza a **tengely** **TimeGenerated** .

A képen látható, hogy az eredményül kapott vonaldiagram jelenik meg az adatkészlet adataival.

![A Power BI vonaldiagram](media/log-analytics-powerbi/walkthrough-linegraph.png)

### <a name="save-the-report"></a>A jelentés mentése
Hogy menti a jelentést, kattintson a Mentés gombra a képernyő tetején, és ellenőrizze, hogy azt most már szerepel a jelentések csoportban kattintson a bal oldali ablaktáblában.

![A Power BI szolgáltatásban](media/log-analytics-powerbi/walkthrough-report.png)

## <a name="next-steps"></a>Következő lépések

- Tudjon meg többet [napló keresések](log-analytics-log-searches.md) a Power BI lehet exportálni lekérdezések összeállításához.
- További tudnivalók a [Power BI](http://powerbi.microsoft.com) -build napló Analytics export alapuló megjelenítéseket.
