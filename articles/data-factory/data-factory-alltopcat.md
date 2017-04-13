<properties
    pageTitle="Adatok gyári szolgáltatás összes témakörök |} Microsoft Azure"
    description="Táblázat összes témakörök az Azure szolgáltatás nevű adatok gyár http://azure.microsoft.com/documentation/articles/, a cím és leírás megtalálható."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="data-factory"
    ms.workload="data-factory"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="spelluru"/>


# <a name="all-topics-for-azure-data-factory-service"></a>Az összes témakörök Azure Data Factory szolgáltatás

Ez a témakör a minden témakör közvetlenül az Azure **Data Factory** szolgáltatás alkalmazott sorolja fel. Kulcsszavak a weblap az aktuális érdeklődésre számot tartó témakörök keresése a **Ctrl + F**, használatával is kereshet.




## <a name="new"></a>Új

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 1 | [Adatok az Amazon Redshift Azure Data Factory használatával áthelyezése](data-factory-amazon-redshift-connector.md) | Tudnivalók az adatokat áthelyezheti az Amazon Redshift Azure Data Factory használatával. |
| 2 | [Adatok az Amazon egyszerű Tárhelyszolgáltatáshoz Azure Data Factory használatával áthelyezése](data-factory-amazon-simple-storage-service-connector.md) | Megismerheti az Azure Data Factory használatával Amazon egyszerű Tárhelyszolgáltatáshoz (S3) adatainak áthelyezése. |
| 3 | [Azure adatok Factory - példány varázsló](data-factory-azure-copy-wizard.md) | További tudnivalók az gyári Azure másolás varázsló segítségével adatok másolása a támogatott adatforrások mosdók. |
| 4 | [Oktatóprogram: Az első Azure adatok gyári adatok gyári REST API segítségével összeállítása](data-factory-build-your-first-pipeline-using-rest-api.md) | Ebben az oktatóanyagban hozzon létre egy minta Azure Data Factory folyamat adatok gyári REST API-t használja. |
| 5 | [Oktatóprogram: Hozzon létre egy folyamat másolás tevékenység .NET API segítségével](data-factory-copy-activity-tutorial-using-dotnet-api.md) | Ebben az oktatóanyagban létrehozása az Azure Data Factory folyamat egy másolatot a tevékenységhez a .NET API használatával. |
| 6 | [Oktatóprogram: Hozzon létre egy folyamat másolás tevékenység REST API segítségével](data-factory-copy-activity-tutorial-using-rest-api.md) | Ebben az oktatóanyagban létrehozása az Azure Data Factory folyamat egy másolatot a tevékenységhez a REST API használatával. |
| 7 | [Az adatok gyári másolás varázsló](data-factory-copy-wizard.md) | További tudnivalók az gyári másolás varázsló segítségével adatok másolása a támogatott adatforrások mosdók. |
| 8 | [Az adatkezelési átjáró](data-factory-data-management-gateway.md) | Adatok áthelyezése fiókok között a helyszíni és felhőbeli adatok az átjáró beállítása. Azure Data Factory az adatkezelési átjáró segítségével helyezze át az adatokat. |
| 9 | [Azure Data Factory használata a helyszíni Cassandra adatbázisból származó adatok áthelyezése](data-factory-onprem-cassandra-connector.md) | Megismerheti az adatok áthelyezése egy helyszíni Cassandra adatbázisból Azure Data Factory használatával. |
| 10 | [A MongoDB Azure Data Factory használatával adatok áthelyezése](data-factory-on-premises-mongodb-connector.md) | Tudnivalók az adatokat áthelyezheti az Azure adatok gyár MongoDB adatbázist. |
| 11 | [Azure adatok gyár használatával a Salesforce-adatok áthelyezése](data-factory-salesforce-connector.md) | Tudnivalók az adatok áthelyezése a Salesforce Azure Data Factory használatával. |


## <a name="updated-articles-data-factory"></a>Frissített cikkek, adatok gyári

Ez a szakasz tárgyak, amelyek a frissített nemrégiben, ha a frissítés nagy, vagy jelentős lett-e. Az egyes frissített cikk durva kódtöredékének, a hozzáadott markdown szöveg jelenik meg. A cikkek lettek frissítve a dátumtartomány **2016-08-22-es** való **2016 – 10-05**belül.

| &nbsp; | A cikk | Módosított szöveget, kódtöredékének | Mikor frissülnek |
| --: | :-- | :-- | :-- |
| 12 | [Azure adatok Factory - .NET API – módosítási napló](data-factory-api-change-log.md) | Ebben a cikkben információt a változásokról Azure adatok gyári SDK adott verziójában. Azure adatok gyár itt (https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) **verzió 4.11.0** szolgáltatás bővítheti a legújabb NuGet csomag talál: / a következő csatolt szolgáltatás típusú hozzá lett adva: - OnPremisesMongoDbLinkedService (https://msdn.microsoft.com/library/mt765129.aspx) - AmazonRedshiftLinkedService (https://msdn.microsoft.com/library/mt765121.aspx) - (https://msdn.microsoft.com/library/mt765144.aspx) AwsAccessKeyLinkedService / az alábbi adatkészlet-típusok hozzá lett adva: AmazonS3Dataset - MongoDbCollectionDataset (https://msdn.microsoft.com/library/mt765145.aspx) - (https://msdn.microsoft.com/library/mt765112.aspx) / a következő másolás forrástípus hozzá lett adva :-(Https://msdn.microsoft.com/en-US/library/mt765123.aspx) **verzió 4.10.0** MongoDbSource / TextFormat felvette a következő opcionális tulajdonságok:-sífelszerelés | 2016-09-22-es hibakód |
| 13-mal | [Adatok áthelyezése és az Azure Blob-Azure Data Factory használatával](data-factory-azure-blob-connector.md) |  / copyBehavior / másolás viselkedését határozza meg, ha a forrás BlobSource vagy formázáshoz.  / **PreserveHierarchy:** megőrzi a fájl hierarchiában a cél mappában. Az adatforrás mappára forrásfájlban relatív elérési útja relatív fájl elérési útját cél célmappába azonos... BR /. BR /. **FlattenHierarchy:** a forrás mappában lévő összes fájlt az első szintű célmappát találhatók. A target fájlokat lehet automatikusan létrejön a neve. .BR /. BR /. **MergeFiles: (alapértelmezett)** egy fájlt a forrás mappában lévő összes fájl egyesíti. A fájl/Blob van megadva, az egyesített fájlnév akkor a megadott neve; egyéb esetben akkor is automatikusan létrehozott fájl nevét.  / Nem és a korábbi verziókkal való kompatibilitás is támogat ezek a két tulajdonságok **BlobSource** . / **treatEmptyAsNull**: Megadja, hogy a null érték nulla vagy üres karakterláncot tekinti-e. / **skipHeaderLineCount** – Itt adhatja meg, hány vonalak szükséges hagyva. Esetében alkalmazható csak ha beviteli adatkészlet által használt TextFormat. Hasonlóképpen a **BlobSink** adik támogatja | 2016-09-28 |
| 14 | [Azure gépi tanulási tevékenységek segítségével prediktív folyamatok létrehozása](data-factory-azure-ml-batch-execution-activity.md) | **Webszolgáltatás több bemenetben van szükség.** Ha a webes szolgáltatásnak több bemeneti adatok alapján, a **webServiceInputs** tulajdonsággal **típus**használata helyett. A **webServiceInputs** által hivatkozott adatkészleteket is szerepelnie kell a tevékenység **bemeneti adatok alapján**. Az Azure Machine Learning kísérlet a web service bemeneti és kimeneti portokat és globális paraméterek alapértelmezett névvel rendelkeznek e ("input1", "input2"), amelyet testre szabhat. A nevek webServiceInputs, webServiceOutputs és globalParameters beállítások használt pontosan egyeznie kell a kísérletek nevek. A minta kérelem tartalom megtekintése az Azure Machine Learning végpontot ellenőrizze a várt leképezést köteg végrehajtás súgó lapon.    {"név": "PredictivePipeline", "Tulajdonságok": {"leírás": "AzureML modell használata", "tevékenységek": {"név": "MLActivity", "típus": "AzureMLBatchExecution", "leírás": "előrejelzési elemzés a bemeneti köteg", "a bemeneti": {"név": "inputDataset1"}, {"név": "inputDatase | 2016-09-13 |
| 15 | [Másolja a tevékenység teljesítmény és a beállítási útmutatója](data-factory-copy-activity-performance.md) | 1. **alapterv létrehozása**. A fejlesztői fázisban tesztelje a folyamat másolás tevékenység ellen képviselő adatok minta használatával. A modell (adatok-factory-ütemezés-és-execution.md time-series-datasets-and-data-slices) szeletelés Data Factory használatakor adatok mennyiségét korlátozni is használhatja.  A **Figyelés és felügyeleti alkalmazás**végrehajtási idő és a teljesítmény jellemzők összegyűjtése A Data Factory kezdőlapján válassza a **Monitor és kezelése** . A fastruktúrájú nézetben válassza ki a **Kimenet adatkészlet**. A **Tevékenység Windows** listában válassza a Másolás tevékenység futtatni. **Tevékenység Windows** sorolja fel, a Másolás tevékenység időtartama és a másolt adatok méretét. A teljesítmény **Tevékenység ablak Explorer**szerepel. Ha többet szeretne megtudni az alkalmazásról, lásd: a Monitor, és Azure Data Factory folyamatok kezelése a figyelő és felügyeleti alkalmazás (adatok-factory-monitor-kezelése – app.md).  ! A tevékenység részleteit (./media/data-factory-copy-activity-performance/mmapp-activity-run-details.pn futtatása | 2016-09-27-es |
| 16 | [Oktatóprogram: Hozzon létre egy folyamat másolás tevékenység Visual Studio segítségével](data-factory-copy-activity-tutorial-using-visual-studio.md) |   Vegye figyelembe az alábbiakat:- **AzureBlob**adatkészlet **típus** értéke.     - **linkedServiceName** **AzureStorageLinkedService**van beállítva. A 2 létrehozott csatolt szolgáltatást.     - **Mappa_útvonala** a **adftutorial** tároló van állítva. Adja meg a nevét a **fájlnév** tulajdonságot használó mappában tárolt blob is. Nem a blob nevét adja meg, mivel minden BLOB-tárolóban adatainak tekinteni egy bemeneti adatok.    -formátum **típus** értéke **TextFormat** - három elem látható az a szöveg fájl ΓÇô **Utónév** és **Vezetéknév** ΓÇô (**columnDelimiter**) karakterrel vesszővel elválasztott - **elérhetőségének** beállítása **óránként** (**gyakoriság** értéke **órát** és **intervallum** értéke **1**). Ezért Data Factory megkeresi a bemeneti adatok óránként blob-tárolóhoz (**adftutorial**) megadott a legfelső szintű mappájára.  Ha Ön nem adja a **bemeneti** adatkészlet egy **fájlnevet** , valamennyi fájlok/BLOB a bemeneti mappából (**Mappa_útvonala**)-e consid | 2016-09-29 |
| 17 | [Hozzon létre, figyelésére és Azure adatok gyárak adatok gyár .NET SDK használatával kezelése](data-factory-create-data-factories-programmatically.md) | Írja le az alkalmazás azonosítója és jelszava (ügyfél titkos), és használja azt a forgatókönyv. **Azure kérjen előfizetést és a bérlői azonosítók** Ha nincs telepítve a számítógépre PowerShell legújabb verzióját, kövesse az utasításokat, miként telepítheti és Azure PowerShell konfigurálása a (... / powershell-telepítés-configure.md) a cikk adunk a telepítés. 1. Indítsa el az Azure PowerShell, és futtassa a következő parancsot a 2. A következő parancsot, és adja meg a felhasználónevet és jelszót, jelentkezzen be az Azure portálra.         Bejelentkezés-AzureRmAccount, ha csak egy Azure szóló előfizetés ehhez a fiókhoz tartozó, nem szükséges hajtsa végre a következő két lépést. 3. Ehhez a fiókhoz az előfizetések megtekintése a következő parancs futtatásával.       Get-AzureRmSubscription 4. Jelölje ki a használni kívánt előfizetést a következő parancs futtatásával. **NameOfAzureSubscription** cserélje le az Azure előfizetése nevére.       Get-AzureRmSubscription - SubscriptionName NameOfAzureSubscription / Set-AzureRmCo | 2016-09-14 |
| 18 | [Folyamatok és Azure Data Factory tevékenységek](data-factory-create-pipelines.md) |       "indítása": "2016 – 07-12T00:00:00Z", "befejezése": "2016 – 07-13T00:00:00Z"}} vegye figyelembe az alábbiakat: és a tevékenységek csoportban található csak egy tevékenység, amelynek a **típus** értéke **másolása**. / A tevékenységhez tartozó bemeneti **InputDataset** van állítva, és a tevékenység kimeneti **OutputDataset**van beállítva. / **TypeProperties** szakaszban **BlobSource** van megadva a forrás típusa és **SqlSink** a gyűjtő típus van megadva. Teljes ismertetését megtalálja a folyamat létrehozása, lásd: oktatóprogram: adatok másolása Blob-tárolóhoz SQL-adatbázis (data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). **Minta átalakítási folyamat** A következő példa során nem **HDInsightHive** a **tevékenységek** szakasz típusú egy tevékenységet. Az ebben a példában a HDInsight-struktúra tevékenység (adatok-factory-struktúra-activity.md) az Azure Blob-tárolóhoz adatainak átalakítások egy Azure hdinsight szolgáltatáshoz Hadoop fürt struktúra parancsfájl futtatásával.  {"név": "TransformPipeline", "p | 2016-09-27-es |
| 19 | [Adatok az Azure Data Factory átalakítása](data-factory-data-transformation-activities.md) | Adatok gyári támogatja a következő adatok átalakítása tevékenységek, manuálisan is hozzáadhatók csővezetékek (adatok-factory-létrehozása-pipelines.md) akár egyenként, vagy egy másik tevékenységeket használó módszere során. .  AZURE. Megjegyzés: a lépésenkénti útmutató létrehozása című témakör tartalmaz egy struktúra átalakítási (data-factory-build-your-first-pipeline.md) a cikk a folyamat. **Tevékenység HDInsight-struktúra** A HDInsight-struktúra tevékenység egy Data Factory során a struktúra lekérdezései saját vagy igény szerinti Linux rendszerhez, a Windows-alapú HDInsight fürt hajt végre. Ez a tevékenység részleteit struktúra tevékenység (adatok-factory-struktúra-activity.md) cikkében talál. **HDInsight malac tevékenység** A HDInsight malac tevékenység egy Data Factory során a malac lekérdezései saját vagy Linux rendszerhez, a Windows-alapú HDInsight-fürt igény szerinti hajt végre. Ez a tevékenység részleteit malac tevékenység (adatok-factory-malac-activity.md) cikkében talál. **HDInsight MapReduce tevékenység** A HDInsight MapReduce tevékenység egy Data Factory során MapReduce programok a y hajt végre. | 2016-09-26 |
| 20 | [Adatok gyári ütemezés- és végrehajtása](data-factory-scheduling-and-execution.md) | CopyActivity2 szeretné futtatni, csak akkor, ha a CopyActivity1 sikeresen lefut és Dataset2 érhető el. Az alábbiakban a minta folyamat JSON: {"név": "ChainActivities", "Tulajdonságok": {"leírás": "Futtatás tevékenységek sorrendben", "tevékenységek": {"típus": "Másolása", "typeProperties": {"forrás": {"típus": "BlobSource"}, "gyűjtő": {"típus": "BlobSink", "copyBehavior": "PreserveHierarchy", "writeBatchSize": "writeBatchTimeout" 0,: "00: 00:00"}}, "ráfordítások": {"név": "Dataset1"}, "kimenet": {"név": "Dataset2"}, "házirend": {"időtúllépés": "01: 00:00"}, "Ütemező": {"gyakoriság": "Óra", "intervallum": 1}, "név": "CopyFromBlob1ToBlob2", "leírás": "Adatok másolása blob a másik"}, {"típus": "Másolása", "typeProperties": {"forrás": {"típus": "BlobSource"}, "gyűjtő" : {"típus": "BlobSink", "writeBatchSize": 0; "writeBatchTimeout": "00: 00:00"}}, "a | 2016-09-28 |





## <a name="tutorials"></a>Oktatóanyagok

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 21 | [Oktatóprogram: Az első folyamat folyamat adatokhoz Hadoop fürt összeállítása](data-factory-build-your-first-pipeline.md) | Azure Data Factory oktatóprogram megtudhatja, hogyan hozhat létre és struktúra parancsfájl használatával a Hadoop fürthöz adatokat feldolgozó adatok gyár ütemezése. |
| 22-es hibakód | [Oktatóprogram: Az első Azure adatok gyári Azure erőforrás-kezelő sablonnal összeállítása](data-factory-build-your-first-pipeline-using-arm.md) | Ebben az oktatóanyagban hozzon létre egy minta Azure adatok gyár folyamat egy erőforrás-kezelő Azure-sablon segítségével. |
| 23 | [Oktatóprogram: Az első Azure adatok gyári Azure portálon összeállítása](data-factory-build-your-first-pipeline-using-editor.md) | Ebben az oktatóanyagban hozzon létre egy minta Azure Data Factory folyamat adatok gyári szerkesztővel az Azure-portálon. |
| 24 | [Oktatóprogram: Az első Azure adatok gyári Azure PowerShell használatával összeállítása](data-factory-build-your-first-pipeline-using-powershell.md) | Ebben az oktatóanyagban hozzon létre egy minta Azure Data Factory folyamat Azure PowerShell használatával. |
| 25 | [Oktatóprogram: Az Azure első Build adatok gyári Microsoft Visual Studio segítségével](data-factory-build-your-first-pipeline-using-vs.md) | Ebben az oktatóanyagban hozzon létre egy minta Azure Data Factory folyamat Visual Studio segítségével. |
| 26 | [Oktatóprogram: Hozzon létre egy folyamat másolás tevékenység Azure portál használatával](data-factory-copy-activity-tutorial-using-azure-portal.md) | Ebben az oktatóanyagban létrehozása az Azure Data Factory folyamat egy másolatot a tevékenységhez a szerkesztőprogrammal az adatok gyári az Azure-portálon. |
| 27 | [Oktatóprogram: Hozzon létre egy folyamat másolás tevékenység Azure PowerShell használatával](data-factory-copy-activity-tutorial-using-powershell.md) | Ebben az oktatóanyagban létrehozása az Azure Data Factory folyamat egy másolatot a tevékenységhez a Azure PowerShell használatával. |
| 28 | [Oktatóprogram: Hozzon létre egy folyamat másolás tevékenység Visual Studio segítségével](data-factory-copy-activity-tutorial-using-visual-studio.md) | Ebben az oktatóanyagban létrehozása az Azure adatok gyár folyamat egy másolatot a tevékenységhez a Visual Studio használatával. |
| 29 | [Adatok másolása Blobtárolóhoz adatok gyár használata SQL-adatbázishoz](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) | Ebben az oktatóanyagban bemutatja, hogyan az Azure adatok gyár folyamat másolás tevékenység használatával adatok másolása blobtárolóhoz SQL-adatbázishoz. |
| 30 | [Oktatóprogram: Hozzon létre egy folyamat másolás tevékenység adatok gyári másolás varázsló használata](data-factory-copy-data-wizard-tutorial.md) | Ebben az oktatóanyagban hoz létre az Azure Data Factory folyamat egy másolatot a tevékenységhez a Data Factory által támogatott másolás varázsló használatával |



## <a name="data-movement"></a>Adatok mozgás

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 31. | [Adatok áthelyezése és az Azure Blob-Azure Data Factory használatával](data-factory-azure-blob-connector.md) | Megtudhatja, hogy miként blob-adatok másolása az Azure Data Factory. A minta: adatok másolása, Azure Blob-tárolóhoz és az Azure SQL-adatbázis. |
| 32 | [Adatok áthelyezése és az Azure tó adattár Azure Data Factory használatával](data-factory-azure-datalake-connector.md) | Hogyan helyezhetők át az adatok/az Azure tó adattár Azure Data Factory használatával |
| 33 | [Adatok áthelyezése és az Azure Data Factory használatával DocumentDB](data-factory-azure-documentdb-connector.md) | Megtudhatja, hogyan adatok áthelyezése és Azure DocumentDB gyűjteményből Azure adatok gyár használatával |
| 34 | [Adatok áthelyezése és az Azure SQL-adatbázis Azure Data Factory használatával](data-factory-azure-sql-connector.md) | Megtudhatja, hogy miként adatokat adatbázisból Azure SQL Azure-adatok gyári használatával. |
| 35-tel | [Adatok áthelyezése és az Azure SQL Azure-adatok gyár használatával adatraktár](data-factory-azure-sql-data-warehouse-connector.md) | Útmutató: adatok áthelyezése és az Azure SQL Azure-adatok gyári használatával adatraktár |
| 36 | [Adatok áthelyezése és az Azure-táblából Azure Data Factory használatával](data-factory-azure-table-connector.md) | Megtudhatja, hogy miként Azure Táblatárolóhoz Azure Data Factory használatával vagy az adatokat. |
| 37 | [Másolja a tevékenység teljesítmény és a beállítási útmutatója](data-factory-copy-activity-performance.md) | Tudnivalók a Másolás tevékenység használatakor a az adatok mozgás az Azure Data Factory teljesítményt befolyásoló főbb tényezők |
| 38 | [Adatok áthelyezése másolás tevékenység használatával](data-factory-data-movement-activities.md) | További tudnivalók: adatok mozgás az adatok gyár folyamatok: adatok áttelepítése felhő tárolja, és egy helyszíni áruház és a felhő üzlet közötti. Használja a Másolás tevékenységet. |
| 39 | [Kibocsátási megjegyzések az adatkezelési átjáró](data-factory-gateway-release-notes.md) | Adatok az adatkezelési átjáró címe kibocsátási megjegyzések |
| 40 | [Helyezze át az adatokat a helyszíni Fájlrendszerhez Azure Data Factory használatával](data-factory-hdfs-connector.md) | Tudnivalók a helyszíni Fájlrendszerhez Azure Data Factory segítségével helyezze át az adatokat. |
| 41 | [Figyelheti és kezelheti az Azure Data Factory folyamatok új figyelő és felügyeleti alkalmazás használata](data-factory-monitor-manage-app.md) | Megtudhatja, hogy miként figyelés és felügyeleti alkalmazás használatával figyelésére és Azure adatok gyárak és folyamatok kezelése. |
| 42 | [Adatok áthelyezése fiókok között a helyszíni adatforrások és az adatkezelési átjáró a felhőben](data-factory-move-data-between-onprem-and-cloud.md) | Adatok áthelyezése fiókok között a helyszíni és felhőbeli adatok az átjáró beállítása. Azure Data Factory az adatkezelési átjáró segítségével helyezze át az adatokat. |
| 43 | [Adatok Azure Data Factory használata a OData az adatforrás áthelyezése](data-factory-odata-connector.md) | Ismerje meg az adatokat áthelyezheti az Azure Data Factory használatával OData-adatforrások használatáról. |
| 44 | [Helyezze át az adatokat az ODBC adatokat tárolja Azure Data Factory használatával](data-factory-odbc-connector.md) | Tudnivalók az adatokat áthelyezheti az Azure Data Factory használatával ODBC adatokat tárolja. |
| 45 | [Azure Data Factory használatával DB2 adatainak áthelyezése](data-factory-onprem-db2-connector.md) | További tudnivalók az adatokat áthelyezheti az Azure Data Factory használatával DB2-adatbázishoz |
| 46 | [Adatok áthelyezése és a helyszíni fájlrendszerből Azure Data Factory használatával](data-factory-onprem-file-system-connector.md) | Útmutató: adatok áthelyezése és a helyszíni fájlrendszerből Azure Data Factory használatával. |
| 47 | [A MySQL Azure Data Factory használatával adatok áthelyezése](data-factory-onprem-mysql-connector.md) | Tudnivalók az adatokat áthelyezheti az Azure Data Factory használatával MySQL-adatbázishoz. |
| 48 | [Adatok áthelyezése és a helyszíni Oracle Azure Data Factory használatával](data-factory-onprem-oracle-connector.md) | Megtudhatja, hogy miként áthelyezése az Oracle-adatbázishoz, amely/származó adatokat a helyszíni Azure Data Factory használatával. |
| 49 | [Az adatokat áthelyezheti az Azure adatok gyár használatával PostgreSQL](data-factory-onprem-postgresql-connector.md) | Tudnivalók az adatokat áthelyezheti az Azure adatok gyár használatával PostgreSQL-adatbázishoz. |
| 50 | [Az adatokat áthelyezheti az Azure Data Factory használatával Sybase](data-factory-onprem-sybase-connector.md) | További tudnivalók Azure Data Factory használatával Sybase-adatbázisból származó adatok áthelyezése. |
| 51 | [Adatok áthelyezése a Teradata Azure Data Factory használatával](data-factory-onprem-teradata-connector.md) | Tudnivalók a Teradata-összekötő a Data Factory szolgáltatás, amellyel a Teradata-adatbázisból származó adatok áthelyezése |
| 52 | [Az adatoknak és a helyszíni SQL Server vagy a IaaS (Azure virtuális) Azure Data Factory áthelyezése](data-factory-sqlserver-connector.md) | További tudnivalók és a helyszíni SQL Server-adatbázis vagy az Azure virtuális Azure Factory-adatok használata az adatok áthelyezése. |
| 53 | [Azure Data Factory használatával webes tábla forrásból származó adatok áthelyezése](data-factory-web-table-connector.md) | További tudnivalók a helyszíni adatok áthelyezése egy weblapot Azure adatok gyár táblázat. |



## <a name="data-transformation"></a>Adatok átalakítása

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 54 | [Azure gépi tanulási tevékenységek segítségével prediktív folyamatok létrehozása](data-factory-azure-ml-batch-execution-activity.md) | Megtudhatja, hogy miként hozhat létre az Azure Data Factory és Azure gépi tanulási prediktív folyamatok létrehozása |
| 55 | [Kapcsolt szolgáltatások kiszámítása](data-factory-compute-linked-services.md) | Megismerheti számítási enviornments, hogy is használhatja az Azure Data Factory folyamatok átalakítás/folyamat adatokat. |
| 56 | [Adatok gyári és a köteg folyamat nagyméretű adatkészletek](data-factory-data-processing-using-batch.md) | Ismerteti, hogyan lehet feldolgozni nagyon nagy mennyiségű adat-Azure adatok gyár folyamat Azure köteg párhuzamos feldolgozási videofunkcióinak használatával. |
| 57 | [Adatok az Azure Data Factory átalakítása](data-factory-data-transformation-activities.md) | Megtudhatja, hogy miként vagy adatok folyamat Azure Data Factory Hadoop, gépi tanulási vagy Azure adatok tó Analytics átalakítása. |
| 58 | [Hadoop Streaming tevékenység](data-factory-hadoop-streaming-activity.md) | Megtudhatja, hogyan használhatja a Hadoop Streaming tevékenység-Azure adatok gyári a programok a folyamatos átvitelű Hadoop-a-igény szerinti /: a saját HDInsight fürt futtatásához. |
| 59 | [Tevékenység struktúra](data-factory-hive-activity.md) | Megtudhatja, hogyan használhatja a struktúra tevékenység-Azure adatok gyári a struktúra lekérdezések futtatása a-a-igény szerinti /: a saját HDInsight fürthöz. |
| 60 | [Az adatok gyár MapReduce programok meghívása](data-factory-map-reduce.md) | Megtudhatja, hogy miként MapReduce programok futtatásával egy Azure hdinsight szolgáltatáshoz fürthöz az Azure adatok gyári az adatfeldolgozás. |
| 61 | [Malac tevékenység](data-factory-pig-activity.md) | Megtudhatja, hogyan használhatja a malac tevékenység-Azure adatok gyári a malac parancsprogramokat futtassanak-a-igény szerinti /: a saját HDInsight fürt-e. |
| 62 | [A külső adatok gyári programok meghívása](data-factory-spark.md) | Megtudhatja, hogy miként meghívni külső programok az Azure adatok gyári a MapReduce tevékenység használatával. |
| 63 | [Az SQL Server tárolt eljárás tevékenység](data-factory-stored-proc-activity.md) | Megtudhatja, hogyan használhatja az SQL Server tárolt eljárás tevékenység egy SQL Azure-adatbázis vagy az Azure SQL-adatraktár egy Data Factory folyamat a tárolt eljárás meghívásához. |
| 64 | [Egyéni tevékenységeknek az Azure Data Factory során](data-factory-use-custom-activities.md) | Megtudhatja, hogyan hozhat létre egyéni tevékenységeket, és használhatja őket az Azure Data Factory folyamat. |
| 65 | [Futtassa U-SQL nyelvben parancsfájlt Azure adatok tó Analytics Azure Data Factory](data-factory-usql-activity.md) | Megtudhatja, hogy miként adatfeldolgozás parancsfájlok U – SQL Azure adatok tó Analytics számítási szolgáltatása futtatásával. |



## <a name="samples"></a>A minták

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 66 | [Azure adatok Factory - minta](data-factory-samples.md) | Ismerteti kiszállítása a példák az Azure Data Factory szolgáltatással. |



## <a name="use-cases"></a>Használati eset

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 67 | [Ügyfél esettanulmányok](data-factory-customer-case-studies.md) | Hogyan néhány ügyfeleink használták Azure Data Factory megismerése |
| 68 | [Használatieset - ügyfelek adatainak összegyűjtése](data-factory-customer-profiling-usecase.md) | Megtudhatja, hogyan használható az Azure Data Factory adatalapú munkafolyamat (folyamat) játék ügyfelek profil létrehozása. |
| 69 | [Használatieset - termék javaslatok](data-factory-product-reco-usecase.md) | Tudjon meg többet az Azure Data Factory más szolgáltatásokkal együtt használatával megvalósított használatieset. |



## <a name="monitor-and-manage"></a>Figyelésére és kezelése

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| a 70 | [Figyelésére és Azure adatok gyár folyamatok kezelése](data-factory-monitor-manage-pipelines.md) | Megtudhatja, hogy miként Azure-portál és Azure PowerShell használatával figyelésére és Azure adatok gyárak és folyamatok létrehozott kezelése. |



## <a name="sdk"></a>SDK

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 71 | [Azure adatok Factory - .NET API – módosítási napló](data-factory-api-change-log.md) | Ismerteti bekövetkezett változásokkal kapcsolatban, a szolgáltatás hozzáadását, a hibajavítás stb... az adatok Azure gyári .NET API az adott verziójában. |
| 72 | [Hozzon létre, figyelésére és Azure adatok gyárak adatok gyári .NET SDK használatával kezelése](data-factory-create-data-factories-programmatically.md) | Megtudhatja, hogy miként programozás útján létrehozása, figyelésére és Azure adatok gyárak kezelése adatok gyári SDK használatával. |
| 73 | [Azure adatok gyári Fejlesztői segédlet](data-factory-sdks.md) | Tudnivalók a változatos módszerekkel, amelyekkel létrehozása, figyelésére és Azure adatok gyárak kezelése |



## <a name="miscellaneous"></a>Vegyes

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 74 | [Azure adatok Factory - gyakran ismételt kérdések](data-factory-faq.md) | Gyakori kérdések a a Azure Data Factory. |
| 75 | [Azure adatok Factory - függvények és a rendszer változói](data-factory-functions-variables.md) | Azure Data Factory és a rendszer változók listája |
| 76 | [Azure adatok Factory - elnevezési szabályai](data-factory-naming-rules.md) | Adatok gyár szervezetek elnevezési szabályai ismerteti. |
| 77 | [Adatok gyári kapcsolatos problémák megoldása](data-factory-troubleshoot.md) | Megtudhatja, hogy miként Azure Data Factory használatával kapcsolatos problémák elhárítása. |

