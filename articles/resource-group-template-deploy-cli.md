<properties
   pageTitle="Azure CLI és a sablonok rendelkező erőforrások telepítése |} Microsoft Azure"
   description="Erőforrás-kezelő Azure és Azure CLI használja a telepítéshez használni egy erőforrás Azure. Az erőforrások vannak definiálva, az erőforrás-kezelő sablon"
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

# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Erőforrás-kezelő sablonok és Azure CLI erőforrások terjesztése

> [AZURE.SELECTOR]
- [A PowerShell](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portál](resource-group-template-deploy-portal.md)
- [REST API-VAL](resource-group-template-deploy-rest.md)

Ebből a témakörből megtudhatja, hogyan használhatja a Azure CLI az erőforrás-kezelő sablonok Azure szeretne telepíteni, az erőforrások.  

> [AZURE.TIP] A hiba hibakeresés a telepítés során című témakörben talál segítséget:
>
> - [Nézet üzembe helyezési műveleteket az Azure CLI](resource-manager-troubleshoot-deployments-cli.md) Ha többet szeretne tudni az első, amely segít a hiba elhárítása
> - [Gyakori hibák elhárítása az Azure erőforrás-kezelő Azure erőforrások telepítésekor](resource-manager-common-deployment-errors.md) módjáról a gyakori telepítési hibáinak elhárítása

A sablon lehet egy helyi fájl vagy URI keresztül elérhető külső fájlba. Ha a sablon a tárterület-fiókjában található, korlátozhatja a hozzáférést a sablont, és adja meg a megosztott aláírás (Társítások) jogkivonat a telepítés során.

## <a name="quick-steps-to-deployment"></a>A rövid lépéseket követve telepítési

Ez a cikk ismerteti a különböző telepítés közben használható beállítások. Azonban gyakran csak akkor van szükség két egyszerű parancsot. Gyorsan elsajátíthatja telepítési, az alábbi parancsokat használhatja:

    azure group create -n ExampleResourceGroup -l "West US"
    azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

Többet szeretne tudni a telepítéshez, előfordulhat, hogy jobban is megfelelnek a levelezés beállításai, folytassa a cikket elolvasva.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-azure-cli"></a>Az Azure CLI terjesztése

Ha, még nem használt Azure CLI az erőforrás-kezelő, lásd: [az Azure CLI for Mac, Linux, és a Windows Azure erőforrások kezelése és használata](xplat-cli-azure-resource-manager.md).

1. Jelentkezzen be az Azure-fiókjába. Adja meg a hitelesítő adatokat, miután a parancs a bejelentkezési eredményét adja vissza.

        azure login
  
        ...
        info:    login command OK

2. Ha több előfizetéssel rendelkezik, adja meg a telepítéshez használni kívánt előfizetés azonosítója.

        azure account set <YourSubscriptionNameOrId>

3. Erőforrás-kezelő Azure modul váltani. Értesítést kap az új mód.

        azure config mode arm
   
        info:     New mode is arm

4. Ha nem egy meglévő erőforráscsoport, erőforráscsoport létrehozása. Adja meg a nevét az erőforráscsoport és helyre van szüksége a megoldás. Meg kell adnia az erőforráscsoport helyét, mivel az erőforráscsoport tárolja a metaadatokat az erőforrásokról. Megfelelőségi okok miatt előfordulhat, hogy szeretne megadni, hogy a metaadatok tároló. Általánosságban elmondható azt javasoljuk, hogy meg egy helyet, akkor a legtöbb az erőforrások tároló. Használata ugyanazon a helyen egyszerűbbé teszi a sablont.

        azure group create -n ExampleResourceGroup -l "West US"

     Az új erőforráscsoport összefoglalását adja vissza.
   
        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Érvényesítse a telepítés előtt futtatnia kell az **azure csoport sablon érvényesítése** parancs futtatásával. Amikor a központi telepítés tesztelése, adja meg a paramétereket pontosan módon a (lásd a következő lépés) telepítésben végrehajtásakor.

        azure group template validate -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup

5. Az erőforráscsoport erőforrások telepítéséhez futtassa a következő parancsot, és adja meg a szükséges paramétereket. A paraméterek egy nevet a telepítéshez, a az erőforráscsoport, az elérési út vagy URL-CÍMÉT a sablont, és a forgatókönyvet szükséges paramétereket tartalmazza. 
   
     Az alábbi három lehetőség közül választhat paraméterértékeket nyújtó van: 

     1. Beágyazott paramétereket és a helyi sablon használata Mindegyik paraméter formátumban: `"ParameterName": { "value": "ParameterValue" }`. A következő példa bemutatja a paramétereket, escape-karakterek vannak.

            azure group deployment create -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     2. Használja a beágyazott paramétereket és a sablon mutató hivatkozást.

            azure group deployment create --template-uri <LinkToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     3. Használjon paraméter fájlt. A sablonfájlt információt [paraméter-fájl](#parameter-file)megtekintéséhez.
    
            azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

     Után az erőforrások számítógépekre telepített keresztül, a fenti módszerek egyikét, látni fogja a telepítési összefoglalását.
  
        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        ...
        info:    group deployment create command OK

     Egy teljes telepítési futtatásához **kész** **mód** beállításához. Ügyeljen arra, hogy ezt a módot használatakor, mint információforrások, amelyek nem találhatók meg a sablon véletlenül törölheti.

        azure group deployment create --mode Complete -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

6. Ha szeretne, amely bármely telepítési hibáinak elhárítása, **a beállítás hibakeresési** paraméter használatával segíthet üzembe helyezésével kapcsolatos további információk naplózása. Megadhatja, hogy kérelem tartalmat vagy válasz tartalom telepítésének művelettel bejelentkezni.

        azure group deployment create --debug-setting All -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

## <a name="deploy-template-from-storage-with-sas-token"></a>A biztonsági jogkivonat tárhelyről sablon terjesztése

A sablonok hozzáadása egy tárterület-fiók és a biztonsági jogkivonat, a telepítés során hivatkozása őket.

> [AZURE.IMPORTANT] Az alábbi lépéseket követve a sablont tartalmazó blob elérhető, csak a fióktulajdonos. A biztonsági jogkivonat a blob-létrehozásakor a blob viszont bárki megnyithatja az adott URI. Ha egy másik felhasználó az URI elfogja, a felhasználó hozzáférhet a sablon lesz. A biztonsági jogkivonat használata a sablonok való hozzáférés korlátozása a jó módszer, de nem terjedhet bizalmas adatokat, például jelszavak közvetlenül a sablonban.

### <a name="add-private-template-to-storage-account"></a>Személyes sablon hozzáadása a tárterület-fiókhoz

Fiók beállítása a tárterület a sablonok között az alábbi lépéseket:

1. Hozzon létre egy erőforrás csoportot.

        azure group create -n "ManageGroup" -l "westus"

2. Tárterület-fiók létrehozása. A tároló fióknév kell egyedi Azure keresztül, a úgy adja meg a saját nevét a fiókhoz.

        azure storage account create -g ManageGroup -l "westus" --sku-name LRS --kind Storage storagecontosotemplates

3. Állítsa a tárhely és a kulcs változói.

        export AZURE_STORAGE_ACCOUNT=storagecontosotemplates
        export AZURE_STORAGE_ACCESS_KEY={storage_account_key}

4. Hozzon létre egy tároló. A jogosultsági **ki az** azt jelenti, hogy a tároló csak a tulajdonos érhető el, amelyek értéke.

        azure storage container create --container templates -p Off 
        
4. A sablon hozzáadása a tároló.

        azure storage blob upload --container templates -f c:\Azure\Templates\azuredeploy.json
        
### <a name="provide-sas-token-during-deployment"></a>Adja meg a biztonsági jogkivonat a telepítés során

Személyes sablon tárterület-fiókjában üzembe helyezéséhez beolvasni a biztonsági jogkivonat, és adja hozzá a sablon a URI.

1. Hozzon létre egy Társítások token olvasási engedélyekkel rendelkező és egy lejárati idő hozzáférésének korlátozására. Állítsa a lejárati idő elég időt a telepítés befejezéséhez. A sablon a biztonsági jogkivonat többek között a teljes URI beolvasásához.

        expiretime=$(date -I'minutes' --date "+30 minutes")
        fullurl=$(azure storage blob sas create --container templates --blob azuredeploy.json --permissions r --expiry $expiretimetime --json  | jq ".url")

2. Telepítse a sablont, mert az URI, amely tartalmazza a biztonsági jogkivonat.

        azure group deployment create --template-uri $fullurl -g ExampleResourceGroup

Példa csatolt sablonok használata a biztonsági jogkivonat olvassa el a [Azure erőforrás-kezelővel csatolt sablonok használata](resource-group-linked-templates.md)című témakört.

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Következő lépések
- Példa üzembe helyezése a .NET-ügyfél tár keresztül erőforrások olvassa el a [Deploy erőforrások .NET-tárak és a sablon használatával](virtual-machines/virtual-machines-windows-csharp-template.md)című témakört.
- A paraméterek az sablont, olvassa el a [sablonok létrehozása](resource-group-authoring-templates.md#parameters).
- A megoldást üzembe helyezné a különböző környezetekben, olvassa el [fejlesztés és a próba-verziót tartalmazó környezetek Microsoft Azure-ban](solution-dev-test-environments.md).
- Biztonságos értékeket adják át KeyVault hivatkozást használatával kapcsolatos részletekért lásd: [a telepítés során biztonságos értékeket adják át](resource-manager-keyvault-parameter.md).

