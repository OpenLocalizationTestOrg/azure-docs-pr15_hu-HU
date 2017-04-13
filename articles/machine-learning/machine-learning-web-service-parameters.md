<properties 
    pageTitle="Azure gépi tanulási Web Service paraméterek használata |} Microsoft Azure" 
    description="Hogyan lehet Azure gépi tanulási webes szolgáltatás paraméterek használata a modell működésének módosítása a webes szolgáltatás megnyitásakor." 
    services="machine-learning" 
    documentationCenter="" 
    authors="raymondlaghaeian" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="raymondl;garye"/>

#<a name="use-azure-machine-learning-web-service-parameters"></a>Azure gépi tanulási Web Service paraméterek használata

Az Azure gépi tanulási webszolgáltatás, amely tartalmazza a modulokat konfigurálható paraméterekkel kísérlet közzétéve jön létre. Egyes esetekben előfordulhat, hogy kívánt modul viselkedésének módosítása a webes szolgáltatás futtatásakor. *Webes szolgáltatás paraméterei* lehetővé teszi, hogy a feladat végrehajtásához. 

Gyakori példa beállítja az [Adatok importálása] [ reader] modul, hogy a felhasználó közzétett webszolgáltatás lehetőségeiről másik adatforrást a webszolgáltatás érhető el. És konfigurálását az [Adatok exportálása] [ writer] modul, hogy egy másik helyre adható meg. Néhány további példa olyan eltolással módosítás [Funkció kivonatolás] [ feature-hashing] modul vagy a számát a [szűrő-alapú szolgáltatásainak kiválasztása] kívánt szolgáltatások[ filter-based-feature-selection] modulra. 

Webes szolgáltatás paramétereket állíthat, és egy vagy több modul paraméterekkel kapcsolhatja őket a kísérlet, és megadhatja, hogy kötelező és választható. A felhasználó a webszolgáltatás is majd adjon meg értéket paraméterértékek azokat a webszolgáltatás hívásakor. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


##<a name="how-to-set-and-use-web-service-parameters"></a>Beállítása és használata a webes szolgáltatás paraméterei

A paraméter egy modul melletti ikonra, és válassza a "Webhely szolgáltatás paraméterként beállítása" definiálása egy webszolgáltatási paraméter. Ezzel létrehoz egy új webszolgáltatási paraméter, és azt, hogy a modul paraméter csatlakozik. Ezután a webszolgáltatás érhető el, amikor a felhasználó adhatja meg a webes szolgáltatás paraméter egy értéket, és a modul paraméter érvényesül.

Miután egy webszolgáltatási paraméter megadása esetén érhető el más modul paraméterhez a kísérlet az. Ha megad egy webszolgáltatási paraméter egy modul paraméter társított, is használhatja, hogy egy webszolgáltatási paraméter más modulhoz mindaddig, amíg a paraméterrel vár, azonos típusú értéket. Például ha a webszolgáltatási paraméter egy numerikus érték, majd azt csak használható számértéket várt modul paramétereinek. Amikor a felhasználó beállítása a webszolgáltatási paraméter egy értéket, azt az összes kapcsolódó modul paraméterei fog vonatkozni.

Beállíthatja, hogy a webszolgáltatási paraméter egy alapértelmezett értéket a szükséges-e. Ha nem tesz, a paraméter megadása nem kötelező, a felhasználó a webszolgáltatás. Ha Ön nem adja meg egy alapértelmezett értéket, majd a felhasználó van szükség írjon be egy értéket, amikor a webszolgáltatás érhető el.

A webszolgáltatás API dokumentációjában többek között a webes szolgáltatás felhasználó a programozás útján megadása a webszolgáltatási paraméter, amikor a webszolgáltatás.

>[AZURE.NOTE] A API-klasszikus webszolgáltatás dokumentáció az webszolgáltatás **IRÁNYÍTÓPULT** gépi tanulási Studio **API súgó, oldal** kapcsolaton keresztül. Új webes szolgáltatás dokumentációjában API a webszolgáltatás az [Azure gépi tanulási Web Services](https://services.azureml.net/Quickstart) portál a **felhasználandó** és **Swagger API** lapon megadva.


##<a name="example"></a>Példa

Példaként tegyük fel van még egy kísérlet- [Adatok exportálása] [ writer] modul, amely adatokat küld a Azure blob-tárolóhoz. Azt fogja egy webszolgáltatási paraméter "Blob-elérési út" nevű határozza meg, hogy a felhasználó webes szolgáltatás módosításához a az elérési utat a blob-tárolóhoz, ha a szolgáltatás érhető el.

1.  Gépi tanulási Studio, kattintson az [Adatok exportálása] [ writer] jelölje ki a modul. Megjeleníti a tulajdonságait a kísérlet vászon jobbra a Tulajdonságok ablaktáblában jelennek meg.

2.  A tároló típusának megadása:

    - "Azure Blob-tárolóhoz" csoportban **Adjon adatok cél**.
    - A **, kérjük, adja meg a hitelesítés típusa**jelölje ki a "Fiókot".
    - Adja meg a fiók adatait az Azure blob-tárolóhoz. 
    <p />

3.  Kattintson a ikonra az **elérési útját blob-tárolóban paraméter kezdődő**jobbra. Hogy néz ki:

    ![Webes szolgáltatás lekérdezésparaméter ikon][icon]

    Jelölje be a "Webhely szolgáltatás paraméterként beállítása".

    Egy bejegyzés megjelenik a **Webes szolgáltatás paraméterei** "Elérési útjának blob-tárolóhoz kezdetű" nevű a Tulajdonságok ablaktábla alján. Most már Web Service paraméter: az [Adatok exportálása] társított[ writer] modul paraméter.

4.  A webszolgáltatási paraméter átnevezéséhez kattintson a nevére, írja be a "Blob-elérési út", és nyomja le az **Enter** billentyűt. 
 
5.  A webszolgáltatási paraméter egy alapértelmezett értéket a szükséges, kattintson a nevétől jobbra látható ikonra, jelölje be az "Adja meg az alapértelmezett érték", írjon be egy értéket (például "container1/output1.csv"), és nyomja le az **Enter** billentyűt.

    ![Webszolgáltatási paraméter][parameter]

6.  Kattintson a **Futtatás**parancsra. 

7.  Kattintson a **Webes szolgáltatás üzembe** , és válasszon **Webes szolgáltatás üzembe [klasszikus]** és a **[Új] webes szolgáltatás üzembe** bevezetését tervezi a webszolgáltatás.

A felhasználó a webszolgáltatás most adhatja meg más rendeltetési [Adatok exportálása] [ writer] a webszolgáltatás elérésekor modulra.

##<a name="more-information"></a>További információ

Részletesebb például lásd: a [Gépi tanulási Blog](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx)a [Webes szolgáltatás paraméterei](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) tételt.

További tájékoztatást a gépi tanulási webszolgáltatás elérése tájékozódhat [a közzétett gépi tanulási webszolgáltatás felhasználni](machine-learning-consume-web-services.md).



<!-- Images -->
[icon]: ./media/machine-learning-web-service-parameters/icon.png
[parameter]: ./media/machine-learning-web-service-parameters/parameter.png


<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
 
