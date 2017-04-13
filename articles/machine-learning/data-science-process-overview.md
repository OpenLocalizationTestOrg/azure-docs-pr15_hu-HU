<properties
    pageTitle="Mi az csapat adatok tudományos folyamat?  | Microsoft Azure"
    description="A csoportwebhely adatok tudományos folyamat rendszeres módszerrel, amely fejlett analitikai kihasználhatja az intelligens alkalmazások készítéséhez."
    keywords="adatok tudományos folyamatábra, adatok tudományos csapatok"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev" />


# <a name="what-is-the-team-data-science-process-tdsp"></a>Mi az a csapat adatok tudományos folyamat (TDSP)?

A csoportwebhely adatok tudományos folyamat (TDSP) a rendszeres megközelítés intelligens alkalmazások létrehozásába, amely lehetővé teszi a csoportoknak a teljes életciklusáról kapcsolja be a termékek ezeket az alkalmazásokat szükséges tevékenységek fölé hatékony együttműködés adatok tudósok biztosít.

Konkrétan a TDSP jelenleg adatok tudományos csoportokat, nyújtja:

- **Módszertan**: azt ismerteti, hogy néhány lépést, amely meghatározza a nyújt útmutatást a probléma meghatározása, a vonatkozó adatok elemzése, a összeállítása és a prediktív modellek felmérése fejlesztési életciklus, és a vállalati alkalmazások az adott levő telepítését.
- **Erőforrások**: eszközök és technológiák, például a adatok tudományos virtuális tudományos a tevékenységekre vonatkozó adatok környezetet és a gyakorlati útmutatást a családi új technológiák beállítása egyszerűsítése érdekében.

Az alábbiakban a fejlesztési életciklus a csapat adatok tudományos folyamat:

![Ábra: Adatok tudományos folyamat csoportok ](./media/data-science-process-overview/data-science-process-for-teams-diagram.png)


A folyamat nem **ismétlődő**: megértéséhez új és meglévő vagy a modell finomítások követi, és a szükséges lépéseket sorrendben korábban szerzett reworking. Meglévő szervezeti fejlesztés és folyamatok tervezése a project és a TDSP definiált néhány lépést használata **egyszerűen módosítani** .

A folyamat lépéseiért diagrammal, és csatolja a [TDSP tanulási javaslat](https://azure.microsoft.com/documentation/learning-paths/data-science-process/) és leírása alább.  


## <a name="planning-and-preparation-steps"></a>És lépések

## <a name="p1-business-and-technology-planning"></a>P1. Üzleti és technológia tervezése

Indítsa el a analytics projektté a vállalati célok és a problémák megadásával. **Üzleti követelmények**értelmez azokat. Ebben a lépésben központi célját azonosítja a kulcsfontosságú üzleti változók (értékesítési előrejelzése vagy valószínűségértékének inverzét számítja ki sorrend alatt álló csalárd, például) az elemzés kell előrejelzésére, hogy ezeknek a követelményeknek, hogy. További tervezési elengedhetetlen majd általában a projekt céljainak megoldására egy analitikus perspektívából szükséges **adatforrások** megértéséhez fejlesztését. Még nem ritka, például, hogy a meglévő rendszerek összegyűjtése és további típusú megoldhatja a problémát, és a projekt céljainak eléréséhez naplózása kell kereséséhez. Olvassa el [a csapat adatok tudományos folyamat a környezet megtervezése](machine-learning-data-science-plan-your-environment.md) és a [fejlett analitikai az Azure gépi tanulási forgatókönyvek](machine-learning-data-science-plan-sample-scenarios.md).  


## <a name="p2-plan-and-prepare-infrastructure"></a>P2. Tervezése és infrastruktúra előkészítése

A csoportwebhely adatok tudományos folyamat analytics környezetben több összetevők foglalja magában:

- Ha az adatok elő van készítve elemzésre és modellezésre, **adatok munkaterületek**
- egy **infrastruktúra feldolgozása** előfeldolgozása, feltárására és modellezése az adatok
- egy **futtatókörnyezet infrastruktúra** az analitikai modelljei üzemeltető, és futtassa az intelligens ügyfélalkalmazások, amely a modellek mobilalkalmazásokban  

A analytics infrastruktúra kell beállítható, hogy az gyakran különböző alapvető műveleti rendszerekből környezet része. De általában használja a és a vállalaton belül több rendszerekből egyaránt a cég külső forrásból származó adatok. A analytics infrastruktúra lehet tisztán felhőalapú, vagy egy helyszíni telepítés, illetve a két hibrid. Beállítások olvassa el az [adatok tudományos környezetekben a adatok tudományos folyamatban használatra beállítása](machine-learning-data-science-environment-setup.md)című témakört.


## <a name="analytics-steps"></a>Analytics lépéseket:  

## <a name="1-ingest-the-data-into-the-data-platform"></a>1. a adatok ingest be az adatok platform

Első lépésként a megfelelő adatokat vihet különböző forrásokból, akár a belül, illetve a nagyvállalati, kívüli egy analytics környezetbe, hogy hol lehet feldolgozni az adatokat. A **Formátum** adatforrás adatait a cél által igényelt formátumúra eltérhetnek. Így néhány átalakítási is lehet végrehajtani, a bevitel szerszámok. Beállítások olvassa el a [Betöltés adatok elemzéséhez tároló környezetekben](machine-learning-data-science-ingest-data.md) című témakört.

Az adatok első bevitel, mellett számos intelligens alkalmazás szükségesek rendszeresen frissítheti az adatokat a egy folyamatban lévő tanulási folyamatának részeként. Ez a **folyamat adatok** vagy a munkafolyamat elvégezhető. Ez a folyamat, amely tartalmazza az újbóli létrehozása és ismételt értékelése az analitikai modelljei a megoldást üzembe helyezné az intelligens alkalmazás által használt közelítéses részét részét képezi. Látható, például [egy helyszíni SQL server Azure Data Factory az SQL Azure adatainak áthelyezése](machine-learning-data-science-move-sql-azure-adf.md).


## <a name="2-explore-and-visualize-the-data"></a>2. a feltárása és ábrázolása az adatok

A következő lépésként beszerzése a az adatok mélyebb megértése vizsgálja meg a saját **Összesítő statisztika**, kapcsolatok és módszerek segítségével ilyen **megjelenítést**. Ez akkor is, ahol az **adatok minőségét** és integritását, például a hiányzó értékeket, adattípusok eltérnek és ellentmondó adatok kapcsolatok, a problémák kezelnek. Előre feldolgozása átalakítások használatával jelenjenek meg a további analytics előtt nyers adatokból és modellezése történhet. Leírása olvassa el a [továbbfejlesztett gépi adatainak előkészítése feladatok tanulási](machine-learning-data-science-prepare-data.md)elemre.


## <a name="3-generate-and-select-features"></a>3. hozza létre, és jelölje be a szolgáltatások

Adatok tudósok tartomány szakértők együtt kell azonosítania a Funkciók, amelyek rögzítése az adatkészlet dolgozhat tulajdonságainak és, amely a tervezés során azonosított kulcsfontosságú üzleti változók előrejelzésére legjobb használható. Új funkciókról származhat a meglévő adatokat, vagy további adatokat szeretne gyűjteni megkövetelheti. Ez a folyamat **szolgáltatás műszaki** nevezik és egyik egy hatékony prediktív elemzési rendszer épület fő lépéseit. Ebben a lépésben a hírcsatornájában, az adatok feltárása lépésben nyert és tartomány szakértelmét kreatív kombinációját szükséges. Olvassa el [az adatok tudományos folyamatban műszaki funkció](machine-learning-data-science-create-features.md).


## <a name="4-create-and-train-machine-learning-models"></a>4. létrehozása és gépi tanulási modellek képzése

Adatok tudósok készítése a legfontosabb változók a tervezési lépést eltávolította adatokkal és featurized meghatározott üzleti követelmények jelölt előrejelzésére analitikai modellek. Gépi tanulási rendszerek támogatja a több **modellezése algoritmusok** , amely számos különböző típusú esetek vonatkoznak. Olvassa el [az Azure gépi tanulási algoritmusok kiválasztásáról](machine-learning-algorithm-choice.md).

Adatok tudósok kell választania a legmegfelelőbb modell a szövegbevitel tevékenységeit, és nem ritka, hogy több modellek származó találatok jelenjenek meg kell-e a legjobb teljesítmény elérése érdekében. A bemeneti adatok modellezése az általában oszlik véletlenszerűen három részből áll:

- egy adathalmaz – oktatás
- egy adatérvényesítési adathalmaz
- tesztelés adatkészlet

A modellek kialakításának a **oktatás adatkészletet**. (Beállítva paraméterekkel) modellek optimális kombináció ki van jelölve a modellek futtatása és az előrejelzési hibák **érvényességi adatkészlet**mérési. Végül az **adatkészlet tesztelése** használják a választott modell független adatok nem láthatja el ismeretekkel vagy a modell érvényesítése használt a teljesítmény ki szeretné számítani.  Bemutatókból megtudhatja, [hogy miként Azure gépi tanulási modell teljesítménye ki szeretné számítani](machine-learning-evaluate-model-performance.md).


## <a name="5-deploy-and-consume-the-models-in-the-product"></a>5. üzembe helyezéséhez és a modellek, a termék felhasználása

Miután felkínálunk egy sor olyan modellek jól végeznek, ez lehet **operationalized** más alkalmazások használhatnak. Attól függően, hogy az üzleti követelmények előrejelzések **valós** idejű vagy egy **Köteg** alapon történik. Operationalized, a modellek kell a egy **API-felület megnyitása** különböző alkalmazásból könnyedén felhasznált kitéve ilyen online-webhelyen, táblázatokat, irányítópultok vagy az üzleti és kódmentes alkalmazások sor. Lásd: [az Azure gépi tanulási webszolgáltatás Deploy](machine-learning-publish-a-machine-learning-web-service.md).


## <a name="summary-and-next-steps"></a>Összefoglaló és a következő lépések

A [Csoportwebhely adatok tudományos folyamat](https://azure.microsoft.com/documentation/learning-paths/data-science-process/) modellezni néhány lépést többször is, az intelligens alkalmazások létrehozásához használandó fejlett analitikai szükséges, hogy **útmutatást** a feladatokat. Minden egyes lépés is részletesen használatáról a Microsoft-technológiák a ismertetett feladatokat.

TDSP nem írhatnak a **dokumentáció** eltérések adott típusú, azt is a dokumentumhoz az adatok feltárása, modellezési és értékelési eredményét, és a lényegesek kódot mentheti, így az elemzés is többször, szükség esetén a legjobb megoldás. Ez is lehetővé teszi, hogy a analytics munka újrafelhasználása más hasonló adatokat és előrejelzési szolgáltatása feladatok érintő alkalmazások használatakor.

Teljes-végpont forgatókönyvek, amelyek bemutatják a **különböző forgatókönyvekben** a folyamat lépéseinek is találhatók. Ezeket megtalálja a [csapat adatok tudományos folyamat forgatókönyvek](data-science-process-walkthroughs.md) témakörben miniatűr leírásokkal.
