<properties
 pageTitle="IoT központi eszköz kezelése – áttekintés |} Microsoft Azure"
 description="Ez a cikk áttekintést nyújt az Azure IoT központi a mobileszköz-kezelés: vállalati eszköz életciklusra, indítsa újra a rendszert, gyár alaphelyzetbe állítása, belső vezérlőprogram frissítését, konfigurációs, eszköz twins, lekérdezések, feladatok"
 services="iot-hub"
 documentationCenter=""
 authors="bzurcher"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/03/2016"
 ms.author="bzurcher"/>

# <a name="overview-of-azure-iot-hub-device-management-preview"></a>Azure IoT központi mobileszköz-kezelés (előzetes verzió) – áttekintés

## <a name="introduction"></a>– Bevezetés

Azure IoT központi tartalmaz, a szolgáltatások és -bővítési modell, amelyek lehetővé teszik az eszköztől és a háttéradatbázist fejlesztők robusztus IoT eszköz management megoldások. IoT eszközök terjedő tartományban korlátozott érzékelők és egyetlen célját mikrovezérlők, az eszközök csoport kommunikáció irányítása hatékony átjárók.  Ezeken kívül az használati eset és IoT operátorok követelményei eltérőek jelentősen ágazatok.  Ezt a változatot ellentétben Azure IoT központi mobileszköz-kezelés a Funkciók, minták és kód tárak gondoskodjon arról, hogy a különböző eszközök és a végfelhasználók halmazára biztosít.

Egyik legfontosabb része a sikeres vállalati IoT megoldás stratégia nyújt a hogyan kezeli az operátorok a a mindennapi felügyeleti feladatokhoz eszközök a webhelycsoport létrehozása. IoT operátorok szükség egyszerű és megbízható eszközökkel és alkalmazásokkal:, amely engedélyezi őket a saját feladatok további stratégiai szempontjait koncentrálhat. Ebben a cikkben:

- Mobileszköz-kezelés Azure IoT központi megközelítése rövid áttekintését.
- Gyakori eszköz irányítási elvek leírását.
- Az eszköz életciklus leírását.
- Gyakori eszköz management mintázatok áttekintése.

## <a name="iot-device-management-principles"></a>IoT eszköz irányítási elvek

IoT megjeleníti az általa a szülőwebhelyhez eszköz management problémáit, és minden nagyvállalati megoldás kell oldania a következő elvek:

![Azure IoT központi eszköz management elvek ábra][img-dm_principles]

- **Méretezés telepített és automatizálási**: a megoldáshoz IoT egyszerű eszközök, amelyek képesek mindennapos feladatok automatizálása, és engedélyezze egy viszonylag kis műveletek oktatói milliónyi eszközök kezeléséhez. Napi, operátorok várt eszköz művelet távolról kezelésére, tömegesen, és csak értesülni arról, hogy problémák merülnek fel, amelyek csak a közvetlen figyelmet.

- **Nyilvánosság és a kompatibilitás**: A IoT eszköz ökológiai kivéve különböző. Felügyeleti eszközök kell igazítani eszköz osztályok, rendszerek és protokollok rengeteg igazodik. Operátorok kell tudja-eszközök a legjobban korlátozott beágyazott egyetlen folyamaton processzorok hatékony és a teljes funkcionalitású számítógépek számos különböző típusú támogatja.

- **Helyi tájékoztatás**: IoT környezetekben, dinamikus és minden eddiginél módosítása. Szolgáltatás megbízhatóság kiemelkedő. Eszköz adatkezelési műveletek kell kiszámíthatja SLA karbantartási windows, a hálózati és a power állapotok, a használati feltételek és eszköz-feloldás földrajzi helye annak biztosítására, hogy karbantartási legrövidebb leállás nem kritikus üzleti műveleteket befolyásoló vagy veszélyes feltételek létrehozása.

- **Sok szerepkörök szolgáltatás**: az egyedi munkafolyamatok és folyamatok IoT műveleti szerepkörök támogatása elengedhetetlen. A műveletek oktatói harmonikus dolgozhat kell az adott korlátokat a belső informatikai részlegeknek.  Azokat meg kell keresnie fenntartható módjai felügyelők és más vállalati irányítási szerepkörök felszíni valós idejű eszköz műveletek adatokat is.

## <a name="iot-device-lifecycle"></a>IoT eszköz életciklus

Van egy csoportja közös az összes vállalati IoT projekt általános eszköz kezelése szakaszban. Az Azure IoT, öt szakaszból áll a IoT eszköz életciklusáról belül:

![Az öt Azure IoT eszköz életciklus-fázisokat: tervezése, kialakítása, konfigurálása, figyelheti, kivonni][img-device_lifecycle]

Belül mindegyik öt ezeket a lépéseket létezik meg kell felelnie a teljes megoldást nyújtanak számos eszközt operátor követelmények:

- **Megtervezése**: engedélyezése operátorok egy eszköz metaadat-alapú Office-téma, amely lehetővé teszi, hogy egyszerűen és pontosan a lekérdezést, és Megcélozhat tömeges műveletekhez eszközök csoport létrehozásához. Az eszköz kettős segítségével a eszköz metaadatokat tárolhatnak formájában, a címkék és a Tulajdonságok parancsot.

    *További olvasnivaló*: [első lépések az eszköz twins][lnk-twins-getstarted], [ismertetése eszköz twins][lnk-twins-devguide], [kettős tulajdonságainak használata][lnk-twin-properties]

- **Kiépítése**: biztonságosan kiépítése új eszközök IoT-hubon keresztül csatlakozott, és azonnal fel az eszköz képességeit operátorok engedélyezése.  A IoT központi eszköz beállításjegyzék rugalmas eszköz identitások és a hitelesítő adatok létrehozásához használja, és ehhez a művelethez tömegesen a feladat használatával. Eszközök: azok a funkciók és feltételek – az eszköz kettős az eszköz tulajdonságainak jelentés készítése.

    *További olvasnivaló*: [kezelése eszköz identitások][lnk-identity-registry], [tömeges kezelése eszköz identitások][lnk-bulk-identity], [kettős tulajdonságainak használata][lnk-twin-properties]

- **Konfigurálása**: tömeges beállítások módosítása, illetve az eszközök belső vezérlőprogram frissítések megkönnyítése állapot és a biztonsági megőrzésével. Műveletek hajthatók végre ezeket eszköz management tömegesen vagy közvetlen módszerek és közvetítési feladatok a kívánt tulajdonságokat használatával.

    *További olvasnivaló*: [közvetlen módszerekkel][lnk-c2d-methods], [Invoke eszközön közvetlen módszer][lnk-methods-devguide], [kettős tulajdonságok használata][lnk-twin-properties], [Ütemezés és közvetítési feladatok][lnk-jobs], [a feladatok ütemezése több eszközön][lnk-jobs-devguide]

- **Monitor**: figyelheti a általános eszköz a webhelycsoport állapot, a folyamatban lévő műveleteket, állapotát, és értesítést operátorok probléma miatt előfordulhat, hogy a figyelmet.  Az eszköz kettős, ha engedélyezni szeretné a jelentés valós idejű működési feltételek és a frissítési művelet állapotának eszközök vonatkoznak. A legközvetlenebb problémákat felszíni eszközök kettős lekérdezések használatával hatékony irányítópult-jelentések készítése.

    *További olvasnivaló*: [kettős tulajdonságok használata][lnk-twin-properties], [lekérdezési nyelv twins és feladatok][lnk-query-language]

- **Kivezetése**: cseréje vagy leválasztása eszközök egy hiba után, ciklus frissítése vagy a szolgáltatás élettartam végén.  Az eszköz kettős segítségével karbantartása eszköz információ, ha a fizikai eszközök folyamatban van cserélni, vagy ha használatból archivált. Használja a IoT központi eszköz beállításjegyzék biztonságosan visszavonása eszköz identitások és a hitelesítő adatait.

    *További olvasnivaló*: [kettős tulajdonságok használata][lnk-twin-properties], [kezelése eszköz identitások][lnk-identity-registry]

## <a name="iot-hub-device-management-patterns"></a>IoT központi eszköz management mintázatok

IoT központi lehetővé teszi, hogy az eszköz management mintázatok következő csoportját.  Az [eszköz management oktatóanyagok] [ lnk-get-started] bemutatják, részletesebben, ezek a minták pontos igényektől igazítása meghosszabbítása és a fő sablonok alapján új mintázatok tervezéséről.

- **Indítsa újra** - a háttéradatbázist alkalmazás arról tájékoztat, az eszköz keresztül közvetlen módszer, hogy a számítógép újraindítása kezdeményezett.  Az eszközt használ, az eszköz kettős jelenteni az eszköz újraindítása állapotának módosítása Tulajdonságok.

    ![Azure IoT központi mobileszköz-kezelés indítsa újra a grafikus minta][img-reboot_pattern]

- **Gyári alaphelyzetbe** – a háttéradatbázist alkalmazás arról tájékoztat, az eszköz keresztül közvetlen módszer, hogy a gyári alaphelyzetbe állítása kezdeményezett.  Az eszközt használ, az eszköz kettős gyári frissítése jelentett tulajdonságok Alapállapot az eszközön.

    ![Azure IoT központi eszköz management gyári mintát ábra visszaállítása][img-facreset_pattern]

- **Konfigurációs** – a háttéradatbázist alkalmazás használja a kívánt kettős eszköz tulajdonságainak konfigurálása szoftver fut az eszközön.  Az eszközt használ, az eszköz kettős jelentett frissíti konfigurációs állapotát az eszköz tulajdonságainak.

    ![Azure IoT központi eszköz management konfigurációs mintát ábra][img-config_pattern]

- **Belső vezérlőprogram frissítése** – a háttéradatbázist alkalmazást arról tájékoztat, az eszköz keresztül közvetlen módszer, hogy a belső vezérlőprogram frissítésének kezdeményezett.  Az eszköz elindít egy többlépéses folyamatot, töltse le a belső vezérlőprogram csomagot, a belső vezérlőprogram ugyanott és végül a IoT központi szolgáltatás újracsatlakozáshoz.  A történik lépésből álló folyamat során az eszközön használja az eszköz kettős jelentett frissítése folyamatát és állapotát az eszköz tulajdonságainak.

    ![Azure IoT központi eszköz management belső vezérlőprogram mintát grafikus elem frissítése][img-fwupdate_pattern]

- **Folyamatban és állapotának jelentése** – az alkalmazás háttéradatbázist futtatja a eszköz kettős lekérdezéseket, egy sor olyan eszközökön állapotát és előrehaladását a eszközökön futó műveletek jelentést át.

    ![Azure IoT központi mobileszköz-kezelés folyamatban és állapot mintát ábra jelentése][img-report_progress_pattern]

## <a name="next-steps"></a>Következő lépések

A Funkciók, a mintázatok és a kód tárakat, amely az Azure IoT központi mobileszköz-kezelés Itt hozhat létre, amelyek megfelelnek vállalati IoT operátor belül minden egyes eszköz életciklus szakaszban IoT alkalmazások is használhatja.

Továbbra is kapcsolatban az Azure IoT központi eszköz-felügyeleti funkciókat, lásd: az [első lépések a mobileszköz-kezelés Azure IoT központi] [ lnk-get-started] oktatóprogram.

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-twins-devguide]: iot-hub-devguide-device-twins.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-query-language]: iot-hub-devguide-query-language.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-methods-devguide]: iot-hub-devguide-direct-methods.md
[lnk-jobs]: iot-hub-schedule-jobs.md
[lnk-jobs-devguide]: iot-hub-devguide-jobs.md