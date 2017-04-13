<properties 
    pageTitle="Lineáris regressziós használata gépi tanulási |} Microsoft Azure" 
    description="Az Excel programban és a Azure gépi tanulási Studio lineáris regressziós modellek összehasonlítása" 
    metaKeywords="" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="cgronlun"  />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/09/2016" 
    ms.author="kbaroni;garye" />

# <a name="using-linear-regression-in-azure-machine-learning"></a>Azure gépi tanulási lineáris regressziós használata

> *Kate Baroni* és *Zsolt Boatman* a Microsoft adatok mélyebb kiválósági központ vállalati megoldás építészek. Ebben a cikkben leírják a meglévő regressziós elemzés csomag áttelepítése az Azure gépi tanulási használatával felhőalapú megoldást lennének.  
 
&nbsp; 
  
[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  
 
## <a name="goal"></a>Cél

A projekt lépések két célok szem előtt:  

1. Használatával a cserélendő analytics tagként a szervezet havi bevételeinek előrejelzések pontosságának javítása  
2. Azure Machine Learning segítségével erősítse meg, a optimalizálása, a sebesség, növelése és az eredmények mértékét.  

Sok vállalkozások, például tagként a szervezet halad, a havi bevételek folyamat előrejelzés át. Az üzleti elemzők kis csoportunk gépi tanulási használatával támogatja a folyamat, és előrejelzési pontosságának növelése lett szolgáltatásait.  A csapat töltött hónapokkal több forrásból származó adatok gyűjtése és az adatok attribútumok átnézett kulcs attribútumok fontos szolgáltatások értékesítési előrejelzés azonosító statisztikai elemzések.  Következő lépések volt prototípusának elkészítéséhez a regressziós statisztikai modellek az adatokat az Excel a kezdéshez.  Néhány héten belül az Excel regressziós modell, amely az aktuális mező és folyamatok előrejelzés finance lett outperforming volt. Ez vált, az eredeti előrejelzési eredményt.  


Azt majd tartott a következő lépés a cserélendő analytics fölé helyezi az Azure Machine Learning megtudhatja, hogy miként javíthatja a Azure Machine Learning prediktív teljesítményére.


## <a name="achieving-predictive-performance-parity"></a>Prediktív teljesítmény eltérés áll fenn elérése

Az első prioritás Azure Machine Learning és az Excel regressziós modelljei közötti eltérés áll fenn eléréséhez volt.  Adott pontos ugyanazokat az adatokat, és az adott felosztás képzés, és azt a cserélendő teljesítmény eltérés áll fenn, Excel és Azure Machine Learning között eléréséhez szeretett volna adatok vizsgálatára.   Az eredetileg nem sikerült. Az Excel modell outperformed az Azure Machine Learning modell.   A hiba történt az alap eszköz beállítás Azure Machine Learning megértése hiánya miatt. Az Azure Machine Learning termékfejlesztők híreit a szinkronizálást azt a beállítást a adatkészletek szükséges számrendszerben jobb megértése szerzett és érhető el, a két modelljei közötti eltérés áll fenn.  

### <a name="create-regression-model-in-excel"></a>Regressziós adatmodell létrehozása az Excelben
Az Excel regressziós használják a szabványos lineáris regressziós modellt az Excel Analysis ToolPak található. 

Számított *abszolút %-os hiba jelenti* azt, és használt a teljesítmény-mérték, a modell.  Az Excelben munka modell érkeznek 3 hónap tartott.  Azt vett fel a tanulási be az Azure Machine Learning kísérletet, amely végül előnyös követelményeinek ismertetése a legtöbb.

### <a name="create-comparable-experiment-in-azure-machine-learning"></a>Összehasonlítható kísérlet létrehozása az Azure gépi tanulási  
Ezeket a lépéseket a kísérlet létrehozása az Azure Machine Learning követett azt:  

1.  Az adatkészlet feltöltött Azure Machine Learning (nagyon kicsi fájl) csv-fájlként
2.  Egy új kísérlet létrehozott és az [Oszlopok kiválasztása az adatkészlet] használt[ select-columns] modulban használható Excel azonos adatokat funkciókat kijelölése   
3.  A [Szétválasztott adatok] használt[ split] modul ( *Relatív kifejezés* módjával), hogy az adatok osztása pontos azonos vonaton beállítása, mint az Excelben elvégzett  
4.  A [Regressziós] végzett kísérletezést[ linear-regression] modul (alapértelmezett beállításai csak), dokumentált, és az eredmények az Excel regressziós modell és összehasonlítása

### <a name="review-initial-results"></a>Kezdeti eredmények áttekintése
Először az Excel-adatmodell pontosan outperformed az Azure Machine Learning modell:  

|   |Excel|Azure Machine Learning|
|---|:---:|:---:|
|Teljesítmény|   |  |
|<ul style="list-style-type: none;"><li>R-négyzet beállítani</li></ul>| 0,96 |A #HIÁNYZIK|
|<ul style="list-style-type: none;"><li>Együtthatója <br />Meghatározása</li></ul>|A #HIÁNYZIK|   0.78<br />(alacsony pontosság)|
|Abszolút jelenti. |  $9. 5M|  $ 19.4 M|
|Abszolút hiba (%) jelenti.|   6.03 %|  12.2 %

Ha probléma adódott a folyamat és az eredmények a fejlesztők és az adatok tudósok a Azure Machine Learning csoport, azok néhány hasznos tippet gyorsan adni.  

* A [Regressziós] használatakor[ linear-regression] Azure Machine Learning moduljának nyújtott két módszer szerint lehetséges:
    *  Online színátmenetes süllyedési: Lehet, alkalmasabb nagyméretű kapcsolatos problémák
    *  Szokásos a legkisebb négyzetek: Ez a legtöbb személy tekintsen úgy, ha lineáris regressziós hallják mód. Kis kimutatásnál hétköznapi a legkisebb négyzetek lehet további optimális választási lehetőséget.
*  Fontolja meg a teljesítmény javítása érdekében a 2 exportügyletek súly paraméter színcsúszkákkal. Beállítás 0,001 alapértelmezés szerint, és a kis az adatkészlet számára, hogy állítsa be a teljesítmény javítása érdekében 0,005.    

### <a name="mystery-solved"></a>A kitalálós végrehajtása!
A javaslatok alkalmazásakor azt Azure Machine Learning, az Excelben azonos eredeti teljesítményét érhető el:   

|| Excel|Azure Machine Learning (kezdőbetűje)|A legkisebb négyzetek w/ Azure Machine Learning|
|---|:---:|:---:|:---:|
|Címkével ellátott érték  |Tényleges (szám)|azonos|azonos|
|Tanuló  |Excel-adatok > regressziós-elemzés >|Lineáris regressziós.|Lineáris regressziós|
|Tanuló beállításai|A #HIÁNYZIK|Alapértelmezés szerint|szokásos a legkisebb négyzetek<br />2 0,005 =|
|Adatok megadása|26 sorok, 3 funkciók, 1 címke.   Az összes numerikus.|azonos|azonos|
|A felosztott: vonaton|Az Excel a először a 18 sorokba, tesztelje a legutóbbi 8 sorokba képzett.|azonos|azonos|
|A felosztott: tesztelése|Az Excel regressziós képletet alkalmazza az utolsó 8 sorok|azonos|azonos|
|**Teljesítmény**||||
|R-négyzet beállítani|0,96|A #HIÁNYZIK||
|A meghatározás vonatkozóan.|A #HIÁNYZIK|0.78|0.952049|
|Abszolút jelenti. |$9. 5M|$ 19.4 M|$9. 5M|
|Abszolút hiba (%) jelenti.|<span style="background-color: 00FF00;">6.03 %</span>|12.2 %|<span style="background-color: 00FF00;">6.03 %</span>|

Ezeken kívül az Excel együtthatók képest jól a szolgáltatás súlyok az Azure képzett modell:

||Az Excel együtthatók|Azure szolgáltatás súlyok|
|---|:---:|:---:|
|METSZ/tapasztalható|19470209.88|19328500|
|A szolgáltatás|0.832653063|0.834156|
|A szolgáltatás B|11071967.08|11007300|
|A szolgáltatás C|25383318.09|25140800|

## <a name="next-steps"></a>Következő lépések

Szeretett volna felhasználása Azure Machine Learning webszolgáltatás az Excel programban.  Az üzleti elemzők Excel támaszkodhat, és azt szükséges, az Azure Machine Learning webszolgáltatásnak az Excel-adatok sor hívja és visszatérés a becsült Excel lehetőséget.   

Azt is szeretett volna optimalizálása a modellt, a beállítások és algoritmusok Azure Machine Learning érhető el.

### <a name="integration-with-excel"></a>Az Excel integrációja
A megoldás volt az Azure Machine Learning regressziós modell üzemeltető a képzett modellből webszolgáltatás létrehozásával.  Néhány percen belül a webszolgáltatással létrehozott és azt sikerült hívja meg közvetlenül az Excel egy várható bevétel értéket adja eredményül.    

A *Web Services Irányítópultjára* szakasz letölthető Excel-munkafüzet tartalmazza.  A munkafüzet megtalálható a webes API-val és a séma szolgáltatásadatok beágyazott előre formázott.   *Excel-munkafüzet letöltése*elemére, nyílik meg, és a helyi számítógépre mentheti.    

![][1]
 
Nyissa meg a munkafüzetet, és másolja a vágólapra az előre definiált paramétereket a kék paraméter szakaszba alább látható módon.  Amikor a paraméterek kerülnek, az Excel hívja a AzureML webszolgáltatás-a becsült pontszáma címkéket a zöld becsült értékek szakasz jeleníti meg.  A munkafüzet továbbra is hozhat létre az előrejelzések paraméterekkel az összes sor azok a rekordok meghatározási paraméterek rész alatti az képzett modellek alapján.   Ezzel a szolgáltatással kapcsolatos további tudnivalókért olvassa el [az Azure gépi tanulási webszolgáltatásnak az Excel használata más](machine-learning-consuming-from-excel.md)című témakört. 

![][2]
 
### <a name="optimization-and-further-experiments"></a>Optimalizálás és további kísérletek
Most, hogy az Excel-modellel alapterv volt, akkor a következő optimalizálhatja az Azure Machine Learning lineáris regressziós modell került.  Akkor használja a modul [szűrő-alapú szolgáltatásainak kiválasztása] [ filter-based-feature-selection] javítható az a kezdeti adatok kijelölés elemeket, és segíteni us elérése a 4.6 %-os teljesítménybeli javulás az abszolút hibaüzenet jelent.   Későbbi projektekhez fogjuk használni ezt a funkciót, ami sikerült menteni us héten léptetés keresse meg a megfelelő szolgáltatáskészletet modellezés használandó adatokat attribútumok keresztül.  

Ezután tervezzük szeretné hozzáadni, például a [Bayesian] további algoritmusok[ bayesian-linear-regression] vagy [Csillapítja döntési fák] [ boosted-decision-tree-regression] a teljesítmény összehasonlítása a kísérlet.    

Kísérletezés a regressziós, próbálja ki egy jó adatkészlet szükség a minta rengeteg numerikus attribútumok Energy hatékonyság regressziós adatkészlet. Az adatkészlet részeként Machine Learning Studio a minta adatkészleteket megadva.  Tanulási modulok számos előre fűtés betöltés vagy a betöltés hűtési is használhatja.  A diagram, az alábbi, szemben a Energy hatékonyság adatkészlet-beli cél változó Cooling betöltés tudomást különböző regressziós teljesítmény összehasonlítása: 

|Modell|Abszolút jelenti.|Legfelső szintű középérték négyzet hiba|Relatív és abszolút hiba|Relatív-négyzet hiba|A meghatározás vonatkozóan.
|---|---|---|---|---|---
|Csillapítja döntési fa|0.930113|1.4239|0.106647|0.021662|0.978338
|Lineáris regressziós (színátmenet süllyedési)|2.035693|2.98006|0.233414|0.094881|0.905119
|A regressziós adagolás hálózati|1.548195|2.114617|0.177517|0.047774|0.952226
|Lineáris regressziós (hétköznapi a legkisebb négyzetek)|1.428273|1.984461|0.163767|0.042074|0.957926  

## <a name="key-takeaways"></a>Fő Takeaways 

Azt a tanultakhoz sokkal által a futó Excel regressziós és Azure gépi tanulási kísérletek párhuzamosan. Az eredeti adatmodell létrehozása az Excelben, és hasonlítsa össze az adatmodellek használatával Azure Machine Learning [Lineáris regressziós] [ linear-regression] segíteni us megtudhatja, hogy Azure Machine Learning, és azt talált adatok kijelölés és a modell teljesítmény javítása lehetőséget.         

Is található, hogy célszerű használni a [szűrő-alapú szolgáltatásainak kiválasztása] [ filter-based-feature-selection] hogy felgyorsítsa az előrejelzési jövőbeli projektekben.  Szolgáltatásainak kiválasztása alkalmazza az adatok, egy továbbfejlesztett modell létrehozhat az Azure Machine Learning jobb általános teljesítményt. 

Az azt jelenti, hogy a cserélendő analitikus előrejelzés az Azure Machine Learning Excel elviselhető átadása lehetővé teszi, hogy jelentősen növeli az azt jelenti, hogy sikeresen széles üzleti felhasználó közönségnek az eredmények javításához.     


## <a name="resources"></a>Erőforrások
Források hasznos segítséget nyújt a regressziós használatakor az jelennek meg:  

* A regressziós az Excelben.  Ha soha nem Megpróbáltam a regressziós az Excelben, ebben az oktatóanyagban egyszerűen: [http://www.excel-easy.com/examples/regression.html](http://www.excel-easy.com/examples/regression.html)
* Előrejelzés a regressziós viewben.  Tyler Chessman írott a blog a cikk az Excel lineáris regressziós egy jó kezdő leírását tartalmazza az előrejelzés idősort miként. [http://sqlmag.com/SQL-Server-Analysis-Services/Understanding-Time-Series-forecasting-Concepts](http://sqlmag.com/sql-server-analysis-services/understanding-time-series-forecasting-concepts)  
*   Szokásos a legkisebb négyzetek lineáris regressziós: Hibákat, problémák és csapdák.  Egy – bevezetés és a regressziós vitafórum: [http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/](http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/ )

[1]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-1.png
[2]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-2.png


<!-- Module References -->
[bayesian-linear-regression]: https://msdn.microsoft.com/library/azure/ee12de50-2b34-4145-aec0-23e0485da308/
[boosted-decision-tree-regression]: https://msdn.microsoft.com/library/azure/0207d252-6c41-4c77-84c3-73bdf1ac5960/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 
