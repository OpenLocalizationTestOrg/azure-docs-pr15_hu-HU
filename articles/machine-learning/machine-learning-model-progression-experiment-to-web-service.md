<properties
    pageTitle="Hogyan gépi tanulási modell halad egy kísérlet az operationalized webszolgáltatásnak |} Microsoft Azure"
    description="Hogyan az Azure gépi tanulási modell előrehaladtával fejlesztési a kísérlet az operationalized webszolgáltatásnak az idejéről áttekintése."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="garye"/>


# <a name="how-a-machine-learning-model-progresses-from-an-experiment-to-an-operationalized-web-service"></a>Hogyan gépi tanulási modell halad egy kísérlet az operationalized webszolgáltatásnak

Azure gépi tanulási Studio egy interaktív vászonra, amely lehetővé teszi, hogy fejlesztése, futtatni, tesztelése és az találta egy ***kísérletezés*** a cserélendő analysis modell, amely tartalmaz. Vannak számos különböző típusú modulokat elérhető is:

* A kísérlet a bemeneti adatok
* Az adatok módosítására
* A modellben gépi tanulási algoritmusok képzése
* A modell pontszám
* Az eredmények értékelése
* Kimeneti utolsó értékek

Ha elégedett a kísérlet, telepítheti azt ***Klasszikus Azure gépi tanulási webszolgáltatás*** vagy egy ***Új Azure gépi tanulási webszolgáltatás*** , hogy a felhasználók küldje el az új adatok és vissza kapni.

Ez a cikk azt adja meg a idejéről hogyan a gépi tanulási modell előrehaladtával fejlesztési a kísérlet az operationalized webszolgáltatásnak áttekintése.

>[AZURE.NOTE] Más módszereiről fejlesztése, és gépi tanulási modellek telepítéséhez, de ez a cikk összpontosít gépi tanulási Studio használatáról. Vitafórum, hogy miként hozhat létre az adott klasszikus prediktív webszolgáltatás R, tekintse meg az [összeállítás és üzembe prediktív Web Apps használata RStudio](http://blogs.technet.com/b/machinelearning/archive/2015/09/25/build-and-deploy-a-predictive-web-app-using-rstudio-and-azure-ml.aspx)és Azure Machine Learning.

Azure gépi tanulási Studio projektcsapatok kialakításához és a *prediktív analysis modell*üzembe, azt is lehet Studio kidolgozása egy kísérlet, amely nem tartalmazza a cserélendő elemzése adatmodell használatával. Például egy kísérlet előfordulhat, hogy csak az adatok beviteléhez, kezelheti és az eredmények majd kimeneti. Hasonlóan egy prediktív analysis kísérlet telepítheti az e-prediktív kísérletből webes szolgáltatásként, de ennek az oka egy egyszerűbb folyamat a kísérlet nem képzés és gépi tanulási modell pontozási. Bár nem ilyen Studio használandó tipikus, azt fogja foglalja bele a vitafórum, hogy azt is megjelenhetnek Studio működése teljes körű leírását.

## <a name="developing-and-deploying-a-predictive-web-service"></a>Nyújt, és a prediktív webes szolgáltatás üzembe helyezése

Az alábbiakban a szakaszok tipikus megoldást követő fejlesztése és gépi tanulási Studio segítségével üzembe:

![Telepítési továbbításához](media\machine-learning-model-progression-experiment-to-web-service\model-stages-from-experiment-to-web-service.png)

*Szám 1 - tipikus prediktív elemzése adatmodell lépései*

### <a name="the-training-experiment"></a>A tanfolyam kísérlet

A webszolgáltatás gépi tanulási Studio elkészítésének kezdeti fázisa ***oktatás kísérletezhet*** . A tanfolyam kísérlet célja fejlesztése, tesztelje, találta és végül képzése egy gépi tanulási modell hely ad. Akkor is is láthatja el ismeretekkel a több modellek egyidejű, keresse meg a legjobb megoldás, de amikor elkészült, kísérletezés választja ki képzett egyetlen modell, és a többi a kísérlet kiküszöbölésére. Példa a cserélendő elemzésre elkészítésének kísérletezhet című témakör [fejlesztése hitelképesség kockázatbecslés az Azure gépi tanulási a cserélendő analytics megoldást](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="the-predictive-experiment"></a>A cserélendő kísérlet

A tanfolyam kísérlet képzett modell is van, kattintson a **Webes szolgáltatás beállítása** , és jelölje ki a **Cserélendő webszolgáltatás** gépi tanulási Studio segítségével a oktatás kísérlet konvertálása egy ***prediktív kísérlet***a megkezdéséhez. A cserélendő kísérlet célja a képzett modell segítségével pontszám az új adatokat, végül az Azure webszolgáltatás, valamit operationalized célját.

Az átalakítás meg történik át az alábbi lépéseket:

-   Egy egyetlen modulba – oktatás használt modulok halmazára konvertálása és mentése képzett modellben

-   Nem kapcsolódik pontozási felesleges modulokat megszüntetése

-   Bemeneti és kimeneti-portok, amelyekkel az esetleges webszolgáltatás hozzáadása

Lehet, hogy a webes szolgáltatás üzembe felkészítése a cserélendő kísérlet további módosításokat. Például ha azt szeretné, hogy a webszolgáltatás kimeneti eredmények egy részét, felveheti a kimeneti port előtt egy szűrési modulra.

A konvertálás folyamat nem távolítja el a tanfolyam kísérlet. Amikor a folyamat befejeződik, van-e két füle Studio: egy, a oktatás kísérlet és a prediktív kísérlet. Ezzel a módszerrel a webes szolgáltatás telepítése, és a prediktív kísérlet újraépítéséhez együtt végezhet módosításokat a a oktatás kísérlet. Vagy egy másolatot a oktatás kísérlet kísérletezés egy másik sort szeretne kezdeni.

>[AZURE.NOTE] **Prediktív webszolgáltatás** kattintáskor a oktatás kísérlet konvertálása egy prediktív kísérlet egy automatikus folyamat elindítása, és a legtöbbször jól működik. Ha a oktatás kísérlet összetettebb (például van – oktatás, amely fűzhetők össze több út), előfordulhat, hogy az átalakítás kézi tegye inkább. További tudnivalókért lásd: [egy gépi tanulási oktatás kísérlet a cserélendő kísérlet konvertálása](machine-learning-convert-training-experiment-to-scoring-experiment.md).

### <a name="the-web-service"></a>A webszolgáltatás

Ha meg van elégedve, hogy készen áll-e a cserélendő kísérlet, telepítheti a szolgáltatást a klasszikus webes szolgáltatásként, vagy egy új webszolgáltatás alapú a Azure erőforrás-kezelő. A modell üzemeltető *Klasszikus gépi tanulási webszolgáltatás*helyezésével, kattintson a **Web szolgáltatás telepítése** , és válassza a **Webes szolgáltatás üzembe [klasszikus]**. *Új gépi tanulási webhely*szolgáltatás üzembe helyezéséhez kattintson a **Webes szolgáltatás telepítése** , és válassza az **[Új] webes szolgáltatás üzembe**. Felhasználók most adatokat küld a modell a webszolgáltatással REST API-t, és fogadni vissza az eredményeket. További információért megtudhatja, [hogy miként felhasználása az Azure gépi tanulási webszolgáltatás, amely egy kísérlet tanulási számítógépről lett telepítve](machine-learning-consume-web-services.md).

## <a name="the-non-typical-case-creating-a-non-predictive-web-service"></a>A nem tipikus eset: nem prediktív webszolgáltatás létrehozása

Ha a kísérletek nem láthatja el ismeretekkel a prediktív elemzése adatmodell, akkor nem kell hozza létre a oktatás kísérlet és egy pontozási kísérlet - csak egy kísérlet van, és webes szolgáltatás üzembe. Gépi tanulási Studio észleli, hogy tartalmaz-e a kísérlet a cserélendő modell használtuk modulokat elemzésével.

Után a kísérlet a többször is van, és elégedve azt:

1.  **Webszolgáltatás beállítása** gombra, és válassza a **Webszolgáltatás átképzés** - automatikusan hozzáadja a bemeneti és kimeneti csomópontok

2.  Kattintson a **Futtatás** gombra

3. Kattintson a **Web szolgáltatás üzembe** , és válassza a **Webes szolgáltatás üzembe [klasszikus]** vagy **Üzembe webszolgáltatás [Új]** , amelyre telepíteni szeretné a környezettől függően.

A webszolgáltatás most már telepítve van, és megtekintheti és kezelheti a cserélendő webszolgáltatás hasonlóan.

## <a name="updating-your-web-service"></a>A webszolgáltatás frissítése

Most, hogy már telepítette a kísérlet webes szolgáltatásként, mi történik, ha frissíteni kell azt?

Ez attól függ, hogy módosítania kell:

**Azt szeretné, módosíthatja a bemeneti és kimeneti, vagy meg szeretné változtatni, hogyan a webszolgáltatás adatokat kezel**

Ha nem módosítani szeretné a modellt, de csak módosítja a webszolgáltatás hogyan kezeli az adatok, is szerkesztése a cserélendő kísérlet kattintson a **Web szolgáltatás üzembe** és válassza a **Webes szolgáltatás üzembe [klasszikus]** vagy **[Új] webes szolgáltatás üzembe** újra. A webes szolgáltatás, a frissített prediktív kísérlet telepítve van, és a webszolgáltatás újraindítása.

Íme egy példa: Tegyük fel, hogy a cserélendő kísérlet az egész sort bemeneti adatok alapján meghatározott eredményét adja vissza. Dönthet úgy, hogy szeretné-e a webes szolgáltatás csak az eredményt adnak. Így **Project oszlopok** modul felvehet a cserélendő kísérlet, jobbra a kimeneti port, az eredmény eltérő oszlopok kizárása előtt. Kattintson a **Web szolgáltatás telepítése** , és jelölje ki újra **Webes szolgáltatás üzembe [klasszikus]** vagy **Üzembe webszolgáltatás [Új]** , a webszolgáltatás frissül.

**Az új adatokat a modell Újraépítés szeretne**

Ha meg szeretné őrizni a gépi tanulási modell, de Újraépítés azt az új adatokat szeretne, akkor két lehetőség közül választhat:

1.  **A webszolgáltatás futása közben a modell Újraépítés** – Ha a modell, miközben a cserélendő Újraépítés webszolgáltatás fut, ezt megteheti, hogy egy ***átképzési kísérlet***legyen a oktatás kísérlet néhány módosításai módosításával is, majd telepítheti ** *átképzési* webszolgáltatás**. Ennek módjáról, tanulmányozza [a gépi tanulási Újraépítés modellek programozás útján](machine-learning-retrain-models-programmatically.md).

2.  **Térjen vissza az eredeti oktatás kísérlet, és használjon másik oktatás adatokat a modell hozzanak** – a cserélendő kísérlet a webszolgáltatás van csatolva, de a oktatás kísérlet közvetlenül nem kapcsolódik az ilyen módon. Ha az eredeti oktatás kísérletet módosítása, és kattintson a **Web szolgáltatás beállítása**, hoz létre egy *Új* prediktív kísérletezés, amelyek rendszerbe, amikor hoz létre egy *Új* webszolgáltatás. Csak akkor nem frissíti az eredeti webszolgáltatás.

    Ha módosítania kell a oktatás kísérlet, nyissa meg, majd kattintson a **Mentés másként** készítsen róla másolatot. Ezzel az eredeti oktatás kísérletet prediktív kísérlet érintetlenül és webszolgáltatás. Most már hozhat létre egy új webszolgáltatás a módosításokat. Ha már telepítette az új webes szolgáltatás, majd eldöntheti, hogy az előző webes szolgáltatás leállítása vagy tarthatja operációs rendszert futtató összegzik az újat.

**Azt szeretné, hogy egy másik modell betanítása**

Ha a kívánt módosításokat az eredeti prediktív kísérlet, például egy másik gépre tanuló algoritmus kijelölése próbálja egy másik oktatás módszer, stb, majd kövesse a második, a modell átképzés ismertető részben szereplő eljárást kell: Nyissa meg a tanfolyam kísérlet, és kattintson a **Mentés másként** készítsen róla másolatot, és indítsa el az új elérési útját a modell elkészítésének lefelé , a cserélendő kísérlet létrehozásáról és a webes szolgáltatás üzembe helyezése. Ez a hoz létre egy új webhely szolgáltatás nem kapcsolódó az eredetihez - eldöntheti, melyiket vagy mindkettő megtartása futó.

## <a name="next-steps"></a>Következő lépések

További információt a fejlesztése, és a kísérlet a folyamat az alábbi cikkekben talál:

-   a kísérlet - [konvertálja a gépi tanulási oktatás kísérlet a cserélendő kísérlet](machine-learning-convert-training-experiment-to-scoring-experiment.md) konvertálása

-   a webszolgáltatás - [Deploy az Azure gépi tanulási webes szolgáltatásainak](machine-learning-publish-a-machine-learning-web-service.md) üzembe helyezése

-   a modell – [Újraépítés gépi tanulási modellek programozás útján](machine-learning-retrain-models-programmatically.md) átképzés

Példák a teljes folyamat című témakörben talál:

-   [Gépi tanulási oktatóprogram: az első kísérlet létrehozása az Azure gépi tanulási Studio](machine-learning-create-experiment.md)

-   [Útmutató: A hitelkártya kockázatbecslés az Azure gépi tanulási prediktív analytics megoldást kidolgozása](machine-learning-walkthrough-develop-predictive-solution.md)