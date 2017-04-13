<properties
 pageTitle="Fejlesztőeszközök útmutató - üzenetküldés |} Microsoft Azure"
 description="Azure IoT központi Fejlesztőeszközök útmutató - eszköz a felhőbe, és felhőalapú-eszköz üzenetküldés"
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

# <a name="send-and-receive-messages-with-iot-hub"></a>Üzenetek küldése és fogadása a IoT központi

## <a name="overview"></a>– Áttekintés

IoT központi a következő üzenetben primitívek kommunikálni egy eszközt kínál:

- [Eszköz a felhőbe] [ lnk-d2c] az alkalmazás a eszközről biztonsági célból.
- [Felhőalapú-eszköz] [ lnk-c2d] alkalmazásokból biztonsági vége (*service* vagy *felhőalapú*).

Alapvető tulajdonságai IoT központi üzenetkezelés funkcióihoz, megbízhatóságának és az üzenetek tartóssági. A tulajdonságok engedélyezése a szakaszos kapcsolódását eszköz oldalán, és töltse be az esemény feldolgozása a felhőben oldalon kiugrásainak megfelelő címtárfrissítések. IoT központi hajtja végre *legalább egyszer* kézbesítési biztosítékokat az eszköz a felhőbe és a felhő-eszköz üzenetküldés használatára.

IoT központi támogatja a több [eszköz elérésű protokollok] [ lnk-protocols] (például MQTT AMQP és HTTP). Zökkenőmentes együttműködés keresztül protokollok támogatásához IoT központi [közös üzenet formátuma] határozza meg[ lnk-message-format] összes eszköz elérésű protokollt támogató.

IoT központi közzététele egy [központi-kompatibilis esemény végpont] [ lnk-compatible-endpoint] ahhoz, hogy olvassa el az eszköz a felhőbe üzeneteket a központi megkapta az alkalmazások háttéradatbázist.

### <a name="when-to-use"></a>Mikor érdemes használni

A fő videofunkcióinak IoT központi üzenetküldés. Akkor, amikor küldésére az eszközről a hátsó végére, vagy a vissza vége üzeneteket küldeni egy eszközt kell.

A központi IoT és esemény hubok szolgáltatások összehasonlítása érdekében című [IoT-összehasonlító központi és esemény][lnk-compare].

## <a name="device-to-cloud-messages"></a>Üzenetek eszköz a felhőbe

Egy eszköz elérésű végpontot (**/devices/ {deviceId} / üzenetek/események**) keresztül eszköz a felhőbe üzenetek küldése A háttéradatbázist szolgáltatásban kap egy szolgáltatás elérésű végpontot (**/messages/events**), [Esemény hubok]szolgáltatással kompatibilis eszköz a felhőbe üzeneteinek[lnk-event-hubs]. Ezért, használhatja a szabványos [esemény hubok integráció és SDK] [ lnk-compatible-endpoint] eszköz a felhőbe üzeneteket fogadni.

IoT központi alkalmazza az eszköz a felhőbe üzenetküldés oly módon, amely hasonlít az [Esemény hubok][lnk-event-hubs]. IoT központi eszköz a felhőbe levelei esemény hubok *események* például több [Szolgáltatás Bus] [ lnk-servicebus] *üzeneteket*.

Ez a megvalósítás a következő szempontokat foglalja magában:

* Hasonlóképpen esemény hubok eseményeket, eszköz a felhőbe üzeneteket, hogy tartós és megőrzött IoT-központban hét napig (lásd: az [eszköz a felhőbe konfigurációs beállítások][lnk-d2c-configuration]).
* Eszköz a felhőbe üzeneteket több készletét partíciót létrehozáskor beállított vannak particionálva (lásd: az [eszköz a felhőbe konfigurációs beállítások][lnk-d2c-configuration]).
* Adatproblémák esemény agyváltók, hogy eszköz a felhőbe üzenetek olvasása ügyfelek kell kezelni partíciók és ellenőrzésipont. Lásd: az [Esemény hubok - események használata más][lnk-event-hubs-consuming-events].
* Esemény hubok eseményeket, például eszköz a felhőbe üzenetek legfeljebb 256 KB is lehet, és egy csoportba történő optimalizálásához küld csoportosíthatók. Csoportba legfeljebb 256 KB és legfeljebb 500 üzenetekre lehet.

Létezik, azonban néhány fontos különbség IoT központi eszköz a felhőbe üzenetküldés és esemény hubok között:

* [IoT központi való hozzáférés szabályozása] című cikkben ismertetett módon[ lnk-devguide-security] szakaszban IoT központi lehetővé teszi, hogy eszköz-hitelesítés és a hozzáférés-vezérlés.
* IoT központi lehetővé teszi, hogy egyidejűleg csatlakoztatott eszközök milliónyi (lásd: [kvótáinak és szabályozásának][lnk-quotas]), míg az esemény hubok 5000 AMQP kapcsolatok száma névtér korlátozódik.
* IoT központi nem engedi tetszőleges szétválasztás egy **PartitionKey**használatával. Eszköz a felhőbe üzenetek particionálva van, azok származó **deviceId**alapján.
* Méretezés IoT központi akkor némileg eltérő esemény hubok méretezését. További tudnivalókért lásd: a [Méretezés IoT központi][lnk-guidance-scale].

> [AZURE.NOTE] Az esemény hubok IoT központi minden esetben nem helyettesítheti. Ha például az egyes események feldolgozása számítások szükség lehet egy másik tulajdonság vagy mező részletez események újrapartícionálása elemzése a adatfolyamok előtt. Ebben az esetben, ha segítségével egy esemény központi decouple folyamat feldolgozása adatfolyam két részre. További tudnivalókért lásd: a *partíciók* az [Azure esemény hubok áttekintés][lnk-eventhub-partitions].

Eszköz a felhőbe üzenetküldés, használata című témakör tartalmaz [IoT központi API -k és SDK][lnk-sdks].

> [AZURE.NOTE] Amikor HTTP eszköz a felhőbe küldésére, tulajdonságok neve és értékek csak ASCII alfanumerikus karaktereket tartalmazhat, valamint ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}``.

### <a name="non-telemetry-traffic"></a>Nem-telemetriai forgalom

Gyakran kívül telemetriai adatpontok, eszközöket is üzeneteket küldhet, és végrehajtás és az alkalmazás üzleti logikai réteg kezelése igénylő kérelmek. Ha például kritikus figyelmeztetések, amelyek a kell elindítani az vissza célból vagy a háttér küldünk parancsokra eszköz adott válaszok egy adott művelet.

A legjobb módja az ilyen típusú üzenet feldolgozása további információkért lásd a [oktatóprogram: IoT központi eszköz a felhőbe üzenetek feldolgozási módjának] [ lnk-d2c-tutorial] oktatóprogram.

### <a name="device-to-cloud-configuration-options"></a>Eszköz a felhőbe beállítási lehetőségek

Egy IoT központi közzététele szabályozására eszköz a felhőbe üzenetküldés engedélyezése a következő tulajdonságokat.

* **Partition száma**. Ezt a tulajdonságot állítsa létrehozási meghatározhatja eszköz a felhőbe esemény szempontjából partíciót számát.
* **Adatmegőrzési idő**. Ez a tulajdonság megadja az eszköz a felhőbe üzenetek adatmegőrzési időpontját. Az alapértelmezett érték egy nap, de a hét napig növelhető.

Ezenkívül adatproblémák esemény agyváltók, hogy IoT központi lehetővé teszi, hogy kezelése eszköz-felhő fogyasztói-csoportok kapnak a végpontot.

Módosíthatja az összes alábbi tulajdonságait, vagy programozás útján a [IoT központi erőforrás szolgáltató REST API -khoz][lnk-resource-provider-apis], vagy az [Azure portal]segítségével[lnk-management-portal].

### <a name="anti-spoofing-properties"></a>Anti hamisított tulajdonságai

Az eszköz a felhőbe az üzenetek IoT központi hamisított eszköz elkerülése érdekében az összes bélyegzőket üzeneteket a következő tulajdonságokat:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

Az első két tartalmaznia **deviceId** és **generationId** a származó eszköz szerinti [eszköz identitás tulajdonságai][lnk-device-properties].

A **ConnectionAuthMethod** tulajdonság szerializálásának JSON-objektumra, és a következő tulajdonságokat tartalmazza:

```
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

## <a name="cloud-to-device-messages"></a>Üzenetek cloud-eszköz

Elküldheti a felhő-eszköz üzeneteket, a szolgáltatás elérésű végpont (**/messages/devicebound**) keresztül. Egy eszközt az eszközre jellemző egyik végpontját (**/devices/ {deviceId} / üzenetek/devicebound**) keresztül kapja őket.

Minden cloud-eszköz üzenet **/devices/ {deviceId} / üzenetek/devicebound** **a tulajdonság** beállításával alkalmazásindítójukban egyetlen eszközt.

>[AZURE.IMPORTANT] Minden eszköz sor legfeljebb 50 cloud-eszköz üzenetek tárolja. Próbálja meg több üzenetet küldeni az azonos eszköz hibaérték az eredmény.

> [AZURE.NOTE] Felhőalapú-eszköz üzenetek küldésére tulajdonságok neve és értékek csak ASCII alfanumerikus karaktereket tartalmazhat, valamint ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}``.

### <a name="message-lifecycle"></a>Üzenet életciklus

Zökkenőmentes legalább egyszer üzenetkézbesítés, a IoT központi továbbra is fennáll a felhőben-eszköz eszköz üzeneteket. Eszközök kifejezetten kell nyugtázza IoT központi kattintva eltávolíthatja őket a sor *befejezését* . Ez garantálja tűrőképessége kapcsolódási és eszköz hibák szemben.

Az alábbi ábra mutatja a felhőben-eszköz üzenet életciklus állapotát ábrázoló.

![Felhőalapú-eszköz üzenet életciklus][img-lifecycle]

A szolgáltatás üzenetet küld, amikor célszerű *sorbaállítva*. Amikor egy eszközt szeretne *kap* egy üzenetet, IoT központi *zárolás:* az üzenet (állítja az állapot **láthatatlan**) lehetővé teszi más szálak ugyanarra az eszközre más üzenetek fogadására indításához. Az üzenet feldolgozása eszköz szál befejeztével *befejezése* az üzenet jelzi IoT központi.

Egy eszközt is van lehetősége:

- *Utasítani* az üzenetet, állítsa be a **Deadlettered** állam IoT elosztót okozó. Megjegyzés: a MQTT csatlakozó eszközök nem elvetése C2D üzeneteket.
- Az üzenet az üzenet helyezze vissza a sorban állapotával IoT központi okozó állítsa *elhagyja* **sorbaállítva**.

A szál üzenet feldolgozása IoT központi értesítő leállhat. Ebben az esetben üzeneteket automatikusan való áttérés **láthatatlan** állapotát a **sorbaállítva** állapotába *láthatóság (vagy a zárolás) időtúllépés*után. Az időtúllépés alapértelmezett értéke egy perc alatt.

Üzenet is lehet a **sorbaállítva** és **láthatatlan** között államok számára legfeljebb hány alkalommal IoT központi a **kézbesítési maximális száma** tulajdonságában megadott. Adott számú áttűnések után IoT központi állítja az állapot, az üzenet **Deadlettered**. Hasonlóképpen, IoT központi állítja be az üzenet állam **Deadlettered** a lejárati idő után (lásd: [élettartam][lnk-ttl]).

A felhő-eszköz üzenetekre tanulásra, lásd: [oktatóprogram: IoT központi cloud-eszköz üzenet küldése annak][lnk-c2d-tutorial]. A különböző API-k és SDK referenciaanyagokat jelenítik meg a felhőben-eszköz használható funkciók körét, lásd [IoT központi API -k és SDK][lnk-sdks].

> [AZURE.NOTE] Általában cloud-eszköz üzenetek végrehajtani, amikor az üzenet elvesztését nem befolyásolja az alkalmazás logika. Az üzenet tartalmának sikeresen állandó helyi tárolóban lévő, vagy a művelet sikeresen végrehajtása. Az üzenet is végző tranziens információkat, amelyek elvesztését volna nem a más funkciókra is hatással az alkalmazás. Időnként előfordulhat hosszabb ideig futó feladatokhoz hajthatja végre a felhőben-eszköz üzenet után a tevékenység leírásának helyi tároló pályától tartós. Kattintson a kérelem háttér egy vagy több eszközön a felhőbe üzenetek értesítheti különböző szakaszaiban a tevékenység előrehaladását.

### <a name="message-expiration-time-to-live"></a>Üzenet elévülési (Live idő)

Minden cloud-eszköz üzenetnek elévülési időt. Ez esetben értéke (a tulajdonságban **ExpiryTimeUtc** ) szolgáltatás vagy IoT központi által megadott IoT központi tulajdonság beállítása alapértelmezett *élettartam* használatával. Lásd: a [felhő-eszköz konfigurációs beállítások][lnk-c2d-configuration].

> [AZURE.NOTE] A közös üzenet elévülési előnyeit, és el kívánja kerülni üzeneteket küldő leválasztott eszközök, módja értékek élő rövid idő beállítása. Ezt a megközelítést éri el a eszköz kapcsolati állapotának fenntartása, miközben hatékonyabb is ugyanazt az eredményt. Ha üzenet nyugták kér, IoT központi jelzi, mely eszközök fogadhat üzeneteket, és milyen eszközöket nem érhetők el vagy sikertelen volt.

### <a name="message-feedback"></a>Üzenet visszajelzés

Ha egy felhőalapú-eszköz üzenetet küld, a szolgáltatás kérhet kézbesítve az üzenet visszajelzés vonatkozó megjelölt üzenetek végleges állapotát.

- Ha a **nyomon követés** tulajdonság **pozitív**, a IoT központi visszajelzés üzenet hoz létre, és csak ha, a felhőben-eszköz üzenet elérhető **kész** állapotát.
- Ha a **nyomon követés** tulajdonság a **negatív**, IoT központi üzenet, visszajelzést készít, csak ha, a felhőben-eszköz üzenet elér **Deadlettered** állapotát.
- Ha a **nyomon követés** tulajdonság **teljes**, IoT központi visszajelzés üzenet mindkét esetben készít.

> [AZURE.NOTE] Ha **Ack** **teljes**, és nem kap visszajelzést, az azt jelenti, hogy a visszajelzés üzenet le. A szolgáltatás nem tudja, mi történt az eredeti üzenetet. A gyakorlatban szolgáltatás kell győződjön meg arról, hogy akkor is dolgozza a visszajelzést, még mielőtt lejárna. A maximális lejárati idő két nap, amely lehetővé teszi, hogy sok időt szánni a szolgáltatás fut ismét hiba esetén.

A [Végpontok]című cikkben ismertetett módon[lnk-endpoints], IoT központi biztosítja a szolgáltatás elérésű végpont (**/messages/servicebound/feedback**) keresztül visszajelzés üzeneteket. Visszajelzés fogadására szemantikáját ugyanaz, mint a felhőben-eszköz üzeneteket, és a ugyanazon [üzenet életciklus][lnk-lifecycle]. Amikor csak lehetséges, üzenet, visszajelzést kötegekbe foglalja a egy üzenetben, a következő formátumban:

| A tulajdonság | Leírás |
| -------- | ----------- |
| EnqueuedTime | Az üzenet létrehozásakor jelző időbélyeg. |
| Felhasználóazonosító | `{iot hub name}` |
| ContentType | `application/vnd.microsoft.iothub.feedback.json` |

A szervezet egy JSON szerializáltként tömböt a rekordok, a következő tulajdonságokat mindegyiket:

| A tulajdonság | Leírás |
| -------- | ----------- |
| EnqueuedTimeUtc | Időbélyeg, jelezve, hogy mikor történt a az így létrejövő az üzenetet. Ha például az eszköz befejeződött, vagy az üzenet lejárt. |
| OriginalMessageId | A felhő-eszköz üzenet, amelyhez ezt az információt a visszajelzés újraszámítja **MessageId** . |
| StatusCode | Szükséges egész szám. Visszajelzés üzenetekben IoT központi által generált használatos. <br/> 0 = success <br/> 1 = üzenet lejárt <br/> 2 = maximális kézbesítési túllépve <br/> 3 = elutasítva üzenet |
| Leírás | Karakterlánc-értékek **StatusCode**. |
| DeviceId | A felhő-eszköz üzenet, visszajelzést ezen részének vonatkozik, amelyhez a célként megadott eszköz **DeviceId** . |
| DeviceGenerationId | A felhő-eszköz üzenet, visszajelzést ezen részének vonatkozik, amelyhez a célként megadott eszköz **DeviceGenerationId** . |


>[AZURE.IMPORTANT] A szolgáltatás meg kell adnia egy **MessageId** engedélyezni szeretné a visszajelzés összehangolására az eredeti üzenetet az a felhő-eszköz üzenet.

A következő példa bemutatja a visszajelzés üzenet törzsét.

```
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

### <a name="cloud-to-device-configuration-options"></a>Felhőalapú-eszköz beállítási lehetőségek

Minden egyes IoT központi közzététele a következő beállítási lehetőségei cloud-eszköz üzenetküldés.

| A tulajdonság | Leírás | Tartomány- és alapértelmezett |
| -------- | ----------- | ----------------- |
| defaultTtlAsIso8601 | Alapértelmezett TTL cloud-eszköz üzenetek. | ISO_8601 intervallum felfelé (2D) (1 minimális perc). Alapértelmezés: 1 óra értéket. |
| maxDeliveryCount | Felhőalapú-eszköz per-eszköz sorban várakozó kézbesítési maximális száma. | 1 és 100 között. Alapértelmezés: 10. |
| feedback.ttlAsIso8601 | Adatmegőrzési szolgáltatás kötött visszajelzés üzenetek. | ISO_8601 intervallum felfelé (2D) (1 minimális perc). Alapértelmezés: 1 óra értéket. |
| feedback.maxDeliveryCount | Visszajelzés várólista kézbesítési maximális száma. | 1 és 100 között. Alapértelmezett: 100. |

További tudnivalókért lásd: [Hozzon létre IoT hubok][lnk-portal].

## <a name="read-device-to-cloud-messages"></a>Eszköz a felhőbe üzenetek olvasása

IoT központi közzététele a központi megkapta az eszköz a felhőbe-üzenetek olvasása a háttéradatbázist szolgáltatások zárólap. A végpontot, az esemény az esemény hubok szolgáltatás lehetővé teszi, hogy mechanizmusok központi-kompatibilis üzenetek olvasása támogatja.

Az [Azure Service Bus SDK a .NET rendszerhez] használatakor[ lnk-servicebus-sdk] vagy [Esemény hubok - esemény processzor Host][lnk-eventprocessorhost], a megfelelő engedélyekkel rendelkező minden olyan központi IoT csatlakozási_karakterlánc is használhatja. Ezután használja az **üzenetek/események** esemény központi nevet.

SDK (vagy a termék integrációs) használata esetén, amelyek IoT központi tudomása, egy esemény központi-kompatibilis végpontot, és az esemény központi-kompatibilis nevét kell beolvasni a IoT központi beállításait az [Azure portál][lnk-management-portal]:

1. Kattintson a IoT központi lap **üzenetküldés**.
2. **Eszköz a felhőbe beállításai** csoportban az alábbi értékek megkeresése: **esemény központi-kompatibilis végpontot**, **esemény központi-kompatibilis nevét**és **partíciót**.

    ![Eszköz a felhőbe beállításai][img-eventhubcompatible]

> [AZURE.NOTE] Ha a SDK **Hostname (állomásnév)** vagy **Namespace** érték van szüksége, a rendszer eltávolítása az **esemény központi-kompatibilis végpontot**. Ha például az esemény központi-kompatibilis végpont **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, a **Hostname (állomásnév)** **iothub-ns-myiothub-1234.servicebus.windows.net**lenne, és a **Namespace** **iothub-ns-myiothub-1234**lenne.

Kattintson bármelyik megosztott biztonsági házirendet, amely tartalmazza az adott esemény-központban összekapcsolása az **ServiceConnect** engedélyekkel használható.

Ha szeretne egy esemény központi kapcsolati karakterlánc készíthet az előző adatokat használatával, a következő mintát használhatja:

```
Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}
```

Az alábbiakban látható SDK és integrációs IoT központi közzététele esemény központi-kompatibilis végpontok használható:

* [Java esemény hubok ügyfél](https://github.com/hdinsight/eventhubs-client)
* [Apache vihar spout](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md). Megtekintheti az [adatforrás spout](https://github.com/apache/storm/tree/master/external/storm-eventhubs) GitHub.
* [A külső Apache-integráció](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md)

## <a name="reference-topics"></a>Hivatkozás témakörök:

Az alábbi hivatkozás témakörök tartalmaznak, az üzenetek cseréje a IoT központi további információt.

## <a name="message-format"></a>Üzenet formátuma

IoT központi üzenetek foglalja magában:

* *A Rendszertulajdonságok*csoportja. Az tulajdonságokat IoT központi értelmezi vagy állítja be. Ez a beállítás előre meghatározott.
* *Szolgáltatásalkalmazás tulajdonságainak*csoportja. Az alkalmazás meghatározó karakterlánc tulajdonságok és a hozzáférés, anélkül, hogy az üzenettörzs deszerializálni szótár. IoT központi soha nem módosítja a tulajdonságokból.
* Átlátszó bináris törzsébe.

Hogyan címként van kódolva az üzenetet a különböző protokollok kapcsolatos további tudnivalókért lásd: a [IoT központi API -k és SDK][lnk-sdks].

Az alábbi táblázat a Rendszertulajdonságok IoT központi üzenetek készlete.

| A tulajdonság | Leírás |
| -------- | ----------- |
| MessageId | Az üzenet esetén a kérés / válasz mintázatok használt felhasználói állítható be azonosítója. Formátum: Kis-és nagybetűket karakterlánc (legfeljebb 128 karakter hosszú) ASCII-7 bites alfanumerikus karaktereket + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| Sorszám | Minden cloud-eszköz üzenethez IoT központi által megadott szám (egyedi egy eszköz várólista). |
| A | A [Felhőben-eszköz] megadott célhelyének[ lnk-c2d] üzeneteket. |
| ExpiryTimeUtc | Dátum és idő: az üzenet elévülési. |
| EnqueuedTime | Dátum és idő az üzenetet megkapta IoT központi. |
| CorrelationId | A kérelem jóváhagyásához kérés / válasz mintázatok, a MessageId általában tartalmazó válaszüzenetet a karakterlánc-tulajdonság. |
| Felhasználóazonosító | Itt adhatja meg az üzenetek forrását azonosító. Üzenetek IoT központi hozza létre, amikor beállítás `{iot hub name}`. |
| Nyomon követés | A visszajelzés üzenet nyilvántartás-készítő alkalmazás. Ez a tulajdonság használják cloud-eszköz üzenetek IoT központi létrehozhat visszajelzés üzeneteket a felhasználás az üzenet eredményeként kérhet az eszköz. A lehetséges értékek: **none** (alapértelmezett): Nincs visszajelzés üzenet jön létre, **pozitív**: Ha befejeződött üzenet, visszajelzést kap **negatív**: visszajelzés üzenet jelenik meg, ha az üzenet lejárt (vagy kézbesítési maximális száma elérte) nélkül végzi az eszközt, vagy **teljes**: pozitív és negatív egyaránt. További tudnivalókért lásd: az [üzenet, visszajelzést][lnk-feedback]. |
| ConnectionDeviceId | Az eszköz a felhőbe üzenetekre IoT központi által meghatározott azonosítója. Az eszköz az üzenetet küldő a **deviceId** tartalmaz. |
| ConnectionDeviceGenerationId | Az eszköz a felhőbe üzenetekre IoT központi által meghatározott azonosítója. A **generationId** tartalmaz (szerinti [eszköz identitás tulajdonságai][lnk-device-properties]) az eszköz az üzenetet küldő. |
| ConnectionAuthMethod | Az eszköz a felhőbe üzenetekre IoT központi által meghatározott hitelesítési módszer. Ez a tulajdonság a hitelesítési módszer az eszközre az üzenetet küldő hitelesíti információkat tartalmazza. További tudnivalókért lásd: a [felhőbe anti hamisított eszköz][lnk-antispoofing].|

## <a name="communication-protocols"></a>Kapcsolati protokollok

IoT központi lehetővé teszi, hogy az eszköz [MQTT][lnk-mqtt], WebSockets, [AMQP]fölé MQTT[lnk-amqp], WebSockets és a HTTP AMQP protokollok eszköz melletti kommunikáció. Az alábbi táblázat ismerteti a magas szintű javaslatok választási protokoll:

| Protocol (protokoll) | Ha meg kell protokoll |
| -------- | ------------------------------------ |
| MQTT <br> MQTT WebSocket keresztül     | Használja az összes eszközön, amelyekhez nincs szükség az azonos TLS-kapcsolaton keresztül különféle eszközökön (minden saját eszköz hitelesítő adatokkal) csatlakozni. |
| AMQP <br> AMQP WebSocket keresztül    | Használja a mező és a felhő átjárók minden eszközön multiplex kapcsolat előnyeit. |
| HTTP    | Más protokollok nem támogató eszköz használható. |

Ha úgy dönt, hogy az eszköz melletti kommunikáció protokollt, vegye figyelembe az alábbiakat:

* **Felhőalapú-eszköz mintát**. HTTP nincsenek hatékonyan server leküldéses végrehajtásához. Ilyen HTTP használatakor eszközök lekérdezik IoT központi cloud-eszköz üzenetek. Ez a módszer hatékony eszköz és IoT központi. Az aktuális HTTP irányelveihez az egyes üzenetek kell lekérdezik 25 percenként vagy több. Kézzel, MQTT és AMQP támogatása server leküldéses cloud-eszköz üzenetek fogadásakor. Az üzenetek azonnali veremmutatót az eszközre IoT központból lehetővé teszik. Kézbesítési késés fontos, MQTT vagy AMQP esetén a legjobb protokollokat használni. Ritkán csatlakoztatott eszközök HTTP is működik.
* **A mező átjárók**. MQTT és a HTTP használata esetén (minden saját eszköz hitelesítő adatokkal) különféle eszközökön nem tud kapcsolódni a azonos TLS-kapcsolaton keresztül. Ezért a [mező átjáró alkalmazási helyzetek][lnk-azure-gateway-guidance], ezek a protokollok optimálisnál, mivel azok van szüksége, egy mező átjáró csatlakozik az egyes eszközök között a mező átjáró és IoT központi TLS-kapcsolatot.
* **Alacsony erőforrás eszközöket**. A MQTT és a HTTP-tárak egy kisebb, mint a AMQP tárak helyigénye van. Például az eszköz korlátozott anyagok (például-nál kisebb 1 MB RAM), ezek a protokollok a csak protokoll végrehajtása lehet.
* **Hálózati átviteli**. A szabványos AMQP protokoll használja a port 5671, miközben MQTT-as port 8883, amelyek problémákat okozhatnak a-HTTP protokollok lezárt hálózatok. MQTT WebSockets fölé, WebSockets és a HTTP AMQP érhetők el ebben az esetben használható.
* **Tartalom mérete**. MQTT és AMQP olyan bináris protokollok, amely a HTTP-nél több kompakt tartalmak számára.

> [AZURE.NOTE] HTTP használata esetén az egyes az 25 percenként vagy több kell lekérdezik cloud-eszköz üzenetek. Során fejlesztése, célszerű azonban elfogadható gyakrabban 25 percenként lekérdezik.

## <a name="port-numbers"></a>Portszámokat

Eszközök kommunikálni IoT különböző protokollok használatával Azure-központját. A választott protokoll hajtja általában az adott követelményeket, a megoldás. Az alábbi táblázat meg kell nyitni egy eszköz adott protokoll használatát engedélyezni szeretné a kimenő portokat:

| Protocol (protokoll) | Port |
| -------- | ------- |
| MQTT     | 8883    |
| MQTT WebSockets keresztül | 443-as    |
| AMQP     | 5671    |
| AMQP WebSockets keresztül | 443-as    |
| HTTP     | 443-as     |
| LWM2M (kezelés) | 5684 |

Miután létrehozott egy IoT központi Azure területen, a központi az adott elosztót élettartama továbbra is ugyanazt a címet. Azonban megőrzéséhez szolgáltatás, minősége, ha a Microsoft helyezi át a IoT központi be más időosztás egységet majd ezt a jogosultságot új IP-címet.

## <a name="notes-on-mqtt-support"></a>Jegyzetek MQTT támogatás

IoT központi hajtja végre az alábbi korlátozások vonatkoznak és az adott viselkedést MQTT v3.1.1 protokollt:

  * **QoS 2 nem támogatott**. Amikor egy eszközt ügyfél **QoS**2 üzenet teszi közzé, a IoT központi bezárása a hálózati kapcsolat. Amikor egy eszközt ügyfél **QoS**2 témakör előfizetője, IoT központi hozzárendeli a maximális QoS 1-es szint a **SUBACK** csomagban.
  * Az **üzenetek megtartása nem marad meg**. Ha eszköz ügyfél közzé egy üzenet, a MEGTARTÁSA jelölővel értéke 1, IoT központi összeadja az **x-kiválaszthatják-megőrzi az** alkalmazás tulajdonság az üzenethez. Ebben az esetben IoT központi nem marad meg az megtartása üzenetet, de inkább átadja a háttéradatbázist alkalmazást.

További tudnivalókért lásd: [IoT központi MQTT támogatja a][lnk-devguide-mqtt].

A végleges szempontokat, tekintse át az [Azure IoT protokoll átjáró] [ lnk-azure-protocol-gateway] , amely lehetővé teszi, hogy közvetlenül a IoT központi felületek nagy teljesítményű egyéni protokoll átjáró telepítése. Az Azure IoT protokoll átjáró lehetővé teszi, hogy az eszköz protokoll brownfield MQTT telepítések igazodik vagy más egyéni protokollok testre. Ezt a megközelítést kell-e, azonban futtatja, és egy egyéni protokoll átjáró működik.

## <a name="additional-reference-material"></a>További anyagminta

A Fejlesztőeszközök útmutató hivatkozás témakörök a következők:

- [IoT központi végpontok] [ lnk-endpoints] ismerteti a különböző végpontok, amely az egyes IoT központi közzététele futtatókörnyezet és kezelés műveletekhez.
- [Szabályozási és kvóták] [ lnk-quotas] a IoT központi szolgáltatás és számíthat, amikor a szolgáltatás használatához a szabályozási viselkedés vonatkozó kvóták ismerteti.
- [IoT központi eszközök és szolgáltatások SDK] [ lnk-sdks] a különböző nyelvi SDK sorolja fel, egy eszköz és a szolgáltatás-alkalmazások használata a IoT központi fejlesztésekor használja.
- [Lekérdezési nyelv twins, módszerek és feladatok] [ lnk-query] ismerteti a lekérdezésnyelv IoT központi eszköz twins, módszerek és feladatok kapcsolatos adatok kinyerése is használhatja.
- [IoT központi MQTT támogatási] [ lnk-devguide-mqtt] IoT központi támogatási további információt tartalmaz a MQTT protokollhoz.

## <a name="next-steps"></a>Következő lépések

Üzenetek küldése és fogadása a IoT központi miként van megtanulta, most, amelyek érdekelhetik a fejlesztői útmutató a következő témakörök:

- [A eszközről fájlok feltöltése][lnk-devguide-upload]
- [Eszköz identitások IoT-központban kezelése][lnk-devguide-identities]
- [IoT központi való hozzáférés szabályozása][lnk-devguide-security]
- [Állapot és a beállítások szinkronizálása az eszköz twins használatával][lnk-devguide-device-twins]
- [Közvetlen metódus eszközön][lnk-devguide-directmethods]
- [A feladatok ütemezése több eszközön][lnk-devguide-jobs]

Ha azt szeretné, próbálja meg néhány a jelen cikkben ismertetett fogalmak, amelyek érdekelhetik az alábbi IoT központi oktatóanyag:

- [Azure IoT központi – első lépések][lnk-getstarted-tutorial]
- [Hogyan IoT központi cloud-eszköz üzenet küldése][lnk-c2d-tutorial]
- [Hogyan IoT központi eszköz felhő folyamat során][lnk-d2c-tutorial]


[img-lifecycle]: ./media/iot-hub-devguide-messaging/lifecycle.png
[img-eventhubcompatible]: ./media/iot-hub-devguide-messaging/eventhubcompatible.png

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-azure-gateway-guidance]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-guidance-scale]: iot-hub-scaling.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md
[lnk-amqp]: http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf
[lnk-mqtt]: http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-event-hubs-consuming-events]: ../event-hubs/event-hubs-programming-guide.md#event-consumers
[lnk-management-portal]: https://portal.azure.com
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-eventhub-partitions]: ../event-hubs/event-hubs-overview.md#partitions
[lnk-portal]: iot-hub-create-through-portal.md

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-compatible-endpoint]: iot-hub-devguide-messaging.md#read-device-to-cloud-messages
[lnk-protocols]: iot-hub-devguide-messaging.md#communication-protocols
[lnk-message-format]: iot-hub-devguide-messaging.md#message-format
[lnk-d2c-configuration]: iot-hub-devguide-messaging.md#device-to-cloud-configuration-options
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-ttl]: iot-hub-devguide-messaging.md#message-expiration-time-to-live
[lnk-c2d-configuration]: iot-hub-devguide-messaging.md#cloud-to-device-configuration-options
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle
[lnk-feedback]: iot-hub-devguide-messaging.md#message-feedback
[lnk-antispoofing]: iot-hub-devguide-messaging.md#anti-spoofing-properties
[lnk-compare]: iot-hub-compare-event-hubs.md

[lnk-devguide-upload]: iot-hub-devguide-file-upload.md
[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx


[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
