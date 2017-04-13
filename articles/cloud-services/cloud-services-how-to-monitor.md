<properties 
    pageTitle="Egy felhőalapú szolgáltatásba figyelése |} Microsoft Azure" 
    description="Megtudhatja, hogy miként figyelheti a felhőszolgáltatások az Azure klasszikus portál használatával." 
    services="cloud-services" 
    documentationCenter="" 
    authors="rboucher" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="robb"/>


# <a name="how-to-monitor-cloud-services"></a>Cloud Services figyelése

[AZURE.INCLUDE [disclaimer](../../includes/disclaimer.md)]

Figyelheti `key` teljesítménymutatók a felhőalapú szolgáltatások az Azure klasszikus portálon. Beállíthatja a minimális és az egyes szolgáltatások szerepkör részletes felügyeleti szintet, és testre szabhatja az ellenőrzési jelenít meg. A részletes felügyeleti adatok egy tároló fiókot, amelyhez bármikor hozzáférhet, a portálon kívüli vannak tárolva. 

Felügyeleti megjeleníti az Azure klasszikus portálon is rendkívül konfigurálható. Megadhatja, hogy a mértékek, a mértékek listája az **Monitor** lapján a figyelni kívánt, és megadhatja, hogy melyik az a **Monitor** lapra, és az irányítópult mértékek diagramok ábrázolandó mértékek. 

## <a name="concepts"></a>Fogalmak

Alapértelmezés szerint minimális figyelése hiányzik egy új felhőalapú szolgáltatásba, a szerepkörök előfordulását (virtuális gépeken futó) operációs rendszer gyűjtött teljesítmény számláló használatával. A minimális mérési módja miatt a Processzor százalékos, az adatok, adatok ki, lemez olvasható átviteli és lemezre írása átviteli korlátozódik. Részletes figyelése konfigurálásával kaphat további mértékek teljesítményadatokat belül a virtuális gépeken futó (szerepkör-példány) alapján. A részletes mérési módja miatt engedélyezése alkalmazás műveletek során előforduló problémák közelebb elemzését.

Alapértelmezés szerint a számláló teljesítményadatokat szerepkör-példányok mintát, és a szerepkör példányból 3 perc időközönként át. Részletes figyelése engedélyezésekor a számláló nyers teljesítményadatokat program összesíti az egyes szerepkör-példány és minden egyes szerepkör szerepkör-példányok között az 5 perc, az 1 órát és 12 órás időközönként. Az összesített adatok 10 nap után van törölni.

Miután engedélyezte a részletes figyelése, összesített adatokat, a táblázatok a tárterület-fiókjában vannak tárolva. Ahhoz, hogy egy szerepkör részletes figyelése, meg kell adnia diagnosztika kapcsolati karakterlánc a tárterület-fiók mutató hivatkozást. A különféle szerepköröket eltérő tárolási fiókokat is használhatja.

Figyelje meg, hogy a részletes figyelemmel kísérését lehetővé tévő növelik a tárhely költségeit adattárolás adatátviteli és tároló tranzakciók. Minimális figyelése nincs szükség a tárterület-fiók. Akkor is, ha beállítja az ellenőrzési szintjét részletes adatait a minimális felügyeleti szintre van téve mérési módja miatt nem tárolja a tárterület-fiókjában.


## <a name="how-to-configure-monitoring-for-cloud-services"></a>Útmutató: figyelése cloud Services konfigurálása

Az alábbi eljárások használatával állítsa be a részletes vagy minimális figyelése az Azure klasszikus portálon. 

### <a name="before-you-begin"></a>Első lépések

- A felügyeleti adatainak tárolására tárterület-fiók létrehozása. A különféle szerepköröket eltérő tárolási fiókokat is használhatja. További tudnivalókért lásd: Súgó a **Tárterület-fiókok**, illetve megtudhatja, [hogy miként tároló fiók létrehozása](/manage/services/storage/how-to-create-a-storage-account/).

- Azure diagnosztika engedélyezése a felhőalapú szolgáltatás szerepkörök. Olvassa el a [Diagnostics for Cloud Services konfigurálása](https://msdn.microsoft.com/library/azure/dn186185.aspx#BK_EnableBefore)című témakört.

Győződjön meg arról, hogy a diagnosztika kapcsolati karakterlánc megtalálható a szerepkör-beállításokat. Nem lehet bekapcsolása részletes felügyeletet igényel, amíg meg nem engedélyezi az Azure diagnosztika és diagnosztika kapcsolati karakterlánc szerepeltetni a szerepkör-beállításokat.   

> [AZURE.NOTE] Azure SDK 2,5 kiválasztásával projekteket nem automatikusan szerepel a diagnosztika kapcsolati karakterláncot a project-sablon. Ezek a projektek frissítenie kell a diagnosztika kapcsolati karakterlánc manuális felvétele a szerepkör-beállításokat.

**Diagnosztikai kapcsolati karakterlánc manuális hozzáadása szerepkör konfigurálása**

1. Nyissa meg a Felhőbeli szolgáltatástól projektet a Visual Studio
2. Kattintson duplán a **szerepkör** a szerepkör-Tervező megnyitásához, és válassza a **Beállítások** lapon
3. Keressen egy **Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString**nevű beállítást. 
4. Ha ez a beállítás nem szerepel a válassza a vegye fel a konfigurációt, és módosítsa az új beállítás típusa **ConnectionString** **Beállítás felvétele** gombra
5. Kapcsolati karakterlánc érték megadása a a **…** gombra kattintva. Ennek hatására megnyílik a megjelenő párbeszédpanelen jelöljön ki egy tároló fiókot próbál.

    ![Visual Studio beállításai](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioDiagnosticsConnectionString.png)

### <a name="to-change-the-monitoring-level-to-verbose-or-minimal"></a>Részletes vagy minimális felügyeleti szintjének módosítása

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com/)nyissa meg **a lap a felhőalapú szolgáltatás telepítéshez** .

2. **Szint**kattintson a **részletes** vagy a **minimális**. 

3. Kattintson a **Mentés**gombra.

A részletes figyelését bekapcsolása után kezdetének jelenik meg az ellenőrzési adatokat az Azure klasszikus portálon órán belül.

A számláló nyers teljesítményadatokat és felügyeleti összesített adatok a tárhely fiók által a szerepkörökhöz telepítési azonosítója minősített táblákban tárolja. 

## <a name="how-to-receive-alerts-for-cloud-service-metrics"></a>Útmutató: felhőalapú szolgáltatás mértékek vonatkozó értesítések fogadása

A felhőalapú szolgáltatás figyelése mértékek alapján értesítések fogadása Az Azure klasszikus portál **Szolgáltatások** lapján hozzon létre egy szabályt, amely figyelmeztetést jelenít meg, amikor a kiválasztott mérőszám eléri a megadott értéket. Is megadhatja, hogy az e-mailek értesítés, amikor a figyelmeztetés jelenik meg. További tudnivalókért lásd: [hogyan: értesítések fogadása és Azure-ban riasztási szabályok kezelése](http://go.microsoft.com/fwlink/?LinkId=309356).

## <a name="how-to-add-metrics-to-the-metrics-table"></a>Útmutató: mértékek elhelyezése a mértékek táblázatban

1. Az [Azure klasszikus portálon](http://manage.windowsazure.com/)nyissa meg a **Monitor** a felhőalapú szolgáltatást.

    A mértékek táblázat alapértelmezés szerint a rendelkezésre álló mérési módja miatt egy részét jeleníti meg. Az ábrán az alapértelmezett részletes mértékek egy felhőalapú szolgáltatásba, vagyis a folyamatoknál megabájt teljesítmény számláló korlátozni a szerepkör szinten összesített adatok számára. **Mértékek felvétele** segítségével válassza ki a Lync-az Azure klasszikus portálon további összesítő és szerepkör szintű mértékek.

    ![Részletes megjelenítése](./media/cloud-services-how-to-monitor/CloudServices_DefaultVerboseDisplay.png)
 
2. Mértékek hozzáadása a mértékek tábla:

    1. Kattintson a **Mértékek hozzáadása** megnyitásához **Válassza a mértékek**, alább látható módon.

        Az első elérhető metrikus ki van bontva, a rendelkezésre álló beállítások megjelenítéséhez. Minden egyes mérőszám a felső beállítás látható összesített felügyeleti összes szerepkörének. Ezeken kívül jeleníthet meg az adatokat egyéni szerepkörök adhatja meg.

        ![Mértékek hozzáadása](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics.png)

    2. Jelölje ki a mértékek megjelenítéséhez

        - Kattintson a lefelé mutató nyílra kattintva bontsa ki az ellenőrzési beállítások a mérőszám.
        - Jelölje be a jelölőnégyzetet, az egyes ellenőrzési beállítások meg szeretné jeleníteni.

        A mértékek táblában legfeljebb 50 mértékek megjelenítésére.

        > [AZURE.TIP] Részletes nyomon követésével, a mérőszámok lista metrikus tucatnyi tartalmazhat. A görgetősáv megjelenítéséhez mutasson a párbeszédpanel jobb oldalán. Szűrheti a listát, kattintson a keresés ikonra, és írja be a szöveget a keresőmezőbe a alább látható módon.
    
        ![Mértékek keresési hozzáadása](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics_Search.png)


3. Mértékek kiválasztása után kattintson az OK (be van jelölve).

    A kijelölt mérési módja miatt bekerülnek a mértékek táblázat alább látható módon.

    ![monitor mérőszámok](./media/cloud-services-how-to-monitor/CloudServices_Monitor_UpdatedMetrics.png)

 
4. Ha törölni szeretne egy mérőszám a mértékek táblából, kattintson a jelölje ki a mérőszám, és válassza a **Törlés metrikus**. (Csak látni **Metrikus törlése** a kijelölt metrikus esetén.)

### <a name="to-add-custom-metrics-to-the-metrics-table"></a>Egyéni mértékek hozzáadása a mértékek tábla

A **részletes** szintű figyelő egy listát ad az alapértelmezett mértékek, hogy figyelheti a portálon. Ezek figyelheti egyéni mértékek vagy a teljesítmény számláló határozza meg az alkalmazást a portálon keresztül.

A következő lépések feltételezik, **részletes** szintű figyelő van kapcsolva, és a van az alkalmazás összegyűjtése és egyéni teljesítmény számláló átadása konfigurálása. 

A portál jeleníthet meg az egyéni teljesítményét számláló módosítania kell a tároló vezérlő üvegvatta a konfiguráció:
 
1. Nyissa meg a a tároló vezérlő üvegvatta blob diagnosztika tárterület-fiókját. Művelet Visual Studio vagy bármely más tároló explorer használható.

    ![Visual Studio Server Explorer](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioBlobExplorer.png)

2. Nyissa meg az blob mappa elérési útját a mintázat **DeploymentId/RoleName/RoleInstance** keresése a szerepkör-példányt az adatokat. 

    ![Visual Studio tároló Explorer](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioStorage.png)
3. A szerepkör-példány a konfigurációs fájl letöltése, és frissítse az, hogy minden olyan egyéni teljesítményét számláló tartalmazza. Példa *az írási bájt/sec* figyelheti a *C meghajtó* hozzáadása a következő **PerformanceCounters\Subscriptions** csomópont alatt

    ```xml
    <PerformanceCounterConfiguration>
    <CounterSpecifier>\LogicalDisk(C:)\Disk Write Bytes/sec</CounterSpecifier>
    <SampleRateInSeconds>180</SampleRateInSeconds>
    </PerformanceCounterConfiguration>
    ```
4. A módosítások mentéséhez és a konfigurációs fájl vissza ugyanazon a helyen felülírása az blob a meglévő fájl feltöltése.
5. Váltás a klasszikus Azure portál konfigurációban részletes módra. Ha már módosította részletes üzemmódban be kell minimális, és vissza szeretne részletes válthat.
6. Az egyéni teljesítmény számláló most érhetők el a **Mértékek hozzáadása** párbeszédpanelen. 

## <a name="how-to-customize-the-metrics-chart"></a>Útmutató: a mértékek diagram testreszabása

1. A mértékek táblázatban jelölje ki a mértékek diagram ábrázolandó legfeljebb 6 mértékek. Egy mérőszám kijelöléséhez kattintson a bal oldalon a jelölőnégyzetet. Ha el szeretne távolítani egy mérőszám a mértékek diagram, törölje a jelet a mértékek tábla neve melletti jelölőnégyzetből.

    Mértékek a mértékek táblázatban jelölje ki, mint a mértékek bekerülnek a mértékek diagram. Egy keskeny megjelenítése egy **További n** legördülő lista metrikus fejlécek, amelyek nem férnek el a megjelenített tartalmazza.

 
2. Váltás a relatív értékeket (végleges értéke csak egyes mérőszám), és abszolút érték (Y tengely jelenik meg) jeleníti meg, jelölje be a relatív és abszolút tetején látható a diagramon.

    ![Relatív és abszolút](./media/cloud-services-how-to-monitor/CloudServices_Monitor_RelativeAbsolute.png)

3. A időtartomány módosítása a mértékek diagram jeleníti meg, jelölje be 1 óra elteltével a 24 óra vagy napján tetején látható a diagramon.

    ![Monitor megjelenítési időszak](./media/cloud-services-how-to-monitor/CloudServices_Monitor_DisplayPeriod.png)

    Kattintson az irányítópult mértékek diagramon a mértékek megrajzolásához módja eltérő. Mértékek terjedő érhető el, és mérőszámok vannak, illetve el a metrikus fejléc kiválasztásával.

### <a name="to-customize-the-metrics-chart-on-the-dashboard"></a>Ha testre szeretné szabni a mértékek diagram az irányítópulton

1. Nyissa meg az irányítópulton a felhőalapú szolgáltatáshoz.

2. Vegye fel vagy a diagram metrikus eltávolítása:

    - Egy új mérőszám ábrázolandó, jelölje be a jelölőnégyzetet a mérőszám a diagram fejlécek. Egy keskeny megjelenő ** *n*??metrics** ábrázolandó egy mérőszám, nem tudja megjeleníteni a diagram élőfej területén kattintson a lefelé mutató nyílra.

    - Egy mérőszám, a diagramon ábrázolt törléséhez törölje a jelet a jelölőnégyzetből, a fejléc.

3. Váltás a **relatív** és **abszolút** jeleníti meg.

4. Válassza ki az 1 óra elteltével a 24 óra, vagy az adatok megjelenítéséhez napján.

## <a name="how-to-access-verbose-monitoring-data-outside-the-azure-classic-portal"></a>Útmutató: az Access részletes kívül az Azure klasszikus portál-adatok figyelése

Táblák az egyes szerepkör megadott tároló fiókok részletes felügyeleti adatok vannak tárolva. A szerepkör minden felhőalapú szolgáltatás telepítéshez hat tábla jön létre. Két tábla minden egyes létrehozott (5 perc, 1 óra értéket, és 12 órás). Az alábbi táblázat egyik tárolja szerepkör szintű összesítések; a másik tábla szerepkör-példányok összesítések tárolja. 

A táblázatok neve a következő formátumban van:

```
WAD*deploymentID*PT*aggregation_interval*[R|RI]Table
```

Ha:

- *deploymentID* hozzárendelve a felhőalapú szolgáltatás telepítése az globálisan egyedi azonosítója

- *aggregation_interval* = 5 M, 1 H vagy 12 H

- szerepkör-szintű összesítések szabadságfokkal

- szerepkör-példányok összesítések j =

Például az alábbi táblázatokban volna adattárolásra részletes felügyeleti összesíti az 1 órás időközönként:

```
WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRTable (hourly aggregations for the role)

WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRITable (hourly aggregations for role instances)
```
