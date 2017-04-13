<properties
    pageTitle="Adatelemzés érzékelő segítségével struktúra, és a Hadoop |} Microsoft Azure"
    description="Megtudhatja, hogy miként elemezhet szenzoradatokat a struktúra lekérdezés konzol a HDInsight (Hadoop), majd a powerview használata a Microsoft Excel az adatok ábrázolása."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016" 
    ms.author="larryfr"/>

#<a name="analyze-sensor-data-using-the-hive-query-console-on-hadoop-in-hdinsight"></a>A lekérdezés struktúra konzol használata a HDInsight Hadoop érzékelő adatok elemzése

Megtudhatja, hogy miként elemezhet szenzoradatokat a struktúra lekérdezés konzol a HDInsight (Hadoop), majd a Microsoft Excel az adatok ábrázolása a Power View segítségével.

> [AZURE.NOTE] A lépéseket a jelen dokumentum csak a Windows-alapú HDInsight fürt dolgozhat.

Ebben a példában a fogja használni struktúra folyamat korábbi adatok készített készítheti el fűtési, szellőztetési és légkondicionáló (fűtés-és Légtechnikai) rendszerek, amelyek nem biztos, hogy a beállított hőmérséklet fenntartására képes rendszerek azonosításához. Megtanulhatja, hogy miként:

- Vesszővel tagolt (CSV) érték fájlokban tárolt adatok lekérdezése a struktúra táblák létrehozása.
- Az adatok elemzéséhez struktúra lekérdezéseket hozhat létre.
- A Microsoft Excel használata HDInsight (megnyitott adatbázis-összekapcsolás (ODBC) használ az adatok beolvasásához.
- Az adatok ábrázolása a Power View segítségével.

![A megoldási folyamatábrája](./media/hdinsight-hive-analyze-sensor-data/hvac-architecture.png)

##<a name="prerequisites"></a>Előfeltételek

* Egy HDInsight (Hadoop) fürthöz: lásd: [a HDInsight rendelkezés Hadoop fürt](hdinsight-provision-clusters.md) fürt létrehozásával kapcsolatos tudnivalók.

* A Microsoft Excel 2013-ban

    > [AZURE.NOTE] A Microsoft Excel adatmegjelenítés a [Power View nézetben](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US)használható.

* [A Microsoft-struktúra ODBC-illesztő](http://www.microsoft.com/download/details.aspx?id=40886)

##<a name="to-run-the-sample"></a>A minta futtatása

1. A webböngészőben nyissa meg az alábbi URL-címet. Csere `<clustername>` a HDInsight fürt a nevet.

        https://<clustername>.azurehdinsight.net

    Amikor a rendszer kéri, a rendszergazdai felhasználónév és jelszó használatával kiépítésekor a fürt hitelesítést végezni.

2. Ekkor megnyílik a weblapon kattintson az **Első lépések gyűjtemény** fülre, és a **megoldások a mintaadatokat tartalmazó** kategóriára, válassza a **Érzékelő adatelemzés** minta.

    ![Első lépések gyűjteménye](./media/hdinsight-hive-analyze-sensor-data/getting-started-gallery.png)

3. Kövesse az utasításokat az weblapon a minta befejezéséhez.
