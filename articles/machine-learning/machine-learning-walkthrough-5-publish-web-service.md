<properties
    pageTitle="5. lépés: A számítógép tanulás webszolgáltatás telepítése |}] A Microsoft Azure"
    description="A fejlesztése a prediktív megoldás forgatókönyv 5. lépés: a prediktív kísérlet gép tanulás Studio webes szolgáltatás telepítését."
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
    ms.date="10/05/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-5-deploy-the-azure-machine-learning-web-service"></a>Forgatókönyv 5. lépés: A Azure gép tanulás webszolgáltatás telepítése

Ez a forgatókönyv, [Azure gép tanulás a prediktív analytics megoldás fejlesztése](machine-learning-walkthrough-develop-predictive-solution.md) az ötödik lépése


1.  [Gépi tanulás munkaterület létrehozása](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [A meglévő adatok feltöltése](machine-learning-walkthrough-2-upload-data.md)
3.  [Hozzon létre egy új kísérlet](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [A vonat és a modell értékelése](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  **A webszolgáltatás telepítése**
6.  [A webszolgáltatás elérésére](machine-learning-walkthrough-6-access-web-service.md)

----------

Ez a forgatókönyv azt már kidolgozott előrejelzési modell használata lehetőséget hajthatjuk, azt telepítse azt Azure a webes szolgáltatás.

Ez idáig azt korábban már való kísérletezés szakképzés a modell. Azonban a telepített szolgáltatás már nem lesz a sorsa a képzés - a modell alapján a felhasználó által beírt pontozási által létrehozott előrejelzéseket. Így fogunk tenni bizonyos előkészületeket e kísérlet át az ***oktatási*** kísérlet a ***prediktív*** kísérletezni. 

Ez a folyamat két lépésből áll:  

1. A *képzés kipróbálni* azt a létrehozott átalakítása a *prediktív kísérlet*
2. A prediktív kísérlet Web szolgáltatás telepítése

De először azt kell e kísérlet metsszük le egy kicsit. Két különböző modellek a kísérletben jelenleg van, de azt szeretné, hogy csak egy modell azt üzembe a webes szolgáltatás.  

Tegyük fel, hogy a fa csillapítja modell volt a jobb modellt használja már döntöttünk. Így az elsőként távolítsa el a [Két-osztály Support Vector gép] [ two-class-support-vector-machine] -modul és a képzés, hogy a használt modulok. Célszerű lehet a kísérletben másolatának elkészítéséhez először gombra kattintva **Mentés másként** a kísérletben vászon alján.

A következő modulok törölni kell:  

- [Két-osztály Support Vector gép][two-class-support-vector-machine]
- [A vonat modell] [ train-model] és a [Pontszám modell] [ score-model] hozzá csatlakoztatott modulok
- [Normalizálása az adatok] [ normalize-data] (mindkettő)
- [Modell értékelése][evaluate-model]

Jelölje ki a modult és nyomja le a Delete billentyűt, vagy kattintson a jobb gombbal a modult, és válassza a **Törlés**.

Most vagyunk telepíthető ez a modell segítségével a [Két osztály csillapítja döntési fa][two-class-boosted-decision-tree].

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>A képzés kísérlet átalakítása prediktív kísérlet

Prediktív kísérlet átalakítása három lépésből áll:

1. A modell azt is képzett, és lecseréli a saját képzési modulok mentése
2. Trim a kísérlet csak szükséges képzési modulok eltávolítása
3. Ha a webszolgáltatás bevitel fogadására, és ahol a kimenetet hoz létre meghatározása

Szerencsére mindhárom lépést **webes beállítása szolgáltatás** , a kísérlet vászon (válassza a **prediktív webszolgáltatás** lehetőséget) alján kattintva elvégezhető.

Ha **webes beállítása szolgáltatás**gombra kattint, a következő események történnek:

- A képzett modell a modul paletta bal oldalán a kísérletben vászon (megtalálja a **Képzett modellek**) egyetlen **Képzett modell** modulként mentéskor.
- Képzési használt modulok törlődnek. Különösen:
  - [Két osztály csillapítja döntési fa][two-class-boosted-decision-tree]
  - [Vonat modell][train-model]
  - [Adatok megosztása][split]
  - a második [R parancsfájl végrehajtása] [ execute-r-script] használt vizsgálati adatok modul
- A mentett képzett modell kerül vissza azokat a kísérletben.
- **Webes szolgáltatás** és **webes szolgáltatás kilépő** modulok kerülnek.

> [AZURE.NOTE] A kísérlet mentette a lap felső részén a kísérletben vászon felvett két részből áll: az eredeti kísérletben képzés **képzés kísérletezhet**a lapon, és az újonnan létrehozott prediktív kísérletet **Predictive kísérlet**alatt.

Ez különösen kísérletnél egy további lépésre van szükségünk.
Hozzáadtuk a két [R parancsfájl végrehajtása] [ execute-r-script] modulok képzés és a tesztelés súlyozási függvény az adatok megadását. A végső modell ehhez nem szükséges.

Gépi tanulás Studio eltávolítani egy [R parancsfájl végrehajtása] [ execute-r-script] ezt az [osztott] eltávolításakor a modul[ split] modul. Most azt most távolítsa el a másik, és csatlakoztassa a [Metaadat-szerkesztő] [ metadata-editor] közvetlenül a [Pontszám modell][score-model].    

A kísérlet most így kell kinéznie:  

![A pontozás a képzett modell][4]  

> [AZURE.NOTE] Talán azon tűnődik, miért az UCI német hitelkártya adatok adatkészlet azt balra a prediktív kísérletben. A szolgáltatás használata a felhasználói adatok nem az eredeti adathalmaz, így miért hagyja az eredeti adatkészlet a modell lesz?
>
>IGAZ, hogy a szolgáltatás nem kell az eredeti hitelkártya-adatokat. De azt a sémát, amely magában foglalja információkat, például hogy hány hasáb van, és mely oszlopok numerikus adatokat szükséges. A séma információra szükség a felhasználói adatok értelmezésére. Ezeket az összetevőket úgy, hogy a pontozási modul a dataset sémával rendelkezik, a szolgáltatás futása közben csatlakoztatva hagyunk. Az adatokat nem használja, csak a séma.  

A kísérlet egy utolsó alkalommal futtatása ( **Futtatás**parancsra). Ha azt szeretné, ellenőrizze, hogy a modell továbbra is működik, kattintson a kimenet a [Pontszám modell] [ score-model] modul, és válassza ki **Az eredmények megtekintése**. Látni fogja, hogy az eredeti adat jelenik meg, a hitel kockázat értékét ("pontszáma feliratok") és a pontozási valószínűségi érték ("pontszáma valószínűségek".) 

## <a name="deploy-the-web-service"></a>A webszolgáltatás telepítése

A kísérlet klasszikus webes szolgáltatás vagy webszolgáltatás alapján Azure Resource Manager telepítheti.

### <a name="deploy-as-a-classic-web-service"></a>Klasszikus Web szolgáltatás telepítése ###

A kísérletben nyert klasszikus webszolgáltatás telepítése, **Webes szolgáltatás telepítéséhez** kattintson a vászon alatt, és válassza ki a **Webes szolgáltatás telepítése [Classic]**. Gépi tanulás Studio telepít egy webes szolgáltatás, a kísérlet, és viszi az irányítópultot, hogy a webes szolgáltatás. Innen vissza a kísérlet (**pillanatfelvételének megtekintését** , vagy **megtekintheti a legújabb**), és egy egyszerű teszt a webes szolgáltatás (lásd: **vizsgálat a webszolgáltatás** alább). A webszolgáltatás (bővebben is, hogy ez a forgatókönyv a következő lépésben) hozzáférő alkalmazások létrehozására szolgáló információ itt is van.

![Webes szolgáltatás irányítópult][6]

A **konfiguráció** fülre kattintva beállíthatja a szolgáltatást. Itt módosíthatja a szolgáltatás neve (azt adta a kísérlet neve alapértelmezés szerint), és adjon neki egy leírást. További rövid címkéket adhat a bemeneti és kimeneti oszlopok is.  

![A webszolgáltatás beállítása][5]  

### <a name="deploy-as-a-new-web-service"></a>Új webes szolgáltatás telepítése

A kísérletben nyert új webes szolgáltatás telepítéséhez kattintson a **Webszolgáltatás telepítése** alatt a vásznat és **[Új] Web szolgáltatás telepítése**. Gépi tanulás Studio telepítése kísérlet Azure gép tanulás Web szolgáltatások átvitele.

Adjon meg egy nevet a webszolgáltatás és az ármegállapítási terv kiválasztása. Ha egy meglévő ármegállapítási terv is választhat, más módon kell tervet hoz létre új ár a szolgáltatás. 

1.  **Ár terv** , a legördülő válassza ki egy már meglévő tervet, vagy válassza a **Válassza ki az új terv** .
2.  A **Séma neve**mezőbe írja be egy nevet, amely azonosítja a számlázási terv.
3.  A **Havi terv meghatározási szintek**közül választhat. A terv rétegek alapértelmezés szerint az alapértelmezett terület és a webes szolgáltatás telepítve van a régióba.

Kattintson a **központi telepítés** és a webes szolgáltatás megnyitása a **Gyorsindítás** oldal.

A szolgáltatás **beállítása** menüpontra kattintva konfigurálhatja. Itt módosíthatja a szolgáltatás neve (azt adta a kísérlet neve alapértelmezés szerint), és adjon neki egy leírást. 

Válassza a webes szolgáltatás teszteléséhez kattintson a **teszt** menüpont (lásd: **a webes szolgáltatás tesztelése** az alábbi). A webszolgáltatás által elérhető alkalmazások létrehozásával kapcsolatos tudnivalókért kattintson a **felhasználás** menüpont (bővebben is, hogy ez a forgatókönyv a következő lépésben).

> [AZURE.TIP] A webszolgáltatás már üzembe helyezése után is frissítheti. Például ha meg szeretné változtatni a modell, a képzés kísérlet szerkesztése, a modell-paraméterek finomhangolása, majd kattintson **Webszolgáltatás telepítése**. Válassza ki a **Webes szolgáltatás telepítése [Classic]** vagy **[Új] Web szolgáltatás telepítése**. Ismét üzembe helyezésekor a kísérletben, lecseréli a webes szolgáltatás, a frissített modellt használ.  

## <a name="test-the-web-service"></a>A webes szolgáltatás tesztelése

### <a name="test-a-classic-web-service"></a>A klasszikus webes szolgáltatás tesztelése

A szolgáltatás a gép tanulás Studio vagy a Azure gép tanulás Web Services portál tesztelheti. A Azure gép tanulás Web Services portál tesztelési megvan az az előnye, hogy lehetővé teszi lehetővé 

**A gépi tanulás Studio tesztelése**

Az **IRÁNYÍTÓPULTOK** oldalon kattintson a **teszt** gombra a **Végpont alapértelmezett**. Egy párbeszédpanel előugró és kéri a bemeneti adatok a szolgáltatás. Ezek a jelent meg az eredeti német hitel kockázat adathalmazban oszlopát.  

Adja meg az adatok, és kattintson az **OK gombra**. 

**Tesztelje a Azure gép tanulás Web Services portál**

Az **IRÁNYÍTÓPULTOK** oldalon **Alapértelmezett végpont**a **vizsgálati** minta hivatkozásra kattintson. A tesztoldal a webes szolgáltatási végpont a Azure gép tanulás Web Services portál megnyílik, és kéri a bemeneti adatok a szolgáltatás. Ezek a jelent meg az eredeti német hitel kockázat adathalmazban oszlopát.

Kattintson a **Kérés-válasz teszt**. 

A webszolgáltatás az adatokat a **webes szolgáltatás bemeneti** modul révén a [Metaadat-szerkesztő] segítségével belép[ metadata-editor] modul, és a [Pontszám modell] [ score-model] ahol pontszáma modul. Az eredmények kerülnek majd kimeneti keresztül a **webes szolgáltatások kibocsátását**a webszolgáltatástól.

**Egy új webes szolgáltatás tesztelése**

Az Azure gép tanulás Web services Portal **vizsgálat** a lap tetején gombra. **A tesztoldal** megnyílik, és a bevitt adatokat a szolgáltatás is. A beviteli mezők jelennek meg az oszlopokat az eredeti német hitel kockázat adathalmazban megjelent felel meg. 

Adja meg az adatok, és kattintson a **Kérés-válasz teszt**.

A vizsgálat eredményét a kimeneti oszlop jobb oldalán megjelenik. 

Az Azure gép tanulás Web Services Portal tesztelésekor engedélyezheti mintaadatokat, amelyek segítségével a kérés-válasz szolgáltatás tesztelése. Ha a webszolgáltatás gép tanulás Studio létrehozott, a mintaadatok származik az adatok a modell vonat a használt.

> [AZURE.TIP] A úgy van beállítva, a prediktív kísérlet a [Pontszám modell] eredményez a teljes[ score-model] modul is megjelennek az eredményben. Ez magában foglalja a bemeneti adatok plusz a hitel kockázat értékét és a pontozási valószínűsége. Vissza valami különböző - Ha például csak a hitel kockázati érték -, majd a [Projekt oszlopok] beszúrásakor sikerült[ project-columns] modul közötti [Pontszám modell] [ score-model] és a **webes szolgáltatások kibocsátását akkor** szeretnénk térni a webszolgáltatás oszlopok kiküszöbölése érdekében. 

## <a name="manage-the-web-service"></a>A webes szolgáltatás kezelése

**A klasszikus webes szolgáltatás kezelése**

Ha már telepítette a klasszikus webes szolgáltatás, kezelheti az [Azure klasszikus portal](https://manage.windowsazure.com).

1. Jelentkezzen be a [Klasszikus Azure portal](https://manage.windowsazure.com).
2. A Microsoft Azure szolgáltatások panelen kattintson a **Gép tanulás**.
3. Kattintson a munkaterület.
4. Kattintson a **webes szolgáltatások** fülre.
5. Kattintson a webszolgáltatás készült.
6. Kattintson az "alapértelmezett" végpont.

Innen mint figyelni, hogyan működik a webszolgáltatás műveleteket végezheti el, és márka teljesítmény beállításokból állnak módosításával, hogy hány egyidejű meghívja a szolgáltatás kezelni tud.
Az Azure piactér is közzéteheti a webszolgáltatáshoz.

További részletekért lásd:

- [Végpontok létrehozása](machine-learning-create-endpoint.md)
- [Webszolgáltatás-méretezés](machine-learning-scaling-webservice.md)
- [Az Azure piactér Azure gép tanulás webszolgáltatás közzététele](machine-learning-publish-web-service-to-azure-marketplace.md)

**Az Azure gép tanulás Web Services Portal webes szolgáltatás kezelése**

Ha már telepítette a webes szolgáltatás, klasszikus vagy új, kezelheti az [Azure gép tanulás Web services portál](https://servics.azureml.net).

A webes szolgáltatás teljesítményének figyelésére:

1. Jelentkezzen be az [Azure gép tanulás Web services portál](https://servics.azureml.net).
2. Kattintson a **webes szolgáltatások**.
3. Kattintson a webes szolgáltatás.
4. Kattintson az **Irányítópult**.

----------

**[Hozzáférés a webszolgáltatás](machine-learning-walkthrough-6-access-web-service.md) tovább:**

[1]: ./media/machine-learning-walkthrough-5-publish-web-service/publish1.png
[2]: ./media/machine-learning-walkthrough-5-publish-web-service/publish2.png
[3]: ./media/machine-learning-walkthrough-5-publish-web-service/publish3.png
[4]: ./media/machine-learning-walkthrough-5-publish-web-service/publish4.png
[5]: ./media/machine-learning-walkthrough-5-publish-web-service/publish5.png
[6]: ./media/machine-learning-walkthrough-5-publish-web-service/publish6.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[project-columns]: https://msdn.microsoft.com/en-us/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/