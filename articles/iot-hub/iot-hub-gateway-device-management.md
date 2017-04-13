<properties
 pageTitle="Felügyelt eszközök mögött egy IoT átjáró engedélyezése |} Microsoft Azure"
 description="A létrehozott IoT átjárón útmutatást a témakör a IoT átjáró SDK együtt IoT központi kezeli eszközök használatával."
 services="iot-hub"
 documentationCenter=""
 authors="chipalost"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="cstreet"/>
 
# <a name="enable-managed-devices-behind-an-iot-gateway"></a>Felügyelt eszközök mögött egy IoT átjáró engedélyezése

## <a name="basic-device-isolation"></a>Egyszerű eszközre elkülönítési

Szervezet gyakran segítségével IoT átjárók általános biztonságosabbá IoT megoldásukat. Egyes eszközök el kell küldenie adatok a felhőben, de nem képes szembeni védelmével kapcsolatban: saját maguk veszélyek az interneten. A külső szálak ezekre az eszközökre úgy, hogy őket a külső világ átjárón keresztül kommunikálni is szolgálja.

Az átjáró a közötti biztonságos környezet, és nyissa meg az internet a szegély helyezkedik el. Az átjáró beszélgetni eszközök, és az átjáró mentén, az üzenetek továbbítja a megfelelő felhő cél. Az átjáró megerősített külső szálak ellen, ezzel az illetéktelen kérések akadályozza, lehetővé teszi, hogy a meghatalmazott kötött a forgalmat, és továbbítja a megfelelő eszköz kötött a hálózati forgalmat.

![][1]

Eszközök mögött egy biztonsági hozzáadott réteget az átjárók védelme maguk is elhelyezheti. Ebben az esetben csak szeretne rögzíteni az átjárót a legújabb biztonsági helyett minden eszközön az operációs rendszer frissítése ellen javítással OS.

## <a name="isolation-plus-intelligence"></a>Elkülönítési plusz intelligenciával kapcsolatos funkciók

Egy megerősített útválasztó nem elegendő az átjárók egyszerűen elkülönítése eszközök. Azonban IoT megoldások gyakran van szükség, hogy az átjáró nyújt további üzletiintelligencia-nál egyszerűen elkülönítése eszközök. Például érdemes kezelheti a a felhőből. Biztosan tudja használható [LWM2M](https://github.com/OpenMobileAlliance/OMA_LwM2M_for_Developers/wiki), egy szabványos eszköz protokollnak, a felhőbeli adatkezelési részét a megoldást. Azonban eszközök küldése egy nem TCP/IP protokollt használó telemetriai engedélyezett Protocol (protokoll) Továbbá eszközök konzerv sok adatot, és csak azokat a töltse fel a telemetriai szűrt részhalmazát szeretné. Készíthet ezzel a paranccsal beépíti egy alkalmas az adatok két különböző adatfolyamok foglalkozó IoT átjáró megoldás. Az átjáró feladatokat kell végrehajtania:

-   A **Telemetriai**megértéséhez, szűrheti azokat, és töltse fel a felhőben, az átjáró keresztül. Az átjáró már nem egy egyszerű útválasztó, amely egyszerűen továbbítja az adatokat az eszköz és a felhő között.

-   Egyszerűen exchange **LWM2M eszköz adatkezelés** az eszközök és a felhő között. Az átjáró nem szükséges, a mikrofonba adatok megértéséhez, és győződjön meg arról, hogy az adatok kap átadását eszközök és a felhő között csak kell.

Az alábbi ábra ebben az esetben:

![][2]

## <a name="the-solution-azure-iot-device-management-and-the-iot-gateway-sdk"></a>A megoldást: Azure IoT mobileszköz-kezelés és a IoT átjáró SDK 

A nyilvános előzetes verziójának megjelenése után a [mobileszköz-kezelés Azure IoT] [ lnk-device-management] és az [Azure IoT átjáró SDK] bétaverziójával engedélyezése ebben az esetben. Az átjáró kezeli az egyes adatfolyam adatok az alábbi képlettel történik:

-   **Telemetriai**: a IoT átjáró SDK használatával generál egy folyamat, amely értelmez, szűrők és telemetriai adatokat küld a felhőben. A IoT átjáró SDK csomagjában talál, amely a folyamat a fejlesztői nevében részei kódot tartalmaz. További információt a architektúra SDK a [IoT átjáró SDK – első lépések] a található[ lnk-gateway-get-started] oktatóanyag.

-   **Mobileszköz-kezelés**: Azure mobileszköz-kezelés itt egy LWM2M ügyfél az eszközt, valamint a felhőben felhasználói felülete kezeléséhez szükséges parancsokat kiadására az eszközön futó.
    
    Minden olyan speciális logikát átjáró nincs szükség, mert azt nem szükséges, az eszköz és a IoT központi között cserélni LWM2M adatfeldolgozás. Internetkapcsolat megosztása, sok modern operációs rendszerek, ahhoz, hogy a LWM2M adatcsere átjáró szolgáltatás engedélyezése Mivel a IoT átjáró SDK támogató operációs rendszerek számos ebben az esetben alkalmas operációs rendszerének adhatja meg. Az alábbiakban internetkapcsolat megosztása a [Windows 10-es] és [Ubuntu], két a sok támogatott operációs rendszerek engedélyezése lépéseit.

Az alábbi ábrán látható a magas szintű architektúrájának engedélyezése ebben az esetben [Azure IoT mobileszköz-kezelés] [ lnk-device-management] és az [Azure IoT átjáró SDK csomagjában talál].

![][3]

## <a name="next-steps"></a>Következő lépések

Használatáról a IoT átjáró SDK kapcsolatos további tudnivalókért lásd: az alábbi oktatóanyagok:

- [IoT átjáró SDK - Linux használatának első lépései][lnk-gateway-get-started]
- [IoT átjáró SDK – használatával Linux szimulált eszközön eszköz a felhőbe az üzenetek küldése][lnk-gateway-simulated]

További feltárása a IoT központi funkcióinak, olvassa el:

- [Fejlesztőeszközök útmutató][lnk-devguide]
- [Egy eszközt a IoT átjáró SDK csomagjában talál, amelyek][lnk-gateway-simulated]

<!-- Images and links -->
[1]: media/iot-hub-gateway-device-management/overview.png
[2]: media/iot-hub-gateway-device-management/manage.png
[Azure IoT átjáró SDK]: https://github.com/Azure/azure-iot-gateway-sdk/
[Windows 10-ben]: http://windows.microsoft.com/en-us/windows/using-internet-connection-sharing#1TC=windows-7
[Ubuntu]: https://help.ubuntu.com/community/Internet/ConnectionSharing
[3]: media/iot-hub-gateway-device-management/manage_2.png
[lnk-gateway-get-started]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-gateway-simulated]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-device-management]: iot-hub-device-management-overview.md

[lnk-devguide]: iot-hub-devguide.md