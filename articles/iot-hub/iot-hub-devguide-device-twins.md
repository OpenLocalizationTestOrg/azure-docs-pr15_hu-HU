<properties
 pageTitle="Fejlesztőeszközök útmutató - eszköz twins megértéséhez |} Microsoft Azure"
 description="Azure IoT központi Fejlesztőeszközök útmutató - használat eszköz twins adatszinkronizálás állapotát és konfigurációs IoT központi és az eszközökre"
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="elioda"/>

# <a name="understand-device-twins---preview"></a>Eszköz twins megértéséhez - megtekintése

## <a name="overview"></a>– Áttekintés

*Eszköz twins* JSON dokumentumokat tároló eszköz állam információk (metaadatok konfigurációk és feltételek). IoT központi továbbra is fennáll, egy eszköz kettős az egyes IoT elosztóhoz csatlakoztatott eszközt. Ez a cikk leírja:

* az eszköz kettős felépítésének: *címkék*, *kívánt* és *Tulajdonságok jelenteni*, és
* a műveletek, hogy az eszköz-alkalmazások és a hátsó végű eszköz twins végezhet.

> [AZURE.NOTE] Ekkor eszköz twins elérhetők csak a MQTT protokollal IoT központi csatlakozó eszközök. Olvassa el a [támogatási MQTT] [ lnk-devguide-mqtt] miként meglévő eszköz alkalmazás konverziójának MQTT használni.

### <a name="when-to-use"></a>Mikor érdemes használni

Az eszköz twins használja:

* Eszköz adott metaadatok tárolni a felhőben, például egy Eladóautomata telepítési helye.
* Aktuális állapot adatai, például a rendelkezésre álló lehetőségek és az eszköz-alkalmazás, például egy mobil- vagy wifi keresztül csatlakozik eszközt feltételek jelentést.
* Szinkronizálása az eszköz alkalmazást, és vissza vége, például a megadása az új belső vezérlőprogram változatot telepítse újra befejezési és a frissítési folyamat különböző fázisaiban jelentéskészítő eszköz alkalmazás közötti hosszan futó munkafolyamatok állapotának.
* A metaadatok eszköz, konfigurációs vagy állapot lekérdezés.

Használható [eszköz a felhőbe üzenetek] [ lnk-d2c] rajta időbélyegző bizonyos eseményeket, például az idősorok szenzoradatokat vagy riasztások, szekvencia. [Felhőalapú-eszköz] módszerekkel[ lnk-methods] eszközök, például egy ventilátorral bekapcsolása interaktív ellenőrzésére.

## <a name="device-twins"></a>Eszköz twins

Eszköz twins eszköz kapcsolatos adatok tárolására, amely:

- Eszköz, és vissza végű segítségével eszköz feltételek és konfigurálása szinkronizálása.
- A háttér alkalmazás segítségével lekérdezés- és célwebhelyek hosszabb ideig futó műveletek.

Egy eszköz kettős életciklusának csatolva van a megfelelő [eszköz identitás][lnk-identity]. Twins implicit módon létrehozott, és törli, ha egy új eszköz identitás létrehozása vagy törlése az IoT-központban.

Egy eszköz kettős JSON dokumentum, amely tartalmazza:

* A **címkék**. A JSON dokumentum olvasása és a háttér által írt. Címkék nem láthatók az eszköz-alkalmazásokhoz.
* A **kívánt tulajdonságokat**. A bejelentett tulajdonságok együtt használva szinkronizálása az eszköz konfigurációs vagy feltétel. A kívánt tulajdonságokat csak akkor állítható vissza az alkalmazás által vége, és eszközhöz alkalmazással is olvashatók. Az eszköz alkalmazás arról is valós időben végrehajtott módosítások, kattintson a kívánt tulajdonságokat.
* **Jelentett tulajdonságait**. A kívánt tulajdonságokat együtt használva szinkronizálása az eszköz konfigurációs vagy feltétel. Bejelentett tulajdonságok csak az eszköz alkalmazást beállíthatja és olvasható, majd a háttér alkalmazás által lekérdezett.

Emellett az eszköz kettős legfelső szintű a megfelelő identitás [eszköz identitás beállításjegyzék]tartalmazza, mint a csak olvasható tulajdonságait tartalmazza[lnk-identity].

![][img-twin]

Íme egy példa eszköz kettős JSON dokumentum:

        {
            "deviceId": "devA",
            "generationId": "123",
            "status": "enabled",
            "statusReason": "provisioned",
            "connectionState": "connected",
            "connectionStateUpdatedTime": "2015-02-28T16:24:48.789Z",
            "lastActivityTime": "2015-02-30T16:24:48.789Z",

            "tags": {
                "$etag": "123",
                "deploymentLocation": {
                    "building": "43",
                    "floor": "1"
                }
            },
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata" : {...},
                    "$version": 1
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": 55,
                    "$metadata" : {...},
                    "$version": 4
                }
            }
        }

A legfelső szintű objektumban Rendszertulajdonságok, és az objektumok tároló `tags` és mindkét `reported` és `desired properties`. A `properties` tárolóban lévő elemeket csak olvasható (`$metadata`, `$etag`, és `$version`) ismertetett a kurzor a [kettős metaadatok] [ lnk-twin-metadata] és [optimista feldolgozási] [ lnk-concurrency] szakaszok.

### <a name="reported-property-example"></a>Példa a jelentett tulajdonság

A fenti példában az eszköz kettős tartalmaz egy `batteryLevel` tulajdonság, amelynek jelenteni az eszköz alkalmazással. Ez a tulajdonság lehetővé teszi a lekérdezés és az utolsó elem jelentett szint alapján eszközökön működik. Egy másik példa szeretné, hogy az eszköz alkalmazás jelentés eszköz képességeinek vagy csatlakozási beállítások.

Megjegyzés: hogyan jelentett tulajdonságok forgatókönyvek, ahol a háttér tulajdonság értékének utolsó ismert iránt érdeklődik egyszerűsítése érdekében. Használható [eszköz a felhőbe üzenetek] [ lnk-d2c] a háttér eszköz telemetriai rajta időbélyegző eseményeket, például az idősorok sorozatok formájában feldolgozása van-e szüksége.

### <a name="desired-configuration-example"></a>Példa a kívánt konfiguráció

A fenti példában a `telemetryConfig` kívánt és jelentett tulajdonságok szinkronizálása az eszköz telemetriai adatokat a befejezési és eszköz vissza alkalmazás használja. Példa:

1. A háttér alkalmazás állítja be a kívánt tulajdonságot a kívánt konfiguráció értéket. Az alábbiakban a dokumentumnak a kívánt tulajdonság:

        ...
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            ...
        },
        ...
        
2. Az eszköz alkalmazás azonnal, ha a csatlakoztatott módosítása, illetve első újbóli értesítést kap. Az eszköz alkalmazást, majd jelentések a frissített konfiguráció (vagy egy hiba feltétel használata a `status` tulajdonság). Az alábbiakban a jelentett tulajdonságok részét:

        ...
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            ...
        }
        ...

3. A háttér alkalmazás tarthatja nyomon követése a konfigurációs művelet eredménye [lekérdezésével] keresztül sok szinkronizált eszköz,[ lnk-query] twins.

> [AZURE.NOTE] A fenti kódrészletek példák, egy lehetséges módja kódolása a eszköz konfiguráció és állapota olvashatóságát optimalizálva. IoT központi nem ír elő egy adott séma a kívánt és jelenteni az eszköz twins tulajdonságok.

Sok esetben twins hosszan futó műveleteket, például a belső vezérlőprogram frissítések szinkronizálásához használt. Olvassa el a használni [kívánt tulajdonságok eszközök konfigurálása] [ lnk-twin-properties] további információt a tulajdonságok használatát segítségével szinkronizálja és nyomon követheti a hosszú futó műveletek minden eszközön.

## <a name="back-end-operations"></a>Háttér-műveletek

A háttér a kettős, használja az alábbi atomi műveletek HTTP keresztül elérhetővé tett a működik:

1. Az **azonosító szerint kettős beolvasásához**. Ez a művelet ad vissza, a kettős dokumentum, többek között címkék, és szükség esetén jelentett és Rendszertulajdonságok.
2. **Kettős részben frissítése**. Ez a művelet lehetővé teszi, hogy a háttér részben és is frissíteni a kettős címkék a kívánt tulajdonságokat. A részleges analitikafrissítés JSON dokumentum hozzáadása vagy frissíti minden említett tulajdonság fejezi ki. Tulajdonságainak `null` törlődnek. Például a következő hoz létre egy új kívánt tulajdonság értékkel `{"newProperty": "newValue"}`, felülírja a meglévő előnyeiről `existingProperty` a `"otherNewValue"`, és teljesen eltávolítja `otherOldProperty`. Nincs módosítás más meglévő a kívánt tulajdonságokat és a címkék fordulhat elő:

        {
            "properties": {
                "desired": {
                    "newProperty": {
                        "nestedProperty": "newValue"
                    },
                    "existingProperty": "otherNewValue",
                    "otherOldProperty": null
                }
            }
        }

3. **Cserélje le a kívánt tulajdonságokat**. Ez a művelet lehetővé teszi, hogy a háttér teljesen felülírja az összes meglévő a kívánt tulajdonságokat és az új JSON dokumentum helyettesítő `properties/desired`.
4. A **címkék cseréje**. Adatproblémák le szeretné cserélni kívánt tulajdonságait, a műveletek lehetővé teszi, hogy a háttér teljesen felülírja a minden meglévő címkéket, és cserélje le a JSON az új dokumentum `tags`.

A fenti műveletek támogatja [Az optimista feldolgozási] [ lnk-concurrency] és a **ServiceConnect** engedélyre van szükségük a [biztonsági] megadottak[ lnk-security] cikk.

Ezek a műveletek mellett a háttér kérdezheti le a twins használata SQL-szerű [lekérdezési nyelv][lnk-query], és a [feladatok]használatával twins nagy adathalmazok műveletek hajthatók végre[lnk-jobs].

## <a name="device-operations"></a>Eszköz műveletek

Az eszköz alkalmazást használja az alábbi atomi műveletek a kettős a működik:

1. **Kettős beolvasásához**. Ehhez a művelethez a kettős dokumentum tartalmát adja vissza (többek között címkék és a kívánt, a jelentett és Rendszertulajdonságok) az éppen csatlakoztatott eszköz.
2. **Részben az update jelentett tulajdonságait**. Ez a művelet lehetővé teszi, hogy a részleges analitikafrissítés az éppen csatlakoztatott eszközt jelentett tulajdonságait. Ugyanazt a JSON frissítés formátumot használja a háttér szemben lévő részleges analitikafrissítés a kívánt tulajdonságokat.
3. **Observe kívánt tulajdonságokat**. Ha értesítést szeretne kapni a kívánt tulajdonságokat frissítések, amint fordulhat elő, hogy az aktuálisan csatlakoztatott eszközt választhat. Az eszköz ugyanazon az űrlapon frissítése (teljes vagy részleges helyettesítő) hajtja végre a háttér kapja.

A fenti műveletek **DeviceConnect** engedélyre van szükségük a [biztonsági] megadottak[ lnk-security] cikk.

Az [Azure IoT eszköz SDK] [ lnk-sdks] könnyen a nyelvek és platformokon a fenti műveletek használata. További információt a kívánt tulajdonságokat szinkronizálás IoT központi primitívek részleteit a program a [eszköz újracsatlakozás folyamat][lnk-reconnection].

> [AZURE.NOTE] Jelenleg eszköz twins elérhetők csak a MQTT protokollal IoT központi csatlakozó eszközök.

## <a name="reference-topics"></a>Hivatkozás témakörök:

A következő hivatkozást témakörök nyújtanak további információt a IoT központi való hozzáférés szabályozása.

## <a name="tags-and-properties-format"></a>Címkék és tulajdonságok formázása

Címkék, kívánt és jelentett tulajdonságok e-mailek JSON objektumok az alábbi korlátozások:

* A JSON-objektumok összes kulcsok a kis-és nagybetűket 128-karakter UNICODE karakterláncok. Engedélyezett karakterek kizárása UNICODE vezérlőkaraktereket (szegmensek C0 és a C1), és `'.'`, `' '`, és `'$'`.
* JSON objektum összes értékek lehetnek az alábbi típusú JSON: logikai érték, szám, a karakterláncot, az objektumot. A tömbök nem használhatók.

## <a name="twin-size"></a>Kettős mérete

IoT központi kényszeríti-8KB méretkorlátja értékeit `tags`, `properties/desired`, és `properties/reported`, kivéve a csak olvasható elemeket.
A méret számítja ki számlálás UNICODE kivételével az összes karakter szabályozhatja a karaktereket (szegmensek C0 és a C1) és a térköz `' '` amikor egy szöveges konstans kívül jelenik meg.
IoT központi hibával elutasítja az összes művelet szeretné ezeket a dokumentumokat a korlátot feletti méretének növelése.

## <a name="twin-metadata"></a>Kettős metaadatok

IoT központi megtartja az egyes JSON objektum kívánt és jelentett tulajdonságok az utolsó frissítés időbélyeg. Az időbélyegeket UTC és [ISO8601] formátumban kódolt `YYYY-MM-DDTHH:MM:SS.mmmZ`.
Példa:

        {
            ...
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": {
                                "$lastUpdated": "2016-03-30T16:24:48.789Z"
                            },
                            "$lastUpdated": "2016-03-30T16:24:48.789Z"
                        },
                        "$lastUpdated": "2016-03-30T16:24:48.789Z"
                    },
                    "$version": 23
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": "55%",
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": "5m",
                            "status": {
                                "$lastUpdated": "2016-03-31T16:35:48.789Z"
                            },
                            "$lastUpdated": "2016-03-31T16:35:48.789Z"
                        }
                        "batteryLevel": {
                            "$lastUpdated": "2016-04-01T16:35:48.789Z"
                        },
                        "$lastUpdated": "2016-04-01T16:24:48.789Z"
                    },
                    "$version": 123
                }
            }
            ...
        }

Ez az információ megőrzése érdekében, hogy távolítsa el az objektum billentyűk frissítések legyen minden szintjén (nem csak a levelek a JSON-struktúra).

## <a name="optimistic-concurrency"></a>Optimista feldolgozási

Címkék, az összes kívánt és jelentett tulajdonságok támogatja az optimista feldolgozási.
Címkék van egy etag [RFC7232], amely a címke JSON ábrázolása szerint. Használhatja ezt a feltételes frissítési műveleteket vissza végéről összhangot.

Bejelentett és a kívánt tulajdonság nincs etags, de van egy `$version` érték, amely növekvő garantált. Adatproblémák egy etag, hogy a verziója használható például (egy eszköz alkalmazás jelentett Tulajdonságok), és a háttér kívánt tulajdonság frissítési fél meg lehessen őrizni a frissítések következetességét.

Változatokat is lehet hasznos, ha egy observing ügynök (többek között az eszköz alkalmazás betartásával a kívánt tulajdonságokat) egyeztetéséhez fajok a lekérés művelet eredménye és a frissítési értesítést között van. A szakasz [eszköz újracsatlakozás folyamat] [ lnk-reconnection] tartalmaz további tájékoztatást.

## <a name="device-reconnection-flow"></a>Eszköz újracsatlakozás továbbításához

IoT központi nem őrzi meg a kívánt tulajdonságokat frissítési értesítések leválasztott eszközök esetén. Következik, hogy a kapcsolódik eszközt kell beolvasni a teljes a kívánt tulajdonságokat dokumentum mellett a frissítési értesítések előfizetés. Frissítési értesítések és a teljes lekérés között fajok lehetőséget adni, a következő folyamat biztosítani kell:

1. Eszköz alkalmazás IoT-hubhoz csatlakozik.
2. Eszköz alkalmazás frissítési értesítések előfizetője a kívánt tulajdonságokat.
3. Eszköz alkalmazás olvassa be a teljes dokumentumban a kívánt tulajdonságokat.

Az eszköz alkalmazás az összes értesítést figyelmen kívül hagyhatja `$version` kisebb vagy egyenlő, mint a teljes beolvasott dokumentum változatát. Ennek oka lehet IoT központi garantálja, hogy mindig növelése verziók.

> [AZURE.NOTE] Ez a módszer már bekerül az [Azure IoT eszköz SDK][lnk-sdks]. Ez a leírás akkor hasznos, csak akkor, ha a az eszköz alkalmazás Azure IoT eszköz SDK nem használható, és a MQTT felület közvetlenül programot kell.

## <a name="additional-reference-material"></a>További anyagminta

A Fejlesztőeszközök útmutató hivatkozás témakörök a következők:

- [IoT központi végpontok] [ lnk-endpoints] ismerteti a különböző végpontok, amely az egyes IoT központi közzététele futtatókörnyezet és kezelés műveletekhez.
- [Szabályozási és kvóták] [ lnk-quotas] a IoT központi szolgáltatás és számíthat, amikor a szolgáltatás használatához a szabályozási viselkedés vonatkozó kvóták ismerteti.
- [IoT központi eszközök és szolgáltatások SDK] [ lnk-sdks] a különböző nyelvi SDK sorolja fel, egy eszköz és a szolgáltatás-alkalmazások használata a IoT központi fejlesztésekor használja.
- [Lekérdezési nyelv twins, módszerek és feladatok] [ lnk-query] ismerteti a lekérdezésnyelv IoT központi eszköz twins, módszerek és feladatok kapcsolatos adatok kinyerése is használhatja.
- [IoT központi MQTT támogatási] [ lnk-devguide-mqtt] IoT központi támogatási további információt tartalmaz a MQTT protokollhoz.

## <a name="next-steps"></a>Következő lépések

Most már meg van értesült az eszköz twins, amelyek érdekelhetik a fejlesztői útmutató a következő témakörök:

- [Közvetlen metódus eszközön][lnk-methods]
- [A feladatok ütemezése több eszközön][lnk-jobs]

Ha azt szeretné, próbálja meg néhány a jelen cikkben ismertetett fogalmak, amelyek érdekelhetik az alábbi IoT központi oktatóanyag:

- [Az eszköz kettős használata][lnk-twin-tutorial]
- [Kettős tulajdonságainak használata][lnk-twin-properties]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-identity]: iot-hub-devguide-identity-registry.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-security]: iot-hub-devguide-security.md

[ISO8601]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-twin-metadata]: iot-hub-devguide-device-twins.md#twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[lnk-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow

[img-twin]: media/iot-hub-devguide-device-twins/twin.png