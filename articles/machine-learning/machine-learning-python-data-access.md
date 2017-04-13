<properties 
    pageTitle="Gépi tanulási Python ügyfél tárral adatkészleteket elérése |} Microsoft Azure" 
    description="Telepítésével és használatával a Python ügyfél tár eléréséhez és Azure gépi tanulási adatok biztonságos kezeléséhez a helyi Python-környezetből." 
    services="machine-learning" 
    documentationCenter="python" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="huvalo;bradsev" />


#<a name="access-datasets-with-python-using-the-azure-machine-learning-python-client-library"></a>Az Access adatkészleteket a Python az Azure gépi tanulási Python ügyfél-tár használata 

A Microsoft Azure gépi tanulási Python ügyfél tár előzetes is biztonságos hozzáférés az Azure gépi tanulási adatkészleteket szeretne egy helyi Python környezetből és létrehozásának és kezelésének adatkészletek munkaterületen.

Ez a témakör nyújt útmutatást olvashat:

* Telepítse a gépi tanulási Python ügyfél-tárban 
* hozzáférés, és töltse fel a adatkészleteket, többek között az Azure gépi tanulási adatkészleteket hozzáférhet a helyi Python környezet engedélyezési beszerzése útmutatást
*  az Access köztes adatkészleteket, a kísérletek
*  számba veheti a webhely adatkészleteket, metaadat-alapú hozzáférés, egy adatkészlet tartalmának felolvasása, hozzon létre új adatkészletek és módosítása meglévő adatkészleteket használható a Python ügyfél könyvtár

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

 
##<a name="prerequisites"></a>Előfeltételek

A Python ügyfél tár teszteléssel csoportban a következő környezetben:

 - A Windows, Mac és Linux
 - Python 2.7, 3.3 és 3.4.

A következő csomagolásán függőség van:

 - kérések
 - Python-dateutil
 - pandas

Azt javasoljuk, hogy egy Python terjesztési, például a fent felsorolt [Anaconda](http://continuum.io/downloads#all) vagy [lombkoronaszint](https://store.enthought.com/downloads/), amely beépített Python, IPython és a három csomagok tartalmaz. Bár IPython nem feltétlenül szükséges, akkor egy nagy környezet kezelésére és adatokat interaktív megjelenítése.


###<a name="installation"></a>Az Azure gépi tanulási Python ügyfél tár telepítése

Az Azure gépi tanulási Python ügyfél tár is telepíteni kell az ebben a témakörben ismertetett műveletek elvégzéséhez. Érhető el a [Python csomag Index](https://pypi.python.org/pypi/azureml). Telepítse a Python környezetben, a következő parancsot a helyi Python környezetből:

    pip install azureml

Azt is megteheti töltse le és telepítse a [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python)forrásokból.

    python setup.py install

Mely számjegy a számítógépre telepítve van, ha a mezőpont közvetlenül a mely számjegy tárházba telepítéséhez is használhatja:

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


##<a name="datasetAccess"></a>Studio kódtöredék adatkészleteket eléréséhez használt

A Python ügyfél tár programozott hozzáférést biztosít a meglévő adatkészleteket a kísérletek már futtatott.

A Studio webes kapcsolatról hozhat létre, amely tartalmazza a szükséges információkat, töltse le és adatkészleteket deszerializálni Pandas DataFrame objektumként a hely a gépen kódtöredék.

### <a name="security"></a>Biztonság az adatokhoz való hozzáférés

A kódtöredék nyújtotta Studio, használja a Python ügyfél tárral is a munkaterület-azonosítóját és engedélyezési jogkivonat. Ezek a munkaterület teljes hozzáférést biztosít, és kell védeni, például a jelszó.

Biztonsági okokból a kód kódtöredékének funkció csak a szerepkör **tulajdonosaként** beállítása a munkaterület rendelkező felhasználók számára elérhető. A szerepkör Azure gépi tanulási Studio megjelenik a **felhasználók** lapon **a beállítások**csoportban.

![Biztonsági][security]

Ha szerepköre **tulajdonosaként**nincs beállítva, vagy egy tulajdonosaként következőre, vagy kérje meg a munkaterület, hogy a kódrészletet biztosítson kérelem is.

A jóváhagyási jogkivonat beszerzéséhez végezheti el az alábbiak egyikét:



- Kérje meg a jogkivonat a tulajdonos. Tulajdonosok a munkaterület Studio alkalmazásban a beállítások lap a engedélyezési tokenek érhető el. A bal oldali ablaktáblában válassza a **Beállítások** , majd kattintson az **Engedélyezés TOKENEK** az elsődleges és másodlagos tokenek megjelenítéséhez.  Az elsődleges vagy a másodlagos engedélyezési tokenek a kódrészletet is használható, bár javasoljuk, hogy a tulajdonosok csak megosztani a másodlagos engedélyezési tokenek.

![](./media/machine-learning-python-data-access/ml-python-access-settings-tokens.png)

- Kérje meg a tulajdonos szerepe előléptethető.  Ehhez a jelenlegi tulajdonosa a munkaterület kell először távolítsa el, akkor a munkaterületen, majd ismételt meghívása, azt egy tulajdonosaként.

Miután beszerzett, hogy a fejlesztők a munkaterület-azonosítóját és engedélyezési jogkivonat, hogy azok a munkaterületen, függetlenül a szerepkör a kódrészletet segítségével elérheti.

Engedély tokenek kezelhetők a **Engedélyezési TOKENEK** lapon **a beállítások**csoportban. Helyreállíthatók őket, de ezt az eljárást az előző token való hozzáférés visszavonása.

### <a name="accessingDatasets"></a>Az Access adatkészleteket egy helyi Python alkalmazásból

1. A gépi tanulási Studióban kattintson a bal oldali navigációs sávján **ADATKÉSZLETEKET** .

2. Jelölje ki az adatkészlet szeretne elérni. A adatkészleteket bármelyikét választhatja a **Saját ADATKÉSZLETEKET** listából, vagy a **minták** listából.

3. Az alsó eszköztáron kattintson az **adat-hozzáférési kódot készítése**. Ha az adatok nem kompatibilis a Python ügyfél tárral formátumban, ez a gomb le van tiltva.

    ![Adatkészletek][datasets]

4. Válassza ki a kódrészletet az ablakban, amely megjelenik, és másolja a vágólapra.

    ![Hozzáférési kódot.][dataset-access-code]

5. Illessze be a kódot a helyi Python alkalmazás a jegyzetfüzetet.

    ![Jegyzetfüzet][ipython-dataset]

## <a name="accessingIntermediateDatasets"></a>Az Access köztes adatkészleteket a gépi tanulási kísérletek

Egy kísérlet a gépi tanulási Studio futtatása után is lehet a köztes adatkészleteket hozzáférhet a kimeneti csomópontok modulok. Köztes adatkészleteket adatok létrehozott és köztes lépést használt modell eszköz futtatásakor.

Köztes adatkészleteket mindaddig, amíg az adatformátum kompatibilis a Python ügyfél tárral is elérhető.

A következő formátumok támogatottak (az olyan ezeknél a `azureml.DataTypeIds` osztály):

 - Egyszerű szöveg
 - GenericCSV
 - GenericTSV
 - GenericCSVNoHeader
 - GenericTSVNoHeader

A formátum megállapítható által mutatva modul kimeneti csomópontot. A csomópont nevét, az elemleírás együtt jelenik meg.

Néhány modulokat, például [felosztott] [ split] modul, kimeneti nevű formátumba `Dataset`, amelyek nem támogatják a Python ügyfél tárat.

![Adatkészlet formázása][dataset-format]

Akkor használja a konvertálás modul, például [CSV átalakítása][convert-to-csv], hogy az első olyan eredménye egy támogatott formátumba.

![GenericCSV formázása][csv-format]

A következő lépések bemutatják egy példa létrehoz egy kísérlet, letöltődött és fér hozzá az köztes adatkészlet.

1. Hozzon létre egy új kísérlet.

2. Szúrjon be egy **bináris besorolás felnőtt népszámlálás bevétel adatkészlet** modulra.

3. [Felosztott] [ split] modult, és csatlakoztassa a bemeneti az adatkészlet modul kimeneti.

4. Beszúrása, [konvertálása CSV] [ convert-to-csv] modult, és csatlakoztassa a bemeneti egy [osztott] [ split] modul exportálja.

5. Mentse a kísérlet, indítsa el, és várja meg, futó befejezéséhez.

6. Kattintson a kimeneti csomópontra a [CSV-fájlok konvertálása] [ convert-to-csv] modulra.

7. Amikor megjelenik a helyi menü, jelölje be az **adat-hozzáférési kódot készítése**.

    ![Helyi menüje][experiment]

8. Jelölje ki a kódrészletet, és a megjelenő ablakban másolja a vágólapra.

    ![Hozzáférési kódot.][intermediate-dataset-access-code]

9. Illessze be a jegyzetfüzet.

    ![Jegyzetfüzet][ipython-intermediate-dataset]

10. Az adatok matplotlib használatával is ábrázolása. Ez a életkorát tartalmazó hisztogram jeleníti meg:

    ![Hisztogram][ipython-histogram]


##<a name="clientApis"></a>A gépi tanulási Python ügyfél tár segítségével elérheti, olvasása, létrehozása és adatkészleteket kezelése

### <a name="workspace"></a>Munkaterület

A munkaterületen található a a Python ügyfél tár. Adja meg a `Workspace` osztály-munkaterületi azonosító és engedélyezési token egy példány létrehozása:

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a>Számba veheti a webhely adatkészleteket

Az adott munkaterületen található összes adatkészleteket számbavétele:

    for ds in ws.datasets:
        print(ds.name)

A csak a felhasználó által létrehozott adatkészleteket számbavétele:

    for ds in ws.user_datasets:
        print(ds.name)

A csak a példa adatkészleteket számbavétele:

    for ds in ws.example_datasets:
        print(ds.name)

Egy adatkészlet neve (a kis-és nagybetűket) érheti el:

    ds = ws.datasets['my dataset name']

Vagy index által elérhető:

    ds = ws.datasets[0]


### <a name="metadata"></a>Metaadatok

Adatkészletek metaadatok tartalom kívül van. (Köztes adatkészleteket kivétel ez alól a szabály, és nem rendelkezik bármely metaadatok.)

A felhasználó által létrehozáskor oszthatók néhány metaadatok értékét:

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

Mások által Azure Machine Learning megadott értékek a következők:

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

Lásd: a `SourceDataset` tudhat meg a rendelkezésre álló metaadatok osztály.


### <a name="read-contents"></a>További tartalma

A kódtöredék gépi tanulási Studio automatikusan megkapja töltse le és az adatkészlet Pandas DataFrame objektum deszerializálni. Ez a lépés a `to_dataframe` módszer:

    frame = ds.to_dataframe()

Töltse le a nyers adatokat, és végezze el a deszerializálás, saját maga szeretné, ha ez egy lehetőséget. Pillanatában így a csak az formátumokat, például az "ARFF", amely a Python ügyfél tár deszerializálni nem.

A szövegként, olvassa el a tartalom:

    text_data = ds.read_as_text()

Bináris olvasása a tartalma:

    binary_data = ds.read_as_binary()

Nyissa meg a tartalomhoz adatfolyam is egyszerűen:

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a>Hozzon létre egy új adatkészletet

A Python ügyfél tár lehetővé teszi az Python programban adatkészleteket feltöltése. Ezeket a adatkészleteket majd érhetők el a saját munkaterületre való használatra.

Az adatok egy Pandas DataFrame, ha használja a következő kódot:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Ha az adatok már szerializált, használható:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

A Python ügyfél médiatár nem tudja a Pandas DataFrame, az alábbi formátumok szerializálni (az olyan ezeknél a `azureml.DataTypeIds` osztály):

 - Egyszerű szöveg
 - GenericCSV
 - GenericTSV
 - GenericCSVNoHeader
 - GenericTSVNoHeader


### <a name="update-an-existing-dataset"></a>Egy meglévő adatkészlet frissítése

Ha egy új adatkészletet, amelynek neve megegyezik egy meglévő adatkészlet feltöltéséhez próbál, szerezheti be egy ütközést hiba.

Egy meglévő adatkészlet frissítéséhez először kell a meglévő adatkészlet hivatkozás:

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Majd `update_from_dataframe` szerializálni és cseréje az adatkészlet Azure a tartalmát:

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Ha az alapértelmezettől eltérő adatok szerializálni, a választható értékének meghatározása `data_type_id` paraméter.

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Másik lehetőségként beállíthatja, hogy az új leírást az érték megadásával a `description` paraméter.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up to feb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to feb 2015'

Másik lehetőségként beállíthatja, hogy egy új nevet az érték megadásával a `name` paraméter. Ettől kezdve a beolvashatja fogja az adatkészlet az új nevet. A következő kódot frissíti az adatokat, nevét és leírását.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up to feb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up to feb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

A `data_type_id`, `name` és `description` paraméterek nem kötelező, és az alapértelmezett az előző értékre. A `dataframe` paraméter mindig szükség.

Ha az adatok már szerializált, `update_from_raw_data` helyett `update_from_dataframe`. Ha a fázis `raw_data` helyett `dataframe`, hasonló módon működik.



<!-- Images -->
[security]:./media/machine-learning-python-data-access/security.png
[dataset-format]:./media/machine-learning-python-data-access/dataset-format.png
[csv-format]:./media/machine-learning-python-data-access/csv-format.png
[datasets]:./media/machine-learning-python-data-access/datasets.png
[dataset-access-code]:./media/machine-learning-python-data-access/dataset-access-code.png
[ipython-dataset]:./media/machine-learning-python-data-access/ipython-dataset.png
[experiment]:./media/machine-learning-python-data-access/experiment.png
[intermediate-dataset-access-code]:./media/machine-learning-python-data-access/intermediate-dataset-access-code.png
[ipython-intermediate-dataset]:./media/machine-learning-python-data-access/ipython-intermediate-dataset.png
[ipython-histogram]:./media/machine-learning-python-data-access/ipython-histogram.png


<!-- Module References -->
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 
