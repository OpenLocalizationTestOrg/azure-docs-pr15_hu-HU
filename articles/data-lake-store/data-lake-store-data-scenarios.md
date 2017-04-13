<properties 
   pageTitle="Adatok felhasználási területei adattár tó érintő |} Microsoft Azure" 
   description="A különböző forgatókönyvek és a eszközök használatával, hogy milyen adatokat is okozhatnak, feldolgozott, letöltött és formájában jelenik meg az adatok tó üzlet ismertetése" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/06/2016"
   ms.author="nitinme"/>

# <a name="using-azure-data-lake-store-for-big-data-requirements"></a>Azure tó adattár használatának nagy adatok vonatkozó követelmények

Nagy adatfeldolgozás négy fő szakaszból áll:

* Nagy mennyiségű adatot ingesting adatok tárba, a valós idejű, vagy egy csoportba
* Az adatok feldolgozása
* Az adatok letöltése
* Az adatok megjelenítése

Ez a cikk azt megnézheti részletez tó adattár Azure megtudhatja, hogy a beállítások és a rendszergazdák számára elérhető nagy adatok igényekhez ezeket a lépéseket.

## <a name="ingest-data-into-data-lake-store"></a>Adatok ingest tó tárba

Ez a szakasz kiemeli a különböző forrásokból, és milyen különböző módokon, amelyben az adatokat egy tó adattár figyelembe okozhatnak.

![Ingest adatok tó tárba] (./media/data-lake-store-data-scenarios/ingest-data.png "Ingest adatok tó tárba")

### <a name="ad-hoc-data"></a>Alkalmi adatok

Ez képviseli kisebb adatkészletek, amelyek egy nagy adatok alkalmazás prototípusának elkészítéséhez használható. Eltérő módon attól függően, hogy az adatforrás ingesting alkalmi adatok.

| Adatforrás        | Ingest használatával                                                                        |
|--------------------|----------------------------------------------------------------------------------------|
| Helyi számítógépen     | <ul> <li>[Azure portál](/data-lake-store-get-started-portal.md)</li> <li>[Azure PowerShell](data-lake-store-get-started-powershell.md)</li> <li>[Azure-platformok CLI](data-lake-store-get-started-cli.md)</li> <li>[Adatok tó eszközeivel for Visual Studio](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md#upload-source-data-files) </li></ul> |
| Tárterület Azure Blob | <ul> <li>[Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)</li> <li>[AdlCopy eszköz](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[A HDInsight fürthöz DistCp](data-lake-store-copy-data-wasb-distcp.md)</li> </ul> |

 
### <a name="streamed-data"></a>Adatfolyamok

Ez a adatok, például alkalmazások, eszközök, érzékelők és stb különböző forrásokból létrehozott képviseli. A okozhatnak ezeket az adatokat egy adatok tó tárolóba különböző eszközök segítségével. Ezek az eszközök általában fog rögzítése és feldolgozása az adatokat az esemény szerint kombinálásával valós idejű, majd írja be az események kötegekben tó tárba, hogy azok is további dolgozható fel. 

Az alábbiakban eszközök, amelyek segítségével használhatja:
 
* [Azure Értékáram-elemzés] (.. / adatfolyam-analytics-adatok-tó-kimeneti) - események esemény hubra okozhatnak írhatók Azure adatok tó használatával olyan Azure tó adattár eredménye.
* [Azure hdinsight szolgáltatáshoz vihar](../hdinsight/hdinsight-storm-write-data-lake-store.md) - írhat adatok közvetlenül tó adattár eltávolítása a vihar.
* [EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost) – események fogadjanak esemény hubok, és jegyezze azt adatok tó áruház az [Adatok tó áruházból .NET SDK](data-lake-store-get-started-net-sdk.md)használatával.

### <a name="relational-data"></a>Relációs adatok

A forrásadatok relációs adatbázisból. Egy időszakra vonatkozóan a relációs adatbázisok gyűjt a nagyon nagy mennyiségű adat, ami lehetővé teszi kulcs háttérismeretek, ha egy nagy adatok folyamat keresztül feldolgozása. A következő eszközök segítségével ezek az adatok áthelyezése tó tárolóba.

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Webes kiszolgáló naplóadatok (a feltöltés egyéni alkalmazások használata)

Az ilyen típusú adatkészlet kifejezetten beállításokkal, mert a web server napló adatok elemzése egy közös használatieset nagy adatok alkalmazások, és fel kell tölteni a adattár tó naplófájlok nagy mennyiségű csak. Írja be a saját parancsfájlok vagy töltse fel ezeket az adatokat az alkalmazások az alábbi eszközök közül használható.

* [Azure-platformok CLI](data-lake-store-get-started-cli.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure tó adattár .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)

Webhely kiszolgálói naplóadatok feltölthető, is feltöltése (például közösségi hangulati elemek) adatokat más típusú, akkor az írni a saját egyéni parancsfájlok alkalmazások, mert azt a rugalmasságot biztosít az adatok összetevő feltöltése a nagyobb nagy adatok alkalmazás részeként felvenni egy jó módszer. Bizonyos esetekben ez a kód jelenthetik parancsfájl vagy az egyszerű parancssori segédprogramot. Egyéb esetben a kód használható nagy adatfeldolgozás integrálása a vállalati verzió alkalmazásba vagy a megoldást.


### <a name="data-associated-with-azure-hdinsight-clusters"></a>Azure hdinsight szolgáltatáshoz fürt társított adatok

A legtöbb HDInsight fürtre típus (Hadoop, HBase, vihar) adatokat tároló összegyűjti tó adattár támogatja. HDInsight fürt elérni az adatokat az Azure tároló BLOB (WASB). Jobb teljesítmény elérése érdekében másolhatja az adatokat a WASB egy fürthöz társított tó adattár-fiókba. A következő eszközöket is használhatja az adatokat.

* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)
* [AdlCopy szolgáltatás](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)

### <a name="data-stored-in-on-premise-or-iaas-hadoop-clusters"></a>A helyszíni vagy IaaS Hadoop tárolt adatok fürt

Nagy mennyiségű adatot meglévő Hadoop fürt, helyben Fájlrendszerhez használatával gépeken tárolható. A Hadoop fürt előfordulhat, hogy a helyszíni példányban vagy lehet Azure-IaaS fürt belül. Követelmények másolhatja át az ezeket az adatokat az Azure tó adattár megközelítésre egyszeri vagy ismétlődő módon lehet. Lehetőség különböző cél használható. Az alábbi van alternatívák és a kapcsolódó kompromisszumok listáját.

| Megközelítés  | Részletek | Előnyei   | Megfontolandó szempontok  |
|-----------|---------|--------------|-----------------|
| Másolja az adatokat közvetlenül a Hadoop fürt Azure tó adattár Azure adatok gyári (ADF) használatával | [ADF Fájlrendszerhez adatforrásként támogatja.](../data-factory/data-factory-hdfs-connector.md) | ADF biztosít Fájlrendszerhez első-végpont kezeléséről és ellenőrzés ki-be a támogatása | Az adatkezelési átjáró a helyszíni, vagy a IaaS fürt telepíthető igényel |
| Adatok exportálása Hadoop-fájlként. Ezután másolja a fájlok Azure tó adattár megfelelő mechanizmusa használatával.                                   | Fájlok másolása Azure tó adattár használata: <ul><li>[A Windows operációs rendszer Azure PowerShell](data-lake-store-get-started-powershell.md)</li><li>[Azure-platformok CLI esetében nem – Windows operációs rendszer](data-lake-store-get-started-cli.md)</li><li>Egyéni alkalmazás bármely adatok tó áruházból SDK használatával</li></ul> | A gyors induláshoz. Testre szabott feltöltések végezhető műveletek                                                   | Lépéseken, amely magában foglalja a több technológiákkal. Kezelés és a figyeléshez fognak nőni bonyolulttá kell adni a testre szabott jellegét, az eszközök időbeli |
| Adatok másolása Hadoop Azure tárolóhoz Distcp használatát. Ezután másolja adatok Azure-tárhelyről használata, megfelelő mechanizmusa tó adattár. | Azure-tárhelyről adatok másolása tó adattár használata: <ul><li>[Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)</li><li>[AdlCopy eszköz](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[Futó HDInsight fürt Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)</li></ul>| Megnyitás-forrás eszközök is használhatja. | Lépéseken, amely magában foglalja a több technológiák |

### <a name="really-large-datasets"></a>Nagyon nagy adathalmazok

Feltölthető adatkészleteket, amely több terabájt a tartományt, fent bemutatott módszerekkel esetenként is lassú és költséges. Ezekben az esetekben az alábbi lehetőségek is használhatja.

* **Készült Azure ExpressRoute használatával**. Készült Azure ExpressRoute magán kapcsolat létesítése között Azure adatközpontokkal és infrastruktúra a helyszíni teszi lehetővé. Ezzel a megoldással a nagy mennyiségű adatot átviteléhez egy megbízható lehetőséget. További információ [a készült Azure ExpressRoute](../expressroute/expressroute-introduction.md)dokumentációjában olvasható.


* **Az adatok feltöltése "Offline"**. Ha a készült Azure ExpressRoute használatával nem lehetséges valamilyen okból, [Importálás/Exportálás Azure service](../storage/storage-import-export-service.md) kiszállítása a merevlemez-meghajtók az adatokkal, amelyek az Azure adatközpont is használhatja. Az adatok első Azure BLOB-tároló van feltöltve. [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store) vagy [AdlCopy eszköz](data-lake-store-copy-data-azure-storage-blob.md) használatával majd Azure BLOB-tároló adatainak másolása tó adattár.

    >[AZURE.NOTE] Miközben az importálás/exportálás szolgáltatással, a fájlméretet a lemezen Azure adatközpontja szállított nem kell 200 GB-nál nagyobb.


## <a name="process-data-stored-in-data-lake-store"></a>Adatok tó tárban tárolt adatok feldolgozása

Adatok tó áruház elérhetővé válik az adatok elemzése az adatokat a támogatott nagy adatok alkalmazások használata a futtathatja. Jelenleg is használhatja Azure hdinsight szolgáltatáshoz és Azure adatok tó Analytics adatainak elemzése feladatok futtatásához az adatok tó tárban tárolt adatok. 

![Adatok elemzés tó adattárhoz] (./media/data-lake-store-data-scenarios/analyze-data.png "Adatok elemzés tó adattárhoz")

Az alábbi példák megnézheti.

* [Hozzon létre egy HDInsight fürtre tó áruházzal tárolására](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Azure adatok tó Analytics használata tó adattárhoz](../data-lake-analytics/data-lake-analytics-get-started-portal.md)



## <a name="download-data-from-data-lake-store"></a>Adatok letöltése a tó adattárhoz

Érdemes azt is, töltse le vagy az adatokat áthelyezheti az Azure tó adattár felhasználási területei például:

* Helyezze át más tárházakban kapcsolatra a meglévő adatkezelési folyamatok az adatokat. Például érdemes lehet tó áruházból adatok áthelyezése SQL Azure-adatbázis vagy a helyszíni SQL Server.
* Adatok letöltése a helyi számítógépre feldolgozásra IDE környezetekben alkalmazás prototípusok közben.

![Kilépési adatok tó tárból] (./media/data-lake-store-data-scenarios/egress-data.png "Kilépési adatok tó tárból")

Ezekben az esetekben is használhatja az alábbi lehetőségek közül:

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)
* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)

Az alábbi módszerek segítségével saját parancsfájl/alkalmazás adatok letöltése a tó áruházból.

* [Azure-platformok CLI](data-lake-store-get-started-cli.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure tó adattár .NET SDK](data-lake-store-get-started-net-sdk.md)

## <a name="visualize-data-in-data-lake-store"></a>Adatok ábrázolása az tó adattárhoz

Kombinálja a szolgáltatások segítségével adatokat tó tárban tárolt adatok ábrázolására.

![Megjelenítés adatainak tó adattárhoz] (./media/data-lake-store-data-scenarios/visualize-data.png "Megjelenítés adatainak tó adattárhoz")

* Elindíthatja [Azure Data Factory adatok tó Store Azure SQL-adatraktár adatok áthelyezése](../data-factory/data-factory-data-movement-activities.md#supported-data-stores) használatával
* Ezután így [a Power BI Azure SQL-adatraktár való integráció](../sql-data-warehouse/sql-data-warehouse-integrate-power-bi.md) hozhat létre az adatok vizuális ábrázolására.
