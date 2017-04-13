<properties
    pageTitle="Tudnivalók a Windows virtuális gépeken futó képe |} Microsoft Azure"
    description="Tudjon meg többet a képek használata a Windows virtuális gépeken futó Azure-ban."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/21/2016"
    ms.author="cynthn"/>

# <a name="about-images-for-windows-virtual-machines"></a>Tudnivalók a Windows virtuális gépeken futó képe

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-about-images](../../includes/virtual-machines-common-classic-about-images.md)]



## <a name="working-with-images"></a>A képek

A rendelkezésre álló képek kezelése Azure-előfizetéséhez a Azure PowerShell-modult is használhatja. Is használhatja az Azure klasszikus portál bizonyos kép feladatok, de a parancssorból lehetőségeket is biztosít a további.


-   **Az összes kép beolvasása**:`Get-AzureVMImage`elérhető az aktuális előfizetése összes képének listáját: a képek, valamint Azure és a partnerek által biztosított. Ez azt jelenti, hogy egy nagyméretű lista jelenhet meg. A következő példák bemutatják, hogyan egy rövidebb listát.
-   **Kép családok első**:`Get-AzureVMImage | select ImageFamily` kép családok listáját megjelenítő karakterláncok **ImageFamily** tulajdonság kap.
-   **Egy adott tartozó összes kép beolvasása**:`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`
-   **Virtuális képek keresése**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` a DataDiskConfiguration tulajdonság, amely a virtuális képek csak érvényes szűréssel működik. Ebben a példában is szűri a kimenet csak a címke és a kép nevét.
-   **Egy általános kép mentése**:`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`
-   **A speciális kép mentése**:`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`
>[Azure.Tip] A OSState paraméter szükség, ha létre szeretne hozni egy virtuális kép adatok lemezre, valamint az operációs rendszer lemezen tartalmazza. Ha a paraméter nem használja, a parancsmag-OS kép hoz létre. A paraméter értéke azt jelzi, hogy a kép általános vagy speciális alapján hogy az operációs rendszer lemezen készült felhasználáshoz.
-   **Kép törlése**:`Remove-AzureVMImage –ImageName "MyOldVmImage"`


## <a name="next-steps"></a>Következő lépések

Témakörök is segíthetnek [a klasszikus portálon Windows gép létrehozása](virtual-machines-windows-classic-tutorial.md)

