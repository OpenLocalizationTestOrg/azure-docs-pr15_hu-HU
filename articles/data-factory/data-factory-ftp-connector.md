<properties 
    pageTitle="Adatok áthelyezése FTP kiszolgálótól |} Microsoft Azure" 
    description="Tudnivalók az adatok áthelyezése egy FTP-kiszolgálóról Azure Data Factory használatával." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/20/2016" 
    ms.author="spelluru"/>

# <a name="move-data-from-an-ftp-server-using-azure-data-factory"></a>Adatok áthelyezése egy FTP-kiszolgálóról Azure Data Factory használatával
Ez a cikk ismerteti, hogyan Azure Data Factory a Másolás tevékenység segítségével helyezze át az adatokat egy FTP kiszolgálótól egy támogatott gyűjtő adattár. Ez a cikk a [tevékenységekre vonatkozó adatok mozgását](data-factory-data-movement-activities.md) cikk adatok mozgás egy általános áttekintése másolás tevékenység és a támogatott adatforrások és mosdók adatokat tárolja listáját megjelenítő épül. 

Adatok gyári jelenleg csak áthelyezése adatot támogatja az FTP kiszolgálótól egyéb adatokat tárolja, de nem az adatok áthelyezése a többi adatokat tárolja az FTP-kiszolgáló. Támogatja-e mindkét a helyszíni és felhőbeli FTP-kiszolgálókon. 

Ha áthelyezni adatok egy **helyszíni** FTP kiszolgálótól egy felhőalapú adatok Store (Példa: Azure Blob-tárolóhoz), telepítése, és használhatja az adatkezelési átjáró. Az adatkezelési átjáró egy ügyfél ügynök, amely a helyszíni gépen, amely lehetővé teszi, hogy a helyszíni erőforráshoz való kapcsolódáshoz felhőszolgáltatások van telepítve. [Az adatkezelési átjáró](data-factory-data-management-gateway.md) talál részletes tudnivalókat az átjárót. Témakört is [a helyszíni helyek és a felhő között mozgó adatok](data-factory-move-data-between-onprem-and-cloud.md) részletes útmutatást az átjáró beállítása, és azt használja. Az átjáró segítségével csatlakoztatása az FTP-kiszolgálóhoz, még akkor is, ha a kiszolgáló egy Azure IaaS virtuális gépen (virtuális). 

Telepíthető az átjáró a helyszíni ugyanarra a gépre vagy a Azure IaaS virtuális az FTP-kiszolgálót. Jó helyen jár azt javasoljuk, hogy egy másik számítógépre vagy egy különálló Azure IaaS virtuális erőforrás kérelem elkerülése érdekében, és a teljesítmény növelése érdekében az átjáró telepítése. Az átjáró telepítése másik számítógépre, amikor a gép láthatja az FTP-kiszolgáló elérésére. 

## <a name="copy-data-wizard"></a>Az adatok varázsló másolása
A hozzon létre egy folyamat, amely FTP-kiszolgálón lévő adatokat másolja a legegyszerűbben a Másolás adatok varázsló használatával. Lásd: [oktatóprogram: hozzon létre egy példány varázslóval folyamat](data-factory-copy-data-wizard-tutorial.md) létrehozása egy folyamat adatok másolása varázsló segítségével a rövid ismertetését megtalálja. 

Az alábbi példák adnia minta JSON-definíciók létrehozása egy folyamat [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) segítségével vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure Powershellhez](data-factory-copy-activity-tutorial-using-powershell.md)használható. 

## <a name="sample-copy-data-from-ftp-server-to-azure-blob"></a>Minta: Adatok másolása FTP kiszolgálótól Azure blob

Ez a példa bemutatja az Azure Blob-tárolóhoz adatok másolása az FTP-kiszolgálóhoz. Azonban adatokat másolt **közvetlenül** valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores) a Másolás tevékenység használata Azure Data Factory is lehet.  
 
A mintában a következő adatok gyári vállalkozások foglalja magában:

- Csatolt szolgáltatás típusú [FTP-kiszolgáló](#ftp-linked-service-properties).
- Csatolt szolgáltatás típusú [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- A beviteli [adatkészlet](data-factory-create-datasets.md) [Results () fájlmegosztáson](#fileshare-dataset-type-properties)típusú.
- Egy kimenet [adatkészlet](data-factory-create-datasets.md) [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)típusú.
- A [folyamat](data-factory-create-pipelines.md) [FileSystemSource](#ftp-copy-activity-type-properties) és [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)használó másolása a tevékenységhez.

A minta adatokat másolja az FTP kiszolgálótól az Azure blob óránként. A minták következő szakaszok ezeket mintákban használt JSON tulajdonságok témakörben olvashat. 

**FTP csatolt szolgáltatás** Ebben a példában az alapszintű hitelesítés a felhasználónevet és jelszót egyszerű szöveges formátumban. Is használhatja az alábbi módszerek egyikével: 

- Névtelen hitelesítés 
- Alapszintű hitelesítés titkosított hitelesítő adatokkal
- FTP-fölé SSL/TLS (FTPS)

Lásd: hitelesítés használható különböző típusú [FTP csatolt szolgáltatás](#ftp-linked-service-properties) szakaszát. 

    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",          
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
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

**Beviteli adatkészlet FTP** Ez az adatkészlet utal, amelyet az FTP-mappa `mysharedfolder` és `test.csv`. A folyamat a megadott helyre másolja át a fájlt. 

"Külső" beállítást: "igaz" tájékoztatja a Data Factory szolgáltatást, hogy az adatkészlet külső adatok gyári és az adatok gyári tevékenységet nem készül.
    
    {
      "name": "FTPFileInput",
      "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
          "folderPath": "mysharedfolder",
          "fileName": "test.csv",
          "useBinaryTransfer": true
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }


**Azure Blob-kimeneti adatkészlet**

Adatok egy új blob íródott óránként (gyakoriság: óra, intervallum: 1). A mappa elérési útját a blob dinamikusan kiértékeli a kezdési időpontot a szelet, amely feldolgozása alapján. A mappa elérési útja használja az év, hónap, nap és a kezdési időpontot óra részei.

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/ftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

A folyamat egy példány tevékenységet, amely a bemeneti és kimeneti adatkészleteket használatára van beállítva, és van ütemezve óránként tartalmazza. A során JSON megadása az **adatforrás** típusa **FileSystemSource** van állítva, és **BlobSink** **gyűjtő** típusának beállítása. 
    
    {
        "name": "pipeline",
        "properties": {
            "activities": [{
                "name": "FTPToBlobCopy",
                "inputs": [{
                    "name": "FtpFileInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "FileSystemSource"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }],
            "start": "2016-08-24T18:00:00Z",
            "end": "2016-08-24T19:00:00Z"
        }
    }

## <a name="ftp-linked-service-properties"></a>FTP csatolt szolgáltatás tulajdonságai

Az alábbi táblázat ismerteti a csatolt szolgáltatás FTP-specifikus JSON elemek leírását.

| A tulajdonság | Leírás | Szükséges | Alapértelmezett |
| -------- | ----------- | -------- | ------- | 
| típus | A type tulajdonság meg kell FTP-kiszolgáló | igen | &nbsp;
| a Host | Az FTP-kiszolgáló neve vagy IP-címe | igen | &nbsp;
| authenticationType | Adja meg a hitelesítés típusa | igen | Egyszerű, névtelen |
| felhasználónév | Felhasználó, aki hozzáfér az FTP-kiszolgáló | nem | &nbsp;
| jelszó | A felhasználó (felhasználónév) jelszava | nem | &nbsp;
| encryptedCredential | Titkosított hitelesítő adatok az FTP-kiszolgáló elérése | nem | &nbsp;
| átjárónév | Az adatkezelési átjáró átjáró a helyszíni FTP-kiszolgálóhoz való csatlakozáshoz | nem    | &nbsp;
| port | Az FTP-kiszolgáló figyel port | nem | 21 |
| enableSsl | Adja meg, hogy az SSL/TLS csatornán FTP használata | nem | Igaz | 
| enableServerCertificateValidation | Adja meg, hogy engedélyezze a kiszolgáló SSL-tanúsítvány érvényességi FTP SSL/TLS csatornán használatakor | nem | Igaz | 

### <a name="using-anonymous-authentication"></a>Névtelen hitelesítés használatával

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {     
                "authenticationType": "Anonymous",
                "host": "myftpserver.com"
            }
        }
    }

### <a name="using-username-and-password-in-plain-text-for-basic-authentication"></a>Alapszintű hitelesítés egyszerű szöveg felhasználónév és jelszó használata
    
    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "username": "Admin",
                "password": "123456"
            }
        }
    }


### <a name="using-port-enablessl-enableservercertificatevalidation"></a>Port, enableSsl, enableServerCertificateValidation használatával

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",  
                "username": "Admin",
                "password": "123456",
                "port": "21",
                "enableSsl": true,
                "enableServerCertificateValidation": true
            }
        }
    }

### <a name="using-encryptedcredential-for-authentication-and-gateway"></a>Hitelesítés és az átjáró encryptedCredential verzióval

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "encryptedCredential": "xxxxxxxxxxxxxxxxx",
                "gatewayName": "mygateway"
            }
        }
    }

Olvassa el [a beállítás hitelesítő adatok és](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) biztonsági egy helyszíni FTP-adatforráshoz tartozó hitelesítő adatok beállításával kapcsolatban további tájékoztatást.

[AZURE.INCLUDE [data-factory-file-share-dataset](../../includes/data-factory-file-share-dataset.md)]   
[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="ftp-copy-activity-type-properties"></a>Tevékenység-típus tulajdonságok FTP másolása

Szakaszok és a tevékenységek definiálásával használható tulajdonságok teljes listáját a [Folyamatok létrehozása](data-factory-create-pipelines.md) témakört is. Az összes tevékenységtípusokhoz tulajdonságait, például a név, leírás, a bemeneti és kimeneti táblák és házirendek érhetők el. 

Minden tevékenység típusa azonban függenek tulajdonságok elérhető typeProperties szakaszában a tevékenységet. Másolás tevékenységhez az a tulajdonságokat attól függően, hogy milyen típusú adatforrások és mosdók változnak.

[AZURE.INCLUDE [data-factory-file-system-source](../../includes/data-factory-file-system-source.md)]   

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>A teljesítmény és a finombeállítása  
Lásd: a [Másolás tevékenység teljesítmény és útmutató beállítása](data-factory-copy-activity-performance.md) Ha többet szeretne tudni a főbb tényezők hatása teljesítmény mozgásának adatok (másolása a tevékenység) az Azure Data Factory és a különféle módokon optimalizálása azt.

## <a name="next-steps"></a>Következő lépések
Az alábbi cikkekben talál: 

- [Másolás tevékenység oktatóprogram](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletes útmutatást a folyamat létrehozása egy másolatot a tevékenységhez. 
