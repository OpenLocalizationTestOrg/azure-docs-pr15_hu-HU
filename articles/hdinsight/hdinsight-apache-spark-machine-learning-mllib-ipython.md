<properties 
    pageTitle="Gépi tanulási alkalmazások HDInsight létrehozásához használandó Apache külső |} Microsoft Azure" 
    description="Lépésenkénti útmutató gépi tanulási alkalmazások létrehozásához használandó Apache külső a jegyzetfüzetek" 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="machine-learning-predictive-analysis-on-food-inspection-data-using-mllib-with-apache-spark-cluster-on-hdinsight-linux"></a>A gép tanulási: MLlib használata Apache a külső adatok HDInsight Linux fürt élelmiszer vizsgálatára prediktív elemzése

> [AZURE.TIP] Ebben az oktatóanyagban érhető el, a külső (Linux) fürtre HDInsight létrehozott Jupyter jegyzetfüzet. A jegyzetfüzet felület teszi lehetővé a Python kódrészletek lebonyolítása a jegyzetfüzetet önmagában. A jegyzetfüzeten belül az oktatóprogram elvégzéséhez hozzon létre egy külső fürt, indítsa el a jegyzetfüzet Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), majd futtassa a jegyzetfüzetnek **a külső gépi tanulási - prediktív élelmiszer ellenőrzési adatok elemzése MLLib.ipynb** a **Python** mappában találja.


Ez a cikk bemutatja, hogyan lehet egy egyszerű prediktív elemzést végezhet a egy megnyitott adatkészlet **MLLib**, külső a beépített gépi tanulási a tárak használatával. MLLib egy core hasznosak gépi tanulási tevékenységek, beleértve alkalmas segédprogramok segédprogramok számos külső tárban:

* Besorolás

* Regressziós

* Fürtözés

* A témakör modellezési

* Egyes érték dekompozíciós (SVD) és a fő összetevő-elemzés (PEM)

* Tesztelés, és a statisztikai minta kiszámításához hipotézis

Ez a cikk bemutatja egy egyszerű *besorolás* keresztül logisztikus regresszió megközelítés.

## <a name="what-are-classification-and-logistic-regression"></a>Mik azok a osztályozás és logisztikus regresszió?

*Osztályozási*, a gyakori gépi tanulási tevékenység, a bemeneti adatok rendezése kategóriákba során a rendszer. A feladat hozzárendelése az Ön által adatok beviteléhez "címkék" megállapítása besorolás algoritmus. Például lehet, hogy elfogadja a bemeneti adataiként tárolt információkat, és a készlet elosztja a két kategóriába gépi tanulási algoritmus számításba: készletek amely kell eladja és készletek amely meg kell tartania.

Logisztikus regresszió a algoritmus, az osztályozási használt. A külső logisztikus regresszió API akkor lehet hasznos *bináris besorolás*vagy a bemeneti adatok osztályozásához két csoportok egyikébe. Logisztikus hátrányait kapcsolatos további tudnivalókért olvassa el a [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression)című témakört.

Összefoglalásként tehát elmondhatjuk a logisztikus regresszió illesztése *logisztikus függvény* annak a valószínűségét, hogy egy beviteli vektoros tartozik, vagy egy másik előrejelzésére használható.  

## <a name="what-are-we-trying-to-accomplish-in-this-article"></a>Mi a Microsoft próbál elvégezheti az ebben a cikkben?

Külső kell használni az egyes prediktív analízist adaton élelmiszer ellenőrzés (**Food_Inspections1.csv**) lett szerezte be, a [Város a Békéscsabai portálon](https://data.cityofchicago.org/)keresztül. Ez az adatkészlet élelmiszer ellenőrzések, amely az egyes élelmiszer létrehozása, amelyek lett ellenőrzött információt, a megsértése talált (ha van ilyen), és a vizsgálat eredményeit, Chicago végezték információkat tartalmazza. Adatok CSV-fájl már érhető el a fürthöz **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**társított tárterület-fiókjában.

Az alábbi lépéseket a modell tekintheti meg, mi szükséges időt sikeres és sikertelen a élelmiszer ellenőrzése fejlesztése. 

## <a name="start-building-a-machine-learning-application-using-spark-mllib"></a>Indítsa el a gépi tanulási-alkalmazást a külső MLlib létrehozása

1. Az [Azure-portált](https://portal.azure.com/)a a startboard kattintson a csempére a külső fürt (Ha a startboard a kiemelt). Az **Összes böngészése**a fürthöz is navigálhat > **Fürt hdinsight szolgáltatásból lehetőségre**.   

2. A külső fürt lap **Fürt irányítópult**elemre, és kattintson a **Jupyter jegyzetfüzet**elemre. Ha a rendszer kéri, írja be a fürt a rendszergazdai hitelesítő adataival.

    > [AZURE.NOTE] Előfordulhat, hogy is elér Jupyter jegyzetfüzet a fürt számára a böngészőben nyissa meg az alábbi URL-címet. __CLUSTERNAME__ cserélje ki a fürt neve:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Hozzon létre új jegyzetfüzetet. Kattintson az **Új**gombra, és kattintson a **PySpark**.

    ![Új Jupyter jegyzetfüzet létrehozása] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.createnotebook.png "Új Jupyter jegyzetfüzet létrehozása")

3. Egy új jegyzetfüzetet létre, és a megnyitott Untitled.pynb nevű. Kattintson a jegyzetfüzet nevére a képernyő tetején, és írjon be egy rövid nevet.

    ![Adjon egy nevet a jegyzetfüzetnek] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.notebook.name.png "Adjon egy nevet a jegyzetfüzetnek")

3. Mivel a PySpark kernelt jegyzetfüzet hozta létre, nem kell bármely környezetek kifejezetten létrehozása. A külső és struktúra környezetek automatikusan létrejön, az első kód cellát futtatásakor. A gépi tanulási alkalmazás ebben az esetben az szükséges fájltípusok importálásával felépíteni elindíthatja. Ehhez helyezze a kurzort abba a cellába, és nyomja le a **SHIFT + ENTER BILLENTYŰKOMBINÁCIÓT**.


        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        from pyspark.sql.functions import UserDefinedFunction
        from pyspark.sql.types import *

## <a name="construct-an-input-dataframe"></a>A beviteli dataframe létrehozása

Ábrázolásakor `sqlContext` végre szeretne hajtani átalakítások strukturált adatok. Az első tevékenység, a mintaadatok ((**Food_Inspections1.csv**)) betölteni egy külső SQL *dataframe*-bA. 

1. Mivel a nyers adatokból egy CSV-formátumban, a külső környezet használatával a fájl minden sorát olvashat be a memória strukturálatlan szövegként szükség Ezután használatával Python a CSV-tár minden egyes sor egyenként elemzése. 


        def csvParse(s):
            import csv
            from StringIO import StringIO
            sio = StringIO(s)
            value = csv.reader(sio).next()
            sio.close()
            return value
        
        inspections = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                        .map(csvParse)


2. A CSV-fájl egy RDD, most már van. Egy sor tudassa velünk beolvashatja a RDD az adatok a séma megértéséhez.


        inspections.take(1)


    Meg kell jelennie a kibocsátás, az alábbihoz hasonló:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [['413707',
          'LUNA PARK INC',
          'LUNA PARK  DAY CARE',
          '2049789',
          "Children's Services Facility",
          'Risk 1 (High)',
          '3250 W FOSTER AVE ',
          'CHICAGO',
          'IL',
          '60625',
          '09/21/2010',
          'License-Task Force',
          'Fail',
          '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of the plumbing section of the Municipal Code of Chicago and Rules and Regulation of the Board of Health. OBSEVERD THE 3 COMPARTMENT SINK BACKING UP INTO THE 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding to protect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED TO REPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN THE REAR CHILDREN AREA,IN THE KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED TO REPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY THE 15MOS AREA. NEED TO BE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY THE EXPOSED HAND SINK IN THE KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: The floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED TO ELEVATE ALL FOOD ITEMS 6INCH OFF THE FLOOR 6 INCH AWAY FORM WALL.  ',
          '41.97583445690982',
          '-87.7107455232781',
          '(41.97583445690982, -87.7107455232781)']]


3. A fenti kimenet lehetőséget ad arról, a bemeneti fájl; a séma a fájl minden létrehozását, létrehozását, a cím, az ellenőrzések és a hely, többek között az adatok típusát nevét tartalmazza. Jelöljük ki néhány oszlopot, amely lehet hasznos, a cserélendő elemzésre és az eredmények csoportosítása egy dataframe, majd használjuk ideiglenes táblázatot szeretne létrehozni, amely szerint.


        schema = StructType([
        StructField("id", IntegerType(), False), 
        StructField("name", StringType(), False), 
        StructField("results", StringType(), False), 
        StructField("violations", StringType(), True)])

        df = sqlContext.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
        df.registerTempTable('CountResults')

4. Most már van egy *dataframe* `df` amelyen azt hajthatják végre az elemzés. Azt is **CountResults**hívás ideiglenes táblázat. Azt már tartalmazza a dataframe érdekes 4 oszlopból: **azonosítója**, a **nevét**, az **eredmények**és a **megsértése**. 
    
    Az adatok kis minta pedig hozzuk működésbe:

        df.show(5)

    Meg kell jelennie a kibocsátás, az alábbihoz hasonló:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +------+--------------------+-------+--------------------+
        |    id|                name|results|          violations|
        +------+--------------------+-------+--------------------+
        |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
        |391234|       CAFE SELMARIE|   Fail|2. FACILITIES TO ...|
        |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
        |413708|BENCHMARK HOSPITA...|   Pass|                    |
        |413722|           JJ BURGER|   Pass|                    |
        +------+--------------------+-------+--------------------+

## <a name="understand-the-data"></a>Az adatok ismertetése

1. Mit tartalmaz az adatkészlet egy értelmezhető megszerezni első. Ha például Mik azok a oszlopban az **eredményeket** a különböző értékek?


        df.select('results').distinct().show()

    
    Meg kell jelennie a kibocsátás, az alábbihoz hasonló:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        +--------------------+
        |             results|
        +--------------------+
        |                Fail|
        |Business Not Located|
        |                Pass|
        |  Pass w/ Conditions|
        |     Out of Business|
        +--------------------+
    
2. Rövid adatábrázolás is segítse a szolgáltatás kapcsolatban a következő eredmények elosztása oka. Az adatok ideiglenes táblázatban **CountResults**már van. Az alábbi SQL-lekérdezés futtatását is lehetővé teszi a táblázat első egy jobban megértheti az eredmények terjesztését ismertető szemben.

        %%sql -o countResultsdf
        SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results

    A `%%sql` mágikus követ `-o countResultsdf` biztosítja, hogy a lekérdezés eredményét a Jupyter kiszolgálón (általában a fürt headnode) helyben tárolt. A kimenet egy [Pandas](http://pandas.pydata.org/) dataframe a megadott név **countResultsdf**, mint az állandó.
    
    Meg kell jelennie a kibocsátás, az alábbihoz hasonló:
    
    ![SQL-lekérdezés eredménye] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/query.output.png "SQL-lekérdezés eredménye")

    További információt a `%%sql` mágikus, valamint egyéb találhatók meg a PySpark kernel magics lásd: [a külső HDInsight fürt Jupyter jegyzetfüzetek mag](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).

3. Matplotlib, az adatok megjelenítési használt tár létrehozása egy ábra is használhatja. Az ábra a helyben tárolt **countResultsdf** dataframe kell létrehozni, mert a kódrészletet kell kezdődnie a `%%local` mágikus. Ezzel biztosíthatja, hogy a kód Jupyter kiszolgálón helyileg futó.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        
        labels = countResultsdf['results']
        sizes = countResultsdf['cnt']
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    Meg kell jelennie a kibocsátás, az alábbihoz hasonló:

    ![Eredmény kimeneti](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_13_1.png)


4. Láthatja, hogy nincsenek-e ellenőrzést rendelkező 5 eltérő eredmények.
    
    * Nem található Business 
    * FAIL:
    * A sikeres
    * Terméktámogatási szolgálat w/ feltételt, és
    * Kijelentkezés a vállalati verzió 

    Tudassa velünk kidolgozása modell, amely az így létrejövő egy élelmiszer ellenőrzés tud törni a megsértése adni. Mivel a logisztikus regresszió egy bináris osztályozási módszer, célszerű az adatok csoportosítására két kategóriába sorolhatók: **nem sikerül** , és **haladnak**. A "megfelelt w/ feltételek" még mindig a fázis, így azt láthatja el ismeretekkel a modellt, ha azt veszi a két eredményt egyenértékű. Az eredmények ("Nem található Business", "Házon kívül üzleti") az adatok sem hasznos, így azt eltávolítja őket a oktatás beállítása. Meg kell lennie rendben, mivel a két kategóriákkal mégis alkotó nagyon kicsi százalékos az eredmények.

5. Tudassa velünk a jóváhagyást, és a meglévő dataframe alakítsa (`df`) be egy új dataframe, amelyben minden egyes ellenőrzés megfelelője a címke-megsértése pár. A lehetőséget választja, a címke `0.0` egy hiba, a címke jelöli `1.0` siker és a címke jelöli `-1.0` néhány eredményeket amellett, e két jelöli. Azt kiszűri ezeket az eredményeket az új adatok keret számításakor.


        def labelForResults(s):
            if s == 'Fail':
                return 0.0
            elif s == 'Pass w/ Conditions' or s == 'Pass':
                return 1.0
            else:
                return -1.0
        label = UserDefinedFunction(labelForResults, DoubleType())
        labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')


    Egy sor nézzük beolvasni a címkével ellátott adatait, hogy lássa a néz ki.


        labeledData.take(1)


    Meg kell jelennie a kibocsátás, az alábbihoz hasonló:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of the food establishment and all parts of the property used in connection with the operation of the establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF THE FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: The flow of air discharged from kitchen fans shall always be through a duct to a point above the roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT TO DINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT TO OFFICE.")]


## <a name="create-a-logistic-regression-model-from-the-input-dataframe"></a>A beviteli dataframe logisztikus regresszió adatmodell létrehozása

Az utolsó művelet a címkével ellátott adatokat táblává logisztikus regresszióval elemezheti formátumban. A bemeneti egy logisztikus regresszió módszerével kell egy sor olyan *címke-szolgáltatás vektoros párokká*, számok, amely a bemeneti pont valamilyen módon vektor esetén a "funkció vektoros". Igen szükség az "megsértése" oszlop, amely a félig strukturált és a szabad szöveges, szeretné, hogy a gép sikerült könnyen értelmezhető valós számok tömbjével megjegyzésének sok tartalmaz konvertálása lehetőséget. 

Egy szabványos gépi tanulási természetes nyelvi feldolgozásra megközelítés, hogy rendeljen minden különálló szó "index", majd vektornak átadni a gépi tanulási algoritmus, hogy az egyes index értéket tartalmazza a relatív gyakoriságot a karakterláncban található szó. 

MLLib egyszerűvé művelet végrehajtásához. Első lépésként azt fogja "tokenize" az egyes szavak lépéseivel a mindegyik szöveges karakterláncot mindegyik megsértése szöveges karakterláncot, és ezután használjuk egy `HashingTF` minden egyes készletben tokenek alakítani, amelyben a modell összeállításához logisztikus regresszió módszerével majd átadandó szolgáltatás vektornak. Azt fogja használata az Office összes ezeket a lépéseket a sorozat "folyamat" használatával.


    tokenizer = Tokenizer(inputCol="violations", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
    
    model = pipeline.fit(labeledData)


## <a name="evaluate-the-model-on-a-separate-test-dataset"></a>A modell egy külön vizsgálat adatkészlet a felmérése

A létrehozott korábbi *előrejelzésére* milyen eredményeinek modell ábrázolásakor új ellenőrzések lesz, az, hogy a megfigyelt megsértése alapján. Ez a modell meg az adatkészlet **Food_Inspections1.csv**képzett azt. Tudassa velünk be egy második adatkészlet, **Food_Inspections2.csv**, *ki* ezt a modellt, kattintson az új adatok erőssége. A második adatkészlet (**Food_Inspections2.csv**) már az alapértelmezett tároló tárolóban fürthöz társított kell lennie.

1. Az alábbi kódtöredékének hoz létre egy új dataframe, **predictionsDf** , amely tartalmazza a modell által generált a szövegbevitel. A kódtöredék is ideiglenes táblát hoz létre **az előrejelzések** a dataframe alapján.


        testData = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                 .map(csvParse) \
                 .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
        testDf = sqlContext.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
        predictionsDf = model.transform(testDf)
        predictionsDf.registerTempTable('Predictions')
        predictionsDf.columns


    Meg kell jelennie a kibocsátás, az alábbihoz hasonló:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
        
        ['id',
         'name',
         'results',
         'violations',
         'words',
         'features',
         'rawPrediction',
         'probability',
         'prediction']

2. Tekintse meg az előrejelzések közül. Futtassa a kódtöredék:

        predictionsDf.take(1)

    A szövegbevitel teszt adatok megadása az első bejegyzés jelenik meg.

3. A `model.transform()` módszer ugyanazt az átalakítást alkalmazása ugyanarra a sémára az új adatokat, és bemutatja, hogyan sorolják be az adatok előrejelzése érkeznek. Hogyan pontos az előrejelzések volt, hogy néhány egyszerű statisztika azt teheti meg:


        numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR 
                                              (prediction = 1 AND (results = 'Pass' OR 
                                                                   results = 'Pass w/ Conditions'))""").count()
        numInspections = predictionsDf.count()
        
        print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
        print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"

    A kimeneti formátumban, például a következőket:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        There were 9315 inspections and there were 8087 successful predictions
        This is a 86.8169618894% success rate


    Logisztikus regresszió használata külső ad us megsértése leírások angolul és arról, hogy a megadott üzleti e volna sikeres és sikertelen egy élelmiszer ellenőrzés közötti kapcsolatra egy pontos típusát. 

## <a name="create-a-visual-representation-of-the-prediction"></a>A szövegbevitel vizuálisan ábrázolhatók létrehozása

Azt is most Egyenletszerkesztővel a végleges képi megjelenítést kell, hogy segítse a szolgáltatás oka kapcsolatban a vizsgálat eredményeit. 

1. Azt a különböző előrejelzések és az eredmények kiolvasó indítsa el a korábban létrehozott **előrejelzések** ideiglenes táblából. A következő lekérdezések külön a kimenet *true_positive*, *false_positive*, *true_negative*és *false_negative*. A lekérdezésekben, az alábbi, akkor kapcsolja ki a képi megjelenítések használatával `-q` és is mentheti a kimenet (használatával `-o`) dataframes, majd kínál, mint a `%%local` mágikus. 

        %%sql -q -o true_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'

        %%sql -q -o false_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')

        %%sql -q -o true_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'

        %%sql -q -o false_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions') 

2. Végül a következő kódtöredékének segítségével **Matplotlib**használatával az ábra létrehozása.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        labels = ['True positive', 'False positive', 'True negative', 'False negative']
        sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')
    
    Meg kell jelennie a következő eredményt.
    
    ![Szövegbevitel kimeneti](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_26_1.png)


    Ez a diagram "pozitív" eredményt hivatkozik a sikertelen élelmiszer vizsgálat, míg a negatív eredmény utal, amelyet a átadott ellenőrzése.

## <a name="shut-down-the-notebook"></a>A jegyzetfüzet leállítása

Miután végzett az alkalmazást futtató, tegye a következőt: leállítás engedje fel az erőforrások jegyzetfüzetet. Ehhez a jegyzetfüzetet a **fájl** menüben kattintson a **bezárása és leállíthatja**. Ez lesz Leállítás és a jegyzetfüzet bezárása.


## <a name="seealso"></a>Lásd még:


* [Áttekintés: A külső Apache a Azure hdinsight szolgáltatáshoz](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Felhasználási területei

* [A BI külső: interaktív adatelemzés használata a külső HDInsight az Üzletiintelligencia-eszközeiről](hdinsight-apache-spark-use-bi-tools.md)

* [A külső és gépi tanulási: használata külső a HDInsight épület hőmérsékleti fűtés-és Légtechnikai adatok elemzéséhez](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [A külső adatfolyam: Használata külső a HDInsight valós idejű adatfolyam alkalmazások készítéséhez](hdinsight-apache-spark-eventhub-streaming.md)

* [Webhely napló analysis HDInsight külső használata](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Létrehozása és futtatása alkalmazások

* [Scala használatával önálló-alkalmazás létrehozása](hdinsight-apache-spark-create-standalone-application.md)

* [Feladat távolról futtatható a külső fürtre Livius használatával](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Eszközök és bővítmények

* [Létrehozása és elküldése külső Scala alkalmazást IntelliJ arról HDInsight eszközök beépülő modul használatával](hdinsight-apache-spark-intellij-tool-plugin.md)

* [A külső alkalmazások távolról hibáinak IntelliJ arról HDInsight eszközök beépülő modul használatával](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [A HDInsight külső fürt Zeppelin jegyzetfüzetek használata](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Elérhető az HDInsight-külső fürthöz Jupyter jegyzetfüzet mag](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Külső csomagok Jupyter jegyzetfüzeteket használata](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter telepítése a számítógépen, és csatlakozzon az HDInsight külső fürthöz](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Erőforrások kezelése

* [A Apache külső fürt Azure hdinsight szolgáltatáshoz a források kezelése](hdinsight-apache-spark-resource-manager.md)

* [A a HDInsight-Apache külső fürthöz nyomon követése és hibakeresési feladatok](hdinsight-apache-spark-job-debugging.md)
