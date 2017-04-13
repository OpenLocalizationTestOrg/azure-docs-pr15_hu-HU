<properties 
    pageTitle="Helyezze át az adatokat a helyszíni Fájlrendszerhez |} Azure Data Factory" 
    description="Tudnivalók a helyszíni Fájlrendszerhez Azure Data Factory segítségével helyezze át az adatokat." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a>Helyezze át az adatokat a helyszíni Fájlrendszerhez Azure Data Factory használatával
Ez a cikk azt ismerteti, hogyan használhatja a Másolás tevékenység az Azure adatok gyári az adatok áthelyezése egy másik adattár egy helyszíni Fájlrendszerhez. Ez a cikk bemutatja az adatok mozgás egy általános áttekintése másolás tevékenység és támogatott adatokat tároló kombinációk témakört a [tevékenységekre vonatkozó adatok mozgását](data-factory-data-movement-activities.md) épül.

Adatok gyári jelenleg csak mozgó adatainak egy helyszíni Fájlrendszerhez egyéb adatokat tárolja, de nem az adatok áthelyezése egy helyszíni Fájlrendszerhez egyéb adatokat tárolja.


## <a name="enabling-connectivity"></a>Kapcsolatok engedélyezése
Adatok gyári szolgáltatás lehetővé teszi, használja az adatkezelési átjáró a helyszíni Fájlrendszerhez csatlakozik. Ha többet szeretne tudni az adatkezelési átjáró és részletes útmutatást talál az átjáró beállítása [a helyszíni helyek és a felhő között mozgó adatok](data-factory-move-data-between-onprem-and-cloud.md) cikkében talál. Az átjáró segítségével Fájlrendszerhez kapcsolódni, akkor is, ha egy IaaS Azure virtuális helyezkedik el. 

Átjáró telepítése a helyszíni ugyanarra a gépre vagy a Azure virtuális, mint a Fájlrendszerhez, miközben azt javasoljuk, hogy az átjáró telepítése egy külön gépi/Azure virtuális IaaS. Problémákat átjárók másik számítógépre csökkenti az erőforrások kérelem és javítja a teljesítményt. Az átjáró telepítése másik számítógépre, amikor a gép látnia kell a Fájlrendszerhez készülék eléréséhez. 


## <a name="copy-data-wizard"></a>Az adatok varázsló másolása
A hozzon létre egy folyamat, amely az adatok másolása a helyszíni Fájlrendszerhez legegyszerűbben a Másolás adatok varázsló használatával. Lásd: [oktatóprogram: hozzon létre egy példány varázslóval folyamat](data-factory-copy-data-wizard-tutorial.md) létrehozása egy folyamat adatok másolása varázsló segítségével a rövid ismertetését megtalálja. 

Az alábbi példák adnia minta JSON-definíciók létrehozása egy folyamat [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) segítségével vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure Powershellhez](data-factory-copy-activity-tutorial-using-powershell.md)használható. Adatok másolása egy helyszíni Fájlrendszerhez az Azure Blob-tárolóhoz mutatnak. Adatok másolni lehet valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores) a Másolás tevékenység használata Azure Data Factory.

## <a name="sample-copy-data-from-on-premises-hdfs-to-azure-blob"></a>Minta: Adatok másolása a helyszíni Fájlrendszerhez Azure Blob

Ez a példa bemutatja az adatok másolása egy helyszíni Fájlrendszerhez Azure Blob-tárolóhoz. Azonban adatokat másolt **közvetlenül** valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores) a Másolás tevékenység használata Azure Data Factory is lehet.  
 
A mintában a következő adatok gyári vállalkozások foglalja magában:

1.  Csatolt szolgáltatás típusú [OnPremisesHdfs](#hdfs-linked-service-properties).
2.  Csatolt szolgáltatás típusú [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  A beviteli [adatkészlet](data-factory-create-datasets.md) [Results () fájlmegosztáson](#hdfs-dataset-type-properties)típusú.
4.  Egy kimenet [adatkészlet](data-factory-create-datasets.md) [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)típusú.
4.  A [folyamat](data-factory-create-pipelines.md) [FileSystemSource](#hdfs-copy-activity-type-properties) és [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)használó másolása a tevékenységhez.

A minta másolja át adatokat egy helyszíni Fájlrendszerhez az Azure blob óránként. A minták következő szakaszok ezeket mintákban használt JSON tulajdonságok témakörben olvashat. 

Első lépésként az adatkezelési átjáró beállítása. A [helyszíni helyek és a felhő között mozgó adatok](data-factory-move-data-between-onprem-and-cloud.md) cikk utasításait. 

**Fájlrendszerhez csatolt szolgáltatás** Ebben a példában a Windows-hitelesítés használatával. Lásd: hitelesítés használható különböző típusú [Fájlrendszerhez csatolt szolgáltatás](#hdfs-linked-service-properties) szakaszát. 

    {
        "name": "HDFSLinkedService",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }

**Azure csatolt tárhelyszolgáltatáshoz**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**A szövegbeviteli adatkészlet Fájlrendszerhez** Ez az adatkészlet hivatkozik, a Fájlrendszerhez mappába DataTransfer/UnitTest /. A folyamat ebbe a mappába összes fájl a megadott helyre másolja. 

"Külső" beállítást: "igaz" tájékoztatja a Data Factory szolgáltatást, hogy az adatkészlet külső adatok gyári és az adatok gyári tevékenységet nem készül.
    
    {
        "name": "InputDataset",
        "properties": {
            "type": "FileShare",
            "linkedServiceName": "HDFSLinkedService",
            "typeProperties": {
                "folderPath": "DataTransfer/UnitTest/"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval":  1
            }
        }
    }




**Azure Blob-kimeneti adatkészlet**

Adatok egy új blob íródott óránként (gyakoriság: óra, intervallum: 1). A mappa elérési útját a blob dinamikusan kiértékeli a kezdési időpontot a szelet, amely feldolgozása alapján. A mappa elérési útja használja az év, hónap, nap és a kezdési időpontot óra részei.

    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Másolás tevékenységeket használó csővezeték**

A folyamat egy másolatot tevékenységet, amely ezeket a bemeneti és kimeneti adatkészleteket használatára van beállítva, és van ütemezve óránként tartalmazza. A során JSON megadása az **adatforrás** típusa **FileSystemSource** van állítva, és **BlobSink** **gyűjtő** típusának beállítása. Az SQL-lekérdezés a **lekérdezés** tulajdonságban meghatározott adatok másolása az elmúlt órában kijelölése
    
    {
        "name": "pipeline",
        "properties":
        {
            "activities":
            [
                {
                    "name": "HdfsToBlobCopy",
                    "inputs": [ {"name": "InputDataset"} ],
                    "outputs": [ {"name": "OutputDataset"} ],
                    "type": "Copy",
                    "typeProperties":
                    {
                        "source":
                        {
                            "type": "FileSystemSource"
                        },
                        "sink":
                        {
                            "type": "BlobSink"
                        }
                    },
                    "policy":
                    {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "00:05:00"
                    }
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="hdfs-linked-service-properties"></a>Csatolt szolgáltatás Fájlrendszerhez tulajdonságai

Az alábbi táblázat ismerteti a leírás JSON elemek Fájlrendszerhez jellemző csatolt szolgáltatás.

| A tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- | 
| típus | Meg kell a típusa tulajdonság: **fájlrendszerhez** | igen | 
| URL-címe | A Fájlrendszerhez URL-címe | igen |
| encryptedCredential | Az access hitelesítő adatokhoz [Új-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) kimenete | nem |
| Felhasználónév | Felhasználónév és a Windows-hitelesítést. | Igen (Windows-hitelesítés)
| jelszó | Windows-hitelesítés jelszava. | Igen (Windows-hitelesítés)
| authenticationType | A Windows vagy névtelen. | igen |
| átjárónév | Az adatok gyári szolgáltatás használandó a Fájlrendszerhez csatlakozhat az átjáró neve. | igen |   

Olvassa el [a beállítás hitelesítő adatok és](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) biztonsági hitelesítő adatok beállítás a helyszíni Fájlrendszerhez kapcsolatban további tájékoztatást.

### <a name="using-anonymous-authentication"></a>Névtelen hitelesítés használatával

    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Anonymous",
                "userName": "hadoop",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }


### <a name="using-windows-authentication"></a>Windows-hitelesítés használatával
    
    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }
 


## <a name="hdfs-dataset-type-properties"></a>Fájlrendszerhez adatkészlet tulajdonságai

Szakaszok és a rendelkezésre álló adatkészleteket definiálása tulajdonságok teljes listáját a [létrehozása adatkészleteket](data-factory-create-datasets.md) témakört is. Szakaszok, például a struktúra, elérhetőségét és egy adatkészletből JSON házirend hasonlóak az adatkészlet diagramtípusokat (Azure SQL Azure blob, Azure táblázat stb.).

A **typeProperties** szakasz eltérő adatkészlet hibatípusonként és az adatok tárolása az adatok helyének adatait. Típus (Fájlrendszerhez adatkészlet tartalmazó) **Results () fájlmegosztáson** adatkészlet az typeProperties szakaszban rendelkezik az alábbi tulajdonságok

A tulajdonság | Leírás | Szükséges
-------- | ----------- | --------
Mappa_útvonala | A mappa elérési útját. Példa:`myfolder`<br/><br/>Escape-karakter használata "\" karakterláncban speciális karakternél. Példa: a folder\subfolder, adja meg a mappa\\\\almappa és d:\samplefolder, állítsa be a d:\\\\mappába.<br/><br/>Ez a tulajdonság kombinálhatja **partitionBy** mappa elérési utak szeletet alapján van kezdete/vége dátum-idő. | igen
Fájlnév | Ha azt szeretné, hogy a táblázatot egy adott fájlra a mappában, adja meg a fájl nevét a **Mappa_útvonala** . Ha nem ad meg ez a tulajdonság értékét a táblázat a mappában lévő összes fájl mutat.<br/><br/>Ha egy kimenet adatkészlet fileName nincs megadva, a létrehozott fájl nevét a következő lesz az alábbi ebben a formátumban: <br/><br/>Adatok. <Guid>.txt (például:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt | nem
partitionedBy | Adja meg a dinamikus Mappa_útvonala, az idő adatsor filename partitionedBy használható. Példa: Mappa_útvonala az adatok óránként paraméteres. | nem
fileFilter | Adja meg, jelölje be az összes fájl, hanem a Mappa_útvonala fájlok csak egy részhalmazát használandó szűrő. <br/><br/>Engedélyezett értékei a következők: `*` (több karakter) és `?` (egyetlen karakter).<br/><br/>Példák 1:`"fileFilter": "*.log"`<br/>Példa 2:`"fileFilter": 2014-1-?.txt"`<br/><br/>**Megjegyzés**: fileFilter egy beviteli Results () fájlmegosztáson adatkészlet alkalmazható | nem
| Formátum | A következő formátumú már támogatott: **TextFormat**, **AvroFormat**, **JsonFormat**, **OrcFormat**és **ParquetFormat**. A tulajdonság **típusa** formátum csoportban az egyik ezeket az értékeket. További információt [Tartalmazó TextFormat](#specifying-textformat), [AvroFormat megadása](#specifying-avroformat), [Megadása JsonFormat](#specifying-jsonformat), [OrcFormat megadása](#specifying-orcformat)és [Megadása ParquetFormat](#specifying-parquetformat) lásd. Ha a fájlokat a másolni kívánt-van közötti fájlalapú tárolók is (bináris) példányát, ugorja át a két bemeneti és kimeneti adatkészlet definíciók a formátuma szakaszban. | nem 
| tömörítés | Adja meg a típus és az adatok tömörítés szintjét. Támogatott típusú: **GZip**, a **Deflate**, és a **BZip2** és a támogatott szintek: **Optimal** és **leggyorsabb**. Tömörítési beállítások jelenleg nem támogatott **AvroFormat** vagy **OrcFormat**adatait. További tudnivalókért lásd: [tömörítés támogatás](#compression-support) szakaszában.  | nem |



> [AZURE.NOTE] fájlnév- és fileFilter egyidejű nem használhatók.


### <a name="using-partionedby-property"></a>PartionedBy tulajdonság használatával

Az előző szakaszban említett, dinamikus Mappa_útvonala, az idő adatsor partitionedBy a fájlnevet is megadhat. A Data Factory makrókat és a rendszer változó SliceStart, az egy adott szeletre logikai időszakhoz jelző SliceEnd megteheti. 

Ha többet szeretne megtudni az idő sorozat adatkészleteket, ütemezés, és a szeletek, cikkek [Létrehozása adatkészleteket](data-factory-create-datasets.md), [Ütemezés és a végrehajtás](data-factory-scheduling-and-execution.md)és [Folyamatok létrehozása](data-factory-create-pipelines.md) . 

#### <a name="sample-1"></a>Minta 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

Ebben a példában {szeletet} Data Factory rendszer változó SliceStart (YYYYMMDDHH) formátumban megadott értékkel helyettesíti. A SliceStart elindítása a szelet hivatkozik. A Mappa_útvonala eltér az egyes szeletek. Példa: wikidatagateway/wikisampledataout/2014100103 vagy wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>Minta 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

Ebben a példában az év, hónap, nap és SliceStart időpontjának kibontása be külön változók Mappa_útvonala és a fájlnevet tulajdonságok által használt.

[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="hdfs-copy-activity-type-properties"></a>Fájlrendszerhez másolás tevékenység típusának tulajdonságai

Szakaszok és a tevékenységek definiálásával használható tulajdonságok teljes listáját a [Folyamatok létrehozása](data-factory-create-pipelines.md) témakört is. Az összes tevékenységtípusokhoz tulajdonságait, például a név, leírás, a bemeneti és kimeneti táblák és házirendek érhetők el. 

Minden tevékenység típusa azonban függenek tulajdonságok elérhető typeProperties szakaszában a tevékenységet. Másolás tevékenységhez azok eltérőek attól függően, hogy milyen típusú adatforrások és mosdók.

Másolás tevékenységhez **FileSystemSource** típusú adatforrások esetén az alábbi tulajdonságok érhetők el typeProperties szakasz:

**FileSystemSource** támogatja a következő tulajdonságokat:

| A tulajdonság | Leírás | Megengedett érték | Szükséges |
| -------- | ----------- | -------------- | -------- |
| rekurzív | Azt jelzi, hogy az adatok olvasható rekurzív az almappák vagy csak a megadott mappába. | IGAZ, hamis (alapértelmezett)| nem |

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>A teljesítmény és a finombeállítása  
Lásd: a [Másolás tevékenység teljesítmény és útmutató beállítása](data-factory-copy-activity-performance.md) Ha többet szeretne tudni a főbb tényezők hatása teljesítmény mozgásának adatok (másolása a tevékenység) az Azure Data Factory és a különféle módokon optimalizálása azt.

