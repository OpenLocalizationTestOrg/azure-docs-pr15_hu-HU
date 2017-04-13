<properties
 pageTitle="Fejlesztőeszközök útmutató - eszköz identitás beállításjegyzék |} Microsoft Azure"
 description="Azure IoT központi Fejlesztőeszközök útmutató – a eszköz identitás beállításjegyzék, és hogyan kezelheti a segítségével leírása"
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

# <a name="manage-device-identities-in-iot-hub"></a>Eszköz identitások IoT-központban kezelése

## <a name="overview"></a>– Áttekintés

Minden IoT hubhoz egy eszköz identitás beállításjegyzék, amely az-központban összekapcsolása engedélyezett eszközök adatait tárolja. Csak egy eszközt egy központi csatlakozhat, egy bejegyzést a központi eszköz identitás beállításjegyzékben eszköz kell. Egy eszközt kell is hitelesítést végezni a hubhoz a eszköz identitás tárolt hitelesítő adatai alapján.

Magas szintű az eszköz identitás beállításjegyzék gyűjteménye többi internetezésre alkalmas eszköz identitás-erőforrások. Amikor felvesz egy bejegyzést a beállításjegyzék, IoT központi egy sor olyan eszközt erőforrások a szolgáltatást, például a várólista repülés cloud-eszköz üzenetek tartalmazó hoz létre.

### <a name="when-to-use"></a>Mikor érdemes használni

Az eszköz identitás beállításjegyzék kell rendelkezni, amely IoT központi csatlakozni, és mikor kell való hozzáférés korlátozása a központi eszköz elérésű végpont eszköz eszközök használatával.

> [AZURE.NOTE] Az eszköz identitás beállításjegyzék nem tartalmaz minden alkalmazás-specifikus metaadatok.

## <a name="device-identity-registry-operations"></a>Eszköz identitás beállításjegyzék műveletek

A központi IoT eszköz identitás beállításjegyzék elérhetővé teszi a következő műveleteket:

* Eszköz azonosító létrehozása
* Identitás eszköz frissítése
* Azonosító szerint eszköz identitás beolvasása
* Eszköz identitás törlése
* A lista legfeljebb 1000 identitások
* Az összes identitást exportálása Azure blob-tárolóhoz
* Azonosítók importálása az Azure blob-tárolóhoz

Ezek a műveletek optimista feldolgozási, használhatja a megadott [RFC7232][lnk-rfc7232].

> [AZURE.IMPORTANT] Csak úgy lehet beolvasni összes identitások egy központi identitás beállításjegyzékében környezetbe [exportálása] [ lnk-export] használható funkciók körét.

Egy központi IoT eszköz identitás beállításjegyzék:

- Minden alkalmazási metaadatok nem tartalmaz.
- Érhetők el a szótár, például a **deviceId** kulcsként használatával.
- Kifejező lekérdezések nem támogatja.

Egy IoT megoldás általában egy külön megoldás-specifikus áruház alkalmazás-specifikus metaadatokat tartalmazó rendelkezik. Például az intelligens épület megoldást megoldás-specifikus áruházra rögzíti a helyiség, amelyben hőmérsékletérzékelő telepítve van.

> [AZURE.IMPORTANT] Csak a eszköz identitás beállításjegyzék mobileszköz-kezelés és használható a kiépítési művelet. Nagy teljesítmény műveletek futásidőben műveletek elvégzéséhez a eszköz identitás beállításjegyzékben nem függ. Ha például egy eszközt kapcsolati állapotának ellenőrzése parancs elküldése előtt nem támogatott mintát. Ellenőrizze, hogy a [Mértékek szabályozásának] [ lnk-quotas] a eszköz identitás beállításjegyzék és a [eszköz szívveréséhez] [ lnk-guidance-heartbeat] mintát.

## <a name="disable-devices"></a>Eszközök letiltása

Eszközök letilthatja a beállításjegyzékben identitás **állapot** tulajdonsága frissítésével. Ez a tulajdonság általában az alábbi két forgatókönyvet használja:

- A kiépítési üzletifolyamat-tervezési folyamat során További tudnivalókért lásd: a [Kiépítési eszköz][lnk-guidance-provisioning].
- Ha valamilyen okból fontolja meg egy eszközt sérül, vagy ezzel az illetéktelen vált.

## <a name="import-and-export-device-identities"></a>Az importálás és exportálás eszköz identitások

Exportálhatja eszköz identitások tömegesen egy IoT központi identitás beállításjegyzékéből aszinkron műveletek a [IoT központi erőforrás szolgáltató végpont]használatával[lnk-endpoints]. Exportnak hosszabb ideig futó feladatokat, olvassa el a identitás beállításjegyzékéből eszköz identitás adatainak mentése egy ügyfél által megadott blob-tároló használó.

Importálhatja eszköz identitások tömegesen egy IoT központi identitás regisztrációs, a [IoT központi erőforrás szolgáltató végpont]aszinkron műveletek használatával[lnk-endpoints]. Import, amelyek egy ügyfél által megadott blob-tárolóban lévő adatokat eszköz identitás adatok írásához a eszköz identitás rendszerleíró hosszabb ideig futó feladatokat.

- Az importálás és exportálás API-khoz részletes információt talál az [IoT központi erőforrás szolgáltató REST API -k][lnk-resource-provider-apis].
- Többet szeretne tudni az importálás futtatása és a feladatok exportálni, olvassa el a [tömeges kezelés IoT központi eszköz identitások][lnk-bulk-identity].

## <a name="device-provisioning"></a>Eszköz kiépítése

Az eszköz adatokat egy adott IoT megoldás tároló attól függ, hogy az adott követelményeket, hogy a megoldás. De legalább egy megoldás kell tárolnia, eszköz identitások és a hitelesítési kulcsokat. Azure IoT központi tartalmazza az identitás rendszerleíró értékeket az egyes eszközök, például azonosítók, hitelesítési kulcsokat és állapotkód tárolható. Megoldás más Azure szolgáltatásokból, például Azure táblatárolóhoz Azure blob-tárolóhoz és Azure DocumentDB segítségével további eszköz adatokat tárolja.

*Eszköz kiépítési* kezdeti eszköz adatok hozzáadása a megoldáshoz az üzletek során a rendszer. Ha engedélyezni szeretné a központban összekapcsolása új eszköz, hozzá kell adnia egy új Eszközazonosítót és a billentyűk a IoT központi identitás beállításjegyzék. A kiépítési folyamat részeként szükség lehet inicializálni eszköz-specifikus adatait más megoldást tárolja.

## <a name="device-heartbeat"></a>Eszköz szívveréséhez

A központi IoT identitás beállításjegyzék **connectionState**nevű mező tartalmazza. A fejlesztés során kell használni a **connectionState** mező csak és hibakeresés IoT megoldások kell nem lekérdezést a mező futásidőben (például ellenőrzése, ha egy eszközt annak érdekében, hogy a felhőben-eszköz üzenet vagy SMS küldése eldöntése csatlakoztatva van-e).

Ha a IoT megoldás tudni, hogy egy eszközt csatlakoztatva van (futásidőben, vagy további pontosságú, mint a **connectionState** tulajdonság), a megoldás végre kell hajtania a *minta szívveréséhez*.

A szívveréséhez mintázat az eszköz eszköz a felhőbe üzeneteket küld legalább egyszer minden rögzített időtartama (például legalább óránként). Ez azt jelenti, hogy akkor is, ha az eszköz nincs semmilyen adatot küldeni, továbbra is elküldi egy üres eszköz a felhőbe üzenetre (ha általában az egy tulajdonság, amely azonosítja a szívveréséhez). A szolgáltatás oldalon a megoldást térkép megtartja a az utolsó szívveréséhez minden eszközhöz kapott, és feltételezi, hogy probléma van egy eszközt, ha nem jelenik egy szívveréséhez üzenet belül a várt időben.

Egy összetettebb végrehajtási [Műveletek figyelése] adatait lehetnek[ lnk-devguide-opmon] eszközök, amelyek a csatlakozás vagy kommunikáció, de adatkapcsolat azonosításához. Ha az szívveréséhez szabályt alkalmazza, ellenőrizze, hogy [IoT központi kvótáinak és a szabályozás][lnk-quotas].

> [AZURE.NOTE] Ha egy IoT megoldás van szüksége a eszköz kapcsolati állapotának kizárólag eldöntheti, hogy a felhőben-eszköz küldésére, és az üzenetek nem közvetítheti vannak eszközök nagy adathalmazok, sokkal egyszerűbb mintát is célszerű figyelembe venni egy rövid lejárati idő. Ez éri el a használatával a szívveréséhez mintát, miközben is hatékonyabb jelentősen eszköz kapcsolat állapota beállításjegyzék fenntartása ugyanazt az eredményt. Lehetőség arra is, üzenet nyugták, ha értesítést szeretne kapni, mely-e fogadhat üzeneteket, és amelyek nem érhetők el vagy meghibásodott IoT központi igénylésével.

## <a name="reference-topics"></a>Hivatkozás témakörök:

A következő hivatkozást témakörök nyújt további információt a devcie identitás beállításjegyzék.

## <a name="device-identity-properties"></a>Eszköz identitás tulajdonságai

A következő tulajdonságokat a JSON-dokumentumként eszköz identitások jelennek meg.

| A tulajdonság | Beállítások | Leírás |
| -------- | ------- | ----------- |
| deviceId | kötelező, a csak olvasható frissítések | ASCII-7 bites alfanumerikus karaktereket kis-és nagybetűket karakterlánc (legfeljebb 128 karakter hosszú) + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| generationId | szükség esetén csak olvasható | Központi által generált, a kis-és nagybetűket karakterlánc legfeljebb hosszú 128 karaktert. Törölt és újból létre, különböző azonos **deviceId**eszközökhöz szolgál. |
| ETag | szükség esetén csak olvasható | Az eszköz identitásához szerinti [RFC7232]egy gyenge etag képviselő karakterlánc, amely[lnk-rfc7232].|
| hitelesítés | nem kötelező. | Hitelesítés információkat és biztonsági anyagokat tartalmazó összetett objektum. |
| auth.symkey | nem kötelező. | Egy összetett objektum, amelyben egy elsődleges és másodlagos kulcsa, base64 formátum tárolja. |
| állapot | szükséges | Az access mutató. **Enabled** és **letiltott**is lehet. Ha **engedélyezve**, az eszköz csatlakozhat. Ha **letiltott**, ez az eszköz nem fér hozzá bármilyen eszközön elérésű végpontot. |
| statusReason | nem kötelező. | 128 karakter hosszú karakterlánc, amely az az oka az Eszközállapot identitás tárolja. Az összes UTF-8 karakterek használhatók. |
| statusUpdateTime | csak olvasható | Időbeli mutató, rajta a dátum- és az állapot utolsó frissítés. |
| connectionState | csak olvasható | A kapcsolat állapotát jelző mező: **Connected** vagy a **kapcsolat nélküli**. Ez a mező eszköz kapcsolati állapotának IoT központi nézetének jelöli. **Fontos**: ezt a mezőt kell használni csak fejlesztés és hibakeresés céljából. A kapcsolati állapot csak olyan MQTT vagy AMQP eszközökhöz frissül. Is (MQTT pingelése vagy AMQP pingelése) protokoll szintű pingelése alapul, és csak az 5 perc maximális késleltetést lehetnek. Ezen okokból kifolyólag lehet téves, például a csatlakoztatott eszközök készként, de a ténylegesen megszakad. |
| connectionStateUpdatedTime | csak olvasható | Időbeli mutató, rajta a dátum és idő utolsó a kapcsolati állapot frissült. |
| lastActivityTime  | csak olvasható | Időbeli mutató, rajta a dátum és idő utolsó az eszköz csatlakozik, kapott, vagy egy elküldött üzenetre. |

> [AZURE.NOTE] Kapcsolati állapot csak megfelelhet a kapcsolat állapotának IoT központi nézetét. Ez az állapot frissítések késve, attól függően, hogy a hálózati feltételek és beállításokat.

## <a name="additional-reference-material"></a>További anyagminta

A Fejlesztőeszközök útmutató hivatkozás témakörök a következők:

- [IoT központi végpontok] [ lnk-endpoints] ismerteti a különböző végpontok, amely az egyes IoT központi közzététele futtatókörnyezet és kezelés műveletekhez.
- [Szabályozási és kvóták] [ lnk-quotas] a IoT központi szolgáltatás és számíthat, amikor a szolgáltatás használatához a szabályozási viselkedés vonatkozó kvóták ismerteti.
- [IoT központi eszközök és szolgáltatások SDK] [ lnk-sdks] a különböző nyelvi SDK sorolja fel, egy eszköz és a szolgáltatás-alkalmazások használata a IoT központi fejlesztésekor használja.
- [Lekérdezési nyelv twins, módszerek és feladatok] [ lnk-query] ismerteti a lekérdezésnyelv IoT központi eszköz twins, módszerek és feladatok kapcsolatos adatok kinyerése is használhatja.
- [IoT központi MQTT támogatási] [ lnk-devguide-mqtt] IoT központi támogatási további információt tartalmaz a MQTT protokollhoz.

## <a name="next-steps"></a>Következő lépések

Most már rendelkezik megtanulta a IoT központi eszköz identitás beállításjegyzék használatát, amelyek érdekelhetik a fejlesztői útmutató a következő témakörök:

- [IoT központi való hozzáférés szabályozása][lnk-devguide-security]
- [Állapot és a beállítások szinkronizálása az eszköz twins használatával][lnk-devguide-device-twins]
- [Közvetlen metódus eszközön][lnk-devguide-directmethods]
- [A feladatok ütemezése több eszközön][lnk-devguide-jobs]

Ha azt szeretné, próbálja meg néhány a jelen cikkben ismertetett fogalmak, amelyek érdekelhetik a következő IoT központi oktatóprogram során:

- [Azure IoT központi – első lépések][lnk-getstarted-tutorial]


<!-- Links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-guidance-provisioning]: iot-hub-devguide-identity-registry.md#device-provisioning
[lnk-guidance-heartbeat]: iot-hub-devguide-identity-registry.md#device-heartbeat
[lnk-rfc7232]: https://tools.ietf.org/html/rfc7232
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-export]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-devguide-opmon]: iot-hub-operations-monitoring.md

[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md