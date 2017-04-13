<properties 
    pageTitle="Struktúra használata a Hadoop webhely napló analysis |} Microsoft Azure" 
    description="Megtudhatja, hogy miként HDInsight webhely naplók elemzéséhez struktúra használata. Akkor fogja használni naplófájlt-HDInsight táblázatba bemeneteként, és HiveQL lekérdezendő adatokat." 
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
    ms.date="05/17/2016" 
    ms.author="nitinme"/>

# <a name="use-hive-with-hdinsight-to-analyze-logs-from-websites"></a>A webhelyekre naplók elemzéséhez HDInsight struktúra használata

Megtudhatja, hogy miként HDInsight webhelyről naplók elemzéséhez HiveQL használata. Webhely napló analysis is használható, oszthatja fel a közönség alapján hasonló tevékenységeket, látogatók kategorizálása demográfiai szerint, és meg, hogy a tartalom azok nézet, a webhelyek származnak, és így tovább.

Az ebben a példában egy HDInsight fürthöz használandó webhely naplófájlok beszerzése a gyakoriság regisztráltak a webhelyen található külső webhelyekről a nap betekintést elemzéséhez. Webhely hibák, amelyek a felhasználói felület összefoglalását is létre fogja. Megtanulhatja, hogy miként:

- Csatlakozás egy Azure Blob-tárolóhoz, mely tartalmazza a webhely naplófájlok.
- Hozzon létre struktúra táblázatok azokat a naplók lekérdezéséhez.
- Az adatok elemzéséhez struktúra lekérdezéseket hozhat létre.
- Használatára a Microsoft Excel HDInsight (az adatok beolvasásához megnyitott adatbázis-összekapcsolás (ODBC) segítségével.

![HDI. Samples.Website.Log.Analysis][img-hdi-weblogs-sample]

##<a name="prerequisites"></a>Előfeltételek

- Azure hdinsight szolgáltatáshoz a Hadoop fürt van kiépítve. Című cikkben olvashat [Rendelkezést HDInsight fürt][hdinsight-provision]. 
- Rendelkeznie kell a Microsoft Excel 2013-ban vagy az Excel 2010 telepítése.
- [A Microsoft struktúra ODBC-illesztőprogram](http://www.microsoft.com/download/details.aspx?id=40886) struktúrából-adatokat importálhatja az Excelbe kell rendelkeznie.


##<a name="to-run-the-sample"></a>A minta futtatása

1. Az [Azure-portálra](https://portal.azure.com/)a Startboard (ha van a fürt, kiemelt) kattintson a minta futtatni szeretné fürt csempére.

2. A fürt lap, a **Tartalom**, a **Fürt irányítópult**elemre, és a a **Fürt irányítópult** lap kattintson **HDInsight fürt irányítópult**. Is közvetlenül megnyitható az irányítópulton a következő URL-cím használatával:

        https://<clustername>.azurehdinsight.net
    
    Amikor a rendszer kéri, a rendszergazdai felhasználónév és jelszó használatával kiépítésekor a fürt hitelesítést végezni.
  
2. Ekkor megnyílik a weblapon kattintson az **Első lépések gyűjtemény** fülre, és a **megoldások a mintaadatokat tartalmazó** kategóriában válassza a **Webhely napló Analysis** minta.

3. Kövesse az utasításokat az weblapon a minta befejezéséhez.

##<a name="next-steps"></a>Következő lépések
Próbálkozzon az alábbi példa: [segítségével a HDInsight struktúra elemzése szenzoradatokat](hdinsight-hive-analyze-sensor-data.md).


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md

[img-hdi-weblogs-sample]: ./media/hdinsight-hive-analyze-website-log/hdinsight-weblogs-sample.png
 
