<properties
    pageTitle="Értékáram-elemzés – első lépések: valós idejű csalás észlelési |} Microsoft Azure"
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

Megtudhatja, hogy miként valós idejű csalás kimutatására végpont megoldás létrehozása az Azure Értékáram-elemzés. Események hozása Azure esemény-elosztóhoz, összesítési vagy riasztási adatfolyam Analytics-lekérdezéseket írni, és az eredmények küldése egy kimeneti gyűjtő, mutasson az adatokra valós idejű feldolgozás betekintést eléréséhez. Valós idejű rendellenességet észlelési távközlési vonatkozik, de a példa technika egyaránt alkalmas csalás észlelési hitelkártyával vagy a személyes adatok eltulajdonítását esetek például más típusú.

Értékáram-elemzés olyan teljes körű felügyelt szolgáltatás, alacsony-késés, könnyen hozzáférhető, méretezhető összetett esemény feldolgozása kezeléséről a felhőben adatfolyam fölé. További tudnivalókért olvassa el a [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)című témakört.


## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Alkalmazási helyzetek: Távközlési és SIM csalás észlelési a lapra

A távközlési vállalat rendelkezik a nagy mennyiségű adatot a bejövő hívásokat. A vállalat van szüksége az adatok az alábbiakat:
* Ezeket az adatokat egy kezelhető összeg lefelé pare, és szerezze be az ügyfél használatáról az összefüggéseket idő- és földrajzi régiók fölé.
* Észleli azokat az SIM csalás (több hívások egyidejűleg köré, de földrajzilag más ugyanazzal az identitással érkező) a lapra, hogy könnyen értesítésével arról, hogy ügyfelei vagy szolgáltatás leállítása is adhatnak.

Kanonikus dolog Internet (IoT) helyzetekben van egy ton telemetriai vagy érzékelő adatok generálása –, és ügyfelek szeretné összesítése őket, vagy a riasztást valós idejű rendellenességeinek fölé.

## <a name="prerequisites"></a>Előfeltételek

- Töltse le a Microsoft letöltőközpontból [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) 
- Nem kötelező: Az esemény létrehozója [GitHub](https://github.com/Azure/azure-stream-analytics/tree/master/DataGenerators/TelcoGenerator) a Forráskód

## <a name="create-an-azure-event-hubs-input-and-consumer-group"></a>Az Azure esemény hubok beviteli és a fogyasztói csoport létrehozása

A minta alkalmazás készítése eseményeket, és a valós idejű feldolgozás esemény központi példányhoz leküldéses őket. Szolgáltatás Bus esemény hubok Értékáram-elemzés az esemény bevitel a használni kívánt módot és, többet is megtudhat az [Azure Service Bus dokumentáció](/documentation/services/service-bus/)esemény hubok.

Egy esemény hubhoz létrehozása:

1.  Az [Azure-portálon](https://manage.windowsazure.com/) kattintson az **Új** > **Alkalmazás szolgáltatások** > **Szolgáltatás Bus** > **Esemény központi** > **Gyors létrehozásához**. Adjon meg egy nevet, a terület és a új vagy meglévő névtér hozzon létre egy új esemény hubon keresztül csatlakozott.  
2.  Az egyes megjelenítő Értékáram-elemzés feladatok legjobb módszer, olvassa el a csoportból egyszeri esemény központi fogyasztói. Azon a folyamaton, az alábbi fogyasztói a csoport létrehozása fogja ismerni, és a [További tudnivalók a fogyasztói csoportok](https://msdn.microsoft.com/library/azure/dn836025.aspx)is. Fogyasztói csoport létrehozásához, az újonnan létrehozott esemény-központban keresse meg és kattintson a **Fogyasztói csoportok** fülre, majd kattintson a **Create** a lap alján, és adja meg a a fogyasztói csoport nevét.
3.  Ha szeretne engedélyt adni az esemény-központban hozzon létre egy megosztott hozzáférési házirendet lesz szükség.  Kattintson a **beállítás** fülre a esemény-központját.
4.  **A megosztott Access-házirendek**résznél hozzon létre egy új házirendet engedélyek **kezelése** .

    ![A megosztott Access házirendek, ahol létrehozhatja a házirend az engedélyek kezelése.](./media/stream-analytics-get-started/stream-ananlytics-shared-access-policies.png)

5.  A lap alján a **Mentés** gombra.
6.  Nyissa meg az **Irányítópult** , és kattintson a **Kapcsolat adatait** a lap alján és majd másolja a vágólapra, és a kapcsolat adatainak mentése.

## <a name="configure-and-start-event-generator-application"></a>Konfigurálhatja és megkezdheti az esemény-készítő alkalmazás

Egy minta bejövő hívás metaadatok készítése és leküldéses, esemény-hubon keresztül csatlakozott ügyfélalkalmazás nyújtott. Kövesse az alábbi lépésekkel állíthatja be az alkalmazás.  

1.  Töltse le a [TelcoGenerator.zip fájlt](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip). Ezután csomagolja ki azt a címtárhoz.

    **Megjegyzés**: a Windows lehet letiltani a letöltött zip-fájl. Kattintson a jobb gombbal a fájlra, és válassza a Tulajdonságok parancsot. Ha az üzenet "a fájl olyasvalakitől származik, másik számítógépről és, mellyel megvédheti a számítógépét blokkolhatja." majd osztásjelek a "Tiltás feloldása" mezőbe, és kattintson az Alkalmaz gombra a zip-fájlt.

2.  Cserélje ki a Microsoft.ServiceBus.ConnectionString és EventHubName **telcodatagen.exe.config** az esemény központi kapcsolati karakterlánc és nevét.

    **Megjegyzés**: az Azure portál helyről másolja a kapcsolati karakterlánc végén a kapcsolat nevét. Távolítsa el a "; EntityPath =<value>"a Hozzáadás kulcs = mező.

3.  Indítsa el az alkalmazást. A használatát a következőképpen történik:

   telcodatagen.exe [#NumCDRsPerHour] [SIM kártya csalás valószínűség] [#DurationHours]

A következő példa 1000 események csalás 20 százalékát valószínűség hoz létre, 2 órán alatt.

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


## <a name="create-stream-analytics-job"></a>Értékáram-elemzés feladat létrehozása
Most, hogy elkészült a távközlési események adatfolyam, azt beállíthatja Értékáram-elemzés feladat hozzáadva elemezheti az események a lapra.

### <a name="provision-a-stream-analytics-job"></a>A Értékáram-elemzés feladatok rendelkezés

1.  Az Azure-portálon kattintson a **Új > Data Services > Értékáram-elemzés > gyors létrehozása**.
2.  Adja meg az alábbi értékeket, és kattintson a **Adatfolyam Analytics projekt létrehozása**:

    * **Feladat neve**: írja be a feladat nevét.

    * **Régió**: Jelölje ki a tartományt, amelyhez a feladat futtatása. Fontolja meg úgy, hogy a feladatot, és az esemény-központban ugyanabban a régióban ahhoz, hogy jobb teljesítményt és annak érdekében, hogy Ön fog nem lehet fizetésre adatátvitel területek között.

    * **Tárterület-fiók**: válassza ki az Azure tároló fut ez régión belüli összes Értékáram-elemzés feladatok nyomon adatok tárolására kívánt fiókot. Ha a beállítás, válasszon ki egy meglévő tárterület-fiókot vagy hozzon létre egy újat.

3.  **Értékáram-elemzés** kattintson a bal oldali ablaktáblában a Értékáram-elemzés feladatok listáját.

    ![Adatfolyam-elemző szolgáltatás ikon](./media/stream-analytics-get-started/stream-analytics-service-icon.png)

4.  Az új feladat **létrehozva**állapotban fog megjelenni. Figyelje meg, hogy a lap alján kattintson a **Start** gomb nem érhető el. A feladat előtt meg kell adnia a feladat beviteli, a kimeneti és a lekérdezés.

### <a name="specify-job-input"></a>Adja meg a projekt beviteli
1.  A Értékáram-elemzés feladatok a **bemeneti adatok alapján** , a lap tetején kattintson, és kattintson a **Beviteli hozzáadása**gombra. A megnyíló párbeszédpanelen azt ismerteti, hogy állítsa be a bemeneti számos keresztül.
2.  Jelölje ki a **adatfolyam**, és kattintson a jobb oldali gombra.
3.  Jelölje ki az **Esemény központi**, és kattintson a jobb oldali gombra.
4.  Írja be vagy válassza ki az alábbi értékeket, a harmadik lapon:

    * **Beviteli Alias**: Adjon meg egy rövid nevet, például *CallStream*bemeneti ehhez a feladathoz. Figyelje meg, hogy fogja használni, akkor ezt a nevet a lekérdezésben később.
    * **Esemény központi**: Ha az esemény-központban létrehozott, a megjelenítő Értékáram-elemzés feladatot azonos előfizetésben, jelölje be a névtér, amely az esemény-központban.

    Ha az esemény központi másik előfizetést, jelölje be a **Használati események központi egy másik előfizetésből** , és kézzel írja be a **Szolgáltatás Bus Namespace**, **Esemény központi neve**, **Esemény központi házirend neve**, **Esemény központi házirend kulcs**és **Események központi partíciót száma**adatait.

    * **Esemény központi neve**: válassza ki a nevét az esemény-központban.

    * **Esemény központi házirend neve**: Jelölje ki az esemény-elosztóhoz házirendet, az oktatóprogram korábbi részében létrehozott.

    * **Esemény központi fogyasztói csoport**: írja be a fogyasztói csoportot az oktatóprogram korábbi részében létrehozott.
5.  Kattintson a jobb gombbal.
6.  Adja meg az alábbi értékeket:

    * **Esemény szerializáló formátum**: JSON
    * **Kódolás**: UTF8
7.  Kattintson az ellenőrzés gombra a forrás hozzáadása, és ellenőrizze, hogy Értékáram-elemzés sikeresen csatlakozhat az esemény-központban.

### <a name="specify-job-query"></a>Feladat-lekérdezés megadása

Értékáram-elemzés egy egyszerű, deklaráció lekérdezés modell támogatja a valós idejű feldolgozás átalakítások leíró. Többet szeretne tudni a nyelvet, lásd: az [Azure adatfolyam Analytics lekérdezési nyelv hivatkozást](https://msdn.microsoft.com/library/dn834998.aspx). Ebben az oktatóanyagban segít a tartalom-előállítás és a több lekérdezés tesztelése a valós idejű adatfolyam hívás adatot felett.

#### <a name="optional-sample-input-data"></a>Nem kötelező: A bemeneti mintaadatokat
A feladat tényleges adatok lekérdezése érvényesítéséhez segítségével a **Mintaadatok** funkció események kinyerése a adatfolyam, és hozzon létre egy. JSON fájl az események vizsgálathoz.  Az alábbi lépések bemutatják, hogyan ehhez, és azt is módon biztosítva van a tesztelésre [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json) mintafájl.

1.  Jelölje be az esemény központi beviteli, majd kattintson a **Mintaadatokat** az oldal alján.
2.  A megjelenő párbeszédpanelen adja meg a **Kezdési idő** adatait, és mennyi további adatok felhasználása az **időtartam** összegyűjtése indítása.
3.  A jelölőnégyzet gombra kattintva indítsa el a beviteli mintavételnél adatok.  Eltarthat néhány percig is az adatfájl készítendő.  A folyamat befejezése után kattintson a **Részletek** gombra, és töltse le és mentse a. Létrehozott JSON-fájlt.

    ![Töltse le és feldolgozott adatok JSON fájl mentése](./media/stream-analytics-get-started/stream-analytics-download-save-json-file.png)

#### <a name="passthrough-query"></a>Átadó lekérdezés

Ha szeretne archiválni minden esemény, olvassa el a tartalom az esemény vagy az üzenet összes mezőjét átadó lekérdezés segítségével. Egy egyszerű átadó lekérdezés kezdeni, tegye a projektek esemény összes mezőjét.

1.  Kattintson a **lekérdezés** , a megjelenítő Értékáram-elemzés feladat lap tetején.
2.  A kód szerkesztése adja hozzá a következő:

        SELECT * FROM CallStream

    > Győződjön meg arról, hogy a a bemeneti forrás neve megegyezik a korábban megadott bemeneti nevét.

3.  Kattintson a **vizsgálat** alatt a Lekérdezésszerkesztő.
4.  Adja meg a tesztfájl, a program által létrehozott, a fenti lépésekkel, vagy használja a [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json).
5.  Kattintson az ellenőrzés gombra, és a találatokat a lekérdezésdefiníció alatt látható.

    ![Lekérdezés megadása eredménye](./media/stream-analytics-get-started/stream-analytics-sim-fraud-output.png)


### <a name="column-projection"></a>Oszlop vetítés

Most már azt fogja csökkentheti a visszaadott mezőket, hogy kevesebb beállítási.

1.  A kód szerkesztő a lekérdezés módosítása:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum
        FROM CallStream

2.  Kattintson a **ismétlése** alatt a Lekérdezésszerkesztő a lekérdezés eredményének megtekintéséhez.

    ![A Lekérdezésszerkesztő kimeneti.](./media/stream-analytics-get-started/stream-analytics-query-editor-output.png)

### <a name="count-of-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Darab a bejövő hívások terület szerint: az összesítés Tumbling ablak

Az összeg összehasonlítása a bejövő hívások / régió azt fogja kihasználhatja a bejövő hívások csoportosítva vannak SwitchNum 5 másodpercenként megszámlálása [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) .

1.  A kód szerkesztő a lekérdezés módosítása:

        SELECT System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount
        FROM CallStream TIMESTAMP BY CallRecTime
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    A lekérdezés használja az **Időbélyegző által** kulcsszó időbélyegző mező használható a időbeli kiszámítása a tartalomban. Ha ez a mező nem lett megadva, az ablakok elrendezése művelet volna hajtható végre használ az egyes események megérkezett esemény központi időt. Lásd: ["Érkezése Viewben alkalmazás időpont" a Értékáram-elemzés a lekérdezési nyelv hivatkozást](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Figyelje meg, hogy az egyes ablak végére időbélyeg a **System.Timestamp** tulajdonság segítségével elérheti.

2.  Kattintson a **ismétlése** alatt a Lekérdezésszerkesztő a lekérdezés eredményének megtekintéséhez.

    ![A Timestand lekérdezés eredménye](./media/stream-analytics-get-started/stream-ananlytics-query-editor-rerun.png)

### <a name="sim-fraud-detection-with-a-self-join"></a>Saját illesztés SIM csalás kimutatás

Azonosítsa az esetleg hamis használatát megnézi fogja ugyanazt a felhasználót, de más származó 5 másodpercen belüli hívásokhoz.  Azt [illesztés](https://msdn.microsoft.com/library/azure/dn835026.aspx) hívás események magával keressen ezekben az esetekben az adatfolyam.

1.  A kód szerkesztő a lekérdezés módosítása:

        SELECT System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1,
        CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
        ON CS1.CallingIMSI = CS2.CallingIMSI
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum

2.  Kattintson a **ismétlése** alatt a Lekérdezésszerkesztő a lekérdezés eredményének megtekintéséhez.

    ![Lekérdezés eredményei illesztés](./media/stream-analytics-get-started/stream-ananlytics-query-editor-join.png)

### <a name="create-output-sink"></a>Kimeneti gyűjtő létrehozása

Most, hogy van egy esemény adatfolyam-esemény elosztót átalakítás végezhet az adatfolyam ingest események és lekérdezés a szövegbeviteli definiált az utolsó lépésként határozza meg a projektre vonatkozóan a kimeneti gyűjtő.  Események csalárd elemeire vonatkozó Blob-tárolóhoz azt kell írni.

Hajtsa végre az alábbi lépéseket követve Blob-tárolóhoz tárolója létrehozása, ha még nem rendelkezik egy.

1.  Egy meglévő tárterület-fiókot használja, vagy új tárterület-fiók létrehozása gombra kattintva **Új > DATA SERVICES > tároló > gyors létrehozása** és a képernyőn megjelenő utasításokat követve.
2.  Jelölje ki a tárterület-fiókot **TÁROLÓK** elemre a lap tetején kattintson, és kattintson a **hozzáadni**.
3.  Adjon meg egy **NEVET** a tároló, és jelölje be a **hozzáférés** nyilvános Blob.

## <a name="specify-job-output"></a>Adja meg a projekt kimeneti

1.  A Értékáram-elemzés feladatok a **kimeneti** a lap tetején kattintson, és válassza a **Kimeneti hozzáadása**gombra. A megnyíló párbeszédpanelen azt ismerteti, hogy a kimeneti beállításához számos keresztül.
2.  Jelölje ki a **BLOB-TÁROLÓHOZ**, és kattintson a jobb oldali gombra.
3.  Írja be vagy válassza ki az alábbi értékeket, a harmadik lapon:

    * **Kimeneti ALIAS**: írja be a feladat kimeneti rövid nevét.
    * **ELŐFIZETÉS**: Ha a létrehozott Blob-tárolóhoz, a megjelenítő Értékáram-elemzés feladatot azonos előfizetésben, jelölje be a **Használatának tárolására fiók aktuális előfizetése**. Ha a tárhely másik előfizetést, válassza a **Use tároló Account egy másik előfizetésből** , és kézzel írja be a **TÁRTERÜLET-FIÓKOT**, **Tároló FIÓKKULCS** **tároló**adatait.
    * **TÁRTERÜLET-fiók**: válassza ki a tárterület-fiók nevét.
    * **Tároló**: válassza ki a tárolóhoz nevét.
    * **Fájlnév ELŐTAG**: Írjon be egy fájl előtagot blob kimeneti írásakor használni.

4.  Kattintson a jobb gombbal.
5.  Adja meg az alábbi értékeket:

    * **Esemény SZERIALIZÁLÓ formátum**: JSON
    * **KÓDOLÁS**: UTF8

6.  Kattintson az ellenőrzés gombra a forrás hozzáadása, és ellenőrizze, hogy Értékáram-elemzés sikeresen a tárhely fiókhoz csatlakozhat.

## <a name="start-job-for-real-time-processing"></a>Valós idejű feldolgozás projekt indítása

Egy feladat beviteli, a lekérdezés és a kimeneti összes megadva, mivel vagyunk készen áll a kezdésre a valós idejű csalás észlelési Értékáram-elemzés feladatot.

1.  **Indítsa el** a lap alján kattintson a feladat **IRÁNYÍTÓPULT**.
2.  A megjelenő párbeszédpanelen jelölje be a **Projekt KEZDÉSI idő** , és válassza az ellenőrzés gombra a párbeszédpanel alján lévő. A feladat állapota **indítása** változik, és **futó**hamarosan helyezi.

## <a name="view-fraud-detection-output"></a>Nézet csalás észlelési kimeneti

[Azure tároló Intéző](https://azurestorageexplorer.codeplex.com/) vagy az [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) például eszköz használatával írták őket a kimeneti valós idejű csalárd események megjelenítése.  

![Csalás észlelési: csalárd események megjelenése a valós idejű](./media/stream-analytics-get-started/stream-ananlytics-view-real-time-fraudent-events.png)

## <a name="get-support"></a>Technikai támogatás kérése
További segítségért próbálja meg az [Azure Értékáram-elemzés fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Következő lépések

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)
