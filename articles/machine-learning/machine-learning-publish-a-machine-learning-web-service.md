<properties
    pageTitle="Gépi tanulás webszolgáltatás telepítése |}] A Microsoft Azure"
    description="Hogyan kell alakítani egy képzési kísérletben prediktív kísérlet, előkészíthető központi, majd központilag telepíti az Azure gép tanulás webszolgáltatás."
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

# <a name="deploy-an-azure-machine-learning-web-service"></a>Az Azure gép tanulás webszolgáltatás telepítése

Borzas gép tanulás lehetővé teszi létrehozása, tesztelése és prediktív elemzési megoldások telepítése.

Az olyan magas szintű nézőpont-beállítóval ezt három lépésből áll:

- **[Létrehozása egy képzési kísérletben]** - Azure gép tanulás Studio egy közös vizuális fejlesztői környezet, amelynek segítségével a vonat, és tesztelje a prediktív analytics modellben oktatási adatokat ad meg.
- **[Konvertálja a prediktív kísérlet]** - egyszer a modell betanítva már meglévő adatokkal, és készen áll a segítségével új adatok pontszám, készít, és a kísérleti előrejelzések készítéséhez egyszerűsíthető.
- **Telepítheti a webes szolgáltatásként** - a cserélendő kísérlet [Új] vagy [Klasszikus] Azure webes szolgáltatásként telepíthető. A felhasználók adatokat küldjenek a modell és a modell-előrejelzéseket kap.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="create-a-training-experiment"></a>Hozzon létre egy képzési kísérlet

A vonat egy analytics előrejelzési modellt, Azure gép tanulás Studio létrehozásához használhatja egy képzési kísérletben ahol képzési adatok betöltése, az adatokat, szükség szerint előkészítése, gépi tanulási algoritmusok alkalmazza, és az eredmények értékelése különböző modulok közé tartozik. A kísérlet találta, és próbálja meg másik gépre tanulási algoritmusok összehasonlítása és az eredmények értékelése.

A folyamat létrehozásának és kezelésének képzési kísérletek alaposabban máshol van. Ezekben a cikkekben talál további információt:

- [Hozzon létre egy egyszerű kísérletben Azure gép tanulás Studio](machine-learning-create-experiment.md)
- [Azure gép tanulás a prediktív megoldás kidolgozása](machine-learning-walkthrough-develop-predictive-solution.md)
- [A képzési adatok importálása Azure gép tanulás Studio](machine-learning-data-science-import-data.md)
- [Kísérlet közelítés Azure gép tanulás Studio kezelése](machine-learning-manage-experiment-iterations.md)

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>A képzés kísérlet átalakítása prediktív kísérlet

A modell már képzett, ha készen áll a képzés kísérlet alakítani egy új adatok pontszám prediktív kísérlet.

Prediktív kísérlet alakításával, kap a képzett modell pontozási webes szolgáltatásként telepítésre kész. A felhasználók a webszolgáltatás bemeneti adatokat küldhet a modell és a modell vissza az előrejelzési eredmények küldi. Prediktív kísérlet konvertálásakor, tartsa szem előtt a mások által használandó modell várhatóan hogyan.

A prediktív kísérlet a képzés kísérlet átalakítása, **futtassa** a kísérlet vászon alján kattintson **Webszolgáltatás beállítása**gombra, majd jelöljük ki a **Cserélendő webszolgáltatás**.

![Kísérlet a pontozás átalakítása](./media/machine-learning-publish-a-machine-learning-web-service/figure-1.png)

Az átalakítás végrehajtásával kapcsolatos további tudnivalókért lásd a [prediktív kísérlet egy gép tanulás képzési kísérlet átalakítani](machine-learning-convert-training-experiment-to-scoring-experiment.md).

Az alábbi lépések leírják a prediktív kísérlet egy új webes szolgáltatás telepítése. A kísérlet klasszikus webes szolgáltatásként is telepítheti.

## <a name="deploy-the-predictive-experiment-as-a-new-web-service"></a>A prediktív kísérlet egy új webes szolgáltatás telepítése

Most, hogy a prediktív kísérlet elő van készítve, mint az Azure webszolgáltatás telepítheti. A webszolgáltatás segítségével felhasználók küldhetnek adatokat a modell és a modell ad vissza az előrejelzéseket.

A prediktív kísérlet telepítéséhez **futtassa** a kísérlet vászon alján kattintson. Után a kísérlet futása befejeződött, kattintson a **Web szolgáltatás telepítése** , és válassza az ** [Új]webes szolgáltatás telepítését**.  A gépi tanulás webszolgáltatás portal telepítési oldalán nyílik meg.

### <a name="machine-learning-web-service-portal-deploy-experiment-page"></a>Gépi tanulás webszolgáltatás portál központi telepítés kísérlet lapján

A kísérlet telepítése lapon adja meg a webes szolgáltatás nevét.
Válassza ki az ármegállapítási terv. Ha egy meglévő ármegállapítási terv is választhat, más módon kell tervet hoz létre új ár a szolgáltatás.

1.  **Ár terv** legördülő, jelöljön ki egy meglévő terv, vagy válassza a **Válassza ki az új tervet** .
2.  A **Séma neve**mezőbe írja be egy nevet, amely azonosítja a számlázási tervet.
3.  A **Havi terv meghatározási szintek**közül választhat. A terv rétegek alapértelmezés szerint az alapértelmezett terület és a webes szolgáltatás telepítve van a régióba.

Kattintson a **központi telepítés** és a webes szolgáltatás megnyitása a **Gyorsindítás** oldal.

Weblap szolgáltatás Gyorsindítás lehetővé teszi útmutató és access elvégezheti egy webszolgáltatás létrehozása után a leggyakrabban használt tevékenységek. Innen könnyen elérheti a tesztoldal és a felhasználás lap.

<!-- ![Deploy the web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)-->

### <a name="test-your-web-service"></a>A webes szolgáltatás tesztelése

Az új webes szolgáltatás teszteléséhez kattintson **web szolgáltatás tesztelése** a közös feladatok területen. A tesztoldal tesztelheti a webszolgáltatás, mint egy kérés-válasz szolgáltatás (RR) vagy kötegelt végrehajtás szolgáltatás (BES).

Az Erőforrásrekordok tesztoldalt a felhasználandó anyagokat, kimeneteket és a kísérlet beállított globális paramétereket jeleníti meg. A webszolgáltatás teszteléséhez manuálisan is adja meg a megfelelő értékeket a felhasználandó anyagokat vagy egy vesszővel tagolt értéklista (CSV) formátumú fájl a vizsgálati értékeit tartalmazó ellátás.

Erőforrásrekordok, használja a lista nézet mód teszteléséhez adja meg a megfelelő értékeket a felhasználandó anyagokat, majd kattintson a **Kérés-válasz teszt**. Az előrejelzési eredmények a kimeneti oszlop bal oldalán jeleníti meg.

![A webszolgáltatás telepítése](./media/machine-learning-publish-a-machine-learning-web-service/figure-5-test-request-response.png)

A BES teszteléséhez kattintson a **Köteg**. Kattintson a Tallózás gombra a beírt adatokat a kötegelt tesztoldalt, és válassza ki a megfelelő minta értékeket tartalmazó CSV-fájlba. Ha nem rendelkezünk egy CSV-fájlt, és a prediktív kísérlet gép tanulás Studio segítségével létrehozott, adatok letöltése a prediktív kísérlet, és azt használja.

Adatok letöltése, nyissa meg a számítógép tanulás Studio. Nyissa meg a prediktív kísérlet, és kattintson a jobb gombbal a kísérletben bemenet. A helyi menüből válassza ki a **dataset** , és válassza a **Letöltés**.

![A webszolgáltatás telepítése](./media/machine-learning-publish-a-machine-learning-web-service/figure-7-mls-download.png)

Kattintson a **teszt**gombra. A kötegfeldolgozás feladat állapotának **Teszt kötegelt feldolgozások**mellett jobbra jeleníti meg.

![A webszolgáltatás telepítése](./media/machine-learning-publish-a-machine-learning-web-service/figure-6-test-batch-execution.png)

<!--![Test the web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)-->

A **konfiguráció** lapon módosíthatja a leírását, cím, frissítse a tároló fiók kulcsot és mintaadatok engedélyezheti a webszolgáltatás.

![A webszolgáltatás beállítása](./media/machine-learning-publish-a-machine-learning-web-service/figure-8-arm-configure.png)

Ha már telepítette a webes szolgáltatás, akkor:

- **Access** azt az webes szolgáltatás API-k.
- **Kezelés** , Azure gép tanulás webportál-szolgáltatások vagy a klasszikus Azure portálon keresztül.
- **Frissítés** , ha a modell megváltozik.

### <a name="access-the-web-service"></a>A webszolgáltatás elérésére

Miután telepíti a webes szolgáltatás gép tanulás Studio, adatokat küldeni a szolgáltatásnak, és programozott válaszokat kapni.

A **felhasználás** lap kínál a webes szolgáltatás eléréséhez szükséges összes információt. Például a API kulcs lehetővé teszi a szolgáltatás engedélyezett hozzáférést adni.

Gép tanulás webszolgáltatás elérésével kapcsolatos további tudnivalókért lásd: [Azure gép tanulás telepített webes szolgáltatás igénybevételére](machine-learning-consume-web-services.md).

### <a name="manage-your-new-web-service"></a>Az új webes szolgáltatás kezelése

A klasszikus szolgáltatások gép tanulás webszolgáltatások webportál kezelheti. [Portál fő lapján](https://services.azureml-test.net/)kattintson a **Webes szolgáltatások**. A webes szolgáltatások lapról törölje, vagy másolja a szolgáltatás. Egy adott szolgáltatás figyelése, jelölje ki a szolgáltatást, és kattintson az **Irányítópult**. A webszolgáltatáshoz társított kötegelt feladatok figyeléséhez kattintson a **Kötegelt kérése naplóra**.

## <a name="deploy-the-predictive-experiment-as-a-classic-web-service"></a>A prediktív kísérlet klasszikus web szolgáltatás telepítése

Most, hogy a prediktív kísérlet kellőképpen elő lett készítve, mint az Azure webszolgáltatás telepítheti. A webszolgáltatás segítségével felhasználók küldhetnek adatokat a modell és a modell ad vissza az előrejelzéseket.

A prediktív kísérlet telepítéséhez **futtassa** a kísérlet vászon alján kattintson, majd **Webszolgáltatás telepítése**. A webszolgáltatás van beállítva és bejelölte a webes szolgáltatás irányítópult.

![A webszolgáltatás telepítése](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)

A gépi tanulás Web Services portal vagy a gép tanulás Studio tesztelheti a webszolgáltatás.

A válasz kérése webszolgáltatás teszteléséhez kattintson a **teszt** gombra a webes szolgáltatás irányítópult. Egy párbeszédpanel jelenik meg, az a szolgáltatás a bemeneti adatok kérésére. Ezek az oszlopok a pontozási kísérlet által várt. Adja meg az adatok, és kattintson az **OK gombra**. A webszolgáltatás által generált eredmények az irányítópult alján jelennek meg.

A **vizsgálati** minta hivatkozásra a szolgáltatás tesztelése a Azure gép tanulás Web Services portál, ahogy korábban az új webes szolgáltatás részben kattinthat.

A kötegelt végrehajtás szolgáltatás teszteléséhez kattintson a Megtekintés hivatkozás **tesztelése** . Kattintson a Tallózás gombra a beírt adatokat a kötegelt tesztoldalt, és válassza ki a megfelelő minta értékeket tartalmazó CSV-fájlba. Ha nem rendelkezünk egy CSV-fájlt, és a prediktív kísérlet gép tanulás Studio segítségével létrehozott, adatok letöltése a prediktív kísérlet, és azt használja.

![A webes szolgáltatás tesztelése](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)

A **konfiguráció** lapon módosítsa a megjelenítési nevet a szolgáltatás és a leírást. A név és leírás, ahol kezelheti a webes szolgáltatások [Azure klasszikus portal](http://manage.windowsazure.com/) jelenik meg.

Minden oszlop a **BEVITELI SÉMÁBÓL**, **Kimeneti SÉMA**és **Webszolgáltatási paraméter**egy karakterlánc beírásával biztosíthat leírását bevitt adatok, a kimeneti adatok és a webes szolgáltatás paramétereit. Ezek a leírások a minta kód dokumentációjában a webszolgáltatás használják.

Engedélyezheti a naplózást is lát a webszolgáltatás elérésekor hibák diagnosztizálásához. További tudnivalókért tanulmányozza a [gép tanulás webes szolgáltatások naplózásának engedélyezése](machine-learning-web-services-logging.md).

![A webszolgáltatás beállítása](./media/machine-learning-publish-a-machine-learning-web-service/figure-4.png)

A végpontok a webszolgáltatás a Azure gép tanulás Web Services portál hasonló a korábban az új webes szolgáltatás részben bemutatott eljárást is beállítható. A lehetőségek eltérőek, hozzáadása, vagy a szolgáltatás leírása, naplózásának engedélyezése és tesztelési engedélyezés példaadatok módosítása.

### <a name="access-the-web-service"></a>A webszolgáltatás elérésére

Miután telepíti a webes szolgáltatás gép tanulás Studio, adatokat küldeni a szolgáltatásnak, és programozott válaszokat kapni.

Az irányítópult a webszolgáltatás eléréséhez szükséges összes információt biztosít. Például a szolgáltatás engedélyezett hozzáférést biztosítja az API-kulcs, és az API súgólapok szolgálnak a programozás megkezdéséhez.

Gép tanulás webszolgáltatás elérésével kapcsolatos további tudnivalókért lásd: [Azure gép tanulás telepített webes szolgáltatás igénybevételére](machine-learning-consume-web-services.md).

### <a name="manage-the-web-service"></a>A webes szolgáltatás kezelése

Vannak különböző műveletek hajthatók végre egy webes szolgáltatás figyelése. Frissíti, és törölje azt. A klasszikus webes szolgáltatás mellett az alapértelmezett végpont üzembe helyezésekor létrehozott adhat további végpontok is.

További információért lásd: [egy gép tanulás Azure munkaterület kezelése](machine-learning-manage-workspace.md) és [az Azure gép tanulás Web Services portal segítségével webes szolgáltatás kezelése](machine-learning-manage-new-webservice.md).

<!-- When this article gets published, fix the link and uncomment
For more information on how to manage Azure Machine Learning web service endpoints using the REST API, see **Azure machine learning web service endpoints**.
-->

## <a name="update-the-web-service"></a>A frissítés a webes szolgáltatás

Módosítsa a webes szolgáltatás, például a modell frissítése további képzési adatokkal, és újbóli telepítése, felülírva az eredeti webes szolgáltatás.

A webszolgáltatás frissítéséhez nyissa meg az eredeti prediktív kísérletet, mellyel a webszolgáltatás telepítése és a **MENTÉS Másként**gombra kattintva egy szerkeszthető másolatot készíteni. Végezze el a módosításokat, és kattintson a **Webes szolgáltatás telepítését**.

A kísérlet előtt már telepítve, mert megkérdezi, ha (klasszikus webszolgáltatás) felülírása vagy módosítása (új webszolgáltatás) a meglévő szolgáltatás. Kattintson az **Igen** vagy a **frissítés** leállítja a meglévő webes szolgáltatást, és telepíti az új prediktív kísérlet telepítik helyére.

> [AZURE.NOTE] Ha a végrehajtott konfigurációs módosítások az eredeti webszolgáltatás, például be egy új nevet vagy leírást, szüksége lesz ismét be ezeket az értékeket.

A webszolgáltatás frissítésének egyik lehetséges módja a modell programozott elhelyezkedni. További tudnivalók: [átképzésük gép tanulási modellek programozással](machine-learning-retrain-models-programmatically.md).


<!-- internal links -->
[Hozzon létre egy képzési kísérlet]: #create-a-training-experiment
[Alakítsa át a prediktív kísérlet]: #convert-the-training-experiment-to-a-predictive-experiment
[új]: #deploy-the-predictive-experiment-as-a-new-Web-service
[klasszikus]: #deploy-the-predictive-experiment-as-a-new-Web-service
[Access]: #access-the-Web-service
[Manage]: #manage-the-Web-service-in-the-azure-management-portal
[Update]: #update-the-Web-service
