<properties
    pageTitle="Hozzon létre egy IoT hubhoz egy erőforrás-kezelő Azure-sablon és a PowerShell használatával |} Microsoft Azure"
    description="Kövesse az ebben az oktatóanyagban Ismerkedés az Azure erőforrás-kezelő sablonok használatával hogyan hozhat létre egy IoT központi PowerShell."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/07/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-powershell"></a>Hozzon létre egy IoT hubhoz PowerShell használatával

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>– Bevezetés

Erőforrás-kezelő Azure hozhat létre és kezelhet az Azure IoT hubok programozás útján is használhatja. Ebben az oktatóanyagban megtudhatja, hogyan hozzon létre egy IoT központi PowerShell egy erőforrás-kezelő Azure-sablon segítségével.

> [AZURE.NOTE] Azure magában az erőforrások létrehozásáról és használatáról a két különböző telepítési modellek: [Azure erőforrás-kezelő és klasszikus](../resource-manager-deployment-model.md).  Ez a cikk bemutatja, hogy az erőforrás-kezelő Azure telepítési modellt használja.

Oktatóprogram elvégzéséhez az alábbiakra van szükség:

- Azure active fiók. <br/>Ha nem rendelkeznek fiókkal, létrehozhat egy [ingyenes fiókot] [ lnk-free-trial] a mindössze néhány perc.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] vagy újabb verziójában.

> [AZURE.TIP] A cikk [Az Azure erőforrás-kezelő Azure-PowerShell használatá] [ lnk-powershell-arm] PowerShell-parancsfájlokat és Azure erőforrás-kezelő sablonok használatáról létrehozása az Azure erőforrások további információt tartalmaz. 

## <a name="connect-to-your-azure-subscription"></a>Csatlakozás az Azure előfizetés

Egy PowerShell a parancssorba írja be a jelentkezzen be az Azure-előfizetéséhez a következő parancsot:

```
Login-AzureRmAccount
```

Az alábbi parancsokat használhatja fel, ahol telepítheti egy IoT központi és a jelenleg támogatott API-verzió:

```
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

Hozzon létre egy erőforrás csoportot tartalmaz, az alábbi paranccsal az egyik IoT központi támogatott helyét a IoT-központját. Ez a példa egy erőforrás **MyIoTRG1**nevű csoportot, hozza létre:

```
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Egy erőforrás-kezelő Azure sablont hozhat létre egy IoT központi elküldése

Hozzon létre egy IoT központi az erőforráscsoport a JSON sablon segítségével. Az erőforrás-kezelő Azure-sablon segítségével egy meglévő IoT hubhoz végezze el a módosításokat.

1. Egy egyszerű szövegszerkesztő programban segítségével nevű **template.json** az alábbi erőforrás-definícióról való hozzon létre egy új szabványos IoT hubon keresztül csatlakozott az erőforrás-kezelő Azure sablon létrehozása. Ebben a példában a IoT központi felveszi a **Kelet-USA -beli** tartományban lévő két fogyasztói csoportok (**cg1** és **cg2**) hoz létre az esemény központi-kompatibilis végpontot és **2016-02-03** API verzióját használja. Ezzel a sablonnal vár, hogy a IoT központi neve nevű **hubName**paraméterként adják át. Az aktuális listáját, amelyek támogatják a IoT központi helyen című [Azure állapot][lnk-status].

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg1')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg2')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

2. Az erőforrás-kezelő Azure sablonfájl a helyi számítógépre mentheti. Ez a példa feltételezi, hogy **c:\templates**nevű mappába menti.

3. A következő parancsot a új IoT-központban nevét a IoT központi átadása paraméterként telepítése. Ebben a példában az a IoT központi neve **abcmyiothub** (figyelje meg, hogy ez nevének egyedinek kell lennie globálisan, a nevét vagy monogramját tartalmaznia kell):

    ```
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```

4. A kimenet az IoT-központban létrehozott billentyűparancsok jeleníti meg.

5. Ellenőrizheti, hogy az alkalmazás az [Azure portálon] megtalálhatók az új IoT-központban hozzáadott[ lnk-azure-portal] és erőforrások, illetve a **Get-AzureRmResource** PowerShell-parancsmag használatával listájának megtekintése.

> [AZURE.NOTE] Példa alkalmazás hozzáadása egy S1 szabványos IoT központi, amelynek számlázható. Törölheti a IoT hubon keresztül az [Azure portál] [ lnk-azure-portal] , vagy ha befejezte a **Eltávolítása-AzureRmResource** PowerShell-parancsmag használatával.

## <a name="next-steps"></a>Következő lépések

Miután telepítette az Azure erőforrás-kezelő sablonnal PowerShell IoT központi, érdemes további feltárása:

- További információ: a [IoT központi erőforrás szolgáltató REST API -t]a funkcióinak[lnk-rest-api].
- Olvassa el az [Azure erőforrás szolgáltatásának áttekintése] [ lnk-azure-rm-overview] Ha többet szeretne tudni az Azure erőforrás-kezelő lehetőségeit.

IoT központi fejlesztésével kapcsolatos további információért olvassa el az alábbiakat:

- [C SDK – bevezetés][lnk-c-sdk]
- [IoT központi SDK][lnk-sdks]

További feltárása a IoT központi funkcióinak, olvassa el:

- [Egy eszközt a IoT átjáró SDK csomagjában talál, amelyek][lnk-gateway]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: ../powershell-install-configure.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../powershell-azure-resource-manager.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
