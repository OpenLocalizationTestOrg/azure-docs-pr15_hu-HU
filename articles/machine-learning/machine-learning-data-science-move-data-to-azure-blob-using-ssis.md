<properties
    pageTitle="Adatok áthelyezése, illetve az SSIS-összekötőkkel Azure Blob-tárolóhoz |} Microsoft Azure"
    description="Adatok áthelyezése vagy az SSIS-összekötőkkel Azure Blob-tárolóhoz onnan."
    services="machine-learning,storage"
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
    ms.date="09/14/2016"
    ms.author="bradsev" />

# <a name="move-data-to-or-from-azure-blob-storage-using-ssis-connectors"></a>Adatok áthelyezése, illetve az SSIS-összekötőkkel Azure Blob-tárolóhoz

Az [SQL Server Integration Services funkciócsomag a Azure](https://msdn.microsoft.com/library/mt146770.aspx) Itt csatlakozhat az Azure-összetevők adatátviteli Azure helyszíni adatforrások és Azure-ban tárolt eljárás adatok között.

Adatok áthelyezése és/vagy az Azure Blob-tárolóhoz használt technológiák útmutatást a itt kapcsolódnak:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


Miután ügyfelek át lett helyezve a helyszíni adatok az a felhő, elérhetik azt bármilyen Azure szolgáltatás használja ki teljes az Azure technológiák az csomagot. Azt is használható, például az Azure gépi tanulási vagy egy HDInsight fürt.

Az általában az első lépés a [SQL](machine-learning-data-science-process-sql-walkthrough.md) és [HDInsight](machine-learning-data-science-process-hive-walkthrough.md) forgatókönyvek lehet.

Kanonikus forgatókönyvek, SSIS elvégezheti az üzleti igények közös hibrid adat integrációs forgatókönyvek a vitafórum tekintse meg [az SQL Server Integration Services funkciócsomag a Azure további tenni](http://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) .

> [AZURE.NOTE] Egy teljes – bevezetés Azure blob-tárolóhoz olvassa el a [Azure Blob](../storage/storage-dotnet-how-to-use-blobs.md) -alapokhoz és [Azure Blob](https://msdn.microsoft.com/library/azure/dd179376.aspx)-szolgáltatásba.

## <a name="prerequisites"></a>Előfeltételek

A jelen cikkben ismertetett műveletek elvégzéséhez, az Azure-előfizetéssel és az Azure tárterület-fiókot kell rendelkeznie. Ismernie kell a tároló Azure nevét és a fiók fiókkulcs fel-vagy letöltése az adatokat.

- Az **Azure előfizetés**beállítani, olvassa el a [hónap próba-előfizetésre](https://azure.microsoft.com/pricing/free-trial/)című témakört.

- Útmutatást a **tárterület-fiók** létrehozása és a fiók és a fontos információkat ismertető [kapcsolatos Azure tároló fiókok](../storage/storage-create-storage-account.md)témakörben találhatók.


Az **SSIS-összekötők**használni, töltse le:

- **SQL Server 2014-es vagy 2016 téli (vagy újabb)**: telepítse az SQL Server Integration Services tartalmazza.
- **A Microsoft SQL Server 2014-es vagy 2016 Integration Services funkciócsomag Azure**: ezek letölthető, a kurzor az [SQL Server 2014-es Integration Services](http://www.microsoft.com/download/details.aspx?id=47366) és az [SQL Server 2016 Integration Services](https://www.microsoft.com/download/details.aspx?id=49492) oldalakat.

> [AZURE.NOTE] Az SSIS SQL Server-telepítve van, de nem szerepel a Express verzió. A mely alkalmazások kaptak az SQL Server különféle kiadásainak a további tudnivalókért lásd [Az SQL Server kiadása](http://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/)

Képzési anyagok a SSIS olvassa el a [Kéz a oktatás SSIS](http://www.microsoft.com/download/details.aspx?id=20766) című témakört.

Olvashat arról, hogyan veheti fel és futtatása a SISS egyszerű kivonása, átalakítási és terhelés (ETL) csomagok, lásd: össze [SSIS oktatóprogram: egyszerű ETL csomag létrehozása](https://msdn.microsoft.com/library/ms169917.aspx).

## <a name="download-nyc-taxi-dataset"></a>Töltse le a következőt: Taxi adatkészlet  
Leírt példát itt használja a nyilvánosan elérhető adatkészlet – a [Következőt: Taxi utakat](http://www.andresmh.com/nyctaxitrips/) adatkészlet. Az adatkészlet körülbelül 173 millió taxi témakör a következőt: az év 2013 áll. Az adatok két típusa van: utazás részletes adatok és jegy ára az adatokat. Mivel az egyes egy fájlt, az összes, amelyek az, hogy körülbelül 2 GB-os nem tömörített 24 fájlok van.


## <a name="upload-data-to-azure-blob-storage"></a>Töltse fel az adatok Azure blob-tárolóhoz
Az adatoknak az SSIS-szolgáltatás Azure blob-tárolóhoz a helyszíni csomag áthelyezéséhez használjuk az [**Azure Blob-feltöltése tevékenység**](https://msdn.microsoft.com/library/mt146776.aspx)ábrán egy példányának:

![Állítsa be-adatok-tudományos-virtuális](./media/machine-learning-data-science-move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)


A paraméterek a tevékenység használó vannak az alábbiakban ismertetjük:


A mező|Leírás|
----------------------|----------------|
**AzureStorageConnection**|Adja meg egy meglévő Azure tároló kapcsolatkezelő vagy létrehoz egy új, amelyre hivatkozik, amely hol vannak tárolva a a blob-fájlok mutat Azure tároló fiók.|
**BlobContainer**|A blob-tárolóhoz BLOB feltöltött fájlként élvező nevét adja meg.|
**BlobDirectory**|A feltöltött fájlt tároló letiltása blob-ként blob könyvtárat adja meg. A blob-címtár virtuális hierarchikus. Ha a blob már létezik, ia helyett az informatikai.|
**LocalDirectory**|Itt adhatja meg a helyi könyvtár, amely a feltölteni kívánt fájlokat tartalmazza.|
**Fájlnév**|Jelölje ki a fájlt a megadott név mintázattal egy név szűrőt ad meg. Ha például MySheet\*.xls\* például MySheet001.xls és MySheetABC.xlsx fájlokat tartalmazza.|
**TimeRangeFrom/TimeRangeTo**|Itt adhatja meg a tartomány idő szűrő értékét. Fájlok módosított *TimeRangeFrom* után, és *TimeRangeTo* előtt számításba veszi.|


> [AZURE.NOTE] A **AzureStorageConnection** hitelesítő adatokat kell helyes, és a **BlobContainer** léteznie kell, mielőtt a továbbított pró.

## <a name="download-data-from-azure-blob-storage"></a>Adatok letöltése a Azure blob-tárolóhoz

Azure blob-tárolóhoz tároló SSIS a helyszíni adatok letöltéséhez használja az [Azure Blob-feltöltése tevékenység](https://msdn.microsoft.com/library/mt146779.aspx)egy példánya.

##<a name="more-advanced-ssis-azure-scenarios"></a>Speciális SSIS-Azure-esetek
Azt vegye figyelembe az alábbi, hogy a SSIS funkciócsomag lehetővé teszi, hogy az összetettebb flow kezeli összecsomagolása feladatok együtt. Ha például a blob sikerült adatcsatorna-közvetlenül egy HDInsight fürthöz, kimenete tölthető le vissza az blob és helyszíni tárolóhoz. Az SSIS futtatását is lehetővé teszi a struktúra, és malac feladatok egy további SSIS-összekötőkkel HDInsight fürt:

- Az SSIS-Azure hdinsight szolgáltatáshoz fürt struktúra parancsfájl futtatásához használja a [Azure HDInsight-struktúra tevékenység](https://msdn.microsoft.com/library/mt146771.aspx).
- Az SSIS-Azure hdinsight szolgáltatáshoz fürt malac parancsfájl futtatásához használja a [Azure hdinsight szolgáltatáshoz malac tevékenység](https://msdn.microsoft.com/library/mt146781.aspx).
