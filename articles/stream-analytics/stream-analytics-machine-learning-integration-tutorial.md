<properties
    pageTitle="Azure Értékáram-elemzés és Azure gépi tanulási sentiment analysis |} Microsoft Azure"
    description="Értékáram-elemzés feladatot egy felhasználó által definiált függvény és gépi tanulási használatáról"
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>


<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="10/04/2016" 
    ms.author="jeffstok"
/>

# <a name="sentiment-analysis-by-using-azure-stream-analytics-and-azure-machine-learning"></a>Azure Értékáram-elemzés és Azure gépi tanulási sentiment elemzése #

Ez a cikk segít pillanatok alatt beállíthatnak egy egyszerű Azure Értékáram-elemzés feladat Azure gépi tanulási integrációval lett tervezve. Fog sentiment analytics gépi tanulási modell a Cortana üzletiintelligencia-gyűjteményből adatfolyam szöveges adatok elemzése céljából, és használjuk határozza meg a sentiment pontszám valós időben. Az információk ebben a cikkben segíthetnek forgatókönyvek, például a Twitteren adatfolyam valós idejű sentiment analytics megértéséhez, az ügyfélszolgálati munkatársak ügyfél csevegések rekordokat elemzése és megjegyzések fórumok, blogok és videók túl sok más valós idejű, prediktív pontozási forgatókönyvek felmérése.

Ez a cikk a minta CSV-fájl szöveggel felajánlja a bemenetként az Azure blobtárolóhoz, az alábbi képen látható. A feladat érinti a sentiment analytics modell (UDF) felhasználó által definiált függvény a szöveg mintaadatok a blob-tárolóból. Ennek eredményeképpen a CSV-fájlban azonos blob-tárolóban lévő kerül. 

![Értékáram-elemzés számítógép-oktatás](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

Az alábbi képen az ebben a konfigurációban mutatja be. További reálisan forgatókönyvön Blob-tárolóhoz cserélje a folyamatos átvitelű az Azure esemény hubok beviteli Twitter adatainak. Emellett az összesítő sentiment a [Microsoft Power BI](https://powerbi.microsoft.com/) valós idejű adatábrázolás sikerült generál.    

![Értékáram-elemzés számítógép-oktatás](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a>Előfeltételek

Ebben a cikkben vannak igazolni-feladatok végrehajtásával előfeltételei a következők:

-   Azure active előfizetés.
-   Néhány adatot tartalmazó CSV-fájlba. Le tudják tölteni a fájlt a [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample Data/sampleinput.csv)jelenik meg az 1, vagy létrehozhat saját fájlt. Ez a cikk feltételezzük, hogy használja-e megadva GitHub tölthet le azt.

Magas szintű a feladatokat, ebben a cikkben igazolni fogja az alábbi műveletekre van:

1.  A beviteli CSV-fájl feltöltése Azure Blob-tárolóhoz.
2.  Sentiment analytics modell a Cortana üzletiintelligencia-dokumentumtárból adhat az Azure gépi tanulási munkaterület.
3.  Ez a modell üzembe webes szolgáltatásként a gépi tanulási munkaterületen.
4.  Hozzon létre egy Értékáram-elemzés feladat, amely felhívja a webszolgáltatás függvény, a bemeneti szöveg sentiment határozza meg.
5.  Indítsa el a Értékáram-elemzés feladatot, és tekintse meg az a kimenet.

## <a name="upload-the-csv-input-file-to-blob-storage"></a>A beviteli CSV-fájl feltöltése Blob-tárolóhoz

Ebben a lépésben bármely CSV-fájlt, például egy korábban megadott GitHub letölthető is használhatja. [Azure tároló Explorer](http://storageexplorer.com/) vagy a Visual Studio használhatja az feltölteni a fájlt, vagy használhatja az egyéni kódot. Példák a Visual Studio alapján használjuk.

1.  A Visual Studióban, kattintson az **Azure** > **tároló** > **Külső tároló csatolni**. Adja meg a **fiók nevét** , és a **Fiókkulcs**.  

    ![A gép tanulási, Server Explorer Értékáram-elemzés](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-server-explorer.png)  

2.  Bontsa ki az 1 csatolt tárolására, **Blob-tároló létrehozása**gombra, és írjon be egy logikai nevet. Miután létrehozta a tároló, a megnyitáshoz tartalmának megtekintéséhez. (Az üres lesz ezen a ponton).  

    ![Adatfolyam-elemző gépi tanulási, blob létrehozása](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-create-blob.png)  

3.  A CSV-fájl feltöltése, kattintson a **Feltöltés Blob**, és válassza a **fájl a helyi lemezről**.  

## <a name="add-the-sentiment-analytics-model-from-the-cortana-intelligence-gallery"></a>A sentiment analytics modell hozzáadása a Cortana üzletiintelligencia-dokumentumtárból

1.  Töltse le a Cortana üzletiintelligencia-gyűjtemény a [cserélendő sentiment analytics modell](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) .  
2.  A gépi tanulási Studióban kattintson a **Megnyitás Studio**lehetőséget.  

    ![Adatfolyam-elemző gépi tanulási, nyissa meg a gépi tanulási Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3.  Nyissa meg a munkaterületet jelentkezzen be. Jelölje ki azt a helyet, amely a leginkább illik a saját hely.
4.  A lap alján a **Futtatás** gombra.  
5.  A folyamat sikeresen futtatása után kattintson a **Webes szolgáltatás telepítése**.
6.  A sentiment analytics modell használatára. Ellenőrizendő, kattintson a **vizsgálat** gombra, és adja meg a szövegbevitel, például "Microsoft hasznos lehet." A próba kell adja vissza az eredményt az alábbihoz hasonló:

`'Predictive Mini Twitter sentiment analysis Experiment' test returned ["4","0.715057671070099"]...`  

![Adatfolyam-Analytics gépi tanulási, adatok elemzése](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-analysis-data.png)  

Az **alkalmazások** oszlopban hivatkozásra az **Excel 2010 és a munkafüzet korábbi** az API kulcs beszerzése vagy kell később beállítása, a megjelenítő Értékáram-elemzés feladat URL-CÍMÉT. (Ezt a lépést csak használatához szükséges gépi tanulási modell egy másik Azure-fiók-munkaterületről. Ez a cikk azt feltételezi, hogy ez a helyzet, az eset megoldására.)  

Megjegyzés: a webes szolgáltatás URL-CÍMEK és az access billentyűt az Excel letöltött fájlból, alább látható módon:  

![Adatfolyam-Analytics gépi tanulási, fontos](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  

## <a name="create-a-stream-analytics-job-that-uses-the-machine-learning-model"></a>Értékáram-elemzés feladat, amely a gépi tanulási modellt létrehozása

1.  Nyissa meg az [Azure-portálon](https://manage.windowsazure.com).  
2.  Kattintson az **Új** > **Data Services** > **Értékáram-elemzés** > **gyors létrehozásához**. Adja meg a projekt nevét a **Projekt neve**, írja be a megfelelő terület **területen**a feladathoz, és válassza a fiók **területi figyelése tárhelyet**fiókjában.    
3.  A **ráfordítások** lapon a feladat létrehozását követően kattintson a **Hozzáadás a bemeneti**.  

    ![Adatfolyam-elemző gépi tanulási, és adja meg a gépi tanulási beviteli](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-input-screen.png)  

4.  A **Beviteli hozzáadása** varázsló első lapján kattintson a **adatfolyam**, és kattintson a **Tovább gombra**. A következő lapon kattintson a **Blob-tárolóhoz** , mint a bemeneti elemre, és kattintson a **Tovább gombra**.  
5.  A varázsló a **Blob-tároló beállításai** lapon adja meg a tárhely blob tároló fióknevet korábban megadott során feltöltött az adatokat. Kattintson a **Tovább**gombra. **Esemény szerializálási formátum**kattintson a **CSV**. Fogadja el a többi a **szerializálási beállítások** lap alapértelmezett értékei. Kattintson az **OK gombra**.  
6.  A **kimeneti értékeket** lapon kattintson a **Hozzáadás a kibocsátás**gombra.  

    ![Adatfolyam-elemző gépi tanulási, és adja meg a kimeneti](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-output-screen.png)  

7.  Kattintson a **Blob-tárolóhoz**, és írja be a paramétereket, kivéve a tároló. A "teszt" nevű tároló olvassák, ahol az **CSV** -fájl feltöltése **bemeneti** érték van beállítva. A **Kimenet**írja be a "testoutput". Tároló kell lenniük, egyéb. Győződjön meg arról, hogy van-e a tároló.     
8.  Kattintson a **Tovább** gombra a kimenet **szerializálási beállításainak**konfigurálása. **Beviteli**a **CSV**gombra, és kattintson az **OK** gombra.
9.  A **függvények** lapon kattintson a **Hozzáadás gépi tanulási függvény**gombra.  

    ![Adatfolyam-elemző gépi tanulási, és adja meg a gépi tanulási függvény](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-ml-function.png)  

10. A **Gépi tanulási webszolgáltatás beállításai** lapon keresse meg a gépi tanulási munkaterület, webes szolgáltatás és az alapértelmezett végpont. Ez a cikk a manuális beállítása bármelyik munkaterületi, webes szolgáltatás, feltéve, hogy az URL-címet, és az API-ja kulcs van ismerős eléréséhez a beállítások alkalmazása Adja meg a végpont **URL-cím** és az **API-ja kulcs**. Kattintson az **OK gombra**.    

    ![Adatfolyam-Analytics gépi tanulási, gépi tanulási webszolgáltatás](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-ml-web-service.png)    

11. A **lekérdezés** lapon a lekérdezés módosításához az alábbi képlettel történik:    

 ```
    WITH subquery AS (  
        SELECT text, sentiment(text) as result from input  
    )  
 
    Select text, result.[Scored Labels]  
    Into output  
    From subquery  
 ```    
12. Kattintson a **Mentés** mentse a lekérdezést.

## <a name="start-the-stream-analytics-job-and-observe-the-output"></a>Indítsa el a Értékáram-elemzés feladatot, és tekintse meg az a kimenet

1.  Kattintson a **Start** a feladat lap alján.
2.  A **Lekérdezés párbeszédpanel indítása**kattintson az **Egyéni idő**, és majd válassza ki a megfelelő előtt, amikor a CSV-fájlok feltöltött Blob-tárolóhoz. Kattintson az **OK gombra**.  

    ![Adatfolyam Analytics gépi tanulási, egyéni ideje](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-custom-time.png)  

3.  Nyissa meg a Blob-tárolóhoz a CSV-fájlban, például a Visual Studio feltöltéséhez használt eszköz használatával.
4.  Néhány perc után azonnal elindul a feladatot, a kimeneti tároló létrejön, és a CSV-fájl van feltöltve.  
5.  Nyissa meg a fájlt az alapértelmezett CSV-szerkesztőben. Az alábbihoz hasonló jelennek meg:  

    ![Adatfolyam Analytics gépi tanulási, CSV megtekintése](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  

## <a name="conclusion"></a>Elfogadásáról

Ez a cikk bemutatja, hogyan hozhat létre, a megjelenítő Értékáram-elemzés feladat, amely adatfolyam szöveges adatokat olvas be, és a valós idejű adatok sentiment analytics vonatkozik. Az összes elvégezhető anélkül, hogy a menő bemutatása sentiment analytics modell tudnivalóit aggódnia. Az egyik előnye a Cortana üzletiintelligencia-programcsomag.

Azure gépi tanulási függvény kapcsolatos mértékek is megtekintheti. Ehhez kattintson a **Monitor** fülre. Három függvény kapcsolatos mértékek jelennek meg.  

- **Függvény kérések** összehívások a gépi tanulási webszolgáltatás számát jelzi.  
- **Függvény-események** azt jelzi, hogy a kérelem események száma. Alapértelmezés szerint a gépi tanulási webszolgáltatás minden kérés legfeljebb 1000 eseményeket tartalmazza.  
- **Sikertelen függvény kérelmek** azt jelzi, hogy a gépi tanulási webszolgáltatás sikertelen kérelmek számát.  

    ![Adatfolyam Analytics gépi tanulási, gépi tanulási monitor megtekintése](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-ml-monitor-view.png)  
