<properties
    pageTitle="A hostname (állomásnév) és az ssl-tanúsítvány MSDeploy használja webes alkalmazások terjesztése"
    description="Az erőforrás-kezelő Azure-sablon használatával webalkalmazást MSDeploy használ, és állítsa be az egyéni hostname (állomásnév) és az SSL-tanúsítvány"
    services="app-service\web"
    manager="erikre"
    documentationCenter=""
    authors="jodehavi"
    />

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="john.dehavilland"/>

# <a name="deploy-a-web-app-with-msdeploy-custom-hostname-and-ssl-certificate"></a>MSDeploy, egyéni hostname (állomásnév) és az SSL-tanúsítvány webes alkalmazások terjesztése

Ez az útmutató végpont telepítés létrehozása az Azure-webappokban, MSDeploy vizsgál, valamint egy egyéni hostname (állomásnév) és az SSL-tanúsítvány felvétele az ARM sablon végigvezeti.

Sablonok létrehozásával kapcsolatos további tudnivalókért lásd: [Microsoft Azure erőforrás-kezelő sablonok létrehozása](../resource-group-authoring-templates.md).

###<a name="create-sample-application"></a>Minta-alkalmazás létrehozása

ASP.NET-webalkalmazások program telepítésével. Első lépésként hozzon létre egy egyszerű webalkalmazást (vagy úgy lehetett dönt, hogy egy meglévőt használ – ez utóbbi esetben kihagyhatja ezt a lépést).

Nyissa meg a Visual Studio 2015, és válassza a fájl > új projektet. A megjelenő párbeszédpanelen válassza a webhely > ASP.NET webalkalmazást. Sablonok csoportban válassza a webhely, és válassza a MVC sablont. Jelölje ki a _hitelesítési típus módosítása_ _Nincs_hitelesítés. Ez a minta alkalmazására, egyszerűen legyen.

Ezen a ponton lehetősége van egy egyszerű ASP.Net web App alkalmazásban a telepítési folyamatának részeként használatára.

###<a name="create-msdeploy-package"></a>MSDeploy csomag létrehozása

Következő lépésként az Azure szeretne telepíteni, a web app-csomag létrehozása. Ehhez a projekt mentése, és futtassa a parancssorból a következő:

    msbuild yourwebapp.csproj /t:Package /p:PackageLocation="path\to\package.zip"

Ez hoz létre egy ZIP-csomag a PackageLocation mappában találja. Az alkalmazás készen áll a telepíthető, amely egy erőforrás-kezelő Azure sablont, tegye meg most készíthet.

###<a name="create-arm-template"></a>ARM-sablon létrehozása
Először lássuk, az egyszerű ARM sablonnal, amelyek a webalkalmazások és üzemeltetési terv (Megjegyzés paramétereket és a változók nem megjelenített fentebb) hoz létre.

    {
        "name": "[parameters('appServicePlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "apiVersion": "2014-06-01",
        "dependsOn": [ ],
        "tags": {
            "displayName": "appServicePlan"
        },
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "sku": "[parameters('appServicePlanSKU')]",
            "workerSize": "[parameters('appServicePlanWorkerSize')]",
            "numberOfWorkers": 1
        }
    },
    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        }
    }

Ezután meg kell módosítása a webes alkalmazás erőforrás beágyazott MSDeploy erőforrás állapotba. Ez lehetővé teszi a csomag korábban létrehozott, és közölje az Azure erőforrás-kezelővel MSDeploy telepítéshez használni a csomag az Azure Webappban ismertetése. Az alábbi példában látható az Microsoft.Web/sites erőforrás a beágyazott MSDeploy erőforrással:

    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        },
        "resources": [
            {
                "name": "MSDeploy",
                "type": "extensions",
                "location": "[resourceGroup().location]",
                "apiVersion": "2015-08-01",
                "dependsOn": [
                    "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
                ],
                "tags": {
                    "displayName": "webDeploy"
                },
                "properties": {
                    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]",
                    "dbType": "None",
                    "connectionString": "",
                    "setParameters": {
                        "IIS Web Application Name": "[variables('webAppName')]"
                    }
                }
            }
        ]
    }

Most már észre fogja, hogy a MSDeploy erőforrás megnyitja a **packageUri** tulajdonság, amely szerint az alábbi:

    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]"

A **packageUri** megnyitja mutat, a tárterület-fiókjába, feltöltődnek ahová tárterület-fiók uri a csomag zip szeretne. Az Azure erőforrás-kezelő program kihasználhatja a [Megosztott Access aláírások](../storage/storage-dotnet-shared-access-signature-part-1.md) lehúzható a csomag helyileg a tárterület-fiókból amikor rendszerbe állítják a sablont. Egy PowerShell parancsprogramot, amely a csomag feltöltése és hívja fel az Azure Management API a billentyűk létrehozásához szükséges, és át azokat a sablonba paraméterként (*_artifactsLocation* és *_artifactsLocationSasToken*) keresztül folyamathoz automatizált lesz. Meg kell paraméterek beállítása a mappát, és a csomag filename csoportban a tárhely tároló feltöltött.

A következő fel kell vennie egy másik beágyazott erőforrás a hostname (állomásnév) kötések használhatja az egyéni tartomány beállítása. Először győződjön meg arról, hogy Öné a hostname (állomásnév), és ellenőrzésre Azure úgy, hogy Ön a tulajdonosa - beállítása az [egyéni tartománynevet az Azure alkalmazás szolgáltatás konfigurálása](web-sites-custom-domain-name.md)című témakört. Miután befejeződött, a következő vehet Microsoft.Web/sites erőforrás csoportjában a sablon:

    {
        "apiVersion": "2015-08-01",
        "type": "hostNameBindings",
        "name": "www.yourcustomdomain.com",
        "dependsOn": [
            "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
        ],
        "properties": {
            "domainId": null,
            "hostNameType": "Verified",
            "siteName": "variables('webAppName')"
        }
    }

Végül, hogy fel kell vennie egy másik legfelső szintű erőforrás, Microsoft.Web/certificates. Ez az erőforrás fogja tartalmazni a SSL-tanúsítvány, és a web app és az Office csomagot, a tagolási szintre megtalálható lesz.

    {
        "name": "[parameters('certificateName')]",
        "apiVersion": "2014-04-01",
        "type": "Microsoft.Web/certificates",
        "location": "[resourceGroup().location]",
        "properties": {
            "pfxBlob": "pfx base64 blob",
            "password": "some pass"
        }
    }

Szüksége lesz érvényes SSL-tanúsítvány van annak érdekében, hogy az erőforrás beállítása. Ha befejezte az érvényes tanúsítványt akkor kell bontsa ki a pfx bájtok base64 karakterláncként. Egy kibontásához Ez azt javasoljuk, hogy az alábbi PowerShell-parancsot:

    $fileContentBytes = get-content 'C:\path\to\cert.pfx' -Encoding Byte

    [System.Convert]::ToBase64String($fileContentBytes) | Out-File 'pfx-bytes.txt'

Tudta majd a sikeres ez paraméterként a ARM telepítési sablonhoz.

Ezen a ponton a ARM sablon készen áll a.

###<a name="deploy-template"></a>Sablon terjesztése

A végleges szereplő lépések a darab ezzel együtt be egy teljes-végpont telepítés. Könnyebb telepítési is élvezheti az **Deploy-AzureResourceGroup.ps1** PowerShell-parancsprogramot, amely megjelenik a Visual Studióban, ami segítheti a feltöltés minden szükséges a sablonban eltérések a projektben Azure erőforráscsoport létrehozásakor. Szükséges, hogy időszakokra használni kívánt tároló számlát hozott létre. Ebben a példában a package.zip fel kell tölteni egy megosztott tárterület-fiók lehet létrehozni. A parancsprogram fog kihasználhatja AzCopy a csomag feltöltése a tárterület-fiókjába. Adja meg a eltérés mappában tartózkodási helyén, és a parancsfájl automatikusan töltse fel a összes fájl a címtáron belül az elnevezett tároló tárolóhoz. Deploy-AzureResourceGroup.ps1 hívása után, majd frissítenie kell az SSL kötések hozzárendelése az egyéni állomásnév az SSL-tanúsítványt.

Az alábbi PowerShell látható a teljes példányban, hívja fel a központi telepítés-AzureResourceGroup.ps1:

    #Set resource group name
    $rgName = "Name-of-resource-group"

    #call deploy-azureresourcegroup script to deploy web app

    .\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation "East US" `
                                    -ResourceGroupName $rgName `
                                    -UploadArtifacts `
                                    -StorageAccountName "name-of-storage-acct-for-package" `
                                    -StorageAccountResourceGroupName "resource-group-name-storage-acct" `
                                    -TemplateFile "web-app-deploy.json" `
                                    -TemplateParametersFile "web-app-deploy-parameters.json" `
                                    -ArtifactStagingDirectory "C:\path\to\packagefolder\"

    #update web app to bind ssl certificate to hostname. This has to be done after creation above.

    $cert = Get-PfxCertificate -FilePath C:\path\to\certificate.pfx

    $ar = Get-AzureRmResource -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -ApiVersion 2014-11-01

    $props = $ar.Properties

    $props.HostNameSslStates[2].'SslState' = 1
    $props.HostNameSslStates[2].'thumbprint' = $cert.Thumbprint
    $props.hostNameSslStates[2].'toUpdate' = $true

    Set-AzureRmResource -ApiVersion 2014-11-01 -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -PropertyObject $props

Ezen a ponton a alkalmazás kell számítógépekre telepített, és tallózással https://www.yourcustomdomain.com keresztül láthatja
