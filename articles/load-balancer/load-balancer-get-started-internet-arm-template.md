<properties
   pageTitle="Hozzon létre egy internetes terheléselosztó az erőforrás-kezelő sablon használatával |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre egy internetes terheléselosztó az erőforrás-kezelő sablon használatával"
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

# <a name="creating-an-internet-facing-load-balancer-using-a-template"></a>Szemben lévő sablon használatával terheléselosztó internetes létrehozása

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Ez a cikk bemutatja az erőforrás-kezelő telepítési modell. Azt is [megtudhatja, hogy miként hozhat létre egy internetes terheléselosztó klasszikus telepítési modell használata](load-balancer-get-started-internet-classic-portal.md)


[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>A sablon üzembe telepítéséhez kattintson használatával

Az űrlapsablonok érhető el a nyilvános adattárban az alapértelmezett értéket a fent leírt forgatókönyv létrehozásához használt tartalmazó paraméter fájlt használ. Ezzel a sablonnal üzembe, kattintson [erre a hivatkozásra](http://go.microsoft.com/fwlink/?LinkId=544801)kattintás használatával üzembe helyezéséhez kattintson a **központi telepítés az Azure**, szükség esetén az alapértelmezett paraméterértékeket cseréje, és kövesse az utasításokat a portálon.

## <a name="deploy-the-template-by-using-powershell"></a>A sablon üzembe PowerShell használatával

A PowerShell használatával letöltött sablont telepítéséhez kövesse az alábbi lépéseket.

1. Ha még sosem használt Azure PowerShell, megtudhatja, [hogy miként telepítése és beállítása Azure PowerShell](../../articles/powershell-install-configure.md) , és a képernyőn megjelenő utasításokat követve teljes mértékben a végén jelentkezzen be az Azure, és jelölje ki azt az előfizetést.

2. A sablon segítségével erőforrás csoport létrehozásához a **New-AzureRmResourceGroupDeployment** parancsmag futtatásával.

        New-AzureRmResourceGroupDeployment -Name TestRG -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.parameters.json'

## <a name="deploy-the-template-by-using-the-azure-cli"></a>A sablon telepítése az Azure CLI használatával

A sablon az Azure CLI telepítéséhez kövesse az alábbi lépéseket.

1. Ha még sosem használt Azure CLI, lásd: [Telepítse és állítsa be az Azure CLI](../../articles/xplat-cli-install.md) , és kövesse az utasításokat a pont, ahol be az Azure-fiók és az előfizetés felfelé.
2. Futtassa az erőforrás-kezelő mód, váltson a **azure konfiguráció mód** parancsot, alább látható módon.

        azure config mode arm

    Az alábbiakban a várt eredménye a fenti parancs:

        info:    New mode is arm

3. Nyissa meg a böngészőjében keresse meg [A quickstart útmutató sablont](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules), a json-fájl tartalmát másolhatja, és illessze be egy új fájlt a számítógépre. Ebben az esetben a, szeretné másolni, alá eső értékek **c:\lb\azuredeploy.parameters.json**nevű fájlban.
4. Futtassa **telepítési azure csoport létrehozása** az új terheléselosztó telepítse a sablon és a paraméterek fájlokat töltötte le, és a fenti módosított segítségével. A kimenet után megjelenik a paramétereket ismerteti.

        azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' -e 'c:\lb\azuredeploy.parameters.json'

## <a name="next-steps"></a>Következő lépések

[Első lépések egy belső terheléselosztó konfigurálása](load-balancer-get-started-ilb-arm-ps.md)

[A betöltés terheléselosztó terjesztési mód konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP időtúllépés beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)
