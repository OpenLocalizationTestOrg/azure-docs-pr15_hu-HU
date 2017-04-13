<properties
 pageTitle="Azure eszközökhöz (macOS 10.10) |} Microsoft Azure"
 description="MacOS Python és Azure parancssori kezelőfelületről Azure telepíthető."
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

# <a name="21-get-azure-tools-macos-1010"></a>2.1-es első Azure eszközök (macOS 10.10)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
- [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="211-what-you-will-do"></a>2.1.1 mit érhetek

Telepítse az Azure parancssori kezelőfelületről Azure. Ha bármilyen problémát eleget tesz, keresés, [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md)megoldások.

## <a name="212-what-you-will-learn"></a>2.1.2 mi ismerkedhet meg

- Hogyan telepítheti az Azure CLI.
- Hogyan lehet Azure CLI IoT részcsoport hozzáadni.

## <a name="213-what-you-need"></a>2.1.3 szükséges

- Mac internetkapcsolattal rendelkező számítógépről
- Azure active előfizetés. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/) .

## <a name="214-install-python"></a>2.1.4 Python telepítése

Bár a beépített Python 2.7 macOS megtalálható, ajánlott Python Homebrew keresztül telepítheti. Lásd: [A macOS telepítése Python](http://docs.python-guide.org/en/latest/starting/install/osx/).

Telepítse a Python és mezőpont a következő parancs futtatásával:

```bash
brew install python
```

## <a name="215-install-the-azure-cli"></a>2.1.5 telepítse az Azure CLI

Az Azure CLI többplatformos parancssori felületet nyújt az Azure, amely lehetővé teszi a kiépítése közvetlenül a parancssorból dolgozik, és a források kezelése. 

Telepítse a legújabb Azure CLI, kövesse az alábbi lépéseket:

1. A következő parancsokat Terminálszolgáltatások ablakban. Azure CLI telepítéséhez öt percig is eltarthat.

    ```bash
    pip install azure-cli-core==0.1.0b7 azure-cli-vm==0.1.0b7 azure-cli-storage==0.1.0b7 azure-cli-role==0.1.0b7 azure-cli-resource==0.1.0b7 azure-cli-profile==0.1.0b7 azure-cli-network==0.1.0b7 azure-cli-iot==0.1.0b7 azure-cli-feedback==0.1.0b7 azure-cli-configure==0.1.0b7 azure-cli-component==0.1.0b7 azure-cli==0.1.0b7
    ```

2. Ellenőrizze a telepítést a következő parancs futtatásával:

    ```bash
    az iot -h
    ```
  
Ha a telepítés sikeres látnia kell a következő eredményt.

![az iot -h](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_osx.png)

## <a name="215-summary"></a>2.1.5 összegzése

Azure CLI telepítette. Folytassa a következő szakaszban létrehozása az Azure IoT központi és eszköz személyazonosságát az Azure CLI használatával.

## <a name="next-steps"></a>Következő lépések

[2.2 létrehozása a IoT-központját, és regisztráljon a málna Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)
