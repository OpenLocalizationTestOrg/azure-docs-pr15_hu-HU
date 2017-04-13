<properties 
    pageTitle="Hozzon létre egy alkalmazás háttérismeretek erőforrást PowerShell-parancsprogramot" 
    description="Az alkalmazás az összefüggéseket erőforrások létrehozásának automatizálása." 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="02/19/2016" 
    ms.author="awills"/>

#  <a name="powershell-script-to-create-an-application-insights-resource"></a>Hozzon létre egy alkalmazás háttérismeretek erőforrást PowerShell-parancsprogramot

*Alkalmazás háttérismeretek az előzetes verzióban.*

Az új alkalmazás - vagy az alkalmazás - [Visual Studio alkalmazás háttérismeretek](https://azure.microsoft.com/services/application-insights/)az új verzió figyelni kívánt, beállíthatja a Microsoft Azure új erőforrás. Ez az erőforrás pedig az alkalmazás a telemetriai adatait az elemezni jelenik meg. 

Új erőforrás kibocsátása automatizálhatja a PowerShell használatával.

Például ha a mobileszköz-at, akkor valószínű, hogy bármikor lesz az alkalmazást használja-e az ügyfeleknek közzétett különböző verzióiban. Nem kívánt vegyes különböző verziókban telemetriai az eredmény érhető el. Így kap hozzon létre egy új erőforrást, az egyes összeállítás a Szerkesztés folyamatot.

## <a name="script-to-create-an-application-insights-resource"></a>Hozzon létre egy alkalmazás háttérismeretek erőforrást parancsfájl

Olvassa el a megfelelő parancsmag specs:

* [Új AzureRmResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [Új AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)


*PowerShell-parancsprogramot*  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Add-AzureRmAccount

# Set the name of the Application Insights Resource

$appInsightsName = "TestApp"

# Set the application name used for the value of the Tag "AppInsightsApp" 
# - http://azure.microsoft.com/documentation/articles/azure-preview-portal-using-tags/
$applicationTagName = "MyApp"

# Set the name of the Resource Group to use.  
# Default is the application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create the Resource and Output the name and iKey
###################################################

#Select the azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create the App Insights Resource

$resource = New-AzureRmResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ Name = "AppInsightsApp"; Value = $applicationTagName} `
  -ResourceType "Microsoft.Insights/Components" `
  -Location "Central US" `
  -PropertyObject @{"Type"="ASP.NET"} `
  -Force

# Give owner access to the team

New-AzureRmRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


#Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-to-do-with-the-ikey"></a>Mi a teendő, ha a iKey

Az egyes erőforrások azonosítja a műszerezettségi kulcs (iKey). A iKey olyan eredménye, az erőforrás létrehozási parancsfájl. Az összeállítás parancsfájl meg kell határoznia, az alkalmazás az összefüggéseket SDK a iKey beágyazott az alkalmazást.

Kétféleképpen a iKey számára elérhetővé szeretné tenni a SDK csomagjában talál:
  
* A [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md): 
 * `<instrumentationkey>`*ikey*`</instrumentationkey>`
* Vagy [inicializálni kódot](app-insights-api-custom-events-metrics.md): 
 * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`



## <a name="see-also"></a>Lásd még:

* [Alkalmazás háttérismeretek és a webes próba erőforrások létrehozása sablonból](app-insights-powershell.md)
* [Állítsa be a PowerShell Azure diagnosztika figyelemmel kísérése](app-insights-powershell-azure-diagnostics.md) 
* [Értesítések beállítása a PowerShell használatával](app-insights-powershell-alerts.md)

 