<properties
    pageTitle="Sablonok Azure egymást fedő PowerShell telepítése |} Microsoft Azure"
    description="Megtudhatja, hogy miként üzembe egy virtuális gép egy erőforrás-kezelő sablon és a PowerShell használatával."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-powershell"></a>A PowerShell használatá Azure Papírhalom sablonok telepítése

Erőforrás-kezelő Azure sablonok telepítse az Azure Papírhalom Ez a PowerShell használatával.  Erőforrás-kezelő sablonok üzembe helyezéséhez és az alkalmazás egyetlen, összehangolt művelet összes erőforrás kiépítése.

## <a name="run-azurerm-powershell-cmdlets"></a>Futtassa a AzureRM PowerShell-parancsmagok

Ebben a példában parancsfájl futtatásakor Azure Papírhalom Ez egy virtuális számítógépre telepíthető erőforrás-kezelő sablon használatával.  A folytatás előtt ellenőrizze, [telepítette és beállította a PowerShell](azure-stack-connect-powershell.md)  

A virtuális, ez a példa sablon használatban alapértelmezett piactér kép (WindowsServer-2012-R2-adatközponthoz).

1.  Lépjen a **101-egyszerű – a windows-virtuális** sablon <http://aka.ms/AzureStackGitHub>, keressen, és mentse a következő helyen található: c:\\sablonok\\azuredeploy-101-egyszerű – a windows-vm.json.

2.  A PowerShell futtassa az alábbi telepítési parancsfájlt. *Felhasználónév* és *jelszó* cserélje a felhasználónevét és jelszavát. A későbbi felhasználás növeli a üzembe felülírás elkerülése érdekében a *$myNum* paraméter értéke.

    ```PowerShell
        # Set Deployment Variables
        $myNum = "001" #Modify this per deployment
        $RGName = "myRG$myNum"
        $myLocation = "local"

        # Create Resource Group for Template Deployment
        New-AzureRmResourceGroup -Name $RGName -Location $myLocation

        # Deploy Simple IaaS Template
        New-AzureRmResourceGroupDeployment `
            -Name myDeployment$myNum `
            -ResourceGroupName $RGName `
            -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
            -NewStorageAccountName mystorage$myNum `
            -DnsNameForPublicIP mydns$myNum `
            -AdminUsername <username> `
            -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
            -VmName myVM$myNum `
            -WindowsOSVersion 2012-R2-Datacenter
    ```

3.  Nyissa meg az Azure Papírhalom portált, kattintson a **Tallózás gombra**, és kattintson a **virtuális gépeken futó**, keresse meg az új virtuális gép (*myDeployment001*).

## <a name="video-example-hybrid-virtual-machine-deployment"></a>Videó példa: a hibrid virtuális gép telepítés

[AZURE.VIDEO microsoft-azure-stack-tp1-poc-hybrid-vm-deployment]

## <a name="next-steps"></a>Következő lépések

[A Visual Studio sablonok telepítése](azure-stack-deploy-template-visual-studio.md)
