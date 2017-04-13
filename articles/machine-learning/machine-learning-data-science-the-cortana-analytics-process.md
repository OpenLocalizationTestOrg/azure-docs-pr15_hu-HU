<properties 
    pageTitle="Mi az csapat adatok tudományos folyamat?  | Microsoft Azure" 
    description="A csoportwebhely adatok tudományos folyamat rendszeres módszerrel, amely fejlett analitikai kihasználhatja az intelligens alkalmazások készítéséhez." 
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

A [Csoportwebhely adatok tudományos folyamat (TDSP)](data-science-process-overview.md) a rendszeres megközelítés intelligens alkalmazások létrehozásába, amely lehetővé teszi a csoportoknak a teljes életciklusáról kapcsolja be a termékek ezeket az alkalmazásokat szükséges tevékenységek fölé hatékony együttműködés adatok tudósok biztosít. A TDSP néhány lépést, amely **útmutatásokat találhat a probléma, az eszközök beállítása és a szükséges, a vonatkozó adatok elemzése, összeállítása és a prediktív modellek felmérése és telepítheti a vállalati alkalmazások az adott levő környezet definiálásáról** ismertet. 

A lépések a következők **csapat adatok tudományos**folyamat:  

![A munkafolyamat Vonalvég](./media/machine-learning-data-science-the-cortana-analytics-process/CAP-workflow.png)

A folyamat nem **ismétlődő**: megértéséhez új és meglévő vagy a modell finomítások követi, és a szükséges lépéseket sorrendben korábban szerzett reworking. Meglévő szervezeti fejlesztés és folyamatok tervezése a project és a TDSP definiált néhány lépést használata **egyszerűen módosítani** . 

A folyamat lépéseiért diagrammal, és csatolja a [TDSP tanulási javaslat](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) és leírása alább.  

## <a name="preparation-steps"></a>Előkészületek 

## <a name="p1-plan-the-analytics-project"></a>P1. A analytics projekt tervezése 

Indítsa el a analytics projektté a vállalati célok és a problémák megadásával. **Üzleti követelmények**értelmez azokat. Ebben a lépésben központi célját azonosítja a kulcsfontosságú üzleti változók (értékesítési előrejelzése vagy valószínűségértékének inverzét számítja ki sorrend alatt álló csalárd, például) az elemzés kell előrejelzésére, hogy ezeknek a követelményeknek, hogy. További tervezési elengedhetetlen majd általában a projekt céljainak megoldására egy analitikus perspektívából szükséges **adatforrások** megértéséhez fejlesztését. Még nem ritka, például, hogy a meglévő rendszerek összegyűjtése és további típusú megoldhatja a problémát, és a projekt céljainak eléréséhez naplózása kell kereséséhez. Olvassa el [a csapat adatok tudományos folyamat a környezet megtervezése](machine-learning-data-science-plan-your-environment.md) és a [fejlett analitikai az Azure gépi tanulási forgatókönyvek](machine-learning-data-science-plan-sample-scenarios.md).  

## <a name="p2-setup-analytics-environment"></a>P2. Analytics-környezet beállítása 

A csoportwebhely adatok tudományos folyamat analytics környezetben több összetevők foglalja magában: 

- Ha az adatok elő van készítve elemzésre és modellezésre, **adatok munkaterületek** 
- egy **infrastruktúra feldolgozása** előfeldolgozása, feltárására és modellezése az adatok
- egy **futtatókörnyezet infrastruktúra** az analitikai modelljei üzemeltető, és futtassa az intelligens ügyfélalkalmazások, amely a modellek mobilalkalmazásokban  

A analytics infrastruktúra kell beállítása, hogy az gyakran különböző alapvető műveleti rendszerekből környezet része. De általában használja a és a vállalaton belül több rendszerekből egyaránt a cég külső forrásból származó adatok. A analytics infrastruktúra lehet tisztán felhőalapú, vagy egy helyszíni telepítés, illetve a két hibrid. Beállítások olvassa el az [adatok tudományos környezetekben a adatok tudományos folyamatban használatra beállítása](machine-learning-data-science-environment-setup.md)című témakört.

## <a name="analytics-steps"></a>Analytics lépéseket:  

## <a name="1-ingest-data-into-the-analytical-environment"></a>1. adatok ingest analitikai környezetbe 

Első lépésként a megfelelő adatokat vihet különböző forrásokból, akár a belül, illetve a nagyvállalati, kívüli egy analitikus környezetekben, ahol az adatok feldolgozhatók. A **Formátum** adatforrás adatait a cél által igényelt formátumúra eltérhetnek. Így néhány átalakítási is lehet végrehajtani, a bevitel szerszámok. Beállítások olvassa el a [Betöltés adatok elemzéséhez tároló környezetekben](machine-learning-data-science-ingest-data.md) című témakört.

Az adatok első bevitel, mellett számos intelligens alkalmazás szükségesek rendszeresen frissítheti az adatokat a egy folyamatban lévő tanulási folyamatának részeként. Ez a **folyamat adatok** vagy a munkafolyamat elvégezhető. Ez a folyamat, amely tartalmazza az újbóli létrehozása és ismételt értékelése az analitikai modelljei a megoldást üzembe helyezné az intelligens alkalmazás által használt közelítéses részét részét képezi. Látható, például [egy helyszíni SQL server Azure Data Factory az SQL Azure adatainak áthelyezése](machine-learning-data-science-move-sql-azure-adf.md).


## <a name="2-explore-and-pre-process-data"></a>2. a feltárása előre adatok és feldolgozása 

A következő lépésként beszerzése a az adatok mélyebb megértése vizsgálja meg a saját **Összesítő statisztika** , kapcsolatok és módszerek segítségével ilyen **megjelenítést**. Ez akkor is, ahol az **adatok minőségét** és integritását, például a hiányzó értékeket, adattípusok eltérnek és ellentmondó adatok kapcsolatok, a problémák kezelnek. Előre feldolgozása átalakítások használatával jelenjenek meg a további analytics előtt nyers adatokból és modellezése történhet. Leírása olvassa el a [továbbfejlesztett gépi adatainak előkészítése feladatok tanulási](machine-learning-data-science-prepare-data.md)elemre.


## <a name="3-develop-features"></a>3. a szolgáltatások kidolgozása 

Adatok tudósok tartomány szakértők együtt kell azonosítania a Funkciók, amelyek rögzítése az adatkészlet dolgozhat tulajdonságainak és, amely a tervezés során azonosított kulcsfontosságú üzleti változók előrejelzésére legjobb használható. Új funkciókról származhat a meglévő adatokat, vagy további adatokat szeretne gyűjteni megkövetelheti. Ez a folyamat **szolgáltatás műszaki** nevezik és egyik egy hatékony prediktív elemzési rendszer épület fő lépéseit. Ebben a lépésben a hírcsatornájában, az adatok feltárása lépésben nyert és tartomány szakértelmét kreatív kombinációját szükséges. Olvassa el [az adatok tudományos folyamatban műszaki funkció](machine-learning-data-science-create-features.md).


## <a name="4-create-predictive-models"></a>4. a cserélendő modellek létrehozása 

Adatok tudósok készítése a legfontosabb változók a tervezési lépést eltávolította adatokkal és featurized meghatározott üzleti követelmények jelölt előrejelzésére analitikai modellek. Gépi tanulási rendszerek támogatja a több **modellezése algoritmusok** , amely számos különböző típusú esetek vonatkoznak. Olvassa el [a csapat Azure gépi tanulási algoritmusok kiválasztásáról](machine-learning-algorithm-choice.md).

Adatok tudósok kell választania a legmegfelelőbb modell a szövegbevitel tevékenységeit, és nem ritka, hogy több modellek származó találatok jelenjenek meg kell-e a legjobb teljesítmény elérése érdekében. A bemeneti adatok modellezése az általában oszlik véletlenszerűen három részből áll:

- egy adathalmaz – oktatás 
- egy adatérvényesítési adathalmaz 
- tesztelés adatkészlet 

A modellek kialakításának a **oktatás adatkészletet**. (Beállítva paraméterekkel) modellek optimális kombináció ki van jelölve a modellek futtatása és az előrejelzési hibák **érvényességi adatkészlet**mérési. Végül az **adatkészlet tesztelése** használják a választott modell független adatok nem láthatja el ismeretekkel vagy a modell érvényesítése használt a teljesítmény ki szeretné számítani.  Bemutatókból megtudhatja, [hogy miként Azure gépi tanulási modell teljesítménye ki szeretné számítani](machine-learning-evaluate-model-performance.md).


## <a name="5-deploy-and-consume-models"></a>5. üzembe helyezéséhez és az adatmodellek felhasználása 

Miután felkínálunk egy sor olyan modellek jól végeznek, ez lehet **operationalized** más alkalmazások használhatnak. Attól függően, hogy az üzleti követelmények előrejelzések **valós** idejű vagy egy **Köteg** alapon történik. Operationalized, a modellek kell a egy **API-felület megnyitása** különböző alkalmazásból könnyedén felhasznált kitéve ilyen online-webhelyen, táblázatokat, irányítópultok vagy az üzleti és kódmentes alkalmazások sor. Lásd: [az Azure gépi tanulási webszolgáltatás Deploy](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="summary-and-next-steps"></a>Összefoglaló és a következő lépések

A [Csoportwebhely adatok tudományos folyamat](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) modellezni néhány lépést többször is, az intelligens alkalmazások létrehozásához használandó fejlett analitikai szükséges, hogy **útmutatást** a feladatokat. Minden egyes lépés is részletesen használatáról a Microsoft-technológiák a ismertetett feladatokat. 

Miközben TDSP nem írhatnak a **dokumentáció** eltérések adott típusú, a dokumentumhoz az adatok feltárása, modellezési és értékelési eredményét a legjobb, és mentése a lényegesek kódot, így az elemzés is többször is szükség esetén. Ez is lehetővé teszi, hogy a analytics munka újrafelhasználása más hasonló adatokat és előrejelzési szolgáltatása feladatok érintő alkalmazások használatakor.

Teljes-végpont forgatókönyvek, amelyek bemutatják a **különböző forgatókönyvekben** a folyamat lépéseinek is találhatók. Látható, például:

- [A csoportwebhely adatok tudományos folyamat működését: az SQL Server használatával](machine-learning-data-science-process-sql-walkthrough.md)
- [a csapat adatok tudományos folyamat működését: HDInsight Hadoop fürt használatával](machine-learning-data-science-process-hive-walkthrough.md).
- [Azure HD.mdnsight használ a külső adatok tudományos](machine-learning-data-science-spark-overview.md)
- [Az Azure adatok tó méretezhető adatok tudományos: egy végpont – útmutató](machine-learning-data-science-process-data-lake-walkthrough.md)

 
