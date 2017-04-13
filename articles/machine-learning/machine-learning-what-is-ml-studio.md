<properties 
    pageTitle="Mi az Azure gépi tanulási Studio? | Microsoft Azure"
    description="Azure Machine Learning Studio, használatra kész tárban modulokat és algoritmusok modelljei gyorsan készítéséhez és húzás eszköz áttekintése."
    keywords="Azure gépi tanulási, azure Machine Learning, Machine Learning studio"
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
    ms.topic="get-started-article"
    ms.date="09/09/2016"
    ms.author="garye"/>

# <a name="what-is-azure-machine-learning-studio"></a>Mi az Azure gépi tanulási Studio?

Microsoft Azure gépi tanulási Studio egy olyan együttműködési, húzással történő áthelyezés eszköz, hozhat létre, tesztelése és az adatokon prediktív analytics-megoldások telepítése. Gépi tanulási Studio, egyszerűen egyéni alkalmazások vagy Üzletiintelligencia-eszközöket, például az Excel által igénybe vehető szolgáltatások webes teszi közzé a modelleket.

Gépi tanulási Studio, ahol adatokat tudományos, prediktív analytics, felhőalapú források és az adatok felel meg.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-machine-learning-studio-interactive-workspace"></a>Interaktív Studio a gépi tanulási munkaterület

Prediktív elemzése adatmodell hozzanak, általában egy vagy több forrásból származó adatok használata, átalakítás és különböző adatfeldolgozás és a statisztikai függvények, hogy az adatok elemzése és létrehozni az eredmények. Ismétlődő eljárással jelennek meg a modell elkészítésének. A különböző függvényekre, és a paraméterek módosítása közben, az eredmények szerveződik mindaddig, amíg meg elégedve, hogy rendelkezik-e egy képzett és hatékony modell.

**Azure gépi tanulási Studio** lépve egy interaktív, képes munkaterület egyszerűen összeállítása, tesztelése és a prediktív analysis modell találta. Húzással történő áthelyezés ***adatkészleteket*** , és az elemzés ***modulok*** alakzatot egy interaktív vászon, csatlakozás őket egy ***kísérletezés***, amely futtatja a gépi tanulási Studio képez. Találta meg a modell tervezés, a kísérlet szerkesztése, ha szükségesnek látja másolatot, és futtassa újra. Ha készen áll, az ***oktatás kísérletezhet*** átalakítása egy ***prediktív kísérlet***, és majd közzétehető ***webszolgáltatás*** , hogy a modell mások által is elérhető.

>[AZURE.TIP] Töltse le és nyomtatása a diagram, amely a gépi tanulási Studio funkcióinak áttekintést, lásd: [Azure gépi tanulási Studio funkciók áttekintése diagram](machine-learning-studio-overview-diagram.md).

Nem kötelező, programozás csak vizuálisan csatlakozás adatkészleteket, és a modulokat a cserélendő analysis modell Egyenletszerkesztővel nem.

![Azure Machine Learning Studio diagram: kísérletek létrehozása, olvassa el az adatok több forrásból, pontszáma adatok írásához, írja be az adatmodellek.][ml-studio-overview]

## <a name="get-started-with-machine-learning-studio"></a>Első lépések a gépi tanulási Studio

Amikor először [Gépi tanulási Studio](https://studio.azureml.net) adja meg a **Kezdőlap** lap látható. További lehetőségek megtekintése a dokumentáció, videókat, webináriumok, és más értékes erőforrások.

Vannak olyan három fül a nézetválasztóval: **Home** (hogy honnan indítja), **Studio**és **gyűjtemény**.

### <a name="studio"></a>Studio

Kattintson a **Studio** fülre, és meg kell adnia, hogy jelentkezzen be Microsoft-fiókja vagy a munkahelyi vagy iskolai fiók segítségével. Miután bejelentkezett, a következő lapokon jelenik meg a bal oldalon:

- **Projektek** - kísérletek, a adatkészleteket, a jegyzetfüzetek és a más források, amely egyetlen projektként gyűjteménye
- **Kísérletek** - kísérletek létrehozott, futtatni és vázlatként mentett
- **WEBSZOLGÁLTATÁSOK** - webszolgáltatásokhoz, a kísérletek a telepített
- **JEGYZETFÜZET** - létrehozott jegyzetfüzetek Jupyter
- **ADATKÉSZLETEK** - feltöltött Studio átalakítása adathalmazok
- **Az ADATMODELLEK képzett** - modellek, a kísérletek képzett és mentett Studio
- **Beállítások** – olyan beállítások, állítsa be a fiók és az erőforrásokat használó gyűjteménye.

### <a name="gallery"></a>Gyűjtemény

Kattintson a **tár** fülre, és fogja venni a Cortana üzletiintelligencia-dokumentumtárba. A gyűjtemény a hely, ahol adatok tudósok és a fejlesztők Közösség összetevőinek a Cortana üzletiintelligencia-programcsomag használatával létrehozott megoldások is megoszthatja.

A gyűjtemény kapcsolatos további tudnivalókért lásd [Megosztás és a Cortana üzletiintelligencia-gyűjtemény megoldások felfedezése](machine-learning-gallery-how-to-use-contribute-publish.md).

## <a name="components-of-an-experiment"></a>Egy kísérlet összetevői

Egy kísérlet összekapcsolása prediktív elemzése adatmodell létrehozása az analitikai modulok elődcelláinak adatkészleteket áll. Egy érvényes kísérlet kifejezetten, ezek a jellemzők foglalja magában:

- A kísérlet tartalmaz legalább egy adatkészlet és egy modul
- Adatkészletek csatlakozhat csak modulok
- Modulok csatlakozhat adatkészleteket vagy más modulok
- Modulokat az összes beviteli portok néhány kapcsolatot az adatfolyam kell rendelkeznie.
- Az összes szükséges minden modulban paramétereinek be kell állítani.

Egy kísérlet nulláról hozhat létre, vagy egy meglévő minta kísérlet sablonként is használhatja. További tudnivalókért olvassa el a [használata minta kísérletek hozhat létre új kísérletek](machine-learning-sample-experiments.md)című témakört.

Példa egy egyszerű kísérlet létrehozásáról olvassa el a [létrehozása az Azure gépi tanulási Studio egy egyszerű kísérlet](machine-learning-create-experiment.md)című témakört.

Részletesebb ismertetését megtalálja a cserélendő analytics megoldás létrehozásakor olvassa el a [fejlesztése Azure gépi tanulási és előrejelzési megoldást](machine-learning-walkthrough-develop-predictive-solution.md)című témakört.

### <a name="datasets"></a>Adatkészletek

Egy adathalmaz, hogy a modellezési folyamat használható gépi tanulási Studio feltöltötte adatokat. Egy minta adatkészletek számos megtalálható a gépi tanulási Studio kísérletezni meg, és feltöltheti további adatkészleteket, szükség szerint. Íme néhány példa része adatkészletek:

- **Különböző Gépkocsikhoz MPG adatok** - km / automobiles jelölt száma henger, lóerő stb értékek gallon (MPG).
- **Mell rák adatok** - mell rák diagnosztikai adatok.
- **Erdő akkor következik be adatainak** - erdő fire méretű északkeleti portugáliai.

Egy kísérlet ahogy választhat a rendelkezésre álló adatkészletek lista bal oldalán a vászonra.

Minta adatkészleteket gépi tanulási Studio szereplő listáját ismerje meg [az Azure gépi tanulási Studio minta adatkészletek](machine-learning-use-sample-datasets.md).

### <a name="modules"></a>Modulok

A modul az adatokon végrehajtható algoritmus. Gépi tanulási Studio számos modulok oktatás, pontozási és érvényességi folyamatok bejövő adatok adatfüggvényei kezdve. Az alábbiakban néhány példát a modulokat tartalmazza:

- [ARFF átalakítása] [ convert-to-arff] – alakítja a .NET szerializálásának adatkészlet attribútum-kapcsolata fájl formátuma (ARFF).
- [Általános iskolai statisztika számítása] [ elementary-statistics] – például középérték, szórás stb általános iskolai statisztika számítja ki.
- [Lineáris regressziós] [ linear-regression] -hoz létre egy online színátmenet süllyedési alapján lineáris regresszióval modellt.
- [Modell pontszám] [ score-model] -Scores képzett besorolás vagy a regressziós modell.

Egy kísérlet ahogy bal oldalán a vászonra választhat a rendelkezésre álló modulok listája.  

Előfordulhat, hogy a modul konfigurálása a modul belső algoritmusok használható paraméterek csoportja. Amikor kijelöl egy modult a vászonra, a modul Paraméterek jelenik meg a **Tulajdonságok** ablaktábla jobb oldalán a vásznat. Módosíthatja a paramétereket, hogy a modell finomhangolása ablaktáblában.

Az egyes súgó navigáláshoz gépi tanulási algoritmusok érhető el a nagyméretű tárat, elolvashatja, [hogyan választhatja ki a Microsoft Azure gépi tanulási algoritmusok](machine-learning-algorithm-choice.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>Prediktív analytics webes szolgáltatás üzembe helyezése

Amikor elkészült a cserélendő analytics modell, webes szolgáltatás közvetlenül az gépi tanulási Studio telepítheti. A ezt a folyamatot, lásd: [az Azure gépi tanulási webszolgáltatás Deploy](machine-learning-publish-a-machine-learning-web-service.md).

[ml-studio-overview]:./media/machine-learning-what-is-ml-studio/azure-ml-studio-diagram.jpg

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
