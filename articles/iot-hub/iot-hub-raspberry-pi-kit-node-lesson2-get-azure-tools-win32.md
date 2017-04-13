<properties
 pageTitle="Azure eszközökhöz (Windows 7 +) |} Microsoft Azure"
 description="Python és Azure parancssori kezelőfelületről Azure telepíthető Windows 7 vagy újabb verziók."
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

# <a name="21-get-azure-tools-windows-7-"></a>2.1 első Azure eszközök (Windows 7 +)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
- [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="211-what-you-will-do"></a>2.1.1 mit érhetek

Telepítés Python és az Azure parancssori felület (Azure CLI). Ha bármilyen problémát eleget tesz, keresés, [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md)megoldások.

## <a name="212-what-you-will-learn"></a>2.1.2 mi ismerkedhet meg

- Hogyan lehet telepíteni Python.
- Hogyan telepítheti az Azure CLI.

## <a name="213-what-you-need"></a>2.1.3 szükséges

- Internetkapcsolattal rendelkező Windows rendszerű számítógépről
- Azure active előfizetés. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/) .

## <a name="214-install-python"></a>2.1.4 Python telepítése

Python telepítse a Windows rendszerű számítógépén. Python 2.7, 3.4 vagy 3.5-ös telepítheti. Ebben az oktatóanyagban Python 2.7 alapul. Ha már telepítette az Python, Ugrás a szakasz 2.1.5.

[Python letöltése a Windows rendszerhez](https://www.python.org/downloads/)

Adja hozzá az elérési útját a mappák, ahol python.exe és pip.exe telepítve van a rendszer szükséges `PATH` környezeti. Alapértelmezés szerint a python.exe telepítve van a `C:\Python27` és pip.exe telepítve van a `C:\Python27\Scripts`.

## <a name="215-install-the-azure-cli"></a>2.1.5 telepítse az Azure CLI

Az Azure CLI többplatformos parancssori felületet nyújt az Azure, amely lehetővé teszi a kiépítése közvetlenül a parancssorból dolgozik, és a források kezelése.

Azure CLI telepítéséhez kövesse az alábbi lépéseket:

1. Nyisson meg egy parancssorablakot rendszergazdaként.
2. Futtassa az alábbi parancsokat:

    ```bash
    pip install azure-cli-core==0.1.0b7 azure-cli-vm==0.1.0b7 azure-cli-storage==0.1.0b7 azure-cli-role==0.1.0b7 azure-cli-resource==0.1.0b7 azure-cli-profile==0.1.0b7 azure-cli-network==0.1.0b7 azure-cli-iot==0.1.0b7 azure-cli-feedback==0.1.0b7 azure-cli-configure==0.1.0b7 azure-cli-component==0.1.0b7 azure-cli==0.1.0b7
    ```
3. Ellenőrizze a telepítést, a következő parancs futtatásával:

    ```bash
    az iot -h
    ```

Ha a telepítés sikeres megjelenik a következő eredményt.

![az iot -h](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_win.png)

## <a name="216-summary"></a>2.1.6 összegzése

Azure CLI telepítette. Folytassa a következő szakaszban létrehozása az Azure IoT központi és eszköz személyazonosságát az Azure CLI használatával.

## <a name="next-steps"></a>Következő lépések

[2.2 létrehozása a IoT-központját, és regisztráljon a málna Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)
