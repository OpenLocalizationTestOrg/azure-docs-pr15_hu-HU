<properties 
    pageTitle="Azure Data Factory kapcsolatos problémák megoldása" 
    description="Megtudhatja, hogy miként Azure Data Factory használatával kapcsolatos problémák elhárítása." 
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
    ms.date="08/31/2016" 
    ms.author="spelluru"/>

# <a name="troubleshoot-data-factory-issues"></a>Adatok gyári kapcsolatos problémák megoldása
Ebben a cikkben hibaelhárítási tippeket a problémák Azure Data Factory használata esetén. Ez a cikk nem jelenik meg az esetleges problémák a szolgáltatás használata esetén, de néhány problémák és általános hibaelhárítási tippeket terjed ki.   

## <a name="troubleshooting-tips"></a>Hibaelhárítási tippek

### <a name="error-the-subscription-is-not-registered-to-use-namespace-microsoftdatafactory"></a>Hiba: Az előfizetés nem regisztrált névtér "Microsoft.DataFactory" használatához
Ez a hibaüzenet jelenik meg, ha az Azure Data Factory erőforrás-szolgáltató nem regisztrálva van a számítógépen. Tegye a következőket: 

1. Indítsa el az Azure PowerShell. 
2. Jelentkezzen be az Azure-fiók használatával az alábbi parancsot.
        Bejelentkezés-AzureRmAccount 
3. A következő parancsot a Azure Data Factory szolgáltató regisztrálásához.
        Register-AzureRmResourceProvider - ProviderNamespace Microsoft.DataFactory

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a>Probléma: Ezzel az illetéktelen hibaüzenet, amikor egy Data Factory parancsmagot
A jobb oldali Azure-fiók vagy -előfizetésre az Azure PowerShell valószínűleg nem használ. Az alábbi parancsmag használatával jelölje ki a jobb oldali Azure-fiók és az Azure PowerShell használata az előfizetéshez. 

1. Bejelentkezés-AzureRmAccount - használja a megfelelő felhasználói Azonosítójával és jelszavával
2. Get-AzureRmSubscription - fióknak az előfizetések megtekintése. 
3. Jelölje ki-AzureRmSubscription &lt;előfizetés neve&gt; -jelölje ki a megfelelő előfizetést. Ugyanarra a számítógépre, az adatok gyár létrehozhatja az Azure portálon használja.

### <a name="problem-fail-to-launch-data-management-gateway-express-setup-from-azure-portal"></a>Probléma: Nem indítsa el az adatkezelési átjáró Express telepítése Azure portálról
Az adatkezelési átjáró Express telepítő az Internet Explorer vagy a kompatibilis Microsoft ClickOnce webböngészőben van szükség. A gyors telepítést nem indul el, tegye a következők valamelyikét: 

- Használja az Internet Explorer vagy egy Microsoft ClickOnce kompatibilis webböngészőben.

    Ha a Chrome használata esetén nyissa meg a [Chrome webes tároló](https://chrome.google.com/webstore/)keressen a "ClickOnce" kulcsszó, válasszon egyet a ClickOnce extensions és telepítheti. 
    
    Tegye ugyanezt a Firefox (telepítés bővítmény). Kattintson a menü megnyitása gombra az eszköztáron (három vízszintes vonalak jobb felső sarokban lévő), kattintson a bővítmények, "ClickOnce" kulcsszó keresést, válasszon egyet a ClickOnce extensions és telepítheti. 

- A **Kézi beállítás** hivatkozás jelenik meg az azonos fel a portálon használja. Használhatja ezt a megközelítést töltheti le a fájlt, és kézi futtatása. A telepítés után sikeres, megjelenik az adatok adatkezelési átjáró beállításai párbeszédpanelen. Másolja a vágólapra a **kulcsot** a portál képernyőn, és használja azt a Konfigurációkezelő manuálisan regisztrálja az átjárót a szolgáltatással.  

### <a name="problem-fail-to-connect-to-on-premises-sql-server"></a>Probléma: Nem sikerül kapcsolódni a helyszíni SQL Server 
Indítsa el **Az adatkezelési átjáró konfigurációkezelőjének** az átjárót futtató számítógépen, és használja a **Hibaelhárítás** lap tesztelje a kapcsolatot az SQL Server a az átjárót futtató számítógépen. Lásd: a kapcsolat/átjáró hibaelhárítási tippek [problémák elhárítása átjáró](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) kapcsolatos problémákat.   
 

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a>Probléma: Beviteli szeletek szerepelnek, így minden korábbinál Várakozás állam

A szeletek különféle okok miatt **Várakozás** állapotban lehet. A leggyakoribb okok egyike, hogy a **külső** tulajdonság nem értéke **Igaz**. Bármely adatkészlet Azure Data Factory hatókörén kívüli létrehozott **külső** tulajdonság megjelölendő. Ez a tulajdonság azt jelzi, hogy az adatok külső és a nem mentett bármely folyamatok az adatok gyári belül szerint. Az adatok szeletek vannak megjelölve **készen áll** a megfelelő tárolóban lévő adatok elérhetővé válik. 

Lásd: a **külső** tulajdonság a következő példa a használatra. Tetszés szerint megadhatja a **externalData*** beállításakor külső igaz.

Lásd: [adatkészleteket](data-factory-create-datasets.md) cikk ezt a tulajdonságot olvashat bővebben.
    
    {
      "name": "CustomerTable",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "MyLinkedService",
        "typeProperties": {
          "folderPath": "MyContainer/MySubFolder/",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          }
        }
      }
    }

A hiba elhárításához a **külső** tulajdonságot, a választható **externalData** szakasz hozzáadása a beviteli tábla JSON definícióját, és hozza létre a táblázatot. 

### <a name="problem-hybrid-copy-operation-fails"></a>Probléma: Hibrid másolása művelet sikertelen lesz
Lásd: az [átjáró kapcsolatos problémák megoldása](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) a lépések másolása a/áruházból a helyszíni adatok használata az adatkezelési átjáró problémáinak. 

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a>Probléma: Igény szerinti HDInsight kiépítési sikertelen
Egy csatolt típusú szolgáltatás HDInsightOnDemand használata esetén szüksége adjon meg egy linkedServiceName, amely Azure Blob-tárolóhoz mutat. Adatok gyári szolgáltatás használja a tárhely naplókról és a igény szerinti HDInsight fürt segédfájlok tárolni.  A következő hiba miatt sikertelen az igény szerinti HDInsight fürt előfordul, hogy kiépítési:

        Failed to create cluster. Exception: Unable to complete the cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.

Ez a hiba általában azt jelzi, hogy hol található a tárterület-fiókot a linkedServiceName megadott nem ugyanazon a helyen adatok központ hol HDInsight-kiépítési történik. Példa: Ha az adatok gyári nyugati USA-beli és a Azure tároló kelet-Amerikai Egyesült Államokban van, az igény szerinti kiépítési meghiúsul nyugati USA-beli.

Ezenkívül van egy második JSON tulajdonság additionalLinkedServiceNames hol további tárterület-fiókokat az igény szerinti HDInsight adható meg. Csatolt tárhely fiókok kell lennie a HDInsight fürt ugyanazon a helyen, vagy a egy hiba miatt sikertelen.

### <a name="problem-custom-net-activity-fails"></a>Probléma: Egyéni .NET tevékenység sikertelen lesz.
Lásd: az [egyéni tevékenységeket használó egy folyamat hibakeresési](data-factory-use-custom-activities.md#debug-the-pipeline) a lépések részletes leírását. 

## <a name="use-azure-portal-to-troubleshoot"></a>Azure portál használatával kapcsolatos hibák elhárítása 

### <a name="using-portal-blades"></a>Pengéit portál használatával
Lásd: [Monitor folyamat](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) lépéseit. 

### <a name="using-monitor-and-manage-app"></a>Monitort használ, és az alkalmazás kezelése
Lásd: [Monitor és Monitor és App kezelése adatok gyári folyamatok kezelése](data-factory-monitor-manage-app.md) további információt. 

## <a name="use-azure-powershell-to-troubleshoot"></a>Azure PowerShell-lel való kapcsolatos hibák elhárítása

### <a name="use-azure-powershell-to-troubleshoot-an-error"></a>Azure PowerShell-lel való hiba elhárítása  
Lásd: [Azure PowerShell használatával Monitor Data Factory folyamatok](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) további információt. 


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: http://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png
 