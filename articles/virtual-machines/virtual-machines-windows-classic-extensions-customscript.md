<properties
   pageTitle="Egyéni parancsfájl-bővítmény a egy Windows virtuális |} Microsoft Azure"
   description="Azure virtuális konfigurációs feladatok automatizálása PowerShell-parancsprogramokat futtassanak egy távoli Windows virtuális az egyéni parancsfájl bővítmény segítségével"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/06/2015"
   ms.author="kundanap"/>

# <a name="custom-script-extension-for-windows-virtual-machines"></a>A Windows virtuális gépeken futó bővítményének egyéni parancsfájl

Ez a cikk áttekintést nyújt az egyéni parancsfájl bővítmény használatáról a Windows VMs Azure PowerShell-parancsmagok az Azure szolgáltatás kiszolgálói API-ja használatával.

Virtuális gép (virtuális) bővítmények Microsoft beépített és a megbízható külső közzétevők, ha ki szeretné terjeszteni a virtuális funkcióit. Virtuális bővítmények áttekintése olvassa el a [Azure virtuális extensions és szolgáltatások](virtual-machines-windows-extensions-features.md)című témakört.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Megtudhatja, miként [végezheti el ezeket a lépéseket az erőforrás-kezelő modell használatával](virtual-machines-windows-extensions-customscript.md).

## <a name="custom-script-extension-overview"></a>Egyéni parancsfájl kiterjesztés – áttekintés

Az egyéni parancsfájl kiterjesztéssel a Windows futtathatja PowerShell-parancsfájlokat egy távoli virtuális rá bejelentkezés nélkül. A parancsfájlok futtathatók a virtuális kiépítése után, vagy a virtuális gép az életciklus során bármikor bármilyen további portokat megnyitása nélkül. A leggyakoribb használati eset egyéni parancsfájl bővítmény futó operációs rendszert futtató telepítése és konfigurálása a virtuális külön szoftver már kiépítve azt követően tartalmazzák.

### <a name="prerequisites-for-running-the-custom-script-extension"></a>Az egyéni parancsfájl bővítmény futtatásának előfeltételei

1. Telepítse az <a href="http://azure.microsoft.com/downloads" target="_blank">Azure PowerShell-parancsmagok</a> verzió 0.8.0 vagy újabb verziójában.
2. Ha azt szeretné, hogy a parancsfájlok futtatásának meg egy meglévő virtuális, feltétlenül virtuális ügynök engedélyezve van-e a virtuális. Ha nincs telepítve, kövesse az alábbi [lépéseket](virtual-machines-windows-classic-agents-and-extensions.md). Ha a virtuális létrehozása az Azure portálról, majd virtuális ügynök alapértelmezés szerint van telepítve.
3. Töltse fel a parancsfájlok, amely a virtuális Azure tárolóhoz futtatni szeretné. A parancsfájlok tároló egy vagy több tároló származnak.
4. A parancsprogram kell szerzője, úgy, hogy a bejegyzés parancsfájl, a bővítmény indítják, amelyek más parancsfájlok induljon.

## <a name="custom-script-extension-scenarios"></a>Egyéni parancsfájl bővítmény felhasználási területei

### <a name="upload-files-to-the-default-container"></a>Fájlok feltöltése a alapértelmezett tároló

A következő példa bemutatja, hogyan futtathatja a parancsfájlok a virtuális a tárterület-tárolóban az alapértelmezett fiók az előfizetése hagyják. A parancsfájlok ContainerName töltse fel. Az alapértelmezett tárterület-fiók használatával ellenőrizheti a **Get-AzureSubscription – alapértelmezett** parancsot.

Az alábbi példa létrehoz egy virtuális, de az ugyanolyan eseteket a egy meglévő virtuális is futtathatók.

    # Create a new VM in Azure.
    $vm = New-AzureVMConfig -Name $name -InstanceSize Small -ImageName $imagename
    $vm = Add-AzureProvisioningConfig -VM $vm -Windows -AdminUsername $username -Password $password
    // Add Custom Script extension to the VM. The container name refers to the storage container that contains the file.
    $vm = Set-AzureVMCustomScriptExtension -VM $vm -ContainerName $container -FileName 'start.ps1'
    New-AzureVM -ServiceName $servicename -Location $location -VMs $vm
    #  After the VM is created, the extension downloads the script from the storage location and executes it on the VM.

    # Viewing the  script execution output.
    $vm = Get-AzureVM -ServiceName $servicename -Name $name
    # Use the position of the extension in the output as index.
    $vm.ResourceExtensionStatusList[i].ExtensionSettingStatus.SubStatusList

### <a name="upload-files-to-a-non-default-storage-container"></a>Fájlok feltöltése az alapértelmezettől tároló tároló

Ebben az esetben használata ugyanabban az előfizetésben belül vagy egy másik előfizetést az alapértelmezettől tároló tároló parancsfájlok és fájlok feltöltése mutatja. Ebben a példában egy meglévő virtuális, de ugyanazokat a műveleteket tud végezni, miközben egy virtuális készít.

        Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileName 'file1.ps1','file2.ps1' -Run 'file.ps1' | Update-AzureVM

### <a name="upload-scripts-to-multiple-containers-across-different-storage-accounts"></a>Több tárolók át más-más tárolási fiókok parancsfájlok feltöltése

  Ha a parancsfájlok több tárolók között találhatók, kell adnia a teljes hozzáférést biztosít a megosztott aláírások (Társítások) URL-CÍMÉT a fájlokat a parancsfájlok futtatásának.

      Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileUri $fileUrl1, $fileUrl2 -Run 'file.ps1' | Update-AzureVM


### <a name="add-the-custom-script-extension-from-the-azure-portal"></a>Az egyéni parancsfájl-bővítmény hozzáadása az Azure portálról

Nyissa meg a virtuális az <a href="https://portal.azure.com/ " target="_blank">Azure-portálra</a> , és a bővítmény hozzáadása a futtatható parancsfájl megadásával.

  ![Adja meg a parancsprogram][5]


### <a name="uninstall-the-custom-script-extension"></a>Az egyéni parancsfájl-bővítmény eltávolítása

A virtuális a következő parancs használatával is eltávolítja az egyéni parancsfájl bővítmény.

      get-azureVM -ServiceName KPTRDemo -Name KPTRDemo | Set-AzureVMCustomScriptExtension -Uninstall | Update-AzureVM

### <a name="use-the-custom-script-extension-with-templates"></a>Az egyéni parancsfájl kiterjesztése a sablonok használata

Az egyéni parancsfájl bővítmény használatáról az erőforrás-kezelő Azure sablonokkal című témakörben talál [a Azure erőforrás-kezelő sablonok Windows VMs egyéni parancsfájl bővítményében](virtual-machines-windows-extensions-customscript.md).

<!--Image references-->
[5]: ./media/virtual-machines-windows-classic-extensions-customscript/addcse.png
