<properties
    pageTitle="Első lépések az Azure Értékáram-elemzés folyamat adatok IoT eszközökről. | Microsoft Azure"
    description="IoT érzékelő címkéket és adatokat megjelenítő Értékáram-elemzés és a valós idejű adatok feldolgozás adatfolyamok"
    keywords="megoldás IOT iot – első lépések"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="10/19/2016"
    ms.author="jeffstok"
/>

# <a name="get-started-with-azure-stream-analytics-to-process-data-from-iot-devices"></a>Első lépések az Azure Értékáram-elemzés folyamat adatok IoT eszközökről

Ebben az oktatóanyagban fog megtudhatja, hogyan hozhat létre az adatok gyűjtése dolog Internet (IoT) eszközökről adatfolyam-feldolgozási logika. A valós életből dolog Internet (IoT) használatieset való bemutatják, hogyan hozhat létre a megoldás, gyorsan és gazdaságilag fogjuk használni.

## <a name="prerequisites"></a>Előfeltételek

-   [Azure előfizetés](https://azure.microsoft.com/pricing/free-trial/)
-   Lekérdezések és adatok mintafájlok [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) letölthető

## <a name="scenario"></a>Eset

A contoso, amely a ipari automatizálási helyen olyan céget, teljesen rendelkezik automatikus a gyártási folyamatokat. A gép az e növény, amelyek képesek valós idejű adatok adatfolyamok vezérlés érzékelők tartalmaz. Ebben az esetben egy gyártási padló manager kíván, hogy a szenzoradatokat mintázatok keres, és azokat a műveleteket a valós idejű összefüggéseket. Fogjuk használni a adatfolyam Analytics lekérdezési nyelv (SAQL) keresztül a szenzoradatokat érdekes mintázatok, a bejövő adatfolyam az adatok keresése.

Az alábbi adatokat létrehozása folyamatban van a Texas eszközök érzékelő címke eszközről.

![Texas eszközök érzékelő címke](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-01.jpg)

A tartalom, az adatok JSON formátumban, és a következőket:


    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  

A való életben esetben sikerült több száz az alábbi események létrehozása adatfolyamként érzékelők. Egy átjáró eszközt ideális esetben az események leküldéses [Azure esemény hubok](https://azure.microsoft.com/services/event-hubs/) vagy [Azure IoT hubok](https://azure.microsoft.com/services/iot-hub/)kódokat futtathatnak tenné. A Értékáram-elemzés feladatok, akinek az események az esemény hubok ingest és valós idejű analytics-lekérdezések futtatása a adatfolyamok szemben. Ezután meg kell küldenie az eredmények az egyik [kimeneti értékeket támogatja](stream-analytics-define-outputs.md).

Az egyszerű használat érdekében ez keresztüli lépések útmutató adatok mintafájl, amely valódi érzékelő címke eszközökről rögzítésének. Lekérdezések futtatása a mintaadatokat, és a találatokat. További oktatóanyagok, a feladatok keresztüli bemenetben ismerkedhet és exportálja és az Azure szolgáltatás üzembe helyezése.

## <a name="create-a-stream-analytics-job"></a>Értékáram-elemzés feladat létrehozása

1. Az [Azure-portálon](http://portal.azure.com)kattintson a pluszjelre, és írja be a **ÉRTÉKÁRAM-elemzés** a szöveg ablak jobbra. Válassza a **feladat Értékáram-elemzés** eredménye listában.

    ![Hozzon létre új feladatot megjelenítő Értékáram-elemzés](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)

2. Írjon be egy feladat egyedi nevet, és ellenőrizze az előfizetést a megfelelő fiókot a projektre vonatkozóan. Ezután hozzon létre egy új erőforráscsoport, vagy jelöljön ki egy meglévőt az előfizetésben.

3. Ezután jelölje ki a projekt helyét. Feldolgozási sebességétől és az adatok költség csökkentése kijelölése ugyanazon a helyen, valamint az erőforráscsoport tervezett tároló fiók átadás ajánlott.

    ![Hozzon létre új Értékáram-elemzés feladat részletei](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.png)

    > [AZURE.NOTE] A tároló fiókot csak egyszer / régió kell létrehoznia. A tároló összes Értékáram-elemzés feladatok az adott régióban készült különböző lesz megosztva.

4. Jelölje be a feladatok elhelyezése az irányítópult, és kattintson a **Létrehozás**gombra.

    ![folyamatban lévő feladat létrehozása](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03a.png)

5. Meg kell jelennie a "telepítés lépések..." a böngésző ablakának jobb felső sarkában a megjelenített. Amint azt változik egy befejezett ablak alább látható módon.

    ![folyamatban lévő feladat létrehozása](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03b.png)

### <a name="create-an-azure-stream-analytics-query"></a>Értékáram-elemzés Azure lekérdezés létrehozása

A feladatok létrehozását követően a rendelkezik időt megnyithatja, és a lekérdezés összeállítása. A feladatok könnyen hozzáférhető, a mozaik gombra kattintva.

![Feladat csempe](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

A **Feladat topológia** ablakban kattintson **a lekérdezésmező a Lekérdezésszerkesztő ugorhat** . **A Lekérdezésszerkesztő** segítségével adja meg a T-SQL-lekérdezés, amely az átalakítás végez a bejövő eseményadatok fölé.

![Lekérdezésmező](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

### <a name="query-archive-your-raw-data"></a>Lekérdezés: A nyers adatok archiválása

A legegyszerűbb lekérdezés, amely a kijelölt kimeneti összes bemeneti adatok archiválása átadó lekérdezés. Töltse le az adatfájl – minta [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) egy helyet a számítógépen. 

1. Illessze be a lekérdezést a PassThrough.txt fájlból. 

    ![Beviteli adatfolyam tesztelése](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)

2. Kattintson a beviteli mellett a három pontra, és jelölje be **a mintaadatokat az importált fájl feltöltése** .

    ![Beviteli adatfolyam tesztelése](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06a.png)

3. Eredményként a jobb oldalon megnyílik egy munkaablak, az azt HelloWorldASA-InputStream.json adatfájl kijelölése a letöltött helyről, és kattintson az **OK gombra** az ablaktábla alján.

    ![Beviteli adatfolyam tesztelése](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06b.png)

4. Kattintson a **vizsgálat** fogaskerék területen az ablak bal felső, majd a lekérdezés tesztelése szemben a minta adatkészlet feldolgozása. Eredmények megnyílik egy a lekérdezés alatt, a feldolgozás befejeződött.

    ![A vizsgálati eredmények](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

### <a name="query-filter-the-data-based-on-a-condition"></a>Lekérdezés: Szűrheti az adatokat egy feltétel alapján

Lássunk egy feltétel alapján a szűrést. Azt szeretné, csak az "sensorA." származnia események eredmény megjelenítése A lekérdezés nem a Filtering.txt fájlban.

![Adatfolyam szűrése](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Figyelje meg, hogy a kis-és nagybetűket lekérdezés hasonlítja össze szöveges értékként. Kattintson a **vizsgálat** fogaskerék ismét a végrehajtja a lekérdezést. A lekérdezés 389 sorok 1860 események ki kell vissza.

![Második kimeneti eredmények a lekérdezés tesztelése](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

### <a name="query-alert-to-trigger-a-business-workflow"></a>Lekérdezés: Üzleti munkafolyamat indítására értesítés

Most, hogy a lekérdezés részletesebb. Minden személy mezőtípushoz szeretnénk figyelése átlaghőmérséklet per 30 másodperces ablak, amely megjeleníti az eredményt, csak ha az átlaghőmérséklet 100 fok felett. Hogy írja be az alábbi lekérdezés, és válassza a **vizsgálati** eredmények megjelenítéséhez. A lekérdezés nem a ThresholdAlerting.txt fájlban.

![30 másodperces szűrő lekérdezések](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Most már látnia kell csak 245 sorok és érzékelők nevét tartalmazó eredmények 100-nál nagyobb mérsékelt átlagát esetén. A lekérdezés csoportosítja az események adatfolyam **dspl**, amely érzékelő név 30 másodperces **Tumbling ablak** felett. A lekérdezések időbeli tartalmaznia kell hogyan szeretnénk idő állapotába. A **IDŐBÉLYEG BY** záradék használatával az **OUTPUTTIME** oszlop időpontok társíthatja az összes időbeli számítások megadott azt. Részletes információkért olvassa el az MSDN cikkek a témában [Időbeosztás](https://msdn.microsoft.com/library/azure/mt582045.aspx) és az [ablakok elrendezése függvények](https://msdn.microsoft.com/library/azure/dn835019.aspx).

### <a name="query-detect-absence-of-events"></a>Lekérdezés: Távollét események észleli

Hogyan azt írhat hiányoznak a bemeneti események keresése egy lekérdezés? Hogy érzékelő küldött adatok, és majd nem küldött eseményeket a következő MINUTE vegyük keresése a legutóbbi. A lekérdezés nem a AbsenseOfEvent.txt fájlban.

![Hiányában események észleli](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

Itt a **Bal oldali külső** illesztés használjuk az azonos adatfolyam (önálló csatlakozni). A **belső** illesztés csak akkor, ha van találat eredményt adja vissza.  A **Bal oldali külső** illesztés Ha az illesztés bal oldalán az esemény nem egyezőket kereső, az összes oszlop jobb oldalán a NULL tartalmazó sor adja vissza. Ez a módszer akkor nagyon hasznos események távollét kereséséhez. További információt a [Csatlakozás](https://msdn.microsoft.com/library/azure/dn835026.aspx)az MSDN dokumentációjában találhat.

## <a name="conclusion"></a>Elfogadásáról

Ebben az oktatóanyagban célja szemléltetik különböző Analytics-lekérdezési nyelv adatfolyam-lekérdezéseket írni, és a találatokat a böngészőben. Jó helyen jár a program csak most ismerkedik. Értékáram-elemzés, annyira további műveletekre van lehetőség. Értékáram-elemzés támogatja a bemenetben számos exportálja és még függvények használhatók az Azure gépi tanulási, így azok adatfolyamok elemzéséhez robusztus eszköz. Elindíthatja a feltárása további tudnivalók a Értékáram-elemzés a a [térkép tanulási](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/). Lekérdezéseket írni kapcsolatos további tudnivalókért olvassa el a [Gyakori lekérdezési mintázatok](./stream-analytics-stream-analytics-query-patterns.md)ismertető cikket.
