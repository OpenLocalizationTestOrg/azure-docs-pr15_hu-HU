<properties
 pageTitle="Fejlesztőeszközök útmutató témakörök IoT központi |} Microsoft Azure"
 description="Azure IoT központi Fejlesztőeszközök útmutatóban használt IoT központi végpontok, biztonsági, eszköz identitás beállításjegyzék, kezelés és üzenetküldés"
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
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="azure-iot-hub-developer-guide"></a>Azure IoT központi Fejlesztőeszközök útmutató

Azure IoT központi, amely segít teljes körű felügyelt szolgáltatás engedélyezése a megbízható és nem biztonságos kétirányú kommunikációt milliónyi IoT eszközök között, és a Befejezés vissza az alkalmazás.

Azure IoT központi biztosít:

* Biztonságos kommunikáció használatával eszköz hitelesítő adatokat, és a hozzáférés-vezérlés.
* Megbízható eszköz a felhőbe és a felhő-eszköz hyper nagyméretű üzenetek.
* A leggyakrabban használt nyelvek és platformokon eszköz tárak egyszerűen eszköz kapcsolatokkal.

Fejlesztőeszközök IoT központi útmutatóban tartalmazza az alábbi cikkekben:

- [Üzenetek küldése és fogadása a IoT központi] [ devguide-messaging] az üzenetek azon funkcióit ismerteti (eszköz a felhőbe és felhőbeli-eszköz) IoT központi közzététele. A cikk is üzenetformátumok, például témakörök információt és a kommunikáció támogatott protokollok és a portszámokat használják.
- [Tölthet fel fájlokat a eszközről] [ devguide-upload] ismerteti, hogyan feltölthet fájlokat a eszközről. A cikk is elküldheti a uplaod folyamat például az értesítéseket témakörök információt.
- [Eszköz identitások IoT-központban kezelése] [ devguide-identities] ismerteti, hogy mit információ minden egyes IoT központi eszköz identitás beállításjegyzék tárolja, és hogyan lehet hozzáférni, és módosítsa.
- [IoT központi való hozzáférés szabályozása] [ devguide-security] a biztonsági modellben használva hozzáférést IoT központi funkciók mindkét eszközök és a felhő összetevők ismerteti. A cikk tokenek és X.509 tanúsítványok és az engedélyeket adhat az részleteit használatával kapcsolatos információt tartalmaz.
- [Állapot és a beállítások szinkronizálása az eszköz twins segítségével] [ devguide-device-twins] *eszköz kettős* fogalma és a funkciókat, például az eszköz szinkronizálása a kettős közzététele ismerteti. A cikk ismerteti az egy eszköz kettős tárolt adatok.
- [Közvetlen metódus eszközön] [ devguide-directmethods] közvetlen módszer, olvashat meghívása módszerek a háttéradatbázist alkalmazásból eszközön, és kezelheti a direkt módszer az eszközön életciklusának ismerteti.
- [Több eszközön a feladatok ütemezése] [ devguide-jobs] ismerteti, hogyan ütemezhet feladatok több eszközön. A cikk ismerteti, hogyan küldhetik feladatokat, hajtsa végre a műveleteket, mint egy használata egy devcie kettős devcie frissítése közvetlen metódus végrehajtása. Azt is megtudhatja, hogy miként lekérdezésére a feladat állapotát.
- [Hivatkozás - IoT központi végpontok] [ devguide-endpoints] ismerteti a különböző végpontok, amely az egyes IoT központi közzététele futtatókörnyezet és kezelés műveletekhez. A cikk is ismerteti, hogyan használhatja a mező az átjárók ahhoz, hogy egyes eszközök esetén a IoT központi végpontok csatlakozni.
- [Hivatkozás - lekérdezési nyelv twins, módszerek és feladatok] [ devguide-query] ismerteti, hogy, amely lehetővé teszi az adatok kinyerése a központi a eszköz twins és a feladatok kapcsolatos lekérdezési nyelv.
- [A hivatkozás - kvótáinak és szabályozásának] [ devguide-quotas] áttekintheti a kvóták beállítása a IoT központi szolgáltatás és az szabályozási működés számíthat, ha több mint kvóta megjelenítéséhez.
- [A hivatkozás - eszközök és szolgáltatások SDK] [ devguide-sdks] is használhatja a SDK listák eszköz és a szolgáltatás, amely a IoT központban vezérléséhez alkalmazások fejlesztői számára. A cikk online API-dokumentáció hivatkozásait tartalmazza.
- [Hivatkozás - IoT központi MQTT támogatási] [ devguide-mqtt] hogyan a IoT központi a MQTT protokollt támogató részletes információt tartalmaz. A cikk ismerteti a beépített a SDK MQTT protokoll támogatását, és közvetlenül a MQTT protokollal információt tartalmaz.
- [Szószedet] [ devguide-glossary] IoT központi kapcsolatos gyakori kifejezések listáját.



[devguide-messaging]: iot-hub-devguide-messaging.md
[devguide-upload]: iot-hub-devguide-file-upload.md
[devguide-identities]: iot-hub-devguide-identity-registry.md
[devguide-security]: iot-hub-devguide-security.md
[devguide-device-twins]: iot-hub-devguide-device-twins.md
[devguide-directmethods]: iot-hub-devguide-direct-methods.md
[devguide-jobs]: iot-hub-devguide-jobs.md
[devguide-endpoints]: iot-hub-devguide-endpoints.md
[devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[devguide-query]: iot-hub-devguide-query-language.md
[devguide-sdks]: iot-hub-devguide-sdks.md
[devguide-mqtt]: iot-hub-mqtt-support.md
[devguide-glossary]: iot-hub-devguide-glossary.md

