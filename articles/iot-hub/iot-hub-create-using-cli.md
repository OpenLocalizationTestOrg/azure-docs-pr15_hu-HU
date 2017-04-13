<properties
    pageTitle="Hozzon létre egy IoT hubhoz Azure CLI használatával |} Microsoft Azure"
    description="Kövesse az ebben a cikkben létrehozása az Azure parancssort IoT központi."
    services="iot-hub"
    documentationCenter=".net"
    authors="BeatriceOltean"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/21/2016"
     ms.author="boltean"/>

# <a name="create-an-iot-hub-using-azure-cli"></a>Hozzon létre egy IoT hubhoz Azure CLI használatával

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>– Bevezetés

Azure parancssor hozhat létre és kezelhet az Azure IoT hubok programozás útján is használhatja. Ez a cikk bemutatja, hogyan egy IoT központi létrehozása az Azure CLI használatával.

Oktatóprogram elvégzéséhez az alábbiakra van szükség:

- Azure active fiók. Ha nem rendelkeznek fiókkal, létrehozhat egy [ingyenes fiókot] [ lnk-free-trial] a mindössze néhány perc.
- [Azure CLI 0.10.4] [ lnk-CLI-install] vagy újabb verziójában. Azure CLI már a jelenlegi verzióval a parancssorban az alábbi paranccsal ellenőrizheti:
```
    azure --version
```

> [AZURE.NOTE] Azure magában az erőforrások létrehozásáról és használatáról a két különböző telepítési modellek: [Azure erőforrás-kezelő és klasszikus](../resource-manager-deployment-model.md). Az Azure CLI Azure erőforrás-kezelő módban kell lennie:
```
    azure config mode arm
```

## <a name="set-your-azure-account-and-subscription"></a>Az Azure-fiók és az előfizetés beállítása 

1. A következő parancsot írja be a parancssor bejelentkezéskor
```
    azure login
```
A javasolt webböngészőben és kód segítségével hitelesítést végezni.

2. Ha több Azure előfizetéssel rendelkezik, csatlakoztatása az Azure fog hozzáférést a hitelesítő adatok társított összes Azure-előfizetésben. Megtekintheti az Azure előfizetések, valamint melyiket, az alapértelmezett, a paranccsal
```
    azure account list 
```

Az előfizetés környezetben, amelyek alapján a többi parancsok használata használni kívánt beállítása

```
    azure account set <subscription name>
```

3. Ha nem rendelkezik egy erőforrás csoportot hozhat létre egy névvel ellátott **exampleResourceGroup** 
```
    azure group create -n exampleResourceGroup -l westus
```

> [AZURE.TIP] A cikk [az Azure CLI Azure erőforrások és erőforrás-csoportok kezelése használata] [ lnk-CLI-arm] Azure CLI használatáról az Azure erőforrások kezelésére további információt tartalmaz. 


## <a name="create-an-iot-hub"></a>Hozzon létre egy IoT központi

A paraméterek szükséges:

```
 azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>  
    - <resourceGroup> The resource group name (case insensitive alphanumeric, underscore and hyphen, 1-64 length)
    - <name> (The name of the IoT hub to be created. The format is case insensitive alphanumeric, underscore and hyphen, 3-50 length )
    - <location> (The location (azure region/datacenter) where the IoT hub will be provisioned.
    - <sku-name> (The name of the sku, one of: [F1, S1, S2, S3] etc. For the latest full list refer to the pricing page for IoT Hub.
    - <units> (The number of provisioned units. Range : F1 [1-1] : S1, S2 [1-200] : S3 [1-10]. IoT Hub units are based on your total message count and the number of devices you want to connect.)
```
Lásd: az összes paramétert elérhető elrejtésével automatizálhatja a Súgó parancsot a parancssor
```
    azure iothub create -h 
```
Gyors példa:

 Hozzon létre egy IoT központi nevű **exampleIoTHubName** az erőforrás csoport **exampleResourceGroup** egyszerűen futtassa a következő parancsot a
```
    azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [AZURE.NOTE] Ez a parancs Azure CLI egy S1 szabványos IoT központi számlázható, amelynek hoz létre. Törölheti a IoT központi **exampleIoTHubName** követően parancs használatával 
```
    azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
```


## <a name="next-steps"></a>Következő lépések
IoT központi fejlesztésével kapcsolatos további információért olvassa el az alábbiakat:
- [IoT központi SDK][lnk-sdks]

További feltárása a IoT központi funkcióinak, olvassa el:

- [Az Azure portál használatával IoT központi kezelése][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]: ../xplat-cli-install.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-CLI-arm]: ../xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
