<properties
   pageTitle="Adatfolyam-adatfolyam Analytics adatainak tó tárolóba |} Azure"
   description="Adatfolyam-adatok Azure adatok tó tárolóba Azure Értékáram-elemzés használatával"
   services="data-lake-store,stream-analytics" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/07/2016"
   ms.author="nitinme"/>

# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a>Adatfolyam adatainak Azure tároló Blob tó tárolóba Azure Értékáram-elemzés használata

Ebben a cikkben megtanulhatja az Azure tó adattár használatáról olyan eredménye megegyezik egy Azure Értékáram-elemzés projektre vonatkozóan. Ez a cikk bemutatja, hogyan egyszerű példa adatokat olvas be egy tároló Azure blob (beviteli), és az adatokat ír adattár tó (kibocsátás).

>[AZURE.NOTE] Most létrehozása és konfigurálása adatfolyam elemzéséhez tó adattár kimeneti értékeket támogatott csak az [Azure klasszikus portálon](https://manage.windowsazure.com). Ebben az oktatóanyagban egyes részeinek emiatt az Azure klasszikus portál fogja használni.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

- **Engedélyezze az Azure előfizetés** tó áruházból nyilvános Adatvillámnézet. [Útmutatást](data-lake-store-get-started-portal.md#signup)találhat.

- **Azure tárterület-fiókot**. Értékáram-elemzés feladat adatok beviteléhez blob-tároló ebben a fiókban fogja használni. Az ebben az oktatóanyagban feltételezik **datalakestoreasa** és a fiók neve **datalakestoreasacontainer**belül tároló nevű tároló fiók létrehozása. Miután létrehozta a tároló, töltse fel a adatfájl – minta azt. Az [Azure adatok tó mely számjegy tárházba](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt)származó adatok mintafájl elérheti. Különböző ügyfelek esetén például az [Azure tároló Explorer](http://storageexplorer.com/)blob-tárolóhoz feltölteni az adatokat is használhatja.

    >[AZURE.NOTE] Ha a fiók létrehozása az Azure portálról ellenőrizze hoz létre a **Klasszikus** telepítési modell. Ezzel biztosíthatja, hogy a tárterület-fiókot is elérhető az Azure klasszikus portálról, mert ez mi használjuk Értékáram-elemzés feladat létrehozásához. Tárterület-fiók létrehozása az Azure portálról a klasszikus példányban használatával kapcsolatos útmutatásért lásd: [Azure tároló fiók létrehozása](../storage/storage-create-storage-account/#create-a-storage-account).
    >
    > Másik lehetőségként úgy lehetett létrehozni, a tárterület-fiókot az Azure klasszikus portálról.

- **Azure tó adattár fiókot**. Kövesse az utasításokat az [első lépések az Azure tó adattár az Azure-portálon](data-lake-store-get-started-portal.md).  


## <a name="create-a-stream-analytics-job"></a>Adatfolyam Analytics feladat létrehozása

Indítsa el a Értékáram-elemzés feladat, amely tartalmaz egy beviteli forrás- és a kimeneti létrehozásával. Az ebben az oktatóanyagban a forrás-Azure blob-tárolóhoz pedig a cél tó adattár.

1. Jelentkezzen az [Azure klasszikus portálon](https://manage.windowsazure.com).

2. Kattintson a bal alsó részén a képernyőn, **Új**, a **Data Services**, a **Megjelenítő Értékáram-elemzés**, **Gyors létrehozása**. Alább látható módon adja meg a értékeket, és kattintson az **Adatfolyam Analytics feladat létrehozása**gombra.

    ![Adatfolyam Analytics feladat létrehozása] (./media/data-lake-store-stream-analytics/create.job.png "Értékáram-elemzés feladat létrehozása")

## <a name="create-a-blob-input-for-the-job"></a>Hozzon létre egy Blob beviteli a projekthez

1. Nyissa meg a lapot, a megjelenítő Értékáram-elemzés projektre vonatkozóan, kattintson a **bemenetben** fülre, és kattintson a **Hozzáadás bemenetként** varázsló elindításához.

2. **Hozzáadása a projekthez bemenetként** lapon jelölje be a **adatfolyam**, és kattintson a továbbítás mutató nyílra.

    ![A feladat egy bemeneti hozzáadása] (./media/data-lake-store-stream-analytics/create.input.1.png "A feladat egy bemeneti hozzáadása")

3. **Hozzáadása a projekthez adatfolyam** lapon jelölje be a **Blob-tárolóhoz**, és kattintson a továbbítás mutató nyílra.

    ![A feladat adatfolyam hozzáadása] (./media/data-lake-store-stream-analytics/create.input.2.png "A feladat adatfolyam hozzáadása")

4. A **Blob-tároló beállításai** lapon adja meg a használni kívánt szövegbeviteli adatforrásként blob-tárolóhoz részletek.

    ![Adja meg a blob tárhely beállításai] (./media/data-lake-store-stream-analytics/create.input.3.png "Adja meg a blob tárhely beállításai")

    * **Beviteli alias meg az ENTER billentyűt**. Ez az Ön megadja-e a tevékenységen a szövegbeviteli egyedi nevet.
    * **Jelölje ki a tárterület-fiókját**. Győződjön meg arról, akkor merülnek fel, amelyekre az adatok áthelyezésére területek között további költség, vagy a tárterület-fiókot, a megjelenítő Értékáram-elemzés feladatot azonos régióban.
    * **Tárterület tároló Provide**. Megadhatja, hogy hozzon létre egy új tároló, vagy jelöljön ki egy meglévő tárolót.

    Kattintson a továbbítás nyílra.

5. A **szerializálási beállítások** lapon értéke lehet **CSV**, **lapon**, elválasztó **UTF8**kódolással szerializálási formátum, és válassza a a bejelölést.

    ![Adja meg a szerializálási beállításait] (./media/data-lake-store-stream-analytics/create.input.4.png "Adja meg a szerializálási beállításait")

6. Ha a varázsló végzett, a blob bemeneti, hozzáadódik az **bemenetben** lapon, és a **diagnosztikai** oszlop jelenítsen meg **az OK gombra**. A kapcsolatot a bemeneti tesztelheti a **Kapcsolat tesztelése** gombra a képernyő alján segítségével közvetlenül is.

## <a name="create-a-data-lake-store-output-for-the-job"></a>Hozzon létre egy tó adattár kimenet a projekthez

1. Nyissa meg a lapot, a megjelenítő Értékáram-elemzés projektre vonatkozóan, kattintson a **kimeneti értékeket** fülre, és kattintson a **Hozzáadás olyan eredménye** egy varázsló elindításához.

2. **Hozzáadása a projekthez olyan eredménye** lapon jelölje be a **Tó adattár**, és kattintson az előre nyíl.

    ![A feladat egy kimenet hozzáadása] (./media/data-lake-store-stream-analytics/create.output.1.png "A feladat egy kimenet hozzáadása")

3. **Engedélyezés kapcsolat** kattintson a lap, ha már létrehozott egy tó adattár fiókot, **Most már engedélyezni**. Egyéb esetben kattintson a **Regisztráció most** hozzon létre egy új fiókot. Az ebben az oktatóanyagban tudassa velünk feltételezik, hogy már rendelkezik egy tó adattár fiókkal létrejönnek (a előfeltétel említett). Lesz automatikusan engedélyezett a hitelesítő adatokat, amelyekkel jelentkezett be az Azure klasszikus Portal segítségével.

    ![Tó adattár engedélyezése] (./media/data-lake-store-stream-analytics/create.output.2.png "Tó adattár engedélyezése")

4. Az **Adatok tó tár beállításai** lap adja meg az adatokat a képernyőképet alább látható módon.

    ![Beállítások megadása tó adattárhoz] (./media/data-lake-store-stream-analytics/create.output.3.png "Beállítások megadása tó adattárhoz")

    * **Kimeneti alias meg az ENTER billentyűt**. Ez a egy egyedi nevet ad a feladat eredménye.
    * **Adja meg egy tó adattár fiókot**. Érdemes már korábban elkészült, a előfeltétel említett.
    * **Adja meg az elérési út előtag mintát**. Ez a kimeneti fájlokat, a megjelenítő Értékáram-elemzés feladat tó adattár írt azonosítása szükséges. Mivel a kimeneti értékeket a feladat által írt módosult globálisan egyedi azonosítója formátumban, előtaggal együtt segítséget nyújt az írásbeli kimeneti azonosítása. Ha azt szeretné, amelyet fel szeretne venni egy dátum- és időbélyeg az előtag részeként felejtse el beírni `{date}/{time}` előtag szerkezetében. Ha ez, a **Dátum **és **Idő formátum** mezők engedélyezett, és a formátum megválasztott.

    Kattintson a továbbítás nyílra.

5. A **szerializálási beállítások** lapon értéke lehet **CSV**, **lapon**, elválasztó **UTF8**kódolással szerializálási formátum, és válassza a a pipa ikonra.

    ![Adja meg a kimeneti formátum] (./media/data-lake-store-stream-analytics/create.output.4.png "Adja meg a kimeneti formátum")

6. Ha a varázsló végzett, a tó adattár eredménye megjelenik a **kimeneti értékeket** lapon, és a **diagnosztikai** oszlop jelenítsen meg **az OK gombra**. A kapcsolat a kimenet tesztelheti a **Kapcsolat tesztelése** gombra a képernyő alján használatával közvetlenül is.

## <a name="run-the-stream-analytics-job"></a>Értékáram-elemzés feldolgozás

Értékáram-elemzés feladat futtatása, futtatnia kell egy lekérdezést a lekérdezés lapon. Az ebben az oktatóanyagban futtathatja a minta lekérdezés lecserélése a feladat bemeneti és kimeneti az aliasokat, a képernyőképet alább látható módon.

![Lekérdezés futtatása] (./media/data-lake-store-stream-analytics/run.query.png "Lekérdezés futtatása")

Kattintson a **Mentés** a képernyőn lentről felfelé, és kattintson a **Start**gombra. A párbeszédpanelen jelölje ki az **Egyéni időt**, és válassza a múlt, például az **1/1/2016**dátum. A jelölőnégyzet be van jelölve a projekt indítása gombra. Azt kitölthetik néhány perc a feladat indításához.

![Feladat időtartamának beállítása] (./media/data-lake-store-stream-analytics/run.query.2.png "Feladat időtartamának beállítása")

A projekt indítása után kattintson az adatok feldolgozásának módját megjelenítéséhez **Monitor** lapján.

![Feladat monitor] (./media/data-lake-store-stream-analytics/run.query.3.png "Feladat monitor")

Végezetül is használhatja az [Azure-portálon](https://portal.azure.com) nyissa meg a tó adattár fiókját, és ellenőrizze, hogy az adatok sikerült írni a fiókhoz.

![A kimenet ellenőrzése] (./media/data-lake-store-stream-analytics/run.query.4.png "A kimenet ellenőrzése")

Az adatok-tallózó panelen figyelje meg, hogy az adatok tó tároló mappába, a megadott módon írott a kimenet kimeneti beállítások (`streamanalytics/job/output/{date}/{time}`).  

## <a name="see-also"></a>Lásd még:

* [Hozzon létre egy HDInsight fürtre adatok tó tároló használatára](data-lake-store-hdinsight-hadoop-use-portal.md)
