<properties
    pageTitle="Értékáram-elemzés valós idejű Twitter sentiment analízis |} Microsoft Azure"
    description="Megtudhatja, hogy miként Értékáram-elemzés használata a valós idejű Twitter sentiment elemzéshez. Adatokat élő irányítópult esemény létrehozása a lépésenkénti útmutatást."
    keywords="valós idejű twitter trend elemzés, sentiment elemzés, közösségi hálózat elemzés, trend analysis példa"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="social-media-analysis-real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a>Közösségi hálózat analysis: valós idejű Twitter sentiment elemzés az Azure Értékáram-elemzés

Megtudhatja, hogy miként hozza létre közösségi hálózat elemzéséhez sentiment analysis megoldást úgy, hogy valós időben Twitter események való betöltésük Azure esemény hubok. Az Azure Értékáram-elemzés lekérdezés hozzáadva elemezheti az adatokat kell írni. Később fogja vagy a későbbi perusal eredményeinek tárolása vagy egy irányítópult és a [Power BI](https://powerbi.com/) segítségével adja meg az összefüggéseket valós időben.

Közösségi hálózat analytics eszközök segítséget nyújtanak a szervezetek trendfigyelésre témakörök, amelyek témákat és rendelkező bejegyzéseket nagy mennyiségű közösségi hálózat szokások megértéséhez. Sentiment-elemzés *véleményét adatbányászati*is nevezik, közösségi hálózat analytics eszközök alapján határozza meg a szokások felé a terméket, arról és így tovább. Valós idejű Twitter trend analysis oka az, a nagy példa a kettős keresztes címke előfizetés modell lehetővé teszi, hogy adott kulcsszavak meghallgatása, és kattintson a hírcsatorna sentiment analysis fejlesztése.

## <a name="scenario-sentiment-analysis-in-real-time"></a>Alkalmazási helyzetek: Sentiment analysis valós időben

Olyan céget, amely tartalmaz egy hírek médiaklipek webhely fölé a versenytársak előny első által közvetlenül az olvasót kapcsolódó webhelytartalom vásárolt érdekli. A vállalati közösségi hálózat analysis témakörök, amelyek a megfelelő olvasók Twitter adatok elemzése valós idejű sentiment módon használja. Pontosabban azonosítása trendfigyelésre témakörök valós idejű, a Twitteren, a vállalat szüksége van kapcsolatos tweetbe mennyiségi és sentiment valós idejű analytics kulcs témakörök. Igen lényegében van szükség sentiment analysis-elemző motor, a hírcsatorna közösségi hálózat alapján.

## <a name="prerequisites"></a>Előfeltételek
-   Fiók és [OAuth jogkivonat](https://dev.twitter.com/oauth/overview/application-owner-access-tokens) Twitter
-   [TwitterClient.zip](http://download.microsoft.com/download/1/7/4/1744EE47-63D0-4B9D-9ECF-E379D15F4586/TwitterClient.zip) a Microsoft letöltőközpontból.
-   Nem kötelező: Twitter-ügyfél az [GitHub](https://aka.ms/azure-stream-analytics-twitterclient) forráskódjának

## <a name="create-an-event-hub-input-and-a-consumer-group"></a>Hozzon létre egy esemény hubhoz beviteli és a fogyasztói csoport

A minta alkalmazás készítése eseményeket, és a leküldéses őket egy esemény hubok példánnyal (egy esemény-központban rövid). Szolgáltatás Bus esemény hubok olyan esemény bevitel Értékáram-elemzés az előnyben részesített metódusát. A [szolgáltatás Bus dokumentációja](/documentation/services/service-bus/)esemény hubok dokumentációjában találhat.

Egy esemény központ létrehozásához kövesse az alábbi lépéseket.

1.  Az Azure-portálon kattintson az **Új** > **Alkalmazás szolgáltatások** > **Szolgáltatás BUS** > **Esemény központi** > **Gyors létrehozása**, és adjon meg egy nevet, a régió és az új vagy meglévő névtér.  
2.  Az egyes megjelenítő Értékáram-elemzés feladatok legjobb módszer, olvassa el a egyszeri esemény hubok fogyasztói csoportból. Fogja ismerni azon a folyamaton, később fogyasztói a csoport létrehozása. További információ a fogyasztói csoportjai [Azure esemény hubok – áttekintés](https://msdn.microsoft.com/library/azure/dn836025.aspx)talál. A fogyasztói csoport létrehozásához nyissa meg az újonnan létrehozott esemény-központban kattintson a **Fogyasztói csoportok** fülre, kattintson a **Létrehozás** az oldal alján és majd meg kell adnia a fogyasztói csoport nevét.
3.  Engedélyt esemény-hubon keresztül csatlakozott, akkor kell hozzon létre egy megosztott hozzáférési házirendet. Kattintson a **beállítás** fülre a esemény-központját.
4.  **A megosztott ACCESS-HÁZIRENDEK**résznél hozzon létre egy új házirendet engedélyek **kezelése** .

    ![A megosztott Access házirendek, ahol létrehozhatja a házirend az engedélyek kezelése.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-ananlytics-shared-access-policies.png)

5.  A lap alján a **MENTÉS** gombra.
6.  Nyissa meg az **IRÁNYÍTÓPULT**, kattintson a **Kapcsolat ADATAIT** a lap alján, és majd másolja a vágólapra, és a kapcsolat adatainak mentése. (Használja a Másolás ikonra a keresés ikonra a megjelenő.)

## <a name="configure-and-start-the-twitter-client-application"></a>Konfigurálhatja és megkezdheti a Twitteren ügyfélalkalmazás

A [Twitter a folyamatos átvitelű API -khoz](https://dev.twitter.com/streaming/overview) gyűjthetők össze az Tweetbe események kapcsolatos témakörökre paraméteres halmazának keresztül Twitter adatokhoz csatlakozó ügyfélalkalmazás nyújtott. A [Sentiment140](http://help.sentiment140.com/) Megnyitás eszköz sentiment értéket rendelhet minden tweetbe szolgál.

- 0 = negatív
- 2 = semleges
- 4 = pozitív

Ezután Tweetbe események elküldte azokat az esemény-hubon keresztül csatlakozott.  

Kövesse ezeket a lépéseket az alkalmazás beállítása:

1.  [Töltse le a TwitterClient megoldás](http://download.microsoft.com/download/1/7/4/1744EE47-63D0-4B9D-9ECF-E379D15F4586/TwitterClient.zip).
2.  Nyissa meg a TwitterClient.exe.config, és oauth_consumer_key, oauth_consumer_secret, oauth_token és oauth_token_secret cserélje ki az értékeket tartalmazó Twitter tokenek.  

    [A lépéseket az OAuth jogkivonat készítése](https://dev.twitter.com/oauth/overview/application-owner-access-tokens)  

    Figyelje meg, hogy meg kell legyen az egy üres alkalmazás jogkivonat létrehozásához.  
3.  A TwitterClient.exe.config EventHubConnectionString és EventHubName értékek cseréje a kapcsolati karakterlánc és a nevét az esemény központi. A kapcsolati karakterlánc, amely az előbb másolt lehetővé teszi a kapcsolati karakterlánc és a nevét az esemény központi, ezért kell őket elválasztani és helyezi el a megfelelő mező minden. Például érdemes megvizsgálni a következő kapcsolati karakterláncot:

        Endpoint=sb://your.servicebus.windows.net/;SharedAccessKeyName=yourpolicy;SharedAccessKey=yoursharedaccesskey;EntityPath=yourhub

    A TwitterClient.exe.config fájl tartalmaznia kell a beállításokat a következő példának megfelelően:

        add key="EventHubConnectionString" value="Endpoint=sb://your.servicebus.windows.net/;SharedAccessKeyName=yourpolicy;SharedAccessKey=yoursharedaccesskey"
        add key="EventHubName" value="yourhub"

    Fontos tudni, hogy a szöveg "EntityPath =" jelent __nem__ jelennek meg a EventHubName értéket.

4.  *Nem kötelező:* Állítsa be a kulcsszavakat szeretne keresni.  Alapértelmezés szerint az alkalmazás "Azure Skype, XBox, Microsoft, Seattle" formátumban.  Ha azt szeretné, módosíthatja a **twitter_keywords** TwitterClient.exe.config, az értékeket.
5.  Futtassa a alkalmazás indításához TwitterClient.exe. Ekkor megjelenik a Tweetbe események értékekkel, **CreatedAt**, **témát**és **SentimentScore** elküldése az esemény-hubon keresztül csatlakozott.

    ![Sentiment analysis: esemény-hubon keresztül csatlakozott elküldött SentimentScore értékek.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-sentiment-output-to-event-hub.png)

## <a name="create-a-stream-analytics-job"></a>Értékáram-elemzés feladat létrehozása

Most, hogy Tweetbe események vannak a folyamatos átvitelű valós idejű, a Twitteren, azt beállíthatja a Értékáram-elemzés feladat hozzáadva elemezheti az események valós időben.

### <a name="provision-a-stream-analytics-job"></a>A Értékáram-elemzés feladatok rendelkezés

1.  Az [Azure-portálon](https://manage.windowsazure.com/)kattintson az **Új** > **DATA SERVICES** > **ÉRTÉKÁRAM-elemzés** > **Gyors létrehozásához**.
2.  Adja meg az alábbi értékeket, és kattintson a **ADATFOLYAM ANALYTICS projekt létrehozása**:

    * **Feladat neve**: írja be a feladat nevét.
    * **Régió**: Jelölje ki a tartományt, amelyhez a feladat futtatása. Fontolja meg úgy, hogy a feladatot, és az esemény-központban ugyanabban a régióban ahhoz, hogy jobb teljesítményt és annak érdekében, hogy Ön fog nem lehet fizetésre adatátvitel területek között.
    * **TÁRTERÜLET-fiók**: válassza ki a kívánt terület belül futnak Értékáram-elemzés feladatokat felügyeleti adatok tárolására Azure tárterület-fiókot. Ha a beállítás, válasszon ki egy meglévő tárterület-fiókot vagy hozzon létre egy újat.

3.  **ÉRTÉKÁRAM-elemzés** kattintson a bal oldali ablaktáblában a Értékáram-elemzés feladatok listáját.  
    ![Adatfolyam-elemző szolgáltatás ikon](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-service-icon.png)

    Az új feladat **LÉTREHOZVA**állapotban fog megjelenni. Figyelje meg, hogy a lap alján kattintson a **START** gomb nem érhető el. A feladat előtt meg kell adnia a feladat beviteli, a kimeneti és a lekérdezés.


### <a name="specify-job-input"></a>Adja meg a projekt beviteli
1.  A Értékáram-elemzés feladatok a **bemeneti adatok alapján** , a lap tetején kattintson, és kattintson a **BEVITELI hozzáadása**gombra. A megnyíló párbeszédpanelen azt ismerteti, hogy állítsa be a bemeneti számos keresztül.
2.  Kattintson a **ADATFOLYAM**, és kattintson a jobb oldali gombra.
3.  Kattintson az **Esemény központi**, és kattintson a jobb oldali gombra.
4.  Írja be vagy válassza ki az alábbi értékeket, a harmadik lapon:

    * **BEVITELI ALIAS**: Adjon meg egy rövid nevet, a bemeneti, például *TwitterStream*projektre vonatkozóan. Figyelje meg, hogy fog használja ezt a nevet a lekérdezés később.
    **Esemény központi**: Ha az Ön által létrehozott események-központban, a megjelenítő Értékáram-elemzés feladatot azonos előfizetésben, jelölje be a névtér, amely az esemény-központban.

        Ha az esemény központi másik előfizetést, kattintson a **Használat esemény központi egy másik előfizetésből**, és írja manuálisan **Szolgáltatás BUS NÉVTERE**, **Esemény központi neve**, **Esemény központi házirend neve**, **Esemény központi házirend kulcs**és **Események központi PARTÍCIÓT száma**adatait.

    * **Esemény központi neve**: válassza ki a nevét az esemény-központban.

    * **Esemény központi házirend neve**: Jelölje ki az esemény központi házirendet, az oktatóprogram korábbi részében létrehozott.

    * **Esemény központi fogyasztói csoport**: írja be a csoport nevét, a fogyasztói az oktatóprogram korábbi részében létrehozott.
5.  Kattintson a jobb gombbal.
6.  Adja meg az alábbi értékeket:

    * **Esemény SZERIALIZÁLÓ formátum**: JSON
    * **KÓDOLÁS**: UTF8

7.  Kattintson a forrás hozzáadása, és ellenőrizze, hogy Értékáram-elemzés sikeresen csatlakozhat az esemény-központban **ellenőrzése** gombra.

### <a name="specify-job-query"></a>Feladat-lekérdezés megadása

Értékáram-elemzés támogatja az egyszerű, deklaráció lekérdezési átalakítások leíró modell. Többet szeretne tudni a nyelvet, lásd: az [Azure adatfolyam Analytics lekérdezési nyelv hivatkozást](https://msdn.microsoft.com/library/azure/dn834998.aspx).  Ebben az oktatóanyagban segít a tartalom-előállítás és a több lekérdezés tesztelése Twitter adatot felett.

#### <a name="sample-data-input"></a>Példa a bevitt adatoknak

A feladat tényleges adatok lekérdezése érvényesítéséhez a **MINTAADATOK** funkció használatával események kinyerése a adatfolyam, és hozzon létre egy .json fájlt, az események teszteléshez.

1.  Jelölje be az esemény központi beviteli, és kattintson a **MINTAADATOKAT** az oldal alján.
2.  A párbeszédpanelen adja meg a **KEZDŐ időpont** gyűjt adatokat, és mennyi további adatok felhasználása az **időtartam** indításához.
3.  Kattintson a **Részletek** gombra, és kattintson a **kattintson ide** hivatkozásra, töltse le és mentse a létrehozott .json fájlt.

#### <a name="pass-through-query"></a>Átadó lekérdezés
Szeretné kezdeni, hogy hajt végre egyszerű átadó lekérdezés adott projekteket esemény összes mezőjét.

1.  Kattintson a **lekérdezés** , a megjelenítő Értékáram-elemzés feladat lap tetején.
2.  A kód szerkesztő cserélje ki az eredeti lekérdezési sablon a következőket:

        SELECT * FROM TwitterStream

    Győződjön meg arról, hogy a bemeneti forrás neve megegyezik-e a bemeneti, amelyet Ön korábban megadott nevét.

3.  Kattintson a **vizsgálat** alatt a Lekérdezésszerkesztő.
4.  Nyissa meg a mintafájl .json.
5.  Kattintson a **ellenőrzése** gombra, és a találatokat a lekérdezésdefiníció alatt.

    ![Eredmények Lekérdezésdefiníció alatt jelenik meg](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sentiment-by-topic.png)

#### <a name="count-of-tweets-by-topic-tumbling-window-with-aggregation"></a>Témakörre twitterre száma: az összesítés Tumbling ablak

Hasonlítsa össze az Említések témakörök közötti számú, a megszámlálása a az Említések témakörre öt másodpercenként a [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) használjuk.

1.  A kód szerkesztő a lekérdezés módosítása:

        SELECT System.Timestamp as Time, Topic, COUNT(*)
        FROM TwitterStream TIMESTAMP BY CreatedAt
        GROUP BY TUMBLINGWINDOW(s, 5), Topic

    A lekérdezés használja az **Időbélyegző által** kulcsszó időbélyegző mező használható a időbeli kiszámítása a tartalomban. Ha ez a mező nem lett megadva, az ablakok elrendezése művelet volna hajtható végre, az egyes események megérkezett az esemény-központban időt használatával.  További [Adatfolyam Analytics lekérdezés hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)a "Érkezése Viewben alkalmazás időpont" című szakaszát.

    Ezt a lekérdezést a **System.Timestamp** tulajdonság segítségével is fér hozzá egy időbélyeg, a minden ablak végére.

2.  Kattintson a **ismétlése** alatt a Lekérdezésszerkesztő a lekérdezés eredményének megtekintéséhez.

#### <a name="identify-trending-topics-sliding-window"></a>Témakörök trendfigyelésre azonosítása: csúsztatható ablak

Témakörök trendfigyelésre azonosítása, megnézi fogja témakörök, amelyek egy adott időmennyiséget @megemlítések egy küszöbértéke cross. Ebben az oktatóanyagban alkalmazásában azt fogja ellenőrizze, hogy témakörök ismertetett több, mint 20 alkalommal az utolsó öt másodperc egy [SlidingWindow](https://msdn.microsoft.com/library/azure/dn835051.aspx)használatával.

1.  A kód szerkesztő a lekérdezés módosítása: VÁLASSZA a System.Timestamp időként, témakört: darab (*), Említések a TwitterStream IDŐBÉLYEG által CreatedAt csoport által SLIDINGWINDOW(s, 5), témakör PROBLÉMÁKAT tartalmazó cellákat SZÁMLÁLNIA (*) > 20

2.  Kattintson a **ismétlése** alatt a Lekérdezésszerkesztő a lekérdezés eredményének megtekintéséhez.

    ![Csúszó ablak lekérdezés eredménye](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-query-output.png)

#### <a name="count-of-mentions-and-sentiment-tumbling-window-with-aggregation"></a>Említések és sentiment száma: az összesítés Tumbling ablak
Lekérdezés végleges azt vizsgálatára kerül sor megtudja, hány Említések, átlag, minimum, maximum és szórása sentiment pontszám az egyes témakörök öt másodpercenként **TumblingWindow** használja.

1.  A kód szerkesztő a lekérdezés módosítása:

        SELECT System.Timestamp as Time, Topic, COUNT(*), AVG(SentimentScore), MIN(SentimentScore),
        Max(SentimentScore), STDEV(SentimentScore)
        FROM TwitterStream TIMESTAMP BY CreatedAt
        GROUP BY TUMBLINGWINDOW(s, 5), Topic

2.  Kattintson a **ismétlése** alatt a Lekérdezésszerkesztő a lekérdezés eredményének megtekintéséhez.
3.  Ez az a lekérdezést, amely az irányítópult fogjuk használni.  A lap alján a **MENTÉS** gombra.


## <a name="create-output-sink"></a>Kimeneti gyűjtő létrehozása

Most, hogy van egy esemény adatfolyam-események és átalakítás végezhet az adatfolyam lekérdezés ingest bevitel esemény központi definiált az utolsó lépésként határozza meg a projektre vonatkozóan a kimeneti gyűjtő.  Azt fogja az összesített tweetbe események írni a feladat lekérdezésből Azure Blob-tárolóhoz.  Azure SQL-adatbázishoz, Azure táblatárolóhoz is sikerült leküldéses az eredmények vagy esemény agyváltók, attól függően, hogy az adott alkalmazás szüksége van.

Kövesse az alábbi lépéseket a tároló Blob-tárolóhoz létrehozása, ha még nem rendelkezik egy:

1.  Egy meglévő tárterület-fiókot használja, vagy új tárterület-fiók létrehozása az **Új**gombra kattintva > **DATA SERVICES** > **tároló** > **Gyors létrehozása**, és kövesse a képernyőn megjelenő utasításokat.
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

## <a name="start-job"></a>Projekt indítása

Egy feladat beviteli, a lekérdezés és a kimeneti összes megadva, mert azt készen áll a Értékáram-elemzés feldolgozás elindításához.

1.  **Indítsa el** a lap alján kattintson a feladat **IRÁNYÍTÓPULT**.
2.  A megnyíló párbeszédpanelen kattintson a **Projekt KEZDÉSI idő**, és kattintson a párbeszédpanel alján a **ellenőrzése** gombra. A feladat állapota **indítása** változik, és **futó**hamarosan változik.


## <a name="view-output-for-sentiment-analysis"></a>Nézet kimeneti sentiment elemzéshez

A feladatok fut, és a valós idejű Twitter-adatfolyam feldolgozása, miután válassza ki a kimenet sentiment elemzéshez megtekintése módját. [Azure tároló Intéző](https://azurestorageexplorer.codeplex.com/) vagy az [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) például eszköz használata tekintheti meg a projekt kimeneti valós időben. További lehetőségek a [Power BI](https://powerbi.com/) segítségével az alkalmazás, amelyet fel szeretne venni egy testre szabott irányítópultot az alábbi képernyőképen hasonló bővítése.

![Közösségi hálózat analysis: Értékáram-elemzés sentiment analysis (véleményét adatbányászati) kimeneti egy Power BI-irányítópult.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-output-power-bi.png)

## <a name="get-support"></a>Technikai támogatás kérése
További segítségért próbálja meg az [Azure Értékáram-elemzés fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Következő lépések

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)
