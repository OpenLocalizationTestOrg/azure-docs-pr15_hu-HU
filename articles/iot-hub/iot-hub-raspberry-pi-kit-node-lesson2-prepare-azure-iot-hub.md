<properties
 pageTitle="A IoT központi létrehozni és a málna Pi 3 regisztrálni |} Microsoft Azure"
 description="Hozzon létre egy erőforrás csoportot, és létrehozása az Azure IoT központi regisztrálhatja a Pi az Azure IoT-központban, az Azure CLI használatával."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="22-create-your-iot-hub-and-register-your-raspberry-pi-3"></a>2.2 létrehozása a IoT-központját, és regisztráljon a málna Pi 3

## <a name="221-what-you-will-do"></a>2.2.1 mit érhetek

- Hozzon létre egy erőforrás csoportot.
- Az erőforráscsoport létrehozása az Azure IoT központi.
- Az Azure IoT-központban, az Azure CLI használatával adja hozzá a málna Pi 3.

A Pi hozzáadása a IoT központi használatakor az Azure CLI a szolgáltatás kulcsot hoz létre egy az a Pi a szolgáltatással hitelesítést végezni. Ha bármilyen problémát eleget tesz, keresés, [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md)megoldások.

## <a name="222-what-you-will-learn"></a>2.2.2 mi ismerkedhet meg

- Hogyan használhatók az Azure CLI létrehozása az Azure IoT központi.
- Hogyan lehet hozzon létre egy eszköz identitás a Pi az Azure-IoT központban.

## <a name="223-what-you-need"></a>2.2.3 szükséges

- Azure-fiók
- A Mac- és az Azure CLI telepítve a Windows rendszerű számítógépről

## <a name="224-create-your-azure-iot-hub"></a>2.2.4 létrehozása az Azure IoT központi

Azure IoT központi segít csatlakozhat, figyelésére és IoT eszközök milliónyi kezelése. Az Azure IoT központ létrehozásához kövesse az alábbi lépéseket:

1. Jelentkezzen be az Azure-fiók a következő parancs futtatásával:

    ```bash
    az login
    ```

    Az összes rendelkezésre álló Azure előfizetések egy sikeres bejelentkezés után jelennek meg.

2. Az alapértelmezett, amely a következő parancs futtatásával használni kívánt Azure előfizetés beállítása:

    ```bash
    az account set -n {subscription id or name}
    ```

    Az előfizetés azonosítója vagy a név megtalálható a kimenő `az login`.

3. A szolgáltató rögzítése a következő parancs futtatásával:

    ```bash
    az resource provider register -n "Microsoft.Devices"
    ```

    A szolgáltató regisztrálnia kell, mielőtt telepítheti az Azure erőforrás, amely a szolgáltató.

    > [AZURE.NOTE] Legtöbb szolgáltató által az Azure-portálra vagy az Azure CLI esetén automatikusan regisztrált, de nem az összes olyan. A szolgáltató kapcsolatos további tudnivalókért olvassa el a [Gyakori Azure-telepítési hibák elhárítása a Azure-kezelő eszközzel](../resource-manager-common-deployment-errors.md) című témakört.

4. A nyugati USA-beli tartományban lévő iot minta neve a következő parancs futtatásával erőforráscsoport létrehozása:

    ```bash
    az resource group create --name iot-sample --location westus
    ```

5. Hozzon létre egy IoT központi iot kétmintás erőforráscsoport a következő parancs futtatásával:

    ```bash
    az iot hub create --name {my hub name} --resource-group iot-sample
    ```

    A létrehozott IoT hubhoz alapértelmezett kiadását érték **F1**, amely **ingyenes**. További tudnivalókért olvassa el a [Azure IoT központi árak](https://azure.microsoft.com/pricing/details/iot-hub/)című témakört.

> [AZURE.NOTE] A IoT központi neve globálisan egyedinek kell lennie.
>
> Azure IoT központi csak egy **F1** kiadását az Azure előfizetés a hozhat létre.

## <a name="225-register-your-pi-in-your-iot-hub"></a>2.2.5 a Pi regisztrálhatja a IoT-központban

Egyes eszközök, amelyek az Azure IoT központi/érkező üzenetek küldése/fogadása regisztrálva van egy egyedi azonosítót.

A Pi regisztrálása az Azure IoT központi futtatása a következő parancsot:

```bash
az iot device create --device-id myraspberrypi --hub {my hub name} --resource-group iot-sample
```

## <a name="226-summary"></a>2.2.6 összegzése

Már létrehozott egy Azure IoT hubhoz, és a Pi regisztrált egy eszköz identitással az Azure IoT-központban. Készen áll áthelyezése a következő lecke című témakörben olvashat a Pi üzeneteket küldeni a IoT-központját.

## <a name="next-steps"></a>Következő lépések

Most már készen megkezdi a lecke 3-as Kiindulás [Azure függvény alkalmazás és a folyamat és IoT központi üzenetek tárolásához Azure tároló fiók létrehozása](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).