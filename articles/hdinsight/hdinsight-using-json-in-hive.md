<properties
   pageTitle="Elemzése és a folyamat JSON HDInsight struktúra tartalmazó dokumentumok |} Microsoft Azure"
   description="Megtudhatja, hogy miként JSON-dokumentumok használata és elemzése őket HDInsight struktúra használatával."
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="06/22/2015"
   ms.author="rashimg"/>


# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a>Folyamat, és elemzése a JSON-dokumentumokból HDInsight struktúra

Megtudhatja, hogy miként folyamat és elemezhetők a JSON-fájlok használatával a struktúra hdinsight szolgáltatásból lehetőségre. Az oktatóprogram fogja használni a következő JSON-dokumentum

    {
        "StudentId": "trgfg-5454-fdfdg-4346",
        "Grade": 7,
        "StudentDetails": [
            {
                "FirstName": "Peggy",
                "LastName": "Williams",
                "YearJoined": 2012
            }
        ],
        "StudentClassCollection": [
            {
                "ClassId": "89084343",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "High",
                "Score": 93,
                "PerformedActivity": false
            },
            {
                "ClassId": "78547522",
                "ClassParticipation": "NotSatisfied",
                "ClassParticipationRank": "None",
                "Score": 74,
                "PerformedActivity": false
            },
            {
                "ClassId": "78675563",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "Low",
                "Score": 83,
                "PerformedActivity": true
            }
        ]
    }

A fájl megtalálható wasbs://processjson@hditutorialdata.blob.core.windows.net/. HDInsight való Azure Blob-tárolóhoz használatáról további tudnivalókért lásd: [használata Fájlrendszerhez-kompatibilis Azure blobtárolóhoz a Hadoop a hdinsight szolgáltatásból lehetőségre](hdinsight-hadoop-use-blob-storage.md). Ha azt szeretné, az alapértelmezett tárolóhoz a fürt is másolja a fájlt.

Ebben az oktatóanyagban a struktúra konzol fogja használni.  A struktúra konzol megnyitása című cikkben olvashat [a távoli asztal HDInsight Hadoop a struktúra használata](hdinsight-hadoop-use-hive-remote-desktop.md).

##<a name="flatten-json-documents"></a>Összeolvasztása JSON-dokumentumok

A következő szakaszban felsorolt módszerek a JSON-dokumentumot, egy sorban szükség. A karakterlánc JSON dokumentumot úgy kell összeolvasztása. Ha már egyszerűsített JSON-dokumentumait, ugorja át ezt a lépést, és adatok elemzése JSON közvetlenül a következő szakaszban lépjen.

    DROP TABLE IF EXISTS StudentsRaw;
    CREATE EXTERNAL TABLE StudentsRaw (textcol string) STORED AS TEXTFILE LOCATION "wasb://processjson@hditutorialdata.blob.core.windows.net/";

    DROP TABLE IF EXISTS StudentsOneLine;
    CREATE EXTERNAL TABLE StudentsOneLine
    (
      json_body string
    )
    STORED AS TEXTFILE LOCATION '/json/students';

    INSERT OVERWRITE TABLE StudentsOneLine
    SELECT CONCAT_WS(' ',COLLECT_LIST(textcol)) AS singlelineJSON
          FROM (SELECT INPUT__FILE__NAME,BLOCK__OFFSET__INSIDE__FILE, textcol FROM StudentsRaw DISTRIBUTE BY INPUT__FILE__NAME SORT BY BLOCK__OFFSET__INSIDE__FILE) x
          GROUP BY INPUT__FILE__NAME;

    SELECT * FROM StudentsOneLine

A nyers JSON-fájl található a **wasbs://processjson@hditutorialdata.blob.core.windows.net/**. A *StudentsRaw* struktúratáblával a nyers nem sík JSON dokumentumra mutat.

*StudentsOneLine* struktúratáblával tárolja az adatokat a HDInsight alapértelmezett ** /json/diákok/elérési út a fájlrendszerben található.

A utasítást a StudentOneLine tábla sík JSON adatokat tartalmazó adataival.

A SELECT utasítás csak térhet vissza 1 sor.

A kimenet a SELECT utasítás a következő:

![A JSON-dokumentum összeolvasztása.][image-hdi-hivejson-flatten]

##<a name="analyze-json-documents-in-hive"></a>A struktúra JSON dokumentumok elemzése

Struktúra lekérdezések futtatása a JSON-dokumentumokon három különböző mechanizmusok nyújtja:

- használja az ISMERKEDÉS\_JSON\_objektum UDF (felhasználó által definiált függvény)
- az JSON_TUPLE UDF használata
- egyéni SerDe használata
- Írja be saját UDF Python vagy más nyelvek használata. Lásd: [Ez a cikk] [ hdinsight-python] a saját Python kód futtatását a struktúra.

### <a name="use-the-getjsonobject-udf"></a>Használja az ISMERKEDÉS\_JSON_OBJECT UDF
Struktúra tartalmaz egy beépített UDF nevű [json objektum beolvasása](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object) JSON lekérdezése futásidőben végre. Ez a módszer két argumentuma – a táblázat neve és a módszer nevét, amelynek a sík JSON-dokumentum és a JSON mező elemezhető kell, hogy tart. Lássunk egy példát, megismerkedhet a UDF működésével.

Az egyes tanulók Vezetéknév és utónév

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

Konzol ablakban a lekérdezés futtatásakor az alábbiakban a kimenet.

![get_json_object UDF][image-hdi-hivejson-getjsonobject]

A get-json_object UDF néhány korlátozások érvényesek.

- Mivel a lekérdezés minden mezője szüksége van, a lekérdezés ismételt értelmezése, a teljesítmény hatással van.
- ELSŐ\_JSON_OBJECT() tömb karakterlánc ábrázolását adja eredményül. Konvertálja a struktúra tömböt, be kell szögletes zárójelek helyett használhatja a reguláris kifejezésekkel ' ["és"] ", és is hívja fel az osztósávot a tömb kérjen.


Ez az az oka annak, hogy a struktúra wiki használatát javasolja json_tuple.  

### <a name="use-the-jsontuple-udf"></a>Az JSON_TUPLE UDF használata

Egy másik UDF struktúra által biztosított hajt végre [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object)jobban [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple) neve. Ez a módszer kulcsok és JSON karakterlánc tart, és egy sor bármelyik eleme egy függvénnyel értékeket adja vissza. A következő lekérdezés a JSON-dokumentumból az iskolai azonosítóját, illetve a besorolási adja eredményül:

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

A kimenet a struktúra konzolban parancsfájl:

![json_tuple UDF][image-hdi-hivejson-jsontuple]

JSON\_sor bármelyik ELEME szintaxisa a [oldalsó megtekintése](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) a struktúra, amely lehetővé teszi, hogy a json\_sor bármelyik eleme virtuális tábla létrehozása a UDT függvényt alkalmazza az eredeti táblázat minden egyes sorára.  Összetett JSONs oldalsó NÉZET ismételt felhasználása miatt túl kezelhetővé vált válnak. Ezenkívül JSON_TUPLE beágyazott JSONs nem tudják kezelni.


###<a name="use-custom-serde"></a>Egyéni SerDe használata

Lehetővé teszi, hogy a JSON séma megadása és a séma használata a dokumentumok elemzésére, SerDe a beágyazott JSON dokumentumok elemzése a legjobb választás. Ebben az oktatóanyagban fogja használni a további népszerű SerDe, amely [rcongiu](https://github.com/rcongiu)identitáskezelési közül.

**Az egyéni SerDe használata:**

1. Telepítse a [Java SE fejlesztési Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR). Válassza a JDK Windows X64 verzióját, ha a fogja használni a Windows központi HDInsight

    >[AZURE.WARNING] Ez a SerDe JDK 1.8 nem működik.

    A telepítés befejezése után adja hozzá az új felhasználó környezeti változó:

    1. Nyissa meg a **Nézet Speciális rendszerbeállítások** a Windows képernyőn.
    2. A **környezeti változók**gombra.  
    3. Adja hozzá az új **JAVA_HOME** környezeti változó mutat **C:\Program Files\Java\jdk1.7.0_55** vagy bárhol a JDK telepítve van.

    ![JDK megfelelő config értékek beállítása][image-hdi-hivejson-jdk]

2. [Maven tesztelése 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip) telepítése

    Adja hozzá a bin mappát az elérési útra Control Panel--> Szerkesztés a fiók Environment változók rendszer változói. Az alábbi kép mutatja be ehhez.

    ![Állítsa be maven tesztelése][image-hdi-hivejson-maven]

3. A projekt [Struktúra-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github webhelyről klónozhatja. Ehhez az "Letöltése Zip-" gombra kattintva az alábbi képernyőképen látható módon.

    ![A projekt klónozhatja][image-hdi-hivejson-serde]

4: Nyissa meg a mappát, ahová letöltötte, és a "mvn csomagja" típus. Meg kell majd másolhatja át a fürthöz szükséges üveg fájlok létrehozása

5: Nyissa meg a célmappát csoportjában a legfelső szintű mappát, amelybe letöltötte az előkészítés. Töltse fel a json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar fájl vezetője-csomópont a fürt. Általában tehettem azt a struktúra bináris mappában találja: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin vagy hasonló.

6: struktúra parancssorba írja be a "üveg /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar hozzáadása". Mivel a lehetőséget választja, a üveg C:\apps\dist\hive-0.13.x\bin mappában, e közvetlenül adhat a üveg nevű alább látható módon:

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

    ![Adding JAR to your project][image-hdi-hivejson-addjar]

Ettől kezdve készen áll a SerDe használatával lekérdezések futtatása a JSON-dokumentum szemben.

Az alábbi utasítás hozzon létre egy táblázatot egy definiált séma

    DROP TABLE json_table;
    CREATE EXTERNAL TABLE json_table (
      StudentId string,
      Grade int,
      StudentDetails array<struct<
          FirstName:string,
          LastName:string,
          YearJoined:int
          >
      >,
      StudentClassCollection array<struct<
          ClassId:string,
          ClassParticipation:string,
          ClassParticipationRank:string,
          Score:int,
          PerformedActivity:boolean
          >
      >
    ) ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
    LOCATION '/json/students';

Az első és utolsó nevét a tanuló listázása

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

Az alábbiakban a struktúra konzolról az eredményt.

![1 SerDe lekérdezés][image-hdi-hivejson-serde_query1]

A JSON-dokumentum értékek összegét számítja ki

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

A fenti lekérdezés használja [oldalsó nézet kihúzása](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF bontsa ki az értékek tömbjét, hogy ezek összesítve is.

Az alábbiakban a kimenet a struktúra konzolról.

![2 SerDe lekérdezés][image-hdi-hivejson-serde_query2]

Megkereséséhez, mely egy adott még több, mint 80 elért pontja hány tárgyak kiválasztása  
      JT. StudentClassCollection.ClassId a json_table jt oldalsó megtekintése (jt. kihúzása StudentClassCollection.Score) webhelycsoport pontszámhoz, ahol pontszám > 80;

A fenti lekérdezés eredménye get eltérően a struktúra tömb\_json\_objektumra, amely karakterláncot ad eredményül.

![3 SerDe lekérdezés][image-hdi-hivejson-serde_query3]

Ha azt szeretné, hogy skil hibás JSON, majd a SerDe [wikilap](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) című cikkben ismertetett módon érhet el, írja be az alábbi kód:  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




##<a name="summary"></a>Összefoglalás
A választott struktúra JSON operátor típusa, tehát igényektől függ. Ha egy egyszerű JSON dokumentum van, és csak egy mezőt a lépések elvégzésével keresheti meg a – van választva használja a struktúra UDF Ismerkedés\_json\_objektumot. Ha egynél több lépések elvégzésével keresheti meg billentyűk majd használhatja json_tuple. Ha egy beágyazott dokumentumot, akkor a JSON SerDe kell használni.

Egyéb kapcsolódó talál.

- [Apache log4j mintafájl elemzéséhez Hadoop HDInsight a struktúra, és HiveQL használata](hdinsight-use-hive.md)
- [Nézetbeli késleltetés adatok elemzése HDInsight struktúra használatával](hdinsight-analyze-flight-delay-data.md)
- [Adatelemzés Twitter segítségével HDInsight struktúra](hdinsight-analyze-twitter-data.md)
- [Egy DocumentDB és HDInsight Hadoop feladat futtatása](../documentdb/documentdb-run-hadoop-with-hdinsight.md)

[hdinsight-python]: hdinsight-python.md

[image-hdi-hivejson-flatten]: ./media/hdinsight-using-json-in-hive/flatten.png
[image-hdi-hivejson-getjsonobject]: ./media/hdinsight-using-json-in-hive/getjsonobject.png
[image-hdi-hivejson-jsontuple]: ./media/hdinsight-using-json-in-hive/jsontuple.png
[image-hdi-hivejson-jdk]: ./media/hdinsight-using-json-in-hive/jdk.png
[image-hdi-hivejson-maven]: ./media/hdinsight-using-json-in-hive/maven.png
[image-hdi-hivejson-serde]: ./media/hdinsight-using-json-in-hive/serde.png
[image-hdi-hivejson-addjar]: ./media/hdinsight-using-json-in-hive/addjar.png
[image-hdi-hivejson-serde_query1]: ./media/hdinsight-using-json-in-hive/serde_query1.png
[image-hdi-hivejson-serde_query2]: ./media/hdinsight-using-json-in-hive/serde_query2.png
[image-hdi-hivejson-serde_query3]: ./media/hdinsight-using-json-in-hive/serde_query3.png
[image-hdi-hivejson-serde_result]: ./media/hdinsight-using-json-in-hive/serde_result.png
