<properties 
    pageTitle="Figyelése és kezelése a PowerShell Értékáram-elemzés feladatok |} Microsoft Azure" 
    description="Megtudhatja, hogy miként figyelheti és kezelheti a feladatokat megjelenítő Értékáram-elemzés Azure Powershellről és annak parancsmagjairól használatával." 
    keywords="Azure powershell, azure powershell-parancsmagok, powershell-parancsot powershell-parancsfájlokat" 
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>


# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a>Lync- és az Azure PowerShell-parancsmagok Értékáram-elemzés feladatok kezelése

Megtudhatja, hogy miként figyelheti és Értékáram-elemzés erőforrásokkal Azure PowerShell-parancsmagok és powershell-parancsfájlokat alapműveletek Értékáram-elemzés végrehajtása.

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a>Értékáram-elemzés az Azure PowerShell-parancsmagok futtató előfeltételei

 - Hozzon létre egy Azure erőforráscsoport előfizetéséhez. Az alábbiakban Azure PowerShell mintaparancsfájl. Azure PowerShell információ [Telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md);  

Azure PowerShell 0.9.8:  

        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group if you have more than one subscription on your account.
        Select-AzureSubscription -SubscriptionName <subscription name>
 
        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>

Azure PowerShell 1.0:  

        # Log in to your Azure account
        Login-AzureRmAccount

        # Select the Azure subscription you want to use to create the resource group.
        Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureRmResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'
        
        # Create an Azure resource group
        New-AzureRMResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>
        


> [AZURE.NOTE] Adatfolyam-elemző feladatok hozta létre nem rendelkezik figyelése alapértelmezés szerint engedélyezve van.  Manuális engedélyezheti az Azure-portálon felügyeletet igényel, Navigálás a feladat Monitor lapra, és kattintson az Engedélyezés gombra, vagy ezt megteheti programozás útján [Azure adatfolyam Analytics - Monitor adatfolyam Analytics feladatok programozottan](stream-analytics-monitor-jobs.md)található lépéseket követve.

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a>Értékáram-elemzés Azure PowerShell-parancsmagok
A következő Azure PowerShell-parancsmagok figyelésére és Azure Értékáram-elemzés feladatok kezelésére használható. Ne feledje, hogy Azure PowerShell különböző verzióival. 
**A példákban szerepel a listában az első parancs van Azure PowerShell 0.9.8, a második parancs nem Azure PowerShell 1.0.** Az Azure PowerShell 1.0 parancsok mindig lesz "AzureRM" a parancsot.

### <a name="get-azurestreamanalyticsjob--get-azurermstreamanalyticsjob"></a>Get-AzureStreamAnalyticsJob |} Get-AzureRMStreamAnalyticsJob
Felsorolja az Azure-előfizetést vagy a megadott erőforráscsoport meghatározott összes Értékáram-elemzés feladatokat, vagy egy erőforrás csoporton belüli adott feladat feladat információt kap.

**Példa 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob

A PowerShell-parancsot a adatfolyam Analytics-feladatok információt az Azure előfizetés adja eredményül.

**Példa 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

A PowerShell-parancsot a adatfolyam Analytics-feladatok információt az erőforráscsoport StreamAnalytics-alapértelmezett – központi-hu adja eredményül.

**Például: 3**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

A PowerShell-parancsot a Értékáram-elemzés feladat StreamingJob információt az erőforráscsoport StreamAnalytics-alapértelmezett – központi-hu adja eredményül.

### <a name="get-azurestreamanalyticsinput--get-azurermstreamanalyticsinput"></a>Get-AzureStreamAnalyticsInput |} Get-AzureRMStreamAnalyticsInput
A bemeneti adatok alapján, az egy adott Értékáram-elemzés feladat definiált listája, vagy egy adott beviteli tájékoztatást kap.

**Példa 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

A PowerShell-parancsot a feladatban StreamingJob definiált összes bemenetben információt adja eredményül.

**Példa 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

A PowerShell-parancsot a bemeneti a feladatban StreamingJob definiált EntryStream nevű információt adja eredményül.

### <a name="get-azurestreamanalyticsoutput--get-azurermstreamanalyticsoutput"></a>Get-AzureStreamAnalyticsOutput |} Get-AzureRMStreamAnalyticsOutput
A kimeneti értékeket, a megadott Értékáram-elemzés projekt definiált listája, vagy egy adott kimenet tájékoztatást kap.

**Példa 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

A PowerShell-parancsot a kimeneti értékeket a feladatban StreamingJob definiált információkat adja eredményül.

**Példa 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

A PowerShell-parancsot a kimenet a feladatban StreamingJob definiált kimeneti nevű információt adja eredményül.

### <a name="get-azurestreamanalyticsquota--get-azurermstreamanalyticsquota"></a>Get-AzureStreamAnalyticsQuota |} Get-AzureRMStreamAnalyticsQuota
Kap egy megadott tartomány egységek streaming kvóta tájékozódhat.

**Példa 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsQuota –Location "Central US" 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsQuota –Location "Central US" 

A PowerShell-parancsot a kvóta és a továbbított egységek használatát információt a központi USA-beli tartományban lévő adja eredményül.

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a>Get-AzureStreamAnalyticsTransformation |} GetAzureRMStreamAnalyticsTransformation
Értékáram-elemzés feladatban definiált adott átalakítás információt kap.

**Példa 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

A PowerShell-parancsot az átalakítás StreamingJob nevű a feladat StreamingJob információt adja eredményül.

### <a name="new-azurestreamanalyticsinput--new-azurermstreamanalyticsinput"></a>Új AzureStreamAnalyticsInput |} Új AzureRMStreamAnalyticsInput
Létrehoz egy új beviteli Értékáram-elemzés feladat, vagy egy meglévő megadott beviteli frissíti.
  
A bemeneti nevét a .json fájlban vagy a parancssorból adható meg. Ha mindkét van megadva, a nevét a parancssorban ugyanaz, mint a fájlban kell lennie.

Ha adja meg, amely már létezik egy beviteli, és nem adja meg a – hatályba paraméter parancsmag fogja kérni-e le szeretné cserélni a meglévő bemeneti.

Ha adja meg a – paraméter kényszerítése, és adja meg a név egy meglévő beviteli, a bemeneti cserélődik megerősítés nélkül.

Részletes információkat a JSON fájlszerkezet és annak tartalmát, olvassa el a [Beviteli létrehozása (Azure Értékáram-elemzés)] [ msdn-rest-api-create-stream-analytics-input] szakaszban a [adatfolyam Analytics REST API -val hivatkozás könyvtár][stream.analytics.rest.api.reference].

**Példa 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

A PowerShell-parancsot a fájlból Input.json hoz létre egy új beviteli. Ha egy meglévő beviteli a bemeneti definíciós fájl a megadott nevű már meg van határozva, a parancsmag megkérdezi, hogy felváltsa-e.

**Példa 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

A PowerShell-parancsot a EntryStream nevű feladat hoz létre egy új beviteli. Ha egy meglévő beviteli ilyen nevű már meg van határozva, a parancsmag megkérdezi, hogy felváltsa-e.

**Például: 3**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

A PowerShell-parancsot lecseréli a meglévő beviteli forrás EntryStream nevű a fájlt a definíció meghatározása.

### <a name="new-azurestreamanalyticsjob--new-azurermstreamanalyticsjob"></a>Új AzureStreamAnalyticsJob |} Új AzureRMStreamAnalyticsJob
Értékáram-elemzés új feladat létrehozása a Microsoft Azure-ban, vagy egy meglévő megadott feladat definícióját frissíti.

A feladat nevét a .json fájlban vagy a parancssorból adható meg. Ha mindkét van megadva, a nevét a parancssorban ugyanaz, mint a fájlban kell lennie.

Ha a feladat nevét, amely már létezik, és nem adja meg a – hatályba paraméter parancsmag megkérdezi, hogy cserélje le a meglévő projekt-e.

Ha adja meg a – paraméter kényszerítése, és adjon meg egy meglévő projekt nevét a feladat definíciója a megerősítést kérő üzenet nélkül ki kell cserélni.

Részletes információkat a JSON fájlszerkezet és annak tartalmát, olvassa el a [Adatfolyam Analytics feladat létrehozása] [ msdn-rest-api-create-stream-analytics-job] szakaszban a [adatfolyam Analytics REST API -val hivatkozás könyvtár][stream.analytics.rest.api.reference].

**Példa 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

A PowerShell-parancsot új feladat JobDefinition.json meghatározása hoz létre. Ha a projekt-definíciós fájl a megadott névvel egy meglévő feladat már meg van határozva, a parancsmag megkérdezi, hogy felváltsa-e.

**Példa 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

A PowerShell parancs a projekt definícióját StreamingJob váltja fel.

### <a name="new-azurestreamanalyticsoutput--new-azurermstreamanalyticsoutput"></a>Új AzureStreamAnalyticsOutput |} Új AzureRMStreamAnalyticsOutput
Létrehoz egy új kimenet Értékáram-elemzés feladat, vagy egy meglévő kimeneti frissíti.  

A kimenet neve a .json fájlban vagy a parancssorból adható meg. Ha mindkét van megadva, a nevét a parancssorban ugyanaz, mint a fájlban kell lennie.

Ha olyan eredménye, amely már létezik adja meg, és nem adja meg a – hatályba paraméter parancsmag megkérdezi, hogy cserélje le a meglévő eredménye-e.

Ha adja meg a – paraméter kényszerítése, és adja meg a név egy meglévő kimeneti, a kimeneti cserélődik megerősítés nélkül.

Részletes információkat a JSON fájlszerkezet és annak tartalmát, olvassa el a [Kimeneti létrehozása (Azure Értékáram-elemzés)] [ msdn-rest-api-create-stream-analytics-output] szakaszban a [adatfolyam Analytics REST API -val hivatkozás könyvtár][stream.analytics.rest.api.reference].

**Példa 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

A PowerShell-parancsot hoz létre egy új kimenet "kimeneti" nevű a feladat StreamingJob. Ha egy meglévő kimeneti ilyen nevű már meg van határozva, a parancsmag megkérdezi, hogy felváltsa-e.

**Példa 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

A PowerShell-parancsot a definíció "kimeneti" a feladat StreamingJob váltja fel.

### <a name="new-azurestreamanalyticstransformation--new-azurermstreamanalyticstransformation"></a>Új AzureStreamAnalyticsTransformation |} Új AzureRMStreamAnalyticsTransformation
Létrehoz egy új átalakítások Értékáram-elemzés feladat, vagy a meglévő transzformáció frissíti.
  
Az átalakítás nevét a .json fájlban vagy a parancssorból adható meg. Ha mindkét van megadva, a nevét a parancssorban ugyanaz, mint a fájlban kell lennie.

Ha adja meg, amely már létezik az átalakítás, és nem adja meg a – hatályba paraméter parancsmag fogja kérni-e le szeretné cserélni a meglévő transzformáció.

Ha adja meg a – paraméter kötelező, és adja meg a létező átalakítása nevét, a program lecseréli az átalakítás megerősítés nélkül.

Részletes információkat a JSON fájlszerkezet és annak tartalmát, olvassa el a [Átalakítása létrehozása (Azure Értékáram-elemzés)] [ msdn-rest-api-create-stream-analytics-transformation] szakaszban a [adatfolyam Analytics REST API -val hivatkozás könyvtár][stream.analytics.rest.api.reference].

**Példa 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

A PowerShell-parancsot a feladat StreamingJob StreamingJobTransform nevű új átalakítás hoz létre. Ha egy meglévő átalakítást ilyen nevű már meg van határozva, a parancsmag megkérdezi, hogy felváltsa-e.

**Példa 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

 A PowerShell-parancsot a feladat StreamingJob StreamingJobTransform definícióját váltja fel.

### <a name="remove-azurestreamanalyticsinput--remove-azurermstreamanalyticsinput"></a>Eltávolítás-AzureStreamAnalyticsInput |} Eltávolítás-AzureRMStreamAnalyticsInput
Egy adott beviteli aszinkron töröl egy Értékáram-elemzés feladatot a Microsoft Azure-ban.  
Ha adja meg a – hatályba paramétert, a bemeneti megerősítés nélkül is törli.

**Példa 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

A PowerShell-parancsot a bemeneti EventStream eltávolítja a feladat StreamingJob.  

### <a name="remove-azurestreamanalyticsjob--remove-azurermstreamanalyticsjob"></a>Eltávolítás-AzureStreamAnalyticsJob |} Eltávolítás-AzureRMStreamAnalyticsJob
Aszinkron törli az adott Értékáram-elemzés feladat Microsoft Azure-ban.  
Ha adja meg a – hatályba paramétert a feladat törlődik a megerősítést kérő üzenet nélkül.

**Példa 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

A PowerShell-parancsot a feladat StreamingJob eltávolítja.  

### <a name="remove-azurestreamanalyticsoutput--remove-azurermstreamanalyticsoutput"></a>Eltávolítás-AzureStreamAnalyticsOutput |} Eltávolítás-AzureRMStreamAnalyticsOutput
Egy adott kimeneti aszinkron töröl egy Értékáram-elemzés feladatot a Microsoft Azure-ban.  
Ha adja meg a – hatályba paramétert, a kimeneti megerősítés nélkül is törli.

**Példa 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

A PowerShell parancs eltávolítása a feldolgozás StreamingJob kimeneti a kimenet.  

### <a name="start-azurestreamanalyticsjob--start-azurermstreamanalyticsjob"></a>Kezdés-AzureStreamAnalyticsJob |} Kezdés-AzureRMStreamAnalyticsJob
Aszinkron üzembe helyezése, és egy Értékáram-elemzés feladat elindítja a Microsoft Azure-ban.

**Példa 1**

Azure PowerShell 0.9.8:  

    Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Azure PowerShell 1.0:  

    Start-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

A PowerShell parancs elindítja a projekt a kezdési időpontjának egyéni kimeneti StreamingJob beállítása December 12, 2012, 12:12:12 UTC.


### <a name="stop-azurestreamanalyticsjob--stop-azurermstreamanalyticsjob"></a>Leállítás-AzureStreamAnalyticsJob |} Leállítás-AzureRMStreamAnalyticsJob
Aszinkron leállítja a Értékáram-elemzés feladat futását, a Microsoft Azure-ban, és vonja osztja ki az erőforrásokat, amelyekre voltak használatban. A feladat definíciója és a metaadatokat azonban nem törlődnek az Azure portal és a kezelés API-hoz, az előfizetést belül érhető el, hogy a feladat szerkeszthető, és újra. Ön nem ráterheljük a feladat Leállítva állapotú.

**Példa 1**

Azure PowerShell 0.9.8:  

    Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Stop-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

A PowerShell parancs leállítja a feladat StreamingJob.  

### <a name="test-azurestreamanalyticsinput--test-azurermstreamanalyticsinput"></a>Próba-AzureStreamAnalyticsInput |} Próba-AzureRMStreamAnalyticsInput
Azt vizsgálja, hogy a kapcsolódási lehetőség a Értékáram-elemzés egy megadott beviteli.

**Példa 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

A PowerShell-parancsot a StreamingJob azt vizsgálja, a bemeneti EntryStream kapcsolat állapotát.  

###<a name="test-azurestreamanalyticsoutput--test-azurermstreamanalyticsoutput"></a>Próba-AzureStreamAnalyticsOutput |} Próba-AzureRMStreamAnalyticsOutput
Azt vizsgálja, hogy a Értékáram-elemzés lehetőségét, a megadott kimeneti csatlakozni.

**Példa 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

A PowerShell parancs teszteket a kimenet kapcsolati állapotának kimenet StreamingJob.  

## <a name="get-support"></a>Technikai támogatás kérése
További segítségért próbálja meg az [Azure Értékáram-elemzés fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics). 


## <a name="next-steps"></a>Következő lépések

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 



[msdn-switch-azuremode]: http://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 
