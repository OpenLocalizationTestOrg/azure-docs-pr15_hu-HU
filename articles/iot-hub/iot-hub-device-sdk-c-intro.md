<properties
    pageTitle="Az Azure IoT eszköz SDK használatának C |} Microsoft Azure"
    description="Tudnivalók a és c-az Azure IoT eszközben SDK a minta kód használata – első lépések"
    services="iot-hub"
    documentationCenter=""
    authors="olivierbloch"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/06/2016"
     ms.author="obloch"/>

# <a name="introducing-the-azure-iot-device-sdk-for-c"></a>Az Azure IoT eszköz SDK Bemutatkozik a C

Az **Azure IoT eszköz SDK csomagjában talál** egy olyan küldő események és az **Azure IoT központi** szolgáltatásban tárolt fogadó üzenetek egyszerűbbé és tárak. Egy adott platform kiválasztásával SDK más változatok, de ez a cikk ismerteti az **Azure IoT eszköz c SDK csomagjában talál**.

Az Azure IoT eszköz SDK c ANSI C (C99) hordozhatóságot íródott. Emiatt a számos különböző platformokon és eszközök, különösen akkor, ha a lemez minimalizálása működik jól illeszkedik és szükséges tárhelyet kiemelt.  

Vannak olyan széles köre platformokon, amelyen a SDK teszteléssel (lásd: az [Azure tanúsítvánnyal IoT eszköz katalógus](https://catalog.azureiotsuite.com/) részletekért). Bár ez a cikk a Windows platformon futó példakódot forgatókönyvek tartalmazza, ne feledje, hogy a jelen cikkben ismertetett kód megegyezik pontosan végig a támogatott platformok cellatartományt.

Ebben a cikkben akkor fogja hozni az Azure IoT eszköz SDK architektúrájú C. Az eszköz tár inicializálni, események küldése IoT-hubon keresztül csatlakozott, valamint üzenetek fogadására hogyan mutatjuk be. Az információk ebben a cikkben kell lennie ahhoz, hogy a SDK használatának első lépései, de további információt a tárak hivatkozásait is tartalmazza.

## <a name="sdk-architecture"></a>SDK architektúra

Az **Azure IoT eszköz c SDK** keresse meg a [Microsoft Azure IoT SDK](https://github.com/Azure/azure-iot-sdks) GitHub adattárban, és az API részleteinek megtekintése a [C API hivatkozást](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html).

A tárak legújabb verzióját, ez a tár a **fő** ág találhatók:

  ![](media/iot-hub-device-sdk-c-intro/01-MasterBranch.PNG)

Ez a tár Azure IoT eszköz SDK az egész család tartalmazza. Ez a cikk azonban az Azure IoT eszközről SDK *c* a **c** mappában találja.

  ![](media/iot-hub-device-sdk-c-intro/02-CFolder.PNG)

* A SDK core végrehajtásának megtalálható a a **iothub\_ügyfél** mappát, amely tartalmazza a legalacsonyabb API réteg a SDK végrehajtása: a **IoTHubClient** tárat. A médiatárban **IoTHubClient** végrehajtási nyers üzenetküldés üzenetküldés IoT-hubon keresztül csatlakozott, valamint a üzenetet kapni az API-khoz. A tár használata esetén (ahányat használatával az alábbiakban ismertetett szerializáló minta) üzenet szerializálási végrehajtási felelős, de más kommunikáció IoT központi részleteit kezelik meg.
* A **szerializáló** mappa segítő funkciók és a minták adatok szerializálni Azure IoT-hubon keresztül csatlakozott az ügyfél-tár használata elküldése előtt ábrázoló tartalmazza. Ne feledje, hogy a szerializáló használata nem kötelező, és csak a kényelem. Ha használja a **szerializáló** tárat, megkezdése modell, amely megadja a naplózandó események IoT központi, valamint a belőle fogadásához várt üzenetek küldése megadásával. Miután a modell van megadva, a SDK biztosít a egy API felület, amely lehetővé teszi, hogy egyszerű használata: események és az üzenetek nem kell aggódnia amiatt, hogy szerializálási részletei nélkül.
A tár megvalósítása (MQTT, AMQP) több protokollok használatával átviteli Megnyitás tárakat függ.
* A **IoTHubClient** tár Megnyitás tárakat függ:
   * Az [Azure C megosztott segédprogram](https://github.com/Azure/azure-c-shared-utility) tár, amely a leggyakoribb funkciókat nyújt a alapműveletek (például szöveg, a lista módjára, IO, stb...) keresztül több Azure kapcsolódó C SDK szükséges
   * A [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) tárban, amely ügyfél egymás végrehajtása AMQP erőforrás kényszer eszközök optimalizálva.
   * A [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) tárban, amely MQTT protokollt végrehajtási általános célú tár és erőforrás kényszer eszközök optimalizálva.

Az adott mindössze könnyebben megérthető, például kód megjeleníti. Az alábbi szakaszok haladjon végig a minta alkalmazások, a SDK csomagjában talál az szereplő néhány. Ezt meg kell adnia a helyes működés az szolgáltatásainak SDK a építészeti rétegeket, valamint az API-k működése bemutatása.

## <a name="before-running-the-samples"></a>A minták futtatása előtt

Futtatása előtt a minták az Azure IoT eszközben SDK c kell létrehoznia a szolgáltatás egy példánya az Azure-előfizetésben Ha még nem rendelkezik, és 2 feladatok elvégzésére:
* Felkészülés a fejlesztői környezet
* Szerezze be a eszköz hitelesítő adatokat.

Ha az Azure IoT központi egy példány létrehozása az Azure-előfizetésben van szüksége, kövesse az utasításokat [Itt](https://github.com/Azure/azure-iot-sdks/blob/master/doc/setup_iothub.md).

A SDK részét képező [Fontos fájl](https://github.com/Azure/azure-iot-sdks/tree/master/c) nyújt útmutatást a fejlesztői környezet előkészítése és eszköz hitelesítő adatok beszerzése.
A következő szakaszok ezeket az utasításokat a néhány további megjegyzéseivel tartalmazzák.

### <a name="preparing-your-development-environment"></a>A fejlesztői környezet előkészítése

Miközben bizonyos platformokon (például NuGet for Windows vagy az Debian és Ubuntu apt_get) csomagok állnak rendelkezésre, és a minták ezekről a csomagokról, ha lehetséges, használja az alábbi lépéseket a részletek hogyan hozhat létre a tárban, és közvetlenül a képernyő a kódot a minták.

Első lépésként meg kell szerezze be a SDK másolatának GitHub, és ezután létrehozhatja a forrás. A forrás másolatát a fájlt a **fő** ág a [GitHub tárházba](https://github.com/Azure/azure-iot-sdks)szerezheti be.

Amikor egy példányát a forrás letöltötte, hajtsa végre ["Előkészítése a fejlesztői környezet"](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md)SDK című témakörben leírt lépéseket.


Az alábbiakban néhány tipp az előkészítés útmutatóban ismertetett eljárás végrehajtásához nyújt segítséget:

-   Ha telepíti a **CMake** segédprogramot, döntene **CMake** hozzáadása a rendszer elérési út az **összes felhasználó** ( **az aktuális felhasználó** működik, valamint hozzáadása):

  ![](media/iot-hub-device-sdk-c-intro/08-CMake.PNG)


-   Mielőtt megnyitná a **fejlesztői parancssor VS2015**, telepítse a mely számjegy parancssori eszközöket. Ezeket az eszközöket telepíteni, hajtsa végre az alábbi lépéseket:

    1. Nyissa meg a **Visual Studio 2015** telepítőprogram (vagy válassza **a Microsoft Visual Studio 2015** a Vezérlőpult **Programok és szolgáltatások** és a választó **módosítása**).
    
    2. Győződjön meg arról, hogy a **Windows mely számjegy** szolgáltatás ki van jelölve, a telepítőprogram, de érdemes ellenőrizni a **Visual Studio GitHub bővítményének** lehetőséget, ha IDE integráció:

        ![](media/iot-hub-device-sdk-c-intro/10-GitTools.PNG)

    3. A beállítási varázsló az eszközök telepítése.

    4. A rendszer **PATH** környezeti változó adja hozzá a mely számjegy eszközök **bin** könyvtár. A Windows a néz ki a következő:

        ![](media/iot-hub-device-sdk-c-intro/11-GitToolsPath.PNG)


Miután elvégezte a ["Előkészítése a fejlesztői környezet"](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md) lap összes lépéseit, készen áll a minta alkalmazások összeállítása.

### <a name="obtaining-device-credentials"></a>Eszköz hitelesítő adatok beszerzése

Most, hogy a fejlesztői környezet be van állítva, hajtsa végre a következő dolog egy sor olyan eszközt hitelesítő adatok eléréséhez.  Egy eszköz fér hozzá egy IoT hubhoz fel kell vennie az eszköz a IoT központi eszköz beállításjegyzék. Az eszköz hozzáadásakor eszköz hitelesítő adatait, amelyek lesz szüksége ahhoz, hogy tud csatlakozni IoT-hubon keresztül csatlakozott az eszköz vissza. A minta alkalmazások, a következő szakaszban áttekintjük számíthat, ezek a hitelesítő adatok **eszköz kapcsolati karakterlánc**formájában.

Létezik néhány eszközeit segíti a IoT központi kezelése SDK Megnyitás tárban tárolnak. Egy Windows nevű eszközt Explorer, a másodikat pedig alkalmazás egy node.js alapú iothub-explorer nevű eszközt, közötti platform CLI egyik. Ezeket az eszközöket kapcsolatban többet is megtudhat [Itt](https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md).

Keresztül, a jelen cikkben a minták futó Windows fogjuk, ahogy azt az eszköz Explorer eszköz használata esetén. De is Intézővel iothub – Ha jobban szereti a CLI eszközök.

A [Eszköz Explorer](https://github.com/Azure/azure-iot-sdks/tree/master/tools/DeviceExplorer) használja az Azure IoT szolgáltatás tárak IoT központi, például a eszközök hozzáadása a különböző függvényekre végrehajtásához. Ha az eszköz hozzáadása eszköz Explorert használ, a megfelelő kapcsolati karakterlánc kapja meg. A kapcsolati karakterláncot, hogy a minta alkalmazások futtatásához szükséges.

Abban az esetben, ha Ön nem már jól ismert, a művelet, az alábbi eljárás ismerteti, hogyan eszköz Intézővel hozzáadása egy eszközt, és szerezze be a kapcsolati karakterlánc eszközt.

A Windows installer az eszköz Explorer eszköz [SDK engedje fel, oldal](https://github.com/Azure/azure-iot-sdks/releases)talál. De közvetlenül a saját kód **[DeviceExplorer.sln](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/DeviceExplorer.sln)** nyissa meg a **Visual Studio 2015** és a megoldás kiépítése is futtathatja az eszközt.

Amikor a program futtatja a kapcsolat jelenik meg:

  ![](media/iot-hub-device-sdk-c-intro/03-DeviceExplorer.PNG)

Az első mezőbe írja be a **IoT központi kapcsolati karakterláncot** , és kattintson a **frissítés**gombra. Ezzel az eszközzel a, hogy IoT központi kommunikálni.

Ha be van állítva a IoT központi kapcsolati karakterlánc kattintson **a lap** :

  ![](media/iot-hub-device-sdk-c-intro/04-ManagementTab.PNG)

Ez a, ahol kezelheti a IoT központi bejegyzett eszközök.

Létrehozhat egy eszközt, kattintson a **Létrehozás** gombra. Előre megadott billentyűk (elsődleges és másodlagos) tartalmazó párbeszédpanel jelenik meg. Kell elvégeznie mindössze egy **Eszközazonosítót** adja meg, és kattintson a **Létrehozás**gombra.

  ![](media/iot-hub-device-sdk-c-intro/05-CreateDevice.PNG)

Amikor az eszköz létrejött, az eszközök listát bejegyzett eszközök, köztük az imént létrehozott frissül. Ha a jobb gombbal az új eszköz, a menü jelenik meg:

  ![](media/iot-hub-device-sdk-c-intro/06-RightClickDevice.PNG)

Ha a **kapcsolati karakterlánc kijelölt eszköz másolása** lehetőséget választja, a kapcsolati karakterláncot az eszközhöz másolja a vágólapra. A kapcsolati karakterlánc másolatának megőrzése. Ha a minta-alkalmazások, a következő szakaszokban ismertetett kell.

A fenti lépések végrehajtása után, Ön készen áll a kezdésre néhány kód futtatását. Mindkét minták állandó van, a fő forrásfájl, amely lehetővé teszi, hogy adja meg a kapcsolati karakterlánc tetején. Ha például a megfelelő sor a **iothub\_ügyfél\_minta\_amqp** alkalmazás a következőképpen jelenik meg.

```
static const char* connectionString = "[device connection string]";
```

Követni kívánt, adja meg az eszköz kapcsolati karakterláncot az alábbi, a megoldást fordítani és is a minta futtatásához.

## <a name="iothubclient"></a>IoTHubClient

Belül a **iothub\_ügyfél** mappát az azure-iot-SDK adattárban van, amely tartalmazza a nevű alkalmazás **minták** mappa **iothub\_ügyfél\_minta\_amqp**.

A Windows verziója a **iothub\_ügyfél\_minta\_ampq** alkalmazás a következő Visual Studio megoldás tartalmazza:

  ![](media/iot-hub-device-sdk-c-intro/12-iothub-client-sample-amqp.PNG)

A megoldás egy projektet tartalmaz. Érdemes megjegyezni, hogy nincsenek-e telepítve a megoldás négy NuGet csomagok:

- Microsoft.Azure.C.SharedUtility
- Microsoft.Azure.IoTHub.AmqpTransport
- Microsoft.Azure.IoTHub.IoTHubClient
- Microsoft.Azure.uamqp

Mindig van szüksége a **Microsoft.Azure.C.SharedUtility** csomag esetén a SDK használata. Mivel ez a példa AMQP támaszkodik, meg kell adni a **Microsoft.Azure.uamqp** és **Microsoft.Azure.IoTHub.AmqpTransport** csomagok (a példadiagramon MQTT és a HTTP egyenértékű csomagok) is. Mivel a minta használja a **IoTHubClient** tárat, fel kell venni a **Microsoft.Azure.IoTHub.IoTHubClient** csomag a megoldás.

A minta alkalmazáshoz végrehajtása talál a **iothub\_ügyfél\_minta\_amqp.c** forrásfájl.

Minta alkalmazás hozunk ismerteti, hogy mi a **IoTHubClient** tár használatához szükséges.

### <a name="initializing-the-library"></a>A tár inicializálása

> [AZURE.NOTE] Mielőtt a tárak együtt kezd dolgozni, előfordulhat, végezze el az egyes platform adott inicializálni. Ha szeretné használni a AMQP Linux például kell a OpenSSL tár inicializálni. A [GitHub tárházba](https://github.com/Azure/azure-iot-sdks) minták hívni a segédprogram függvény **platform_init** az ügyfél elindul, és hívja meg a **platform_deinit** függvényt bezárása előtt. Ezek a funkciók a "platform.h" Élőfej-fájlban vannak deklarálva. Ezeket a funkciókat meghatározását kell vizsgálja meg a következő a cél platform a [tárházba](https://github.com/Azure/azure-iot-sdks) határozza meg a szükséges platform inicializálni kódrészleteket szerepeltetése az ügyfélnek.

A tárak használata indításához kell először lefoglalhat egy központi IoT ügyfél fogópont:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Figyelje meg, hogy azt is átadása az eszköz kapcsolati karakterlánc másolatát ezt a funkciót (egy azt nyert eszköz Explorer). Azt is kijelölhet, azt a használni kívánt Protocol (protokoll). Ez a példa AMQP, de MQTT és a HTTP-is beállítást.

Ha van érvényes **IOTHUB\_ügyfél\_KEZELNI**, hívja fel az események üzenetek küldése és fogadása IoT központból API-k elindíthatja. Áttekintjük a Tovább gombra.

### <a name="sending-events"></a>Események küldése

Események küldene IoT központi igényel, hogy az alábbi lépések elvégzésével:

Első lépésként hozzon létre egy üzenetet:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_Over_AMQP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText);
```

Következő lépésként küldje el az üzenetet:

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Minden alkalommal, amikor egy üzenetet küld, adja meg az adatokat küldi hív meg visszahívási függvény mutató hivatkozás:

```
static void SendConfirmationCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    EVENT_INSTANCE* eventInstance = (EVENT_INSTANCE*)userContextCallback;
    (void)printf("Confirmation[%d] received for message tracking id = %d with result = %s\r\n", callbackCounter, eventInstance->messageTrackingId, ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
    /* Some device specific action code goes here... */
    callbackCounter++;
    IoTHubMessage_Destroy(eventInstance->messageHandle);
}
```

Megjegyzés: a hívást a **IoTHubMessage\_Destroy** fog működni, amikor elkészült az üzenettel. Ha az üzenet létrehozásakor a feladatoknak a hívás kell végeznie.

### <a name="receiving-messages"></a>Üzenetek fogadása

Aszinkron műveletet üzenetet. Első lépésként regisztrálnia a visszahívás képezheti, amikor az eszköz üzenetet kapja:

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

Az utolsó paraméter szeretne, tetszőleges érvénytelenítése mutató. A minta egész szám mutató de összetettebb adatstruktúra mutatót lehet. A visszahívási függvény működtetéséhez megosztott állapotának kíván a hívó féllel Ez a funkció lehetővé teszi.

Ha az eszköz üzenetet kap, a visszahívás regisztrált függvény meghívott:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    int* counter = (int*)userContextCallback;
    const char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, (const unsigned char**)&buffer, &size) == IOTHUB_MESSAGE_OK)
    {
        (void)printf("Received Message [%d] with Data: <<<%.*s>>> & Size=%d\r\n", *counter, (int)size, buffer, (int)size);
    }

    /* Some device specific action code goes here... */
    (*counter)++;
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Látható, hogy használja az **IoTHubMessage\_GetByteArray** beolvasásához, amelyek ebben a példában egy olyan karakterlánc, az üzenet függvény.

### <a name="uninitializing-the-library"></a>A tár uninitializing

Amikor elkészült a küldő események és érkező üzeneteket, akkor is meghívná a IoT tárban. Ehhez a következő függvény hívásával hiba:

```
IoTHubClient_Destroy(iotHubClientHandle);
```

Ez szabadítja, az erőforrások korábban osztja fel a **IoTHubClient\_CreateFromConnectionString** függvény.

Amint látható, akkor egyszerűen események küldhet és fogadhat üzeneteket a **IoTHubClient** tárral. A tár IoT központi, többek között a mely protokoll használatára kommunikáció részleteit kezeli (a fejlesztői szemszögéből, ez a beállítás egy egyszerű konfigurálása).

A **IoTHubClient** tár is tartalmaz az események IoT központi küld az eszköz szerializálni hogyan pontosan szabályozható. Bizonyos esetekben ez az az előnye, de más esetekben ez egy végrehajtása részlet, amellyel nem kívánt érintett. Ez a helyzet, ha akkor fontolja meg a **szerializáló** tár, amelyek leírják azt fogja, a következő szakaszban használatával.

## <a name="serializer"></a>Szerializáló

A **szerializáló** tár adatbázistáblákhoz fölött a **IoTHubClient** tárat a SDK helyezkedik el. Akkor használja, a **IoTHubClient** tár IoT központi mögöttes kommunikációt, de adatmodellezési funkciókat kínál, hogy az üzenet szerializálási kezelésének terhet eltávolítása a fejlesztői hozzáadja. Példa igazolnia hogyan a tár működése a legjobb.

A **szerializáló** belül az azure-iot-SDK adattárban mappában nevű alkalmazás tartalmazó **minták** mappa **simplesample\_amqp**. Ez a példa a Windows-verzió a következő Visual Studio megoldás tartalmazza:

  ![](media/iot-hub-device-sdk-c-intro/14-simplesample_amqp.PNG)

Az előző minta az e több NuGet csomagok foglalja magában:

- Microsoft.Azure.C.SharedUtility
- Microsoft.Azure.IoTHub.AmqpTransport
- Microsoft.Azure.IoTHub.IoTHubClient
- Microsoft.Azure.IoTHub.Serializer
- Microsoft.Azure.uamqp

Láthatta, akkor ezeket az előző minta a legtöbb, de új **Microsoft.Azure.IoTHub.Serializer** . Szükség, amikor a **szerializáló** tár használjuk.

A minta-alkalmazás végrehajtásának megtalálhatja a **simplesample\_amqp.c** fájlt.

Az alábbi szakaszok ismerteti, hogy ez a példa főbb részei között.

### <a name="initializing-the-library"></a>A tár inicializálása

Indítsa el a **szerializáló** tárban dolgozik, hívja a a inicializálni API-hoz:

```
serializer_init(NULL);

IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);

ContosoAnemometer* myWeather = CREATE_MODEL_INSTANCE(WeatherStation, ContosoAnemometer);
```

A hívást a **szerializáló\_init** függvény egyszeri hívást, és az alapul szolgáló tárat inicializálni használható. Ezután, hívja a **IoTHubClient\_CreateFromConnectionString** függvény, amely az azonos API, ahogy a **IoTHubClient** minta. A hívás állítja be az eszközt kapcsolati karakterlánc (Ez akkor is ha úgy dönt, hogy a protokoll használni kívánt). Megjegyzés: Ez a példa AMQP használja az adatátviteli, de HTTP használt.

Végül, hívja fel a **létrehozása\_MODELL\_példány** függvény. Figyelje meg, hogy **WeatherStation** a modell névtere pedig **ContosoAnemometer** a modell nevét. A modell példány jön létre, miután használhatja üzenetek események küldéséhez és fogadásához indításához. Azonban fontos megértéséhez milyen a modell.

### <a name="defining-the-model"></a>Az adatmodell meghatározása

A **szerializáló** tárban modell határozza meg, hogy az eseményeket, az eszköz IoT központi és az üzenetek elnevezésű *Műveletek* megkaphatja a modellezési nyelv küldhet. A modell egy, a C makrók készlet segítségével megadhatja a **simplesample\_amqp** alkalmazás minta:

```
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(double, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

A **MEGKEZDÉSÉHEZ\_NÉVTÉR** és **vége\_NÉVTÉR** makrókat is készíthet a névtér argumentumaként a modell. Valószínű, hogy semmit, ezek a makrók között-e a modellek és a modellek használó adatstruktúrák definícióját.

Ebben a példában **ContosoAnemometer**nevű egyetlen modellben van. Ez a modell határozza meg, amelyek az eszköz küldhetnek IoT központi két esemény: **DeviceId** és **szélsebesség**. Is határozza meg, hogy az eszköz kaphatnak a három műveletek (üzenetek): **TurnFanOn** **TurnFanOff**és **SetAirResistance**. Egyes események típus van, és minden egyes műveletnek nevét (és tetszőlegesen a paraméterek beállítása).

Az események és a modell definiált műveleteket definiálhat egy API-felület, amely események küldése IoT-hubon keresztül csatlakozott, valamint az eszközre küldött üzenetek megválaszolása is használhatja. A legjobb értendő példa keresztül.

### <a name="sending-events"></a>Események küldése

A modell az eseményeket, elküldheti a IoT központi határozza meg. Ebben a példában, amely azt jelzi, hogy a két esemény definiált a **WITH_DATA** makróval közül. Ha például szeretné elküldeni a **szélsebesség** esemény IoT-hubon keresztül csatlakozott, esetén néhány lépést Ez akkor történhet meg, hogy részt. Az első, a Ha az adatokat, hogy el szeretné küldeni beállítása:

```
myWeather->WindSpeed = 15;
```

A korábban definiált modell lehetővé teszi, hogy us ehhez a **struktúra**tagja megadásával. Ezután az eseményt, hogy el szeretné küldeni szerializálni azt:

```
unsigned char* destination;
size_t destinationSize;

SERIALIZE(&destination, &destinationSize, myWeather->WindSpeed);
```

Ez a kód serializes az esemény egy pufferelési (hivatkozhat **cél**). Végezetül rendszerünk egy e-az esemény IoT hubon keresztül csatlakozott, a kód:

```
sendMessage(iotHubClientHandle, destination, destinationSize);
```

Ez a segítő függvény a szerializált esemény küld IoT központi minta alkalmazásban:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId) != IOTHUB_CLIENT_OK)
        {
            printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
            printf("IoTHubClient accepted the message for delivery\r\n");
        }

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
    messageTrackingId++;
}
```

Kód nagyon hasonlít mi bekerül az a **iothub\_ügyfél\_minta\_amqp** alkalmazást, amelyben azt üzenet létrehozott egy bájt tömbből, és majd használt **IoTHubClient\_SendEventAsync** IoT hubhoz küldhet. Ezt követően azt csak rendelkezik az üzenet fogópont felszabadításához, és azt a korábbi lefoglalt adatok puffer szerializálásának.

Az utolsó paramétere: a második **IoTHubClient\_SendEventAsync** az adatok sikeres küldésekor meghívott visszahívási függvény a hivatkozás mutat. Íme egy példa, a visszahívás függvény:

```
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    int messageTrackingId = (intptr_t)userContextCallback;

    (void)printf("Message Id: %d Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

A második paraméter felhasználói környezet; mutató a azonos mutató azt át a **IoTHubClient\_SendEventAsync**. A környezet ebben az esetben egy egyszerű számláló, de semmi, amelyet lehet.

Ez az összes van, és eseményeket küldött. A hátralévő terjed ki beállítást, hogy hogyan jelennek meg.

### <a name="receiving-messages"></a>Üzenetek fogadása

Works hasonlóan az üzenetek hogyan működik a **IoTHubClient** tárban üzenetet kap. Első lépésként regisztrálnia visszahívási üzenet függvény:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Ezután írhat a visszahívás függvény hív meg, amikor egy üzenet jelenik meg:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
            result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
            memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

Ez a kód újrafelhasználható, – a azonos bármely megoldás. Ez a függvény az üzenetet kap, és a hívást a megfelelő funkciót irányítás gondoskodik **végrehajtás\_parancs**. A hívott függvény ezen a ponton a műveleteket a modell definícióját függ.

Művelet a modell definiálásakor, az eszköz a megfelelő üzenetet kap, amikor meghívott függvény végrehajtása esetén szükséges. Ha például a modell határozza meg, ezt a műveletet:

```
WITH_ACTION(SetAirResistance, int, Position)
```

Meg kell adni ezt az aláírást tartalmazó függvényt.

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

Ne feledje, hogy a függvény neve megegyezik-e a művelet a modell nevét, hogy a függvény a paraméterek egyezik-e a művelet megadott paraméterek. Az első paraméterként mindig szükség, és a modell példányának mutató tartalmazza.

Ha az eszköz, amely megfelel az aláírás üzenetet kap, a megfelelő függvény neve. Azon problémákat, ha meg szeretné jeleníteni a **IoTHubMessage**újrafelhasználható kódot, üzenetek fogadására ezért csak egy egyszerű függvény a modellben definiált műveleteket meghatározása a témát.

### <a name="uninitializing-the-library"></a>A tár uninitializing

Amikor elkészült a küldő adatok és érkező üzeneteket, akkor is meghívná a IoT tár:

```
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

Ezek a függvények három mindegyike igazítja korábban leírt három inicializálni függvénnyel. Hívja fel a következő API-khoz biztosítja, hogy a korábban hozzárendelt erőforrások ingyenes.

## <a name="next-steps"></a>Következő lépések

Ez a cikk vonatkozik az a tárakat az **Azure IoT eszköz c SDK**használatának alapjai. Azt ellátni, akkor elegendő információt, ha meg szeretné érteni, hogy mi minden található az SDK annak architektúrája, és hogyan a Windows minták használatának megkezdéséhez. A következő cikk, hogy [További információt a IoTHubClient tár](iot-hub-device-sdk-c-iothubclient.md)továbbra is a SDK leírását.

IoT központi fejlesztésével kapcsolatos további tudnivalókért lásd: a [IoT központi SDK][lnk-sdks].

További feltárása a IoT központi funkcióinak, olvassa el:

- [Egy eszközt a IoT átjáró SDK csomagjában talál, amelyek][lnk-gateway]


[lnk-file upload]: iot-hub-csharp-csharp-file-upload.md
[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
