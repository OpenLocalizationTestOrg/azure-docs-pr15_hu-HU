<properties
   pageTitle="Azure IoT protokoll átjáró |} Microsoft Azure"
   description="Azure IoT protokoll átjáró segítségével funkciók és Azure IoT központi protokoll támogatásának kiterjesztése ismerteti."
   services="iot-hub"
   documentationCenter=""
   authors="kdotchkoff"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-hub"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/23/2016"
   ms.author="kdotchko"/>

# <a name="supporting-additional-protocols-for-iot-hub"></a>További protokollt támogató IoT központi

Azure IoT központi natív módon támogatja a kommunikációt a MQTT AMQP és HTTP protokollon keresztül. Egyes esetekben az eszközök vagy a mező átjárók előfordulhat, hogy nem tud e szabványos protokollok egyikének használatára, és protokoll kiigazítás van szükség. Ebben az esetben egyéni az átjárók is használhatja. Egyéni az átjárók protokoll kiigazítás IoT központi végpontok engedélyezheti a forgalom IoT központból hidat. Is használhatja az [Azure IoT protokoll átjáró](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) egyéni az átjárók ahhoz, hogy az IoT központi protokoll kiigazítás.

## <a name="azure-iot-protocol-gateway"></a>Azure IoT protokoll átjáró

Az Azure IoT protokoll átjáró protokoll kiigazítás magas szintű, kétirányú eszközök közötti kommunikáció IoT központi készült kerete. A protocol átjáró összetevő átadó, amely egy adott protokollon keresztül elfogadja a eszköz kapcsolatok. Azt kulcsösszetevő IoT-hubon keresztül csatlakozott a forgalom AMQP 1.0 fölé. Az Azure IoT protokoll átjáró rugalmasságot biztosít a protokollok és a protokoll verziók számos támogatása hozzáadására szolgáló Megnyitás-forrás szoftver projektként érhető el.

Azure Cloud Services dolgozó szerepkörök használatával telepítheti az Azure protokoll átjáró erősen méretezhető útján. Ezeken kívül az protokoll átjáró a helyszíni környezetben, például mező átjárók elvégezhető.

Az Azure IoT protokoll átjáró tartalmaz egy MQTT protokoll listaelemre, lehetővé teszi, hogy a MQTT protokoll beállításainak testreszabása, ha szükséges. Mivel IoT központi beépített támogatást nyújt a MQTT v3.1.1 protokollt, csak érdemes a MQTT protokoll listaelemre, ha protokoll testreszabások szükség vagy az adott követelményeket további funkciók.

A MQTT kártya is bemutatja a programozási modell más protokollok protokoll kártya készítéséhez. Ezeken kívül az Azure IoT protokoll átjáró programozási modell lehetővé teszi a beépülő modul egyéni összetevők speciális feldolgozásra, például egyéni hitelesítési, üzenet átalakítások, a tömörítési és kibontási vagy titkosítás/visszafejtés forgalom az eszközök és IoT központi között.

Rugalmasság érdekében az átjáró Protocol (protokoll) és a MQTT végrehajtása találhatók Megnyitás-forrás szoftver projektben. Ez lehetővé teszi, hogy szükség szerint testre szabhatja a végrehajtását.

## <a name="next-steps"></a>Következő lépések

Az Azure IoT protokoll átjáró és használatáról, és azokat a IoT megoldás részeként kapcsolatos további tudnivalókért lásd:

* [GitHub Azure IoT protokoll átjáró tárházából](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md)
* [Azure IoT protokoll átjáró Fejlesztőeszközök útmutató](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/docs/DeveloperGuide.md)

A központi IoT üzembe megtervezésével kapcsolatos további információért lásd:

- [Esemény hubok összehasonlítása][lnk-compare]
- [Méretezés, a HA és a DR][lnk-scaling]

További feltárása a IoT központi funkcióinak, olvassa el:

- [Fejlesztőeszközök útmutató][lnk-devguide]
- [Egy eszközt a IoT átjáró SDK csomagjában talál, amelyek][lnk-gateway]

[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
