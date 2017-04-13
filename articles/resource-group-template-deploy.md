<properties
   pageTitle="Erőforrások PowerShell és a sablonok telepítése |} Microsoft Azure"
   description="Erőforrás-kezelő Azure és Azure PowerShell használata a telepítéshez használni egy erőforrás Azure. Az erőforrások vannak definiálva, az erőforrás-kezelő sablon"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Források az erőforrás-kezelő sablonok és Azure PowerShell telepítése

> [AZURE.SELECTOR]
- [A PowerShell](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portál](resource-group-template-deploy-portal.md)
- [REST API-VAL](resource-group-template-deploy-rest.md)

Ebből a témakörből megtudhatja, Azure PowerShell használata az erőforrás-kezelő sablonokkal Azure szeretne telepíteni, az erőforrások.  

> [AZURE.TIP] A hiba hibakeresés a telepítés során című témakörben talál segítséget:
>
> - [Azure PowerShell telepítésének műveletek megtekintése](resource-manager-troubleshoot-deployments-powershell.md) , ha többet szeretne tudni az első, amely segít a hiba elhárítása
> - [Gyakori hibák elhárítása az Azure erőforrás-kezelő Azure erőforrások telepítésekor](resource-manager-common-deployment-errors.md) módjáról a gyakori telepítési hibáinak elhárítása

A sablon lehet egy helyi fájl vagy URI keresztül elérhető külső fájlba. Ha a sablon a tárterület-fiókjában található, korlátozhatja a hozzáférést a sablont, és adja meg a megosztott aláírás (Társítások) jogkivonat a telepítés során.

## <a name="quick-steps-to-deployment"></a>A rövid lépéseket követve telepítési

Ez a cikk ismerteti a különböző telepítés közben használható beállítások. Azonban gyakran csak akkor van szükség két egyszerű parancsot. Gyorsan elsajátíthatja telepítési, az alábbi parancsokat használhatja:

    New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
    New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

Többet szeretne tudni a telepítéshez, előfordulhat, hogy jobban is megfelelnek a levelezés beállításai, folytassa a cikket elolvasva.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-powershell"></a>A PowerShell telepítése

1. Jelentkezzen be az Azure-fiókjába.

        Add-AzureRmAccount

     A fiók összefoglalását adja vissza.

        Environment : AzureCloud
        Account     : someone@example.com
        ...

2. Ha több előfizetéssel rendelkezik, adja meg a **Set-AzureRmContext** paranccsal telepítéshez használni kívánt előfizetés azonosítója. 

        Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

3. A szokásos új sablon telepítésekor szeretne létrehozni, amely tartalmazhat, az erőforrások erőforráscsoport. Ha egy meglévő erőforráscsoport telepítéshez használni kívánt, ugorja át ezt a lépést, és használja az erőforráscsoport. 

     Erőforráscsoport létrehozásához adja meg a nevét és helyét a erőforráscsoport. Meg kell adnia az erőforráscsoport helyét, mivel az erőforráscsoport tárolja a metaadatokat az erőforrásokról. Megfelelőségi okok miatt előfordulhat, hogy szeretne megadni, hogy a metaadatok tároló. Általánosságban elmondható azt javasoljuk, hogy meg egy helyet, akkor a legtöbb az erőforrások tároló. Használata ugyanazon a helyen egyszerűbbé teszi a sablont.

        New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
   
     Az új erőforráscsoport összefoglalását adja vissza.
   
        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
             Actions  NotActions
             =======  ==========
             *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

4. A telepítés előtt, ellenőrizheti a telepítési beállítások. A **Próba-AzureRmResourceGroupDeployment** parancsmag lehetővé teszi, hogy problémák keresése a tényleges erőforrások létrehozása előtt. A következő példa bemutatja, hogyan ellenőrzése a telepítés.

        Test-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

5. Az erőforráscsoport erőforrások telepítéséhez futtassa a **New-AzureRmResourceGroupDeployment** parancsot, és adja meg a szükséges paraméterek. A paraméterek egy nevet a telepítéshez, az erőforráscsoport, az elérési út vagy URL-CÍMÉT az létrehozott sablonra nevét és az igényektől szükséges paramétereket tartalmazza. Ha a **mód** paraméter nincs megadva, az alapértelmezett érték **Léptetési távolság** használják. Egy teljes telepítési futtatásához **kész** **mód** beállításához. Ügyeljen arra, hogy a teljes mód használatakor, mint információforrások, amelyek nem találhatók meg a sablon véletlenül törölheti.

     Helyi sablon üzembe helyezéséhez **TemplateFile** paraméter használatával:

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

     Egy külső sablon üzembe helyezéséhez **TemplateUri** paraméter használatával:

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate>
   
     Paraméterértékeket kezeléséről az alábbi lehetőségekből választhat: 
   
     1. Beágyazott paraméterek használata.

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -myParameterName "parameterValue"

     2. A paraméter objektum használja.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterObject $parameters

     3. Helyi paraméter fájlt használ. A sablonfájlt információt [paraméter-fájl](#parameter-file)megtekintéséhez.

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

     4. Külső paraméter fájlt használ. A sablonfájlt információt [paraméter-fájl](#parameter-file)megtekintéséhez. 

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate> -TemplateParameterUri <LinkToParameterFile>

        Egy külső paraméter fájl használatakor nem lehet a sikeres egyéb értékek vagy beágyazott vagy egy helyi fájl. További tudnivalókért olvassa el a [paraméter elsőbbségi](#parameter-precendence)című témakört.

     Az erőforrások telepítését követően jelenik meg a telepítési összefoglalását.

        DeploymentName    : ExampleDeployment
        ResourceGroupName : ExampleResourceGroup
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2015 7:00:27 PM
        Mode              : Incremental
        ...

     Ha a sablon neve megegyezik az paramétereknek a paraméter szerepel a PowerShell-parancsot, a program kéri a adjon meg egy értéket az adott paraméter. A sablon alapján a paraméter utólagos javítás **FromTemplate**is tartalmazni fogja. Ha például nevű paraméter **ResourceGroupName** a **ResourceGroupName** paramétert a [New-AzureRmResourceGroupDeployment](https://msdn.microsoft.com/library/azure/mt679003.aspx) parancsmagot a sablon ütközik. A szükséges értéket **ResourceGroupNameFromTemplate**kéri. Általánosságban elmondható ne a fejetlenséget által nem elnevezési paraméterek paraméterként használt üzembe helyezési műveleteket azonos nevű.

6. Ha szeretne, amely bármely telepítési hibáinak elhárítása, **DeploymentDebugLogLevel** paraméter használatával segíthet üzembe helyezésével kapcsolatos további információk naplózása. Megadhatja, hogy kérelem tartalmat vagy válasz tartalom telepítésének művelettel bejelentkezni.

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -DeploymentDebugLogLevel All -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate>
        
     A hibakeresési tartalom használatával kapcsolatos hibák elhárítása telepítések kapcsolatos további tudnivalókért olvassa el a [Hibaelhárítás erőforrás csoport telepítési környezetének Azure PowerShell](resource-manager-troubleshoot-deployments-powershell.md)című témakört.

## <a name="deploy-template-from-storage-with-sas-token"></a>A biztonsági jogkivonat tárhelyről sablon terjesztése

A sablonok hozzáadása egy tárterület-fiók és a biztonsági jogkivonat, a telepítés során hivatkozása őket.

> [AZURE.IMPORTANT] Az alábbi lépéseket követve a sablont tartalmazó blob elérhető, csak a fióktulajdonos. A biztonsági jogkivonat a blob-létrehozásakor a blob viszont bárki megnyithatja az adott URI. Ha egy másik felhasználó az URI elfogja, a felhasználó hozzáférhet a sablon lesz. A biztonsági jogkivonat használata a sablonok való hozzáférés korlátozása a jó módszer, de nem terjedhet bizalmas adatokat, például jelszavak közvetlenül a sablonban.

### <a name="add-private-template-to-storage-account"></a>Személyes sablon hozzáadása a tárterület-fiókhoz

Fiók beállítása a tárterület a sablonok között az alábbi lépéseket:

1. Hozzon létre egy erőforrás csoportot.

        New-AzureRmResourceGroup -Name ManageGroup -Location "West US"

2. Tárterület-fiók létrehozása. A tároló fióknév kell egyedi Azure keresztül, a úgy adja meg a saját nevét a fiókhoz.

        New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates -Type Standard_LRS -Location "West US"

3. Állítsa be az aktuális tárterület-fiókját.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

4. Hozzon létre egy tároló. A jogosultsági **ki az** azt jelenti, hogy a tároló csak a tulajdonos érhető el, amelyek értéke.

        New-AzureStorageContainer -Name templates -Permission Off
        
5. A sablon hozzáadása a tároló.

        Set-AzureStorageBlobContent -Container templates -File c:\Azure\Templates\azuredeploy.json
        
### <a name="provide-sas-token-during-deployment"></a>Adja meg a biztonsági jogkivonat a telepítés során

Személyes sablon tárterület-fiókjában üzembe helyezéséhez beolvasni a biztonsági jogkivonat, és adja hozzá a sablon a URI.

1. Ha úgy módosította az aktuális tárterület-fiók, aktuális tároló fiókot beállítani a sablonok tartalmazó egy.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

2. Hozzon létre egy Társítások token olvasási engedélyekkel rendelkező és egy lejárati idő hozzáférésének korlátozására. A sablon a biztonsági jogkivonat többek között a teljes URI beolvasásához.

        $templateuri = New-AzureStorageBlobSASToken -Container templates -Blob azuredeploy.json -Permission r -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

3. Telepítse a sablont, mert az URI, amely tartalmazza a biztonsági jogkivonat.

        New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri $templateuri

Példa csatolt sablonok használata a biztonsági jogkivonat olvassa el a [Azure erőforrás-kezelővel csatolt sablonok használata](resource-group-linked-templates.md)című témakört.

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="parameter-precedence"></a>Paraméter elsőbbségi sorrendjéről

Beágyazott paramétereket és a helyi paraméter fájl azonos telepítési művelet is használhatja. Például adja meg a helyi paraméter fájl bizonyos értékeket, és más értékek szövegközti felvétele a telepítés során. Ha a helyi paraméter fájl-és a beágyazott paraméter értékeket ad, a beágyazott érték elsőbbséget.

Beágyazott paraméterek azonban egy külső paraméter fájllal nem használhatók. Paraméter fájl a **TemplateParameterUri** paramétert ad meg, amikor a program figyelmen kívül hagyja összes beágyazott paramétert. Meg kell adnia a külső fájlban lévő összes paraméterértékeket. Ha a sablont, amely a paraméter fájlban nem lehet bizalmas értéket tartalmaz, ez az érték hozzáadása egy fő tárolóból elemre, és hivatkozást a fő tárolóból elemre a külső paraméter fájl vagy dinamikusan adja meg az összes paramétert értékek beágyazott.

Biztonságos értékeket adják át KeyVault hivatkozást használatával kapcsolatos részletekért lásd: [a telepítés során biztonságos értékeket adják át](resource-manager-keyvault-parameter.md).

## <a name="next-steps"></a>Következő lépések
- Példa üzembe helyezése a .NET-ügyfél tár keresztül erőforrások olvassa el a [Deploy erőforrások .NET-tárak és a sablon használatával](virtual-machines/virtual-machines-windows-csharp-template.md)című témakört.
- A paraméterek az sablont, olvassa el a [sablonok létrehozása](resource-group-authoring-templates.md#parameters).
- A megoldást üzembe helyezné a különböző környezetekben, olvassa el [fejlesztés és a próba-verziót tartalmazó környezetek Microsoft Azure-ban](solution-dev-test-environments.md).

