<properties
 pageTitle="Az eszköz beállítása |} Microsoft Azure"
 description="Állítsa be az első alkalommal használatra a málna Pi 3, és telepítse a Raspbian-OS, a szabad operációs rendszert, amely a Pi málna hardver van optimalizálva."
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

# <a name="11-configure-your-device"></a>1.1-es, állítsa be az eszközt

## <a name="111-what-you-will-do"></a>1.1.1 mit érhetek

Állítsa be az első alkalommal használatra a Pi, és telepítse az ingyenes operációs rendszerű málna Pi hardver optimalizált Raspbian operációs rendszerben. Ha bármilyen problémát eleget tesz, keresés, [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md)megoldások.

## <a name="112-what-you-will-learn"></a>1.1.2 mi ismerkedhet meg

Ebben a szakaszban megismerheti:

- A Pi Raspbian telepítése
- Indulás a Pi egy USB-kábel használatával hogyan
- A Pi keresztüli a egy Ethernet-kábelt vagy vezeték nélküli hálózathoz
- Hogyan vehet fel egy LED a breadboard, és csatlakoztassa a Pi

## <a name="113-what-you-need"></a>1.1.3 szükséges

Ez a szakasz befejezéséhez a következő részekből a málna Pi 3 Starter Kit kell:

- A 3-as Pi málna fal
- A 16 GB-os MicroSD kártya
- Az 5 v 2A power megad hat ragadós micro USB-kábel
- A breadboard
- Összekötő kábelek
- 560 Ohm ellenállás
- Egy szórt 10mm LED
- Az Ethernet-kábelt

![Szempontok a Starter csomagban](media/iot-hub-raspberry-pi-lessons/lesson1/starter_kit.jpg)

Is szükség van:

- A Pi való csatlakozáshoz vezetékes vagy vezeték nélküli kapcsolat
- Egy USB-Ft kártya vagy mini-Ft kártya írni az operációs rendszer képe a MicroSD kártya be.
- A Windows, Mac vagy Linux futtató számítógép. A számítógép Raspbian telepítése a MicroSD kártya szolgál.
- A szükséges eszközök és a szoftver letöltése internetkapcsolat

## <a name="114-install-raspbian-on-the-microsd-card"></a>1.1.4 Raspbian telepítése a MicroSD kártya

Felkészülés a MicroSD kártya a Raspbian képre írni.

1. Töltse le a Raspbian.
  1. [Töltse le](https://www.raspberrypi.org/downloads/raspbian/) a Raspbian Jessie képpont a zip-fájl.
  2. Bontsa ki a Raspbian kép egy mappába a számítógépén.
2. Telepítse a MicroSD kártya Raspbian.
  1. [Töltse le](https://www.etcher.io) és telepítse a Etcher Ft kártya író segédprogramot.
  2. Etcher futtatni, és válassza ki a kibontott Raspbian kép lépés: 1.
  3. Jelölje ki a MicroSD kártya meghajtót.
    Megjegyzés: Etcher előfordulhat, hogy már ki van választva a megfelelő meghajtó.
  4. Kattintson a Flash Raspbian telepítése a MicroSD kártyára.
  5. A MicroSD kártya eltávolítása a számítógépről, Miután befejeződött.
    Megjegyzés: Célszerű közvetlenül eltávolítása a MicroSD kártya, mert Etcher automatikusan kiadása vagy a MicroSD kártya befejeztével leválasztja biztonságos.
  6. A Pi a MicroSD kártya beszúrni.

![A Ft kártya beszúrása](media/iot-hub-raspberry-pi-lessons/lesson1/insert_sdcard.jpg)

## <a name="115-power-on-your-pi"></a>1.1.5 a Pi bekapcsolás

A Pi micro USB-kábel és a power ellátási bekapcsolás.

![A Power](media/iot-hub-raspberry-pi-lessons/lesson1/micro_usb_power_on.jpg)

> [AZURE.NOTE] Fontos, hogy a power ellátási használja a csomagban, amely legalább 2A kattintva győződjön meg arról, hogy a málna elég power megfelelően működjön a dokumentum.

## <a name="116-connect-your-raspberry-pi-3-to-the-network"></a>1.1.6 a málna Pi 3 csatlakozzon a hálózathoz

A Pi lehet csatlakozni, egy vezetékes vagy vezeték nélküli hálózathoz. Győződjön meg arról, hogy a Pi ugyanabba a hálózatba, mint a számítógép csatlakozik-e. Ha például csatlakozhat a Pi az azonos kapcsoló, amely a számítógép csatlakozik.

### <a name="1161-connect-to-a-wired-network"></a>1.1.6.1 Csatlakozás vezetékes hálózatot használ

Csatlakozhat az Ethernet-kábelt a Pi a vezetékes hálózatot használ. A két LED a Pi a kapcsolja be, ha a kapcsolat.

![Csatlakoztassa a kábel használatával](media/iot-hub-raspberry-pi-lessons/lesson1/connect_ethernet.jpg)

### <a name="1162-connect-to-a-wireless-network"></a>1.1.6.2 Csatlakozás vezeték nélküli hálózathoz

A Pi csatlakozni a vezeték nélküli hálózat a málna Pi Foundation végezze el az [utasításokat](https://www.raspberrypi.org/learning/software-guide/wifi/) . Ezeket az utasításokat szükség az, hogy először csatlakozzon a Pi monitor és a billentyűzet.

## <a name="117-connect-the-led-to-your-pi"></a>1.1.7 a Pi csatlakozás a LED

A feladat végrehajtásához használja a [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), az összekötő kábelek, a LED és a ellenállás. Csatlakoztat a az [Általános célú bemeneti és kimeneti](https://www.raspberrypi.org/documentation/usage/gpio/) (GPIO) portokat a pi matematikai. 

![Breadboard LED és ellenállás](media/iot-hub-raspberry-pi-lessons/lesson1/breadboard_led_resistor.jpg)

1. Csatlakozás a LED, a dolgozók rövidebb láb **GPIO GND (PIN-kód 6)**.
2. A hosszabb láb a LED a csatlakozás a ellenállás az egyik láb.
3. Csatlakozás a ellenállás a többi láb **GPIO 4 (PIN-kód 7)**.

Ne feledje, hogy a LED polaritás fontos. A polaritás beállítás aktív alacsony gyakran nevezik.

![Átalakítókábelre](media/iot-hub-raspberry-pi-lessons/lesson1/pinout_breadboard.png)

Congratulation! Sikeresen beállította a Pi.

## <a name="118-summary"></a>1.1.8 összegzése

Ebben a részben megtanulta már beállításáról a Pi Raspbian telepítése, a Pi a hálózatokhoz történő csatlakozás és a Pi LED csatlakozás. Látható, hogy a LED még nem bekapcsolható. A következő szakaszban telepíteni a szükséges eszközök és előkészítése a minta alkalmazást futtató a Pi a szoftvert.

![Készen áll a hardver](media/iot-hub-raspberry-pi-lessons/lesson1/hardware_ready.jpg)

## <a name="next-steps"></a>Következő lépések

[Az 1.2 beszerzése a eszközök](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
