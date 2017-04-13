<properties
    pageTitle="Az Azure erőforráscsoport projekt telepítési parancsfájlt áttekintése |} Microsoft Azure"
    description="Ismerteti, hogyan működik a az PowerShell parancsprogramot az Azure erőforráscsoport telepítési projektben."
    services="visual-studio-online"
    documentationCenter="na"
    authors="tfitzmac"
    manager="timlt"
    editor="" />

 <tags
    ms.service="azure-resource-manager"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/26/2016"
    ms.author="tomfitz" />

# <a name="overview-of-the-azure-resource-group-project-deployment-script"></a>Az Azure erőforráscsoport projekt telepítési parancsfájlt áttekintése

Azure erőforráscsoport telepítési projektek súgó szakaszra, és az Azure fájlok és egyéb eltérések telepítése. A Visual Studio egy erőforrás-kezelő Azure telepítési projektet hoz létre, egy PowerShell-parancsprogramot **Deploy-AzureResourceGroup.ps1** nevű nem adja hozzá a projekthez. Ez a témakör a Mit jelent ez a parancsfájl, és hogyan belül és kívül Visual Studio hajtható végre.

## <a name="what-does-the-script-do"></a>Mire szolgál a parancsfájl?

A központi telepítés-AzureResourceGroup.ps1 parancsfájl két dolog, amit a telepítési munkafolyamat fontos tartalmaz.

- Töltse fel a fájlokat vagy a sablon telepítéshez szükséges eltérések
- A sablon terjesztése

A parancsfájl első részét feltölti a fájlokat és a telepítéshez eltéréseket, és az utolsó parancsmagot a parancsfájl ténylegesen üzembe a sablont. Például ha egy virtuális gép parancsfájl kell beállítani, a telepítési parancsfájlt először biztonságosan feltölti a konfigurációs parancsfájl Azure tárterület-fiókkal. Ez számára elérhetővé teszi azt az Azure erőforrás-kezelő a virtuális gép konfigurációs kiépítési során.

Nem az összes sablont telepítések további eltéréseket, hogy fel kell tölteni szükség van, mert *uploadArtifacts* nevű kapcsoló paraméter kiértékeli. Ha bármelyik eltérések kell fel kell tölteni, adja meg a *uploadArtifacts* kapcsolót a parancsfájl hívásakor. Fontos tudni, hogy a fő sablonfájl és a paraméterek fájl nem tölthető fel. Csak más fájlok konfigurációs parancsfájlok, például beágyazott telepítési sablonokat, és alkalmazások fájljai kell fel kell tölteni.

## <a name="detailed-script-description"></a>Részletes parancsfájl leírása

Az alábbiakban a központi telepítés-AzureResourceGroup.ps1 Azure PowerShell parancsprogramot ne milyen választó szakaszainak leírását.

>[AZURE.NOTE] Ez a 1.0 verziójú a központi telepítés-AzureResourceGroup.ps1 parancsfájl mutatja be.

1.  Erőforrás-kezelő Azure környezetben Project szükséges paramétereket az elemeket rekordként. Egyes paraméterek az alapértelmezett értékeket, ha a projekt létrehozásának beállított van. Ezek a parancsfájl az alapértelmezett értékek módosítása, vagy másik paraméterértékeket felvételéhez, mielőtt a, hajtsa végre a parancsfájlt.

    ```
    Param(
      [string] [Parameter(Mandatory=$true)] $ResourceGroupLocation,
      [string] $ResourceGroupName = 'AzureResourceGroup1',
      [switch] $UploadArtifacts,
      [string] $StorageAccountName,
      [string] $StorageAccountResourceGroupName,
      [string] $StorageContainerName = $ResourceGroupName.ToLowerInvariant() + '-stageartifacts',
      [string] $TemplateFile = '..\Templates\azuredeploy.json',
      [string] $TemplateParametersFile = '..\Templates\azuredeploy.parameters.json',
      [string] $ArtifactStagingDirectory = '..\bin\Debug\staging',
      [string] $AzCopyPath = '..\Tools\AzCopy.exe',
      [string] $DSCSourceFolder = '..\DSC'
    )
    ```

  	|Paraméter|Leírás|
  	|---|---|
  	|$ResourceGroupLocation|Adatok és a régió középre igazítása az erőforráscsoport, például a **Nyugati USA -beli** vagy **kelet-ázsiai**helyét.|
  	|$ResourceGroupName|Az Azure erőforráscsoport neve.|
  	|$UploadArtifacts|Bináris érték, amely jelzi, hogy az eltéréseket szüksége van-e fel kell tölteni Azure a rendszer.|
  	|$StorageAccountName|Hol van feltöltve. az eltérések a Azure tárterület-fiók neve.|
  	|$StorageAccountResourceGroupName|Az Azure erőforráscsoport, amely tartalmazza a tárterület-fiók neve.|
  	|$StorageContainerName|A feltölthető eltérések használt tárterület tároló neve.|
  	|$TemplateFile|A telepítési fájl elérési útját (`<app name>.json`) a Azure erőforráscsoport projektben.|
  	|$TemplateParametersFile|A fájl elérési útját a paraméterek (`<app name>.parameters.json`) a Azure erőforráscsoport projektben.|
  	|$ArtifactStagingDirectory|A rendszer hol eltérések vannak helyileg töltött fel, beleértve a PowerShell parancsprogramot legfelső szintű mappa elérési útja. Az elérési út lehet abszolút viszonyított vagy a parancsprogram-helyet.|
  	|$AzCopyPath|Hol a AzCopy.exe eszköz a .zip fájlokat másol, többek között a PowerShell parancsprogramot legfelső szintű mappa elérési útját. Az elérési út lehet abszolút viszonyított vagy a parancsprogram-helyet.|
  	|$DSCSourceFolder|A mappa elérési útját DSC (állapot konfiguráció szükséges) forrás, beleértve a PowerShell parancsprogramot gyökérmappa. Az elérési út lehet abszolút viszonyított vagy a parancsprogram-helyet. Lásd [az Azure PowerShell DSC (kívánt állam konfigurációja) bővítmény bemutatása](http://blogs.msdn.com/b/powershell/archive/2014/08/07/introducing-the-azure-powershell-dsc-desired-state-configuration-extension.aspx), szükség esetén további információt.|

1.  Ellenőrizze, hogy az eltéréseket szüksége van-e Azure fel kell tölteni. Ha nem, akkor ugorjon a 11. Egyéb esetben hajtsa végre az alábbi lépéseket.

1.  A relatív elérési utak változói átalakítása abszolút elérési út. Módosítsa az elérési például például `..\Tools\AzCopy.exe` való `C:\YourFolder\Tools\AzCopy.exe`. Ezenkívül inicializálni *ArtifactsLocationName* és *ArtifactsLocationSasTokenName* NULL változók. *ArtifactsLocation* és *SaSToken* lehetséges, hogy a sablon paramétert. Értékük null után a paraméterek fájlban olvasási, a parancsfájl őket létrehoz értékeket.

    Az Azure-eszközöket használja a paraméter értékek *_artifactsLocation* és *_artifactsLocationSasToken* a sablonban eltérések kezeléséhez. Ha a PowerShell-parancsprogramot megkeresi azokat a neveket a paramétereket, de nem rendelkeznek a paraméterértékeket, a parancsfájl feltöltések megjelenítése az eltéréseket, és azokat a paramétereket megfelelő értékeket adja vissza. Azt átadja őket a via parancsmag `@OptionsParameters`.

  	|Változó|Leírás|
  	|---|---|
  	|ArtifactsLocationName|Hol találhatók az Azure eltérések elérési útja.|
  	|ArtifactsLocationSasTokenName|Társítások (megosztott Access-aláírás) jogkivonat neve szolgáltatás Bus hitelesíteni a parancsfájl által használt. [A megosztott Access aláírás hitelesítési szolgáltatás Bus](service-bus-shared-access-signature-authentication.md) talál további információt.|

    ```
    if ($UploadArtifacts) {
    # Convert relative paths to absolute paths if needed
    $AzCopyPath = [System.IO.Path]::Combine($PSScriptRoot, $AzCopyPath)
    $ArtifactStagingDirectory = [System.IO.Path]::Combine($PSScriptRoot, $ArtifactStagingDirectory)
    $DSCSourceFolder = [System.IO.Path]::Combine($PSScriptRoot, $DSCSourceFolder)

    Set-Variable ArtifactsLocationName '_artifactsLocation' -Option ReadOnly
    Set-Variable ArtifactsLocationSasTokenName '_artifactsLocationSasToken' -Option ReadOnly

    $OptionalParameters.Add($ArtifactsLocationName, $null)
    $OptionalParameters.Add($ArtifactsLocationSasTokenName, $null)
    ```

1.  A ellenőrzések e szakasz a <app name>. parameters.json fájlrészt (a "paraméterek fájl") van elnevezett **Paraméterek** szülő csomópont (a a `else` blokk). Ellenkező esetben nem szülő csomópont rendelkezik. Bármelyik formátumhoz elfogadható.
    
    ```
    if ($JsonParameters -eq $null) {
            $JsonParameters = $JsonContent
        }
        else {
            $JsonParameters = $JsonContent.parameters
        }
    ```

1.  Végigléptetés JSON-paraméterek gyűjteménye. Ha a paraméter értéke *_artifactsLocation* vagy *_artifactsLocationSasToken*van rendelve, majd állítsa a változó *$OptionalParameters* ezeket az értékeket tartalmazó. Ez megakadályozhatja, hogy a parancsprogram véletlenül felülírása kiszámlázásakor paraméterértékeket.

    ```
    $JsonParameters | Get-Member -Type NoteProperty | ForEach-Object {
        $ParameterValue = $JsonParameters | Select-Object -ExpandProperty $_.Name

        if ($_.Name -eq $ArtifactsLocationName -or $_.Name -eq $ArtifactsLocationSasTokenName) {
            $OptionalParameters[$_.Name] = $ParameterValue.value
        }
    }
    ```

1.  Szerezze be a fiókkulcs tárhely és a környezet a tárhely fiók erőforrás a telepítéshez eltérések tárolására szolgál.

    ```
    $StorageAccountKey = (Get-AzureRMStorageAccountKey -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Key1

    $StorageAccountContext = (Get-AzureRmStorageAccount -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Context
    ```

1.  Ha PowerShell DSC virtuális gép beállítása használata esetén a DSC meghosszabbításához el szeretné helyezni a egyetlen zip-fájl a eltérések. Igen hozzon létre egy .zip archív mappák a DSC konfiguráció. Művelet, ellenőrizze, hogy ha $DSCSourceFolder szerepel-e. Ha a DSC konfiguráció létezik, távolítsa el azt, és hozza létre dsc.zip nevű új tömörített fájlt.

    ```
    # Create DSC configuration archive
    if (Test-Path $DSCSourceFolder) {
    Add-Type -Assembly System.IO.Compression.FileSystem
        $ArchiveFile = Join-Path $ArtifactStagingDirectory "dsc.zip"
        Remove-Item -Path $ArchiveFile -ErrorAction SilentlyContinue
        [System.IO.Compression.ZipFile]::CreateFromDirectory($DSCSourceFolder, $ArchiveFile)
    }
    ```

1.  Nincs elérési út az Azure eltérések a paraméterek fájlt, amennyiben beállítása a PowerShell parancsprogramot eltérések feltöltésekor használandó elérési útját. Ehhez hozzon létre egy mappa elérési útját a tárterület-fiók végpont elérési utat és a tárhely tároló nevével kombinációi. Az új elérési út majd frissítse a paraméterek fájlt.

    ```
    # Generate the value for artifacts location if it is not provided in the parameter file
    $ArtifactsLocation = $OptionalParameters[$ArtifactsLocationName]
    if ($ArtifactsLocation -eq $null) {
        $ArtifactsLocation = $StorageAccountContext.BlobEndPoint + $StorageContainerName
        $OptionalParameters[$ArtifactsLocationName] = $ArtifactsLocation
    }
    ```

1.  A segédprogrammal **AzCopy** (a **eszközök** pontban a Azure erőforráscsoport telepítési projekt része) fájlok másolása a helyi tároló legördülő elérési út az online tárhely Azure-fiókjába. Ha ez a lépés nem sikerül, lépjen ki a parancsfájl óta a telepítési valószínűleg nem szükséges az eltéréseket nélkül sikertelen.

    ```
    # Use AzCopy to copy files from the local storage drop path to the storage account container
    & $AzCopyPath """$ArtifactStagingDirectory""", $ArtifactsLocation, "/DestKey:$StorageAccountKey", "/S", "/Y", "/Z:$env:LocalAppData\Microsoft\Azure\AzCopy\$ResourceGroupName"
    if ($LASTEXITCODE -ne 0) { return }
    ```

1.  Ha egy Társítások jogkivonat eltérések helyének nem adja meg a paraméterek fájlban, hozzon létre egyet az online tárhelyek tároló ideiglenes olvasási hozzáférést biztosít. Ezután adják át az adott biztonsági jogkivonat, megjelenítheti a parancssor, egy "optionalParameter." Figyelje meg, hogy a parancssor bármely átadott elsőbbséget élveznek a paraméterek fájl a megadott értékeket.

    ```
    # Generate the value for artifacts location SAS token if it is not provided in the parameter file
    $ArtifactsLocationSasToken = $OptionalParameters[$ArtifactsLocationSasTokenName]
    if ($ArtifactsLocationSasToken -eq $null) {
       # Create a SAS token for the storage container - this gives temporary read-only access to the container
       $ArtifactsLocationSasToken = New-AzureStorageContainerSASToken -Container $StorageContainerName -Context $StorageAccountContext -Permission r -ExpiryTime (Get-Date).AddHours(4)
       $ArtifactsLocationSasToken = ConvertTo-SecureString $ArtifactsLocationSasToken -AsPlainText -Force
       $OptionalParameters[$ArtifactsLocationSasTokenName] = $ArtifactsLocationSasToken
    }
    ```

1.  Az erőforráscsoport létrehozása, ha még nem léteznek és jelölje be a sablon és a paraméterek fájlt, amelyek megakadályozzák a sikeres az üzembe érvényesítési hibákat.

    ```
    # Create or update the resource group using the specified template file and template parameters file
    New-AzureRMResourceGroup -Name $ResourceGroupName -Location $ResourceGroupLocation -Verbose -Force -ErrorAction Stop

    Test-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFile -TemplateParameterFile $TemplateParametersFile @OptionalParameters -ErrorAction Stop
    ```

1. Végül telepítse a sablont. Ez a kód létrehoz egy egyedi nevet a telepítéshez, az időbélyegző használatával.

    ```
    New-AzureRMResourceGroupDeployment -Name ((Get-ChildItem $TemplateFile).BaseName + '-' + ((Get-Date).ToUniversalTime()).ToString('MMdd-HHmm')) `
        -ResourceGroupName $ResourceGroupName `
        -TemplateFile $TemplateFile `
        -TemplateParameterFile $TemplateParametersFile `
        @OptionalParameters `
        -Force -Verbose
    ```

## <a name="deploy-the-resource-group"></a>Az erőforráscsoport terjesztése

### <a name="to-deploy-the-resource-group-in-visual-studio"></a>Az erőforráscsoport a Visual Studióban terjesztése

1. Válassza a helyi menü a Azure erőforráscsoport projekt **Deploy** > **Új telepítési**.

    ![][0]

1. **Az erőforráscsoport Deploy** párbeszédpanelen vagy telepítéséhez, vagy válassza ki a **legördülő listájában válassza a meglévő erőforráscsoport &lt;hozzon létre új... &gt;** Új erőforráscsoport létrehozásához.

    ![][1]

1. Ha a rendszer kéri, adja meg az erőforrás-csoport nevét és helyét a **Erőforráscsoport létrehozása** párbeszédpanelen, és kattintson a **Létrehozás** gombra.

    ![][2]

1. A **Paraméterek szerkesztése** párbeszédpanel megjelenítése, és írja a hiányzó paraméterértékeket a **Szerkesztése paramétereinek** gombot választva.

    ![][3]

    >[AZURE.NOTE] A szükséges paramétereket értékekre van szüksége, ha ez a párbeszédpanel automatikusan megjelenik, amikor rendszerbe.

    ![][4]

1. Ha végzett paraméterértékek megadása, kattintson a **Mentés** gombra, és kattintson a **központi telepítés** gombra.

    A telepítési parancsfájlt (Deploy-AzureResourceGroup.ps1) fut, és a sablon bármely eltéréseket, valamint az Azure üzembe helyezése.

### <a name="to-deploy-the-resource-group-by-using-powershell"></a>Az erőforráscsoport üzembe PowerShell használatával

Ha szeretne futtatása nélkül, a Visual Studio üzembe parancs és a felhasználói felület, a helyi menü a parancsfájl, válassza a **Megnyitás PowerShell ISE**.

![][5]


## <a name="command-deployment-examples"></a>Parancs telepítési példák

### <a name="deploy-using-default-values"></a>Alapértelmezett értékek használata terjesztése

Ez a példa bemutatja, hogyan futtassa a az alapértelmezett paraméter értékeivel parancsfájlt. (A hely paraméter nincs alapértelmezett értékét, mert meg kell adnia egy.)

`.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus`

### <a name="deploy-overriding-the-default-values"></a>Az alapértelmezett értékeket felülbírálása terjesztése

Ez a példa bemutatja, hogyan futtassa a sablon és a paraméterek fájlokat, amelyek eltérnek az alapértelmezett értékeket telepítése.

```
.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus –TemplateFile ..\templates\AnotherTemplate.json –TemplateParametersFile ..\templates\AnotherTemplate.parameters.json
```

### <a name="deploy-using-uploadartifacts-for-staging"></a>Átmeneti UploadArtifacts verzióval terjesztése

Ez a példa bemutatja, hogyan futtassa a töltse fel az eltérések a Megjelenés mappából, és nem alapértelmezett sablonok telepítése.

```
.\Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory ..\bin\release\staging
```

Ez a példa bemutatja, hogyan futtassa a Visual Studio online Powershellhez Azure tevékenység.

```
$(Build.StagingDirectory)/AzureResourceGroup1/Scripts/Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory $(Build.StagingDirectory)
```

## <a name="next-steps"></a>Következő lépések
További tudnivalók a Azure erőforrás-kezelő [Azure erőforrás szolgáltatásának áttekintése](azure-resource-manager/resource-group-overview.md)elolvasásával.

További példák az Azure erőforráscsoport projektek használata című témakörben talál [Deploy és Azure erőforrások](https://github.com/Microsoft/HealthClinic.biz/wiki/Deploy-and-manage-Azure-resources) a [bemutató](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/) [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 csatlakozás. További QuickStarts HealthClinic.biz bemutatója a csomagban a [Azure fejlesztői eszközökkel QuickStarts csomagban](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts)című témakör tartalmaz.

[0]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy1c.png
[1]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy2bc.png
[2]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy3bc.png
[3]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy4bc.png
[4]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy5c.png
[5]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy6c.png
