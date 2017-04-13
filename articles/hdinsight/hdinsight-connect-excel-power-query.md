<properties
    pageTitle="Az Excel csatlakoztatása a Power Query Hadoop |} Microsoft Azure"
    description="Megtudhatja, hogy miként kihasználhatja az üzletiintelligencia-összetevői, és az Excel Power Query segítségével a HDInsight Hadoop tárolt access-adatok."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>


#<a name="connect-excel-to-hadoop-by-using-power-query"></a>Az Excel Power Query használatával Hadoop csatlakozni

Egy kulcs a Microsoft nagy adatok megoldás funkció a Microsoft business üzletiintelligencia-összetevők és Azure hdinsight szolgáltatáshoz a Hadoop fürt integrációt. Egy integrálás elsődleges példája fiókhoz való csatlakozáshoz Excel az Azure tárhely, amely tartalmazza az adatokat az Excel-bővítmény a Microsoft Power Query használatával a Hadoop fürt társított lehetősége. Ez a cikk végigvezeti beállítása és kezelése HDInsight Hadoop fürt társított lekérdezés adatok Power Query használatával.

> [AZURE.NOTE] Ez a cikk lépéseinek Linux vagy Windows-alapú HDInsight fürt kínál, miközben Windows az ügyfél-munkaállomás szükség.

### <a name="prerequisites"></a>Előfeltételek

Ez a cikk megkezdése előtt a következőket kell rendelkeznie:

- **Egy HDInsight fürt**. Egy konfigurálásához lásd: [első lépések az Azure hdinsight szolgáltatáshoz][hdinsight-get-started].
- **A munkaállomás** Windows 7, Windows Server 2008 R2 vagy újabb operációs rendszert futtató.
- **Office 2013 Professional Plus, Office 365 ProPlus, az Excel 2013 önálló verzió vagy az Office 2010 Professional Plus**.


## <a name="install-power-query"></a>A Power Query telepítése

Adatok importálása különböző forrásokból a Microsoft Excelbe, ahol azt power BI eszközök, például a PowerPivot és a Power View Power Query használható. Különösen a Power Query importálhatja az adatokat, amely már kimeneti vagy, amely a egy HDInsight fürthöz Hadoop feladat létrehozva.

Töltse le a Microsoft Power Query az Excel, a [Microsoft letöltőközpontból] [ powerquery-download] és telepítse az eszközt.

## <a name="import-hdinsight-data-into-excel"></a>HDInsight-adatokat importálhatja az Excelbe

A Power Query bővítmény az Excel egyszerűen importálandó adatokat az Excelbe, ahol Üzletiintelligencia-eszközeiről, például a PowerPivot és a Power Map vizsgálja meg, hogy használhatók elemezni, és az adatok HDInsight fürt.

**Adatok importálása egy HDInsight fürthöz**

1. Nyissa meg az Excelt.

2. Hozzon létre egy új üres munkafüzet.

3. Kattintson a **Power Query** menüre, kattintson **Az Azure**, és válassza a **Microsoft Azure HDInsight**.

    ![HDI. PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]

    **Megjegyzés:** Ha a **Power Query** menü nem látható, kattintson a **fájl** > **Beállítások** > **Bővítmények**, és válassza **a COM-bővítmények** a **kezelés** legördülő az oldal alján. Kattintson az **Ugrás...** gombra, és ellenőrizze, hogy a Power Query az Excel-bővítmény jelölőnégyzetét ellenőrizte.

    **Megjegyzés:** A Power Query segítségével is importálhatja az adatokat Fájlrendszerhez **Az egyéb adatforrásból**gombra kattintva.

3. A **Fiók nevét**adja meg a fürt társított Azure Blob-tároló fiók nevét, és kattintson **az OK**gombra. Ez az [alapértelmezett tárterület-fiók](hdinsight-administer-use-management-portal.md#find-the-default-storage-account) és egy csatolt tárterület-fiókot is lehet.  A formátum *https://<StorageAccountName>.blob.core.windows.net/*.

4. A **Fiókkulcs**adja meg a kulcsot a Blob-tároló fiók, és kattintson a **Mentés**gombra. (Szüksége ezzel csak az első alkalommal is hozzáférhet az üzlet.)

5. A **kezelő** ablaktáblában, a bal oldali a Lekérdezésszerkesztő kattintson duplán a Blob tároló tároló nevére. A tároló neve alapértelmezés szerint ki neve megegyezik a csoport nevét.

6. Keresse meg a **név** oszlop (a mappa elérési útja **… **(hivesampledata.txt)** / struktúra, / / hivesampletable/raktári**), és válassza a **bináris** bal szélén (hivesampledata.txt). Az összes fürt megtalálható (hivesampledata.txt). Tetszés szerint a saját fájl is használhatja.

    ![HDI. PowerQuery.ImportData][image-hdi-powerquery-importdata]

7. Ha azt szeretné, az oszlopnevek átnevezheti. Ha készen áll, kattintson a **Bezárás és betöltés**gombra.  Az adatok be van töltve a munkafüzetben:

    ![HDI. PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a name="next-steps"></a>Következő lépések

Ez a cikk az adatok beolvasásához hdinsight az Excelbe a Power Query használata megtanulta azt. Hasonlóképpen meghallgathatja adatok hdinsight Azure SQL-adatbázisba. Akkor is szeretné feltölteni adatok hdinsight szolgáltatásból lehetőségre. További tudnivalókért lásd: az alábbi cikkekben:

* [Az Excel csatlakozni a Microsoft-struktúra ODBC-illesztőprogram hdinsight szolgáltatáshoz][hdinsight-ODBC]
* [Töltse fel az adatok hdinsight szolgáltatáshoz][hdinsight-upload-data]

[hdinsight-ODBC]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[image-hdi-powerquery-hdi-source]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.SelectHdiSource.png
[image-hdi-powerquery-importdata]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportData.png
[image-hdi-powerquery-imported-table]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportedTable.PNG

[powerquery-download]: http://go.microsoft.com/fwlink/?LinkID=286689
