<properties
 pageTitle="Fejlesztőeszközök útmutató - kvótáinak és szabályozásának |} Microsoft Azure"
 description="Azure IoT központi Fejlesztőeszközök útmutató - IoT központi és az elvárt szabályozási viselkedés vonatkozó kvóták leírása"
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

# <a name="reference---quotas-and-throttling"></a>A hivatkozás - kvótáinak és szabályozása

## <a name="quotas-and-throttling"></a>Kvóták és szabályozása

Minden egyes Azure előfizetés beállíthatja, hogy legfeljebb 10 IoT hubok.

Minden egyes IoT központi a megadott számú adhatja meg egy adott Termékváltozat már kiépítve (további tudnivalókért lásd: [Azure IoT központi árak][lnk-pricing]). A Termékváltozat és a mennyiség határozza meg a küldhető üzenetek maximális napi kvóta.

A Termékváltozat is IoT központi be kényszeríti összes művelet a szabályozási korlát határozza meg.

## <a name="operation-throttles"></a>A művelet szabályozás

A művelet szabályozás alkalmazva vannak a percek tartományokban lévő és visszaélés elkerülése elsősorban ráta korlátozások is. IoT központi próbálja visszaadása, amikor csak lehetséges hibák elkerülése érdekében, de a kivételek visszaadása, ha a szabályozási sérül túl sokáig kezdődik.

Az alábbiakban megtalálja a kényszerített szabályozás listáját. Olvassa el az értékeket az egyes központi.

| Szabályozása | Egy központi érték |
| -------- | ------------- |
| Identitás beállításjegyzék műveletek (létrehozása, beolvasásához, lista, frissítése és törlése) | 5000/min/egység (S3) <br/> 100/min/egység (S1 és S2). |
| Eszköz kapcsolatok | 6000/sec/egység (S3), 120/sec/egység (S2), 12/sec/egység (S1). <br/>100/sec legalább. <br/> Ha például két S1 egységek 2\*12 = 24/sec, de legalább 100/sec végig a mennyiség. Kilenc S1 mennyiség, az, ha van 108/sec (9\*12) a mennyiség keresztül. |
| Küldje eszközt a felhőbe | 6000/sec/egység (S3), 120/sec/egység (S2), 12/sec/egység (S1). <br/>100/sec legalább. <br/> Ha például két S1 egységek 2\*12 = 24/sec, de legalább 100/sec végig a mennyiség. Kilenc S1 mennyiség, az, ha van 108/sec (9\*12) a mennyiség keresztül. |
| Felhőalapú-eszköz küldése | 5000/min/egység (S3), 100/min/egység (S1 és S2). |
| Felhőalapú-eszköz kapja | 50000/min/egység (S3), 1000/min/egység (S1 és S2). |
| Fájl feltöltése műveletek | 5000 fájlfeltöltési értesítések/min/egység (S3), a 100 fájl feltöltése értesítések/min/egység (az S1 és S2). <br/> Társítások URL-címe 10000 lehet ki az Azure tároló ügyfélhez egy időben.<br/> 10 Társítások URL-címe/eszköz használható-e ki egy időben. | 

Fontos, hogy az *eszköz kapcsolatok* szabályozási szabályozza a ráta, ahol új eszköz kapcsolatok egy IoT hubhoz, és nem egyidejű csatlakoztatott eszközök maximális száma létrehozott tisztázása. A szabályozási attól függ, hogy az a központi kiépített egységek számát.

Például ha egy S1 egységet vásárol, kapja a szabályozási másodpercenként 100 kapcsolatok. Ez azt jelenti, hogy 100 000 eszközök csatlakoztatásához vesz igénybe legalább 1000 másodperc (16 KB perc). Annyi egyidejű csatlakoztatott eszközök regisztrálni a eszköz identitás beállításjegyzékben eszközök megvan azonban is.

A IoT központi egy mélyreható vitafórum szabályozásának viselkedés, tekintse meg az [IoT központi szabályozásának]és,[lnk-throttle-blog].

>[AZURE.NOTE] Az adott időpontban kvóták vagy szabályozási korlátai növeléséhez IoT-központban kiépített mennyiségek növelésével.

>[AZURE.IMPORTANT] Futási idejű használja a mobileszköz-kezelés és esetek kiépítési szánt identitás beállításjegyzék műveletek. Olvasása vagy frissítése sok eszköz azonosítók [importálása és exportálása a feladatok]között támogatott[lnk-importexport].

## <a name="next-steps"></a>Következő lépések

Hivatkozás témakörök IoT központi Fejlesztőeszközök tartalom a következők:

- [IoT központi végpontok][lnk-devguide-endpoints]
- [Lekérdezési nyelv twins, módszerek és feladatok][lnk-devguide-query]
- [IoT központi MQTT támogatás][lnk-devguide-mqtt]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-throttle-blog]: https://azure.microsoft.com/blog/iot-hub-throttling-and-you/
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md