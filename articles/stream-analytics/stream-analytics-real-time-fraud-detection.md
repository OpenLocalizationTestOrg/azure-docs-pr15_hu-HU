<properties
    pageTitle="Értékáram-elemzés: Valós idejű csalás észlelési |} Microsoft Azure"
    description="Megtudhatja, hogy miként valós idejű csalás észlelési megoldást Értékáram-elemzés létrehozása. Esemény valós idejű feldolgozás egy esemény hubhoz használatával."
    keywords="rendellenességet észlelése, csalás észlelése, valós idejű rendellenességet kimutatására"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok" />



# <a name="get-started-using-azure-stream-analytics-real-time-fraud-detection"></a>Értékáram-elemzés Azure használatának első lépései: valós idejű csalás kimutatására

Megtudhatja, hogy miként valós idejű csalás kimutatására végpont megoldás létrehozása az Azure Értékáram-elemzés. Események összhangba Azure esemény hubok, összesítési vagy riasztási adatfolyam Analytics-lekérdezéseket írni, és az eredmények küldése egy kimenet gyűjtő valós idejű feldolgozás adatok fölé az összefüggéseket eléréséhez. Valós idejű rendellenességet észlelési távközlési magyarázza, de a példa technika egyaránt alkalmas csalás észlelési hitelkártyával vagy a személyes adatok eltulajdonítását esetek például más típusú.

Értékáram-elemzés olyan teljes körű felügyelt szolgáltatás, amely alacsony-késés, könnyen hozzáférhető, méretezhető, összetett esemény feldolgozása keresztül adatfolyam a felhőben. További tudnivalókért olvassa el a [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)című témakört.


## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Alkalmazási helyzetek: Távközlési és SIM csalás észlelési valós időben

A távközlési vállalat rendelkezik a nagy mennyiségű adatot a bejövő hívásokat. A vállalat van szüksége az adatok az alábbiakat:

* Adatok csökkentése kezelhető egészt, és szerezze be az ügyfél használatáról az összefüggéseket, az idő és a földrajzi régiók.
* Valós idejű SIM csalás (ugyanazzal az identitással egyszerre köré, de földrajzilag más több hívásait) észleli, úgy, hogy a vállalat egyszerűen válaszolhat értesítésével arról, hogy ügyfelei vagy szolgáltatás leállítása.

Kanonikus dolog Internet (IoT) esetek van egy tonna telemetriai vagy érzékelők adatait. Ügyfelek az adatok összesítése vagy rendellenességeinek figyelmeztetést kap valós időben szeretne.

## <a name="prerequisites"></a>Előfeltételek

- Töltse le a Microsoft letöltőközpontból [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip)
- Nem kötelező: Az esemény létrehozója [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator) a Forráskód

## <a name="create-azure-event-hubs-input-and-consumer-group"></a>Azure esemény hubok beviteli és a fogyasztói csoport létrehozása

A minta alkalmazás eseményeket hozzon létre, és a valós idejű feldolgozás esemény hubok példányhoz leküldéses őket. Szolgáltatás Bus esemény hubok olyan esemény bevitel Értékáram-elemzés az előnyben részesített metódusát. Akkor talál további információt az [Azure Service Bus dokumentáció](/documentation/services/service-bus/)esemény hubok.

Egy esemény hubhoz létrehozása:

1.  Az [Azure-portálon](https://manage.windowsazure.com/)kattintson az **Új** > **Alkalmazás szolgáltatások** > **Szolgáltatás BUS** > **Esemény központi** > **Gyors létrehozásához**. Adjon meg egy nevet, a terület és a új vagy meglévő névtér hozzon létre egy új esemény hubon keresztül csatlakozott.  
2.  Az egyes megjelenítő Értékáram-elemzés feladatok legjobb módszer, olvassa el a egyszeri esemény központi fogyasztói csoportból. Fogja ismerni azon a folyamaton, később fogyasztói a csoport létrehozása. [További tudnivalók a fogyasztói csoportok](https://msdn.microsoft.com/library/azure/dn836025.aspx). A fogyasztói csoport létrehozásához nyissa meg az újonnan létrehozott esemény-központban kattintson a **Fogyasztói csoportok** fülre, kattintson a **Létrehozás** az oldal alján és majd meg kell adnia a fogyasztói csoport nevét.
3.  Engedélyt esemény-hubon keresztül csatlakozott, akkor kell hozzon létre egy megosztott hozzáférési házirendet. Kattintson a **beállítás** fülre a esemény-központját.
4.  **A megosztott ACCESS-HÁZIRENDEK**résznél a **MANAGE** engedélyekkel rendelkező új házirend létrehozása.

    ![A megosztott Access házirendek, ahol létrehozhatja a házirend az engedélyek kezelése.](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-shared-access-policies.png)

5.  A lap alján a **MENTÉS** gombra.
6.  Nyissa meg az **Irányítópult**, kattintson a **Kapcsolat ADATAIT** a lap alján, és majd másolja a vágólapra, és a kapcsolat adatainak mentése.

## <a name="configure-and-start-the-event-generator-application"></a>Konfigurálhatja és megkezdheti az esemény-készítő alkalmazás

Azt, amely minta bejövő hívás metaadatok készítése és leküldéses meg az esemény hubok ügyfélalkalmazás megadott. Ez az alkalmazás beállításához kövesse az alábbi lépéseket.  

1.  Töltse le a [TelcoGenerator.zip fájlt](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip), és bontsa ki az egy mappába.

    > [AZURE.NOTE] Előfordulhat, hogy a Windows blokkolja a letöltött zip-fájl. Kattintson a jobb gombbal a fájlra, és válassza a **Tulajdonságok parancsot**. Ha megjelenik "a fájl olyasvalakitől származik, másik számítógépről és, mellyel megvédheti a számítógépét blokkolhatja" üzenet, jelölje be a **Tiltás feloldása** , és kattintson a zip-fájl alkalmazni.

2.  Cserélje ki a Microsoft.ServiceBus.ConnectionString és EventHubName telcodatagen.exe.config az esemény központi kapcsolati karakterlánc és nevét.

    A kapcsolati karakterláncot az Azure portálról másolt végén helyezi el a kapcsolat nevét. Ne felejtse el eltávolítása "; EntityPath =<value>"a a" kulcs hozzáadása = "mező.

3.  Indítsa el az alkalmazást. A használatát a következőképpen történik:

    telcodatagen.exe [#NumCDRsPerHour] [SIM kártya csalás valószínűség] [#DurationHours]

A következő példa csalás 20 százalékát valószínűség 1000 események hoz létre a két óra alatt.

    telcodatagen.exe 1000 .2 2

Ekkor megjelenik az esemény hubhoz küldött rekordok. Az alábbi néhány kulcsát, azt a valós idejű csalás észlelési alkalmazás által használt meghatározása:

| Rekord | Meghatározása |
| ------------- | ------------- |
| CallrecTime | A kezdő időpont időbélyeg a híváshoz. |
| SwitchNum | Telefon kapcsoló, használja a híváshoz. |
| CallingNum | A hívó telefonszámát. |
| CallingIMSI | Nemzetközi mobil előfizetői identitás (IMSI).  A hívó egyedi azonosítója. |
| CalledNum | A hívott fél készülékén telefonszámát. |
| CalledIMSI | Nemzetközi mobil előfizetői identitás (IMSI).  A hívott fél készülékén egyedi azonosítója. |


## <a name="create-a-stream-analytics-job"></a>Értékáram-elemzés feladat létrehozása
Most, hogy elkészült a távközlési események adatfolyam, azt beállíthatja Értékáram-elemzés feladat hozzáadva elemezheti az események valós időben.

### <a name="provision-a-stream-analytics-job"></a>A Értékáram-elemzés feladatok rendelkezés

1.  Az Azure-portálon kattintson az **Új** > **DATA SERVICES** > **ÉRTÉKÁRAM-elemzés** > **Gyors létrehozásához**.
2.  Adja meg az alábbi értékeket, és kattintson a **ADATFOLYAM ANALYTICS projekt létrehozása**:

    * **Feladat neve**: írja be a feladat nevét.

    * **Régió**: Jelölje ki a tartományt, amelyhez a feladat futtatása. Fontolja meg úgy, hogy a feladatot, és az esemény-központban ugyanabban a régióban ahhoz, hogy jobb teljesítményt és annak érdekében, hogy Ön fog nem lehet fizetésre adatátvitel területek között.

    * **TÁRTERÜLET-fiók**: válassza ki a kívánt terület belül futnak Értékáram-elemzés feladatokat felügyeleti adatok tárolására Azure tárterület-fiókot. Ha a válasszon ki egy meglévő tárterület-fiókot vagy hozzon létre egy újat.

3.  **ÉRTÉKÁRAM-elemzés** kattintson a bal oldali ablaktáblában a Értékáram-elemzés feladatok listáját.

    ![Adatfolyam-elemző szolgáltatás ikon](./media/stream-analytics-real-time-fraud-detection/stream-analytics-service-icon.png)

    Az új feladat **LÉTREHOZVA**állapotban fog megjelenni. Figyelje meg, hogy a lap alján kattintson a **START** gomb nem érhető el. A feladat előtt meg kell adnia a feladat beviteli, a kimeneti és a lekérdezés.

### <a name="specify-job-input"></a>Adja meg a projekt beviteli
1.  A Értékáram-elemzés feladatok a **bemeneti adatok alapján** , kattintson a lap tetején kattintson, és kattintson a **BEVITELI hozzáadása**gombra. A megnyíló párbeszédpanelen azt ismerteti, hogy néhány lépésben állíthatja be a bemeneti.
2.  Kattintson a **ADATFOLYAM**, és kattintson a jobb oldali gombra.
3.  Kattintson az **Esemény központi**, és kattintson a jobb oldali gombra.
4.  Írja be vagy válassza ki az alábbi értékeket, a harmadik lapon:

    * **BEVITELI ALIAS**: Írjon be egy rövid nevet, például *CallStream*, ehhez a feladathoz. Figyelje meg, hogy fogja használni, akkor ezt a nevet a lekérdezésben később.

    * **Esemény központi**: Ha az Ön által létrehozott események-központban, a megjelenítő Értékáram-elemzés feladatot azonos előfizetésben, jelölje be a névtér, amely az esemény-központban.

        Ha az esemény központi másik előfizetést, jelölje be a **Használati események központi egy másik előfizetésből**, és írja manuálisan **Szolgáltatás BUS NÉVTERE**, **Esemény központi neve**, **Esemény központi házirend neve**, **Esemény központi házirend kulcs**és **Események központi PARTÍCIÓT száma**adatait.

    * **Esemény központi neve**: válassza ki a nevét az esemény-központban.

    * **Esemény központi házirend neve**: Jelölje ki az esemény központi házirendet, az oktatóprogram korábbi részében létrehozott.

    * **Esemény központi fogyasztói csoport**: írja be a csoport nevét, a fogyasztói az oktatóprogram korábbi részében létrehozott.

5.  Kattintson a jobb gombbal.
6.  Adja meg az alábbi értékeket:

    * **Esemény SZERIALIZÁLÓ formátum**: JSON
    * **KÓDOLÁS**: UTF8
7.  Kattintson a forrás hozzáadása, és ellenőrizze, hogy Értékáram-elemzés sikeresen csatlakozhat az esemény-központban **ellenőrzése** gombra.

### <a name="specify-job-query"></a>Feladat-lekérdezés megadása

Értékáram-elemzés támogatja a valós idejű feldolgozás átalakítások ismerteti, hogy egyszerű, deklaráció lekérdezés modell. Többet szeretne tudni a nyelvet, lásd: az [Azure adatfolyam Analytics lekérdezési nyelv hivatkozást](https://msdn.microsoft.com/library/dn834998.aspx). Ebben az oktatóanyagban segít a tartalom-előállítás és a több lekérdezés tesztelése a valós idejű adatfolyam hívás adatot felett.

#### <a name="optional-sample-input-data"></a>Nem kötelező: A bemeneti mintaadatokat
A feladat tényleges adatok lekérdezése érvényesítéséhez segítségével a **MINTAADATOK** funkció események kinyerése a adatfolyam, és hozzon létre egy. JSON fájl az események vizsgálathoz.  A következő lépések bemutatják, hogyan ehhez. Azt is módon biztosítva van a tesztelésre [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json) mintafájl.

1.  Jelölje be az esemény központi beviteli, és kattintson a **MINTAADATOKAT** az oldal alján.
2.  A párbeszédpanelen adja meg a **KEZDŐ időpont** gyűjt adatokat, és mennyi további adatok felhasználása az **időtartam** indításához.
3.  A **ellenőrzése** gombra kattintva indítsa el a beviteli mintavételnél adatok.  Eltarthat néhány percig is az adatfájl készítendő.  Amikor a folyamat befejeződik, kattintson a **Részletek**gombra, töltse le a létrehozott. JSON fájlt, és mentse.

    ![Töltse le és feldolgozott adatok JSON fájl mentése](./media/stream-analytics-real-time-fraud-detection/stream-analytics-download-save-json-file.png)

#### <a name="pass-through-query"></a>Átadó lekérdezés

Ha szeretne archiválni minden esemény, olvassa el a tartalom az esemény vagy az üzenet összes mezőjét átadó lekérdezés segítségével. Kövesse egy egyszerű átadó lekérdezés adott projekteket esemény összes mezőjét.

1.  Kattintson a **lekérdezés** , a megjelenítő Értékáram-elemzés feladat lap tetején.
2.  A kód szerkesztése adja hozzá a következő:

        SELECT * FROM CallStream

    > [AZURE.IMPORTANT] Győződjön meg arról, hogy a bemeneti forrás neve megegyezik-e a bemeneti, amelyet Ön korábban megadott nevét.

3.  Kattintson a **vizsgálat** alatt a Lekérdezésszerkesztő.
4.  Adjon meg egy tesztfájl témakörre. Ön által létrehozott a fenti lépésekkel használja, vagy [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/SampleDataFiles/Telco.json).
5.  Kattintson a **ellenőrzése** gombra, és a találatokat a lekérdezésdefiníció alatt látható.

    ![Lekérdezés megadása eredménye](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sim-fraud-output.png)


### <a name="column-projection"></a>Oszlop vetítés

A visszaadott mezők kevesebb beállítási most azt fogja csökkentésére.

1.  A kód szerkesztő a lekérdezés módosítása:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum
        FROM CallStream

2.  Kattintson a **ismétlése** alatt a Lekérdezésszerkesztő a lekérdezés eredményének megtekintéséhez.

    ![A Lekérdezésszerkesztő kimeneti.](./media/stream-analytics-real-time-fraud-detection/stream-analytics-query-editor-output.png)

### <a name="count-of-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Darab a bejövő hívások terület szerint: az összesítés Tumbling ablak

Hasonlítsa össze a bejövő hívások / régió száma, a bejövő hívások csoportosítva vannak **SwitchNum** öt másodpercenként megszámlálása egy [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) használjuk.

1.  A kód szerkesztő a lekérdezés módosítása:

        SELECT System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount
        FROM CallStream TIMESTAMP BY CallRecTime
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    A lekérdezés használja az **Időbélyegző által** kulcsszó időbélyegző mező használható a időbeli kiszámítása a tartalomban. Ha ez a mező nem lett megadva, az ablakok elrendezése művelet volna hajtható végre, az egyes események megérkezett az esemény-központban időt használatával. Lásd: ["Érkezése Viewben alkalmazás időpont" a Értékáram-elemzés a lekérdezési nyelv hivatkozást](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Figyelje meg, hogy az egyes ablak végére időbélyeg a **System.Timestamp** tulajdonság segítségével elérheti.

2.  Kattintson a **ismétlése** alatt a Lekérdezésszerkesztő a lekérdezés eredményének megtekintéséhez.

    ![A Timestand lekérdezés eredménye](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-query-editor-rerun.png)

### <a name="sim-fraud-detection-with-a-self-join"></a>Saját illesztés SIM csalás kimutatás

Azonosítsa az esetleg hamis használatát, megnézi fogja származnak, ugyanazt a felhasználót, de más 5 másodpercen belüli hívásokhoz.  Azt [illesztés](https://msdn.microsoft.com/library/azure/dn835026.aspx) hívás események magával keressen ezekben az esetekben az adatfolyam.

1.  A kód szerkesztő a lekérdezés módosítása:

        SELECT System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1,
        CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
        ON CS1.CallingIMSI = CS2.CallingIMSI
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum

2.  Kattintson a **ismétlése** alatt a Lekérdezésszerkesztő a lekérdezés eredményének megtekintéséhez.

    ![Lekérdezés eredményei illesztés](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-query-editor-join.png)

### <a name="create-output-sink"></a>Kimeneti gyűjtő létrehozása

Most, hogy van egy esemény adatfolyam-események és átalakítás végezhet az adatfolyam lekérdezés ingest bevitel esemény központi definiált az utolsó lépésként határozza meg a projektre vonatkozóan a kimeneti gyűjtő. Azure Blob-tárolóhoz csalárd elemeire vonatkozó események azt kell írni.

Ha még nem rendelkezik egy Blob-tárolóhoz tárolója létrehozásához kövesse az alábbi lépéseket.

1.  Egy meglévő tárterület-fiókot használja, vagy új tárterület-fiók létrehozása gombra kattintva **Új > DATA SERVICES > tároló > gyors létrehozása**, és kövesse a képernyőn megjelenő utasításokat.
2.  Jelölje ki a tárterület-fiókot **TÁROLÓK** elemre a lap tetején kattintson, és kattintson a **hozzáadni**.
3.  A tároló **nevét** , és állítsa az **ACCESS** **Nyilvános Blob**.

## <a name="specify-job-output"></a>Adja meg a projekt kimeneti

1.  A Értékáram-elemzés feladatok a **KIMENET** : a lap tetején kattintson, és válassza a **Kimeneti hozzáadása**. A megnyíló párbeszédpanelen azt ismerteti, hogy a kimeneti beállítása több lépések.
2.  Kattintson a **BLOB-TÁROLÓHOZ**, és kattintson a jobb oldali gombra.
3.  Írja be vagy válassza ki az alábbi értékeket, a harmadik lapon:

    * **Kimeneti ALIAS**: írja be a feladat kimeneti rövid nevét.
    * **ELŐFIZETÉS**: Ha az Ön által létrehozott Blob-tárolóhoz, a megjelenítő Értékáram-elemzés feladatot azonos előfizetésben, kattintson a **Használatának tárolására fiók aktuális előfizetése**. Ha a tárhely másik előfizetést, kattintson a **Használata tárterület-fiókot a másik előfizetést**, és a manuális adja meg az adatokat **Tároló partner**, a **FIÓKKULCS TÁRHELY**és a **tároló**.
    * **TÁRTERÜLET-fiók**: válassza ki a tárterület-fiók nevét.
    * **Tároló**: válassza ki a tárolóhoz nevét.
    * **Fájlnév ELŐTAG**: írja be a fájl egy előtagot, amelyet blob kimeneti írásakor.

4.  Kattintson a jobb gombbal.
5.  Adja meg az alábbi értékeket:

    * **Esemény SZERIALIZÁLÓ formátum**: JSON
    * **KÓDOLÁS**: UTF8

6.  Kattintson a forrás hozzáadása, és ellenőrizze, hogy Értékáram-elemzés is tud csatlakozni a tárterület-fiók **ellenőrzése** gombra.

## <a name="start-job-for-real-time-processing"></a>Valós idejű feldolgozás projekt indítása

Egy feladat beviteli, a lekérdezés és a kimeneti összes megadva, mert azt készen áll a valós idejű csalás kimutatására Értékáram-elemzés feladat indításához.

1.  **Indítsa el** a lap alján kattintson a feladat **IRÁNYÍTÓPULT**.
2.  A megnyíló párbeszédpanelen kattintson a **Projekt KEZDÉSI idő**, és kattintson a párbeszédpanel alján a **ellenőrzése** gombra. A feladat állapota **indítása** változik, és **futó**hamarosan változik.

## <a name="view-fraud-detection-output"></a>Nézet csalás észlelési kimeneti

[Azure tároló Intéző](http://storageexplorer.com/) vagy az [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) például eszköz használatával csalárd események megjelenítése írták őket a kimeneti valós időben.  

![Csalás észlelési: csalárd események megjelenése a valós idejű](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-view-real-time-fraudent-events.png)

## <a name="get-support"></a>Technikai támogatás kérése
További segítségért próbálja meg az [Azure Értékáram-elemzés fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Következő lépések

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)
