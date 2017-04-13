<properties
 pageTitle="Fejlesztőeszközök útmutató – a közvetlen módszerek |} Microsoft Azure"
 description="Azure IoT központi Fejlesztőeszközök útmutató - használata közvetlen módszerek meghívásához kódot az eszközein"
 services="iot-hub"
 documentationCenter=".net"
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="nberdy"/>

# <a name="invoke-a-direct-method-on-a-device-preview"></a>Közvetlen metódus eszközön (előzetes verzió)

## <a name="overview"></a>– Áttekintés

IoT központi lehetőséget nyújt módszerek az eszközökön a felhőből meghívásához. Módszerek a kérés / válasz kapcsolati hasonlóan, mint a HTTP-hívás eszközön jelenítik meg abban a sikeresek, vagy azonnal (a felhasználó által megadott időtúllépése) után, hogy a felhasználónak, hogy a hívás állapotának sikertelen lesz. Ez akkor hasznos, ahol a tanfolyam azonnali művelet eltér attól függően, hogy az eszköz tudott válaszolni, például egy SMS ébresztő küldi eszközre, ha egy eszközt offline (SMS küldése több drága, mint a módszerrel hívás) felhasználási területei.

Érdemes elképzelnie módszer szerint egy távoli eljáráshívás közvetlenül az eszközön. Csak olyan módszereket, amelynek eszközön végrehajtotta meghívhatja a felhőből. Ha egy eszközön, ez a módszer definiált nem rendelkező metódus megkísérel a felhőben, a módszer hívás sikertelen lesz.

Minden egyes eszköz módszer célként egyetlen eszközt. [Feladatok] [ lnk-devguide-jobs] meghívása több eszközön módszereket kínál, és módszer meghívási leválasztott eszközök sorba.

A központi IoT a **service csatlakozás** engedélyekkel rendelkező összes eszközön módszer alkalmazhatja.

### <a name="when-to-use"></a>Mikor érdemes használni

Eszköz módszerek hasonlítanak a [felhőben-eszköz üzenetek] [ lnk-devguide-messages] annak, hogy mindkettő lehetővé teszi az a felhő háttéradatbázist át az információkat egy eszközt, de vannak közöttük a különböző alapvető módjai. Ebben az értelemben módszereket szinkron és nem tartós közben cloud-eszköz üzenetek aszinkron eltarthatóság legfeljebb 48 óra.

Módszerek a hajtsa végre a kérelem-válasz mintát, és nem tartós. Tartóssági hiánya két azonnali előnyöket nyújtja, amikor eszközök, amelyek commanding:

- **Végrehajtási mód azonnali visszajelzést** azt jelenti, hogy nincs szükség az, hogy a kérelem és a válasz között korrelációs kezelése.
- **Nagyobb teljesítmény** azt jelenti, hogy a műveleteket hajthatják gyorsabb, mert IoT központi bármely tartóssági nem biztosít. IoT központi lehetővé teszi, hogy a felhő-eszköz üzenetek-nél több módszer-hívások egységnyi.

Felhőalapú-eszköz üzenetek nem feltétlenül parancsok az eszközre, de egy felhőalapú szolgáltatásba, néhány adatát bit átadása, az eszköz, ahhoz, hogy a saját szabadidő emelje fel, és amelyet az eszközre vagy nem válaszol, hanem képviseli. Felhőalapú-eszköz üzenetek hosszabb időtúllépés időt igénybe (legfeljebb 48 óra) módszerek közben gyorsan sokkal több lejár.

Azonnali parancs meghívása egy eszközön, és a feladatok parancs eszközön ütemezett meghíváshoz eszköz módszerekkel.

## <a name="method-lifecycle"></a>A módszer életciklus

Módszerek végrehajtása az eszközre, és kérheti a módszer tartalomban megfelelően elindítását nulla vagy több bemenetben. Akkor meghívása keresztül szolgáltatás elérésű URI közvetlen módszer (`{iot hub}/twins/{device id}/methods/`). Egy eszközt kap egy adott eszközhöz MQTT témakör keresztül közvetlen módszerek (`$iothub/methods/POST/{method name}/`). Előfordulhat, hogy támogatjuk módszerek további eszköz melletti hálózati protokollokat a jövőben.

> [AZURE.NOTE] Egy eszközön a közvetlen módszer meghívásakor tulajdonságok neve és értékek csak tartalmazhat USA-beli ASCII-nyomtatható alfanumerikus, kivéve a következő beállítás: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.

Módszerek állnak szinkron, és bármelyik sikeres vagy sikertelen a határidő lejárta után (alapértelmezett: 30 másodperc állítható be 3600 másodpercre). A módszereket, amelyhez egy eszközt használni, ha, és csak ha az eszköz online és fogadás parancsokat, például a telefonról fény bekapcsolásával interaktív esetekben hasznos. Ez a helyzet szeretne látni a egy azonnali sikeres vagy sikertelen, így a felhőbeli szolgáltatástól működhet az eredmény a minél korábban beállítást. Az eszköz vissza előfordulhat, hogy az egyes üzenettörzsbe a módszerrel eredményeként, de nem szükséges ezt a módszert. Nem a rendezés nem garancia vagy bármely verzió-ellenőrzési szemantikáját módszer hívásokat.

Eszköz módszer hívásokat, hogy csak HTTP felhő oldaláról, és csak MQTT eszköz oldaláról.

## <a name="reference-topics"></a>Hivatkozás témakörök:

A következő hivatkozást témakörök nyújtanak közvetlen módszerek használatáról további információt.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Közvetlen metódus háttéradatbázis alkalmazásból

### <a name="method-invocation"></a>A módszer meghívása

Közvetlen módszer meghívásához eszközön HTTP hívásokat foglalja magában:

- Az eszköz adott *URI* (`{iot hub}/twins/{device id}/methods/`)
- A bejegyzés *módszer*
- *Fejlécek* tartalmazó az engedély kérése azonosítója, a webhelytartalom-típus és a tartalom kódolást
- Egy áttetsző JSON *törzsébe* a következő formátumban:

```
{
    "methodName": "reboot",
    "timeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}
```

  Időtúllépési a másodpercben megadott érték. Ha nincs beállítva a időtúllépés, lesz az alapértelmezett 30 másodperces.
  
### <a name="response"></a>Válasz

A háttéradatbázist magában foglaló választ kap:

- *HTTP-állapotkód*, használt lesz a IoT központból, például egy 404-es hiba jelenleg nem csatlakozó eszközök hibákkal
- Tartalmazza a etag, *élőfejek* azonosítója, a webhelytartalom-típus és a tartalom kódolást kérése
- A JSON *törzsébe* a következő formátumban:

```
{
    "status" : 201,
    "payload" : {...}
}
```
  
   Mindkét `status` és `body` az eszköz által biztosított, és használja az eszköz saját állapotkód és/vagy a leírás válaszolni.

## <a name="handle-a-direct-method-on-a-devcie"></a>Kattintson egy devcie közvetlen módszer fogópont

### <a name="method-invocation"></a>A módszer meghívása

Eszközök közvetlen módszer kérések MQTT című témakör jelenik meg:`$iothub/methods/POST/{method name}/?$rid={request id}`

A szervezetet, amely az eszköz kapja a következő formátumban van:

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Módszer kérések QoS 0.

### <a name="response"></a>Válasz

Az eszköz adott válaszok küldése `$iothub/methods/res/{status}/?$rid={request id}`, ahol:

 - A `status` tulajdonsága metódus végrehajtása eszköz által biztosított állapotát.
 - A `$rid` tulajdonsága IoT központból kapott módszer meghívását a kérelem azonosítója.

A szervezet állítja be az eszközt, és bármelyik állapotát.

## <a name="additional-reference-material"></a>További anyagminta

A Fejlesztőeszközök útmutató hivatkozás témakörök a következők:

- [IoT központi végpontok] [ lnk-endpoints] ismerteti a különböző végpontok, amely az egyes IoT központi közzététele futtatókörnyezet és kezelés műveletekhez.
- [Szabályozási és kvóták] [ lnk-quotas] a IoT központi szolgáltatás és számíthat, amikor a szolgáltatás használatához a szabályozási viselkedés vonatkozó kvóták ismerteti.
- [IoT központi eszközök és szolgáltatások SDK] [ lnk-sdks] a különböző nyelvi SDK sorolja fel, egy eszköz és a szolgáltatás-alkalmazások használata a IoT központi fejlesztésekor használja.
- [Lekérdezési nyelv twins, módszerek és feladatok] [ lnk-query] ismerteti a lekérdezésnyelv IoT központi eszköz twins, módszerek és feladatok kapcsolatos adatok kinyerése is használhatja.
- [IoT központi MQTT támogatási] [ lnk-devguide-mqtt] IoT központi támogatási további információt tartalmaz a MQTT protokollhoz.

## <a name="next-steps"></a>Következő lépések

Most már rendelkezik megtanulta közvetlen módszerek használatáról, amelyek érdekelhetik a fejlesztői útmutató következő témakörben:

- [A feladatok ütemezése több eszközön][lnk-devguide-jobs]

Ha azt szeretné, próbálja meg néhány a jelen cikkben ismertetett fogalmak, amelyek érdekelhetik a következő IoT központi oktatóprogram során:

- [Módszerek cloud-eszköz használata][lnk-methods-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md
