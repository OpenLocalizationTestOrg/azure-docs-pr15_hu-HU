<properties
 pageTitle="Azure IoT központi áttekintése |} Microsoft Azure"
 description="Azure IoT központi szolgáltatás áttekintése: Mit nevezünk iot központi, eszköz kapcsolódási, internet dolog, amit szokások és a szolgáltatás által támogatott kommunikációs minta"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/25/2016"
 ms.author="dobett"/>

# <a name="what-is-azure-iot-hub"></a>Mi az Azure IoT központi?

Üdvözli a Azure IoT központi. Ez a cikk áttekintést nyújt az Azure IoT központi, és ismerteti, hogy miért kell használni ezt a szolgáltatást egy dolog Internet (IoT) megoldás végrehajtásához. Azure IoT központi teljes körű felügyelt szolgáltatás, amely lehetővé teszi, hogy a megbízható és nem biztonságos kétirányú kommunikációt IoT eszközök milliónyi és a megoldás vissza vége között. Azure IoT központi:

- Megbízható eszköz felhő és a felhő-eszköz a méretezés üzenetküldés biztosít.
- Engedélyezi a biztonságos kommunikáció eszköz biztonsági hitelesítő adataival, és a hozzáférés-vezérlés.
- Itt eszköz kapcsolódási és eszköz identitás kezelése eseményeinek figyelése teljes körű.
- A leggyakrabban használt nyelvek és platformokon eszköz tárak tartalmazza.

A cikk [IoT-összehasonlító központi és esemény] [ lnk-compare] ismerteti a főbb különbségek vannak az alábbi két szolgáltatások között, és kiemeli IoT elosztót használ a IoT megoldásokban előnyeit.

![Azure IoT központi, felhőalapú átjáró az internet dolog, amit megoldás][img-architecture]

> [AZURE.NOTE] A [Microsoft Azure IoT hivatkozás architektúra]talál részletes ismertető IoT architektúra,[lnk-refarch].

## <a name="iot-device-connectivity-challenges"></a>IoT eszköz-kapcsolat kihívásokkal kapcsolatban

IoT központi és az eszköz tárak segítséget felel meg a megbízható és biztonságos módon eszközök keresztüli a megoldást háttér problémáit. IoT eszközök:

- Beágyazott rendszerek nem emberi operátorral gyakran történik.
- Távoli helyeken fizikai access esetén költséges lehet.
- Csak akkor érhető el a megoldást háttér keresztül.
- Előfordulhat, hogy korlátozott power és erőforrások feldolgozása.
- Lehet szakaszos, lassú vagy drága a hálózati kapcsolat.
- Saját, egyéni vagy iparágban-specifikus alkalmazás protokollokkal lehet.
- Hozhat létre egy nagy népszerű hardverre és szoftverre platformokon készlet segítségével.

Nemcsak a fenti követelményeknek bármely IoT megoldás kell is használhat az előadáshoz skála, biztonság és megbízhatóság. Kapcsolódás követelményeit eredő kemény és időigényesebbé válik, hagyományos technológiák, például a webes tárolók és üzenetben kereskedők használatakor végrehajtásához.

## <a name="why-use-azure-iot-hub"></a>Miért érdemes használni az Azure IoT központi

Azure IoT központi szünteti meg a eszköz-kapcsolat problémáit az alábbi módon:

-   **Eszköz hitelesítés és a biztonságos kapcsolatok**. Akkor is kiépítése minden eszközön saját [biztonsági kulcs] [ lnk-devguide-security] lehetővé teszi a IoT központban összekapcsolása. A [központi IoT identitás beállításjegyzék] [ lnk-devguide-identityregistry] megoldást eszköz identitások és a billentyűk tárolja. A megoldást vissza befejezési engedélyezése vagy tiltása a listák teljes kézben tarthatók az eszköz-hozzáférési egyes eszközöket is hozzáadhat.

-   **Figyelés eszköz kapcsolódási műveletek**. Eszköz identitás kezelése műveletek és eszköz kapcsolódási események kapcsolatos részletes művelet naplók kaphat. Ezzel a szolgáltatással nyomon lehetővé teszi, hogy a IoT megoldás azonosítja a csatlakozási problémákat tapasztal, például való csatlakozáskor a megfelelő hitelesítő adatokkal, eszközök küldésére is gyakran vagy elutasítása üzenetekhez cloud-eszközt.

-   **Az eszköz tárak teljes körű csoportja**. [Azure IoT eszköz SDK] [ lnk-device-sdks] elérhetők és a különféle nyelvek és platformokon – C-sok Linux terjesztését, a Windows és a valós idejű operációs rendszerek támogatott. Azure IoT eszköz SDK támogatja a felügyelt nyelven, például a C#, Java és JavaScript is.

-   **IoT protokollok és bővítési**. Ha a megoldás nem használja az eszköz tárak, a IoT központi közzététele egy nyilvános protokoll, amely lehetővé teszi, hogy a eszközök: MQTT v3.1.1, HTTP 1.1-es és AMQP 1.0 protokollok natív módon használhatja. Is bővítheti IoT központi támogatást nyújt egyéni protokollok szerint:

    - Mező az átjárók létrehozása az [Azure IoT átjáró SDK] [ lnk-gateway-sdk] , amely konvertál az egyéni protokoll a IoT központi értelmezni három protokoll közül. 
    - Az [Azure IoT protokoll átjáró]testreszabása[protocol-gateway], egy megnyitott forrásösszetevőt fut, a felhőben.

-   **Méretezés**. Azure IoT központi egyidejű csatlakoztatott eszközök milliónyi és események másodpercenként milliónyi méretezze át.

Alábbi előnyökkel jár általános sok kommunikációs mintákat is. IoT központi jelenleg lehetővé teszi a következő adott szokások végrehajtása:

-   **Esemény-alapú eszköz a felhőbe bevitel.** IoT központi megbízható kaphatnak másodpercenként események milliónyi készülékekről. Akkor is majd dolgozza fel őket a meleg útvonalon egy esemény processzor motor használatával. Azt is tárolhatja őket a hideg úthoz tartozó elemzéshez. IoT központi megtartja az esemény adatai megbízható feldolgozás zökkenőmentes, és a betöltés csúcsok felvegye hét napig.

-   * *Megbízható üzenetkezelés cloud-eszköz (vagy *parancsok*). ** a megoldást háttér IoT központi segítségével az a legkisebb – egyszeri kézbesítési garancia üzenet küldése az egyes eszközök. Minden üzenet tartalmaz egy egyéni time-to-live beállítást, és a háttér kérhet szállítási és a lejárat visszaigazolások. A visszaigazolások győződjön meg róla, teljes láthatóság be az életciklusának cloud-eszköz üzenet. Ezután alkalmazhat, amely tartalmazza az eszközön futó műveletek logikájának.

-   **Töltse fel fájlokat és a gyorsítótárban tárolt szenzoradatokat a felhőben.** Az eszközök segítségével kezeli meg IoT központi Társítások URL-címe Azure tárolóhoz feltölthet fájlokat. Ahhoz, hogy a háttér feldolgozni azokat a felhőben fájlok érkezésekor IoT központi miatt értesítéseket.

## <a name="gateways"></a>Átjárók

A IoT megoldás az átjáró általában az mindkét [átjáró protokoll] [ lnk-gateway] , hogy telepítve van az a felhő vagy [mező átjáró] [ lnk-field-gateway] , hogy telepítve van helyileg az eszközén. Protocol (protokoll) az átjárók fordítási protokollt, például hogy AMQP MQTT hajt végre. Mező az átjárók analytics futtassa a szegélyt, időérzékeny döntések csökkentése késés, eszköz management szolgáltatások nyújtása a, a hivatkozási biztonsági és adatvédelmi kényszerek, és protokoll fordítási is elvégezhet. Mindkét átjáró típusok között az eszközén, és a IoT központi működjön.

Mező az átjárók egy egyszerű alapú forgalmat útválasztási eszköz (például egy hálózati cím fordítási eszköz vagy a tűzfalat) különböznek egymástól általában hajt végre, ezért aktív szerepet az access és az információkat a megoldás áramlás kezelésében.

Megoldás Protocol (protokoll) és a mező átjárók tartalmazhat.

## <a name="how-does-iot-hub-work"></a>Hogyan működik a IoT központi?

Azure IoT központi végrehajtja a [szolgáltatás által támogatott kommunikációs] [ lnk-service-assisted-pattern] mintázat az eszközén, és a megoldás vissza vége közötti kölcsönhatás résidőkiosztással gombra. A szolgáltatás által támogatott kommunikációs célja, hogy megbízható-e létrehozni, a kétirányú kommunikációt elérési utak ellenőrző rendszer, például IoT központi és speciális célú eszközök a telepített között nem megbízható hely. A minta hoz létre a következő elvek:

- Biztonsági bontásakor összes funkcióját.
- Eszközök nem fogadja el a kéretlen hálózati adatok. Egy eszközt csak kimenő módon minden kapcsolatot és útvonalak hoz létre. A háttér parancs fogadni eszköz az eszköz rendszeresen kapcsolat keresésének feldolgozása függőben lévő parancsok kell kezdeményezni.
- Eszközök kell csatlakozni, vagy csak azok is peered, például IoT központi ismert szolgáltatások útvonalak létrehozni.
- Az alkalmazás protokoll rétegben védett kommunikációs elérési eszközök és szolgáltatások vagy eszközön, és az átjáró között.
- Rendszer szintű engedélyt, és a hitelesítéshez identitások eszköz – alapulnak. Azok ellenőrizze access hitelesítő adatokkal és engedélyekkel majdnem azonnali visszavonható.
- Az eszközök időnként power vagy az adatkapcsolat miatt aggályokat kétirányú kommunikációt megkönnyítheti tartsa a parancsok és eszköz-értesítések, amíg meg nem kap őket csatlakozik egy eszközt. IoT központi eszköz-specifikus sorban várakozó a parancsok, küldjön kezeli.
- Alkalmazás tartalom adatokat külön-külön védett vonatkozó védett egy adott szolgáltatás átjárók keresztül.

A mobil iparágban használatban van a szolgáltatás által támogatott kommunikációs minta hatalmas skála a leküldéses értesítést szolgáltatásokból, például a [Windows leküldéses értesítést Services]végrehajtásához[lnk-wns], [A Google Cloud üzenetküldés][lnk-google-messaging], és az [Apple leküldéses értesítéseket kezelő szolgáltatása][lnk-apple-push].

IoT központi támogatott készült ExpressRoute meg nyilvános peering elérési út fölé.

## <a name="next-steps"></a>Következő lépések

Hogyan Azure IoT központi lehetővé teszi, hogy szabványos-alapú IoT az mobileszköz-kezelés távolról kezelheti, beállítása és frissítése az eszközén című témakörben talál [mobileszköz-kezelés Áttekinté az Azure IoT központi][lnk-device-management].

Számos különböző eszköz hardver platformokon és operációs rendszerek ügyfélalkalmazásokban végrehajtásához az IoT eszköz SDK is használhatja. A IoT eszköz SDK tárakra, amelyek megkönnyítik a küldő telemetriai egy IoT központi és a felhő-eszköz parancsokat fogadó tartalmazzák. A SDK használata esetén választhat különböző hálózati protokollokat IoT központi kommunikálni. További tudnivalókért lásd: az [eszköz SDK adatai][lnk-device-sdks].

Első lépésként néhány kódírás és néhány minták futtatása a [IoT központi – első lépések] című[ lnk-get-started] oktatóprogram.

[img-architecture]: media/iot-hub-what-is-iot-hub/hubarchitecture.png


[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[protocol-gateway]: https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md
[lnk-service-assisted-pattern]: http://blogs.msdn.com/b/clemensv/archive/2014/02/10/service-assisted-communication-for-connected-devices.aspx "Szolgáltatás támogatott kommunikációs, Clemens Vasters blogbejegyzés"
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-gateway]: iot-hub-protocol-gateway.md
[lnk-field-gateway]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-devguide-identityregistry]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-wns]: https://msdn.microsoft.com/library/windows/apps/mt187203.aspx
[lnk-google-messaging]: https://developers.google.com/cloud-messaging/
[lnk-apple-push]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-device-management]: iot-hub-device-management-overview.md
