<properties
   pageTitle="Hozzon létre egy belső terheléselosztó sablon használatával az erőforrás-kezelő |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre egy belső terheléselosztó, az erőforrás-kezelő sablon használatával"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-using-a-template"></a>Hozzon létre egy belső terheléselosztó sablon használatával

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][Klasszikus telepítési modell](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>A sablon üzembe telepítéséhez kattintson használatával

Az űrlapsablonok érhető el a nyilvános adattárban az alapértelmezett értéket a fent leírt forgatókönyv létrehozásához használt tartalmazó paraméter fájlt használ. Ezzel a sablonnal üzembe, kattintson [erre a hivatkozásra](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer)kattintás használatával üzembe helyezéséhez kattintson a **központi telepítés az Azure**, szükség esetén az alapértelmezett paraméterértékeket cseréje, és kövesse az utasításokat a portálon.

## <a name="deploy-the-template-by-using-powershell"></a>A sablon üzembe PowerShell használatával

A PowerShell használatával letöltött sablont telepítéséhez kövesse az alábbi lépéseket.

1. Ha még sosem használt Azure PowerShell, megtudhatja, [hogy miként telepítése és beállítása Azure PowerShell](../../articles/powershell-install-configure.md) , és a képernyőn megjelenő utasításokat követve teljes mértékben a végén jelentkezzen be az Azure, és jelölje ki azt az előfizetést.
2. A paraméterek fájl letöltése a merevlemezre.
3. Szerkessze a fájlt, és mentse.
4. A sablon segítségével erőforrás csoport létrehozásához a **New-AzureRmResourceGroupDeployment** parancsmag futtatásával.

        New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
            -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'

## <a name="deploy-the-template-by-using-the-azure-cli"></a>A sablon telepítése az Azure CLI használatával

A sablon az Azure CLI telepítéséhez kövesse az alábbi lépéseket.

1. Ha még sosem használt Azure CLI, lásd: [Telepítse és állítsa be az Azure CLI](../../articles/xplat-cli-install.md) , és kövesse az utasításokat a pont, ahol be az Azure-fiók és az előfizetés felfelé.
2. Futtassa az erőforrás-kezelő mód, váltson a **azure konfiguráció mód** parancsot, alább látható módon.

        azure config mode arm

    Az alábbiakban a várt eredménye a fenti parancs:

        info:    New mode is arm

3. Nyissa meg a paraméterek fájlt, jelölje ki a tartalmát, és mentse a fájlt a számítógépen. Ebben a példában *parameters.json*a paraméterek fájlt menti azt.

4. Futtassa az új belső terheléselosztó telepítse a sablon és a paraméterek fájlokat töltötte le, és a fenti módosított segítségével **telepítési azure csoport létrehozása** parancsot. A kimenet után megjelenik a paramétereket ismerteti.

        azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json

## <a name="next-steps"></a>Következő lépések

[Betöltési terheléselosztó terjesztési mód forrás IP affinitás konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP időtúllépés beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)



