<properties
    pageTitle="Azure IoT eszköz SDK C - szerializáló |} Microsoft Azure"
    description="Többet szeretne tudni a szerializáló tárban az Azure IoT eszközben SDK használatáról a c"
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

# <a name="microsoft-azure-iot-device-sdk-for-c--more-about-serializer"></a>Microsoft Azure IoT eszköz SDK c – további információ: szerializáló

A sorozat [első cikk](iot-hub-device-sdk-c-intro.md) az **Azure IoT eszköz c SDK**jelent meg. A következő cikk részletes leírását a [**IoTHubClient**](iot-hub-device-sdk-c-iothubclient.md)adni. Ez a cikk a SDK körét befejeződik, mert a hátralévő összetevő részletesebb leírása: a **szerializáló** tárat.

Bevezető témakört a eseményeket üzenetek küldése és fogadása IoT központból használata a **szerializáló** tár leírását. Ebben a cikkben a a vitafórum, mert egy, az adatok **szerializáló** makró nyelvvel modell hogyan teljesebb magyarázat kibővíthetjük. A cikk is magában foglalja a részletesebb információkat, hogy az a tár serializes üzenetek (és sőt bizonyos esetekben hogyan szabályozhatja, hogy a szerializálási viselkedés). Azt fogja ismertetik egyes paraméterek módosíthatja, amelyek meghatározzák a hoz létre az adatmodellek méretének is.

Végül a cikk revisits súgótémakörökben foglalt az előző bekezdésre, például az üzenetet, és a tulajdonságok kezelése. Azt találhatja, ezeket a funkciókat használja a **szerializáló** tár, mint a **IoTHubClient** tárral ugyanúgy működjön.

Mindent a jelen cikkben ismertetett **szerializáló** SDK minták alapul. Ha követni kívánt, olvassa el a **simplesample\_amqp** és **simplesample\_http** alkalmazásokat tartalmaz az Azure IoT eszköz SDK az C.

Az **Azure IoT eszköz c SDK** keresse meg a [Microsoft Azure IoT SDK](https://github.com/Azure/azure-iot-sdks) GitHub adattárban, és az API részleteinek megtekintése a [C API hivatkozást](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html).

## <a name="the-modeling-language"></a>A modellezési nyelv

A [bevezető a cikk](iot-hub-device-sdk-c-intro.md) a sorozat jelent meg az **Azure IoT eszköz c SDK** végig a példát a megadott nyelvi modellezési a **simplesample\_amqp** alkalmazás:

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

Amint látható, a modellezési nyelv C makrók alapul. Együtt a definícióját mindig lépések **KEZDŐ\_NÉVTÉR** mindig a végén pedig használja **vége\_NÉVTÉR**. Általában a névtér nevezze el a vállalat vagy, ahogy az ebben a példában a projektet, amely az aktív.

A névtér belül tartalmának definiálják modell. Ebben az esetben van egy olyan anemometer egyetlen modellt. Még egyszer a modell nevet adhat semmit, de általában ennek neve az eszköz vagy a IoT központi cserélni kívánt adatok típusát.  

Az adatmodellek meghatározása az események és az üzenetek ( *Műveletek*) központból IoT kaphat bejövő adatok IoT hubhoz (az *adatok*) is tartalmaz. A példában látható, mint események van-e a típus és egy név; műveletek van egy név és a választható paraméterek (minden típusú).

Mi nem a a mintában igazolni a SDK által támogatott típusú további adatokat. Fogja foglalkozunk, hogy a Tovább gombra.

> [AZURE.NOTE] Az adatok egy eszközt küld *eseményeket*, mint közben modellezési nyelv hivatkozik, mint *adatok* (megadott **WITH_DATA**) IoT központi hivatkozik. Hasonlóképpen az adatokat küldi eszközök *üzeneteket*, miközben a modellezési nyelv hivatkozik, mint *Műveletek* (megadott **WITH_ACTION**) IoT központi hivatkozik. Ne feledje, hogy ezeket a kifejezéseket is felhasználható szót azonos értelemben ebben a cikkben.

### <a name="supported-data-types"></a>Támogatott adattípusok

A következő adattípusokat a **szerializáló** tárral létrehozott modellekben támogatottak:

| Típus                    | Leírás                            |
|-------------------------|----------------------------------------|
| dupla                  | dupla pontosság lebegőpontos szám |
| int                     | 32 bites egész szám                         |
| Lebegtetés                   | egyetlen pontosság lebegőpontos szám |
| hosszú                    | hosszú egész szám                           |
| int8\_t                 | 8 bites egész szám                          |
| Int16\_t                | 16 bites egész szám                         |
| Int32\_t                | 32 bites egész szám                         |
| Int64\_t                | 64 bites egész szám                         |
| logikai                    | logikai érték                                |
| ASCII\_karakter\_ptr        | ASCII-karakterlánc                           |
| EDM\_dátum\_idő\_ELTOLÁSA | dátum-idő eltolása                       |
| EDM\_globálisan egyedi azonosítója               | GLOBÁLISAN EGYEDI AZONOSÍTÓJA                                   |
| EDM\_bináris             | bináris                                 |
| DEKLARÁLHATNAK\_struktúra         | összetett adattípus                      |

Kezdjük az utolsó adattípusát. A **DECLARE\_struktúra** megadhatja az összetett adattípusok, amelyek az egyéb egyszerű típusokhoz csoportosítását. Ez a csoportosítás meghatározása a modellt, így néz ki, hogy mi engedélyezése:

```
DECLARE_STRUCT(TestType,
double, aDouble,
int, aInt,
float, aFloat,
long, aLong,
int8_t, aInt8,
uint8_t, auInt8,
int16_t, aInt16,
int32_t, aInt32,
int64_t, aInt64,
bool, aBool,
ascii_char_ptr, aAsciiCharPtr,
EDM_DATE_TIME_OFFSET, aDateTimeOffset,
EDM_GUID, aGuid,
EDM_BINARY, aBinary
);

DECLARE_MODEL(TestModel,
WITH_DATA(TestType, Test)
);
```

A modell egy egyetlen adatok típusú eseményt **TestType**tartalmazza. **TestType** együttesen bemutatják a nyelvi modellezési **szerializáló** által támogatott egyszerű típusokhoz tagok tartalmazó összetett típusú.

Egy a modellben jelennek meg azt is kódírás adatokat küldeni IoT-hubon keresztül csatlakozott, amely a következőképpen jelenik meg:

```
TestModel* testModel = CREATE_MODEL_INSTANCE(MyThermostat, TestModel);

testModel->Test.aDouble = 1.1;
testModel->Test.aInt = 2;
testModel->Test.aFloat = 3.0f;
testModel->Test.aLong = 4;
testModel->Test.aInt8 = 5;
testModel->Test.auInt8 = 6;
testModel->Test.aInt16 = 7;
testModel->Test.aInt32 = 8;
testModel->Test.aInt64 = 9;
testModel->Test.aBool = true;
testModel->Test.aAsciiCharPtr = "ascii string 1";

time_t now;
time(&now);
testModel->Test.aDateTimeOffset = GetDateTimeOffset(now);

EDM_GUID guid = { { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x0F } };
testModel->Test.aGuid = guid;

unsigned char binaryArray[3] = { 0x01, 0x02, 0x03 };
EDM_BINARY binaryData = { sizeof(binaryArray), &binaryArray };
testModel->Test.aBinary = binaryData;

SendAsync(iotHubClientHandle, (const void*)&(testModel->Test));
```

Éppen alapvetően azt oszt ki egy értéket a **próba** -struktúra minden tagja, és utána meghívja **SendAsync** a **vizsgált** adatokat esemény küldeni a felhőben. **SendAsync** , amely egy egyetlen adatok esemény küld IoT központi segítő függvény:

```
void SendAsync(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const void *dataEvent)
{
    unsigned char* destination;
    size_t destinationSize;
    if (SERIALIZE(&destination, &destinationSize, *(const unsigned char*)dataEvent) ==
    {
        // null terminate the string
        char* destinationAsString = (char*)malloc(destinationSize + 1);
        if (destinationAsString != NULL)
        {
            memcpy(destinationAsString, destination, destinationSize);
            destinationAsString[destinationSize] = '\0';
            IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromString(destinationAsString);
            if (messageHandle != NULL)
            {
                IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)0);

                IoTHubMessage_Destroy(messageHandle);
            }
            free(destinationAsString);
        }
        free(destination);
    }
}
```

Ez a függvény a megadott adatok esemény serializes, és elküldi azt használatával IoT központi **IoTHubClient\_SendEventAsync**. Az előző bekezdésre (**SendAsync** beágyazza a logika a kényelmes függvény) tárgyalt ugyanazt kódot.

Egy másik segítő az előző kódban használt funkciója **GetDateTimeOffset**. Ez a függvény az adott idő alatt átalakítások típusú értékké alakítja **EDM\_dátum\_idő\_ELTOLÁS**:

```
EDM_DATE_TIME_OFFSET GetDateTimeOffset(time_t time)
{
    struct tm newTime;
    gmtime_s(&newTime, &time);
    EDM_DATE_TIME_OFFSET dateTimeOffset;
    dateTimeOffset.dateTime = newTime;
    dateTimeOffset.fractionalSecond = 0;
    dateTimeOffset.hasFractionalSecond = 0;
    dateTimeOffset.hasTimeZone = 0;
    dateTimeOffset.timeZoneHour = 0;
    dateTimeOffset.timeZoneMinute = 0;
    return dateTimeOffset;
}
```

Ha ez a kód, a következő üzenet érkezik IoT központi:

```
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

Ne feledje, hogy a szerializálási JSON, amelyek formátuma az generálja a **szerializáló** tárat. Tartsa szem előtt, hogy a szerializált JSON objektum minden tagjának megfelel-e a modellben definiált **TestType** tagjainak. Az értékek is pontosan egyeznie a kód használtakkal. Jó helyen jár, ne feledje, hogy a bináris adatokat base64 kódolású: "AQID" a base64 kódolása a {0x01, 0x02, 0x03}.

Ez a példa bemutatja az az előnye, hogy a **szerializáló** tár használata--lehetővé teszi, hogy nekünk küldeni JSON a felhőben, anélkül, hogy az alkalmazás szerializálási kifejezetten foglalkozik. Azt kell aggódnia amiatt, hogy az összes van állítsa be az értékeket az adatok események az adatmodelleket és a majd hívja fel az események küldése a felhőbe egyszerű API-khoz.

Ezekkel az információkkal azt a cellatartományt támogatott adattípusok, beleértve az összetett típusú (azt is tartalmazhatnak összetett típusú más összetett típusú belül) tartalmazó modellek adhatja meg. Jó helyen jár, ő szerializálásának által generált JSON a fenti példa ezt a lehetőséget választva egy fontos pontra. *Hogyan* elküldjük Önnek a **szerializáló** tárral adatok határozza meg, hogy pontosan hogyan a JSON lett létrehozva. Hogy az adott, mit fog foglalkozunk Tovább gombra.

## <a name="more-about-serialization"></a>További információ: szerializálási

Az előző szakaszban kiemeli a példa a kimenet generálja a **szerializáló** tárat. Ebben a szakaszban megismerheti miként az a tár serializes adatokat, és hogyan szabályozhatja, hogy a szerializálási API-k használata viselkedését.

Annak érdekében, hogy a szerializálási beszélgetés előzetes, azt fog dolgozni egy termosztát alapuló új modell. Először nézzük tájékoztatatást nyújt az eset címét próbálja azt.

Egy termosztát, amely hőmérséklet és páratartalom méri modell szeretnénk. Minden adatot fog kapni IoT központi másképp. Alapértelmezés szerint a termosztát ingresses a hőmérsékleti esemény 2 percenként; a páratartalom esemény ingressed 15 percenként. Ha bármelyik esemény ingressed, tartalmaznia kell, hogy időt mutatja, hogy a megfelelő hőmérsékleti vagy páratartalom mérték időbélyeg.

Ebben az esetben, mutatjuk be az adatok kétféleképpen, és az effektus megismerheti, hogy a szerializált kimenet modellezési van.

### <a name="model-1"></a>1 modell

Az alábbiakban a modell, amely támogatja az előző forgatókönyvben első verziója:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(TemperatureEvent,
int, Temperature,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_STRUCT(HumidityEvent,
int, Humidity,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureEvent, Temperature),
WITH_DATA(HumidityEvent, Humidity)
);

END_NAMESPACE(Contoso);
```

Megjegyzés: a modell tartalmaz a két adatok események: **hőmérséklet** és **páratartalom**. Előző példákban eltérően az egyes események típusú egy megadott struktúra **DECLARE\_struktúra**. **TemperatureEvent** tartalmazza a hőmérsékleti mérési és időbélyeg; **HumidityEvent** páratartalom mérték és időbélyeg tartalmazza. Ez a modell ad nekünk az adatok a fentebb ismertetett eset természetes módon. Esemény azt küldeni a felhőben, amikor egy hőmérsékleti időbélyeg vagy páratartalom/időbélyeg két vagy értesítjük.

A hőmérsékleti eseményeket a felhőben, például a következő kód használatával küldhet azt:

```
time_t now;
time(&now);
thermostat->Temperature.Temperature = 75;
thermostat->Temperature.Time = GetDateTimeOffset(now);

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Azt fogja csomagolásukkor értékek használata az hőmérséklet és a minta kódban páratartalom, de a elképzelheti, hogy azt ténylegesen olvas be ezeket az értékeket a termosztátot meg a megfelelő érzékelőket mintavételezéssel.

A fenti kódot használ a **GetDateTimeOffset** segítő, amely a korábban jelent meg. Törölje a jelet a későbbi válnak okokból kód kifejezetten szerializálása és az esemény küldése a feladat választja el. Az előző kódot be egy pufferelési serializes a hőmérsékleti eseményt. Ezután **sendMessage** feladata segítő (szereplő **simplesample\_amqp**), amely elküldi az esemény IoT központi:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId);

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
}
```

Ez a kód, azt nem forrás fölé ismét az előző szakaszban leírt **SendAsync** segítő része.

A korábbi kódot, és küldje el a hőmérsékleti esemény futtatásakor a szerializált képernyőn az esemény IoT hubhoz küldi el:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Azt is küld egy hőmérsékleti, amely **TemperatureEvent** típusú, és a struktúra tartalmaz **hőmérsékleti** és az **idő** tagja. Ez közvetlenül a szerializált adatok megjelennek.

Hasonlóképpen elküldjük kód egy páratartalom esemény:

```
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

A szolgáltatás elküldi a IoT központi szerializált űrlapot a következőképpen jelenik meg:

```
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Újra az elvárásoknak megfelelően.

Ebben a modellben el is lehet képzelni hogyan további események egyszerűen manuálisan is hozzáadhatók. További szerkezetek használatával meghatározhatja **DECLARE\_struktúra**, és mellékelje a megfelelő eseményt a modell használata **WITH\_adatok**.

Most vegyük módosíthassa a modellt, így tartalmazza ugyanazokat az adatokat, de egy másik struktúra.

### <a name="model-2"></a>2 modell

Fontolja meg a fentihez e alternatív modell:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Ebben az esetben azt már számolni a **DECLARE\_struktúra** makrók és egyszerűen definiálása a forgatókönyv a az modellezési nyelv egyszerű típusú adatok elemet.

Csak a pillanatra vegyük figyelmen kívül hagyása **az esemény** . Az adott függetlenül az alábbiakban a bejövő **hőmérsékleti**adatok kódot:

```
time_t now;
time(&now);
thermostat->Temperature = 75;

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Ez a kód a következő szerializált esemény küld IoT központi:

```
{"Temperature":75}
```

És a kódot a páratartalom esemény küldésének a következőképpen jelenik meg:

```
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Ez a kód IoT hubhoz küld ez:

```
{"Humidity":45}
```

Az eddigi vannak még mindig nem érheti. Most vegyük módosítása a SZERIALIZÁLT makró használata.

A **SZERIALIZÁLT** makró argumentumként több adat eseményeket vehet igénybe. Lehetővé teszi a **hőmérsékleti** és **páratartalom** esemény közös szerializálni, és küldje el nekik IoT központi egy hívás us:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Előfordulhat, hogy találat, hogy a kód eredménye két események küldött IoT-hubon keresztül csatlakozott:

[

{"Hőmérsékleti": 75},

{"Páratartalom": 45}

]

Más szóval azt valószínűleg gondolná, hogy ez a kód megegyezik a **hőmérsékleti** és **páratartalom** külön-külön elküldésével. Csak a könnyebb mindkét események átadni **SZERIALIZÁLT** az adott hívásban. Jó helyen jár, hogy nem a helyzet. Ehelyett a fenti kódot küld az adatok egyetlen eseményt IoT központi:

{"Hőmérsékleti": 75, "páratartalom": 45}

Ezzel tűnhet ismeretlen, mivel a modell határozza meg **hőmérséklet** és **páratartalom** két *külön* események:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

További a pontra, azt az események ugyanabban a szerkezetben **hőmérséklet** és **páratartalom** esetén nem modell:

```
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

Ez a modell használatakor könnyebben megérthető, hogyan **hőmérséklet** és **páratartalom** küldi el az azonos szerializált üzenetben lenne. Azonban nem lehet törlése oka annak, ha sikeres mindkét adatok események **SZERIALIZÁLT** a modell 2 segítségével ily módon működik.

Ez a jelenség akkor könnyebben megérthető, ha tudja, hogy a feltételezések, amely lehetővé teszi a **szerializáló** tár. Az adott értelmezéséhez táblázatkapcsolat térjen vissza a a modellt:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Érdemes a objektumorientált feltételek a modellt. Ebben az esetben azt is modellezése a fizikai eszközök (termosztát), és, hogy az eszköz például **hőmérséklet** és **páratartalom**attribútumokat tartalmazza.

Szeretne küldeni egy teljes állapotát a modellt, például a következő kódot:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Feltételezve, hogy a hőmérsékleti értékeket, páratartalom és az idő beállítása, ilyen IoT központi küldött esemény volna láthatja:

```
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Időnként előfordulhat, hogy csak szeretne a modell *egyes* tulajdonságai küldése a felhőbe (Ez a különösen igaz, ha a modell tartalmaz nagyszámú adat események). Célszerű adatok eseményeket, csak egy részhalmazát küldhet például korábbi példában:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Ez hozza létre pontosan az azonos szerializált eseményt, mintha egy **TemperatureEvent** **hőmérsékleti** és az **idő** tagjával volna definiált ugyanúgy, mint azt did és 1 modellt. Ebben az esetben azt tudott egy másik modell (modell 2) használatával, mert azt egy másik módszert **SZERIALIZÁLT** nevű pontosan a azonos szerializált esemény létrehozásához.

A fontos pontot, amely akkor, ha sikeres több adat események **SZERIALIZÁLT,** hogy azt azt feltételezi, hogy egyes események egy tulajdonság, a JSON-objektumra, majd is.

A legjobb megközelítés, és hogyan megfontolni a modell függ. Ha "események" a felhőben küld üzenetet, és egyes események meghatározott tulajdonságait tartalmazza, az első megközelítésre értelemben sok teszi. Ebben az esetben használható **DECLARE\_struktúra** meghatározása az egyes események szerkezetét, és ezután beleveheti a modell a **WITH\_adatok** makrót. Ezután egyes események küldése, mint azt a fenti példában szereplő. Az eljárás csak egy egyetlen adatok eseményt szeretne át a **SZERIALIZÁLÓ**.

Ha úgy gondolja, hogy a modell objektumorientált módon kapcsolatos, a második megközelítés előfordulhat, hogy megfeleljen meg. Ebben az esetben az elemeket megadott **WITH\_adatok** az objektum "Tulajdonságok". Események bármely részét átadni, amely tetszik, attól függően, hogy mennyi a "objektum" állapot el szeretné küldeni a felhőbe **SZERIALIZÁLT** .

Nether megközelítés, a jobbra vagy hibás. Csak a tartsa szem előtt a **szerializáló** tár működéséről, és válassza ki a modellezési megközelítés, amely a legjobban megfelel az igényeinek.

## <a name="message-handling"></a>Üzenet kezelése

Ez a cikk az eddigi van csak tárgyalt küldő események IoT-hubon keresztül csatlakozott, és nem címzett üzenetek fogadására. Ennek oka az, hogy a Mit kell tudni az üzenetek fogadására nagymértékben szabályozott van egy [korábbi cikk](iot-hub-device-sdk-c-intro.md). A cikkben visszahívása, hogy üzeneteket regisztrálása a visszahívási üzenet függvény dolgoz:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

A visszahívási függvény hív meg, amikor egy üzenet érkezik majd ír:

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

A **IoTHubMessage** végrehajtása a modell összes művelet szólít fel az adott függvény. Ha például a modell határozza meg, ezt a műveletet:

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

**SetAirResistance** majd neve, amikor ezt az üzenetet küldi el az eszközére.

Mi azt még nem magyarázata még nem üzenet szerializált változata néz ki. Más szóval ha szeretne egy **SetAirResistance** elküldése az eszközre, amely néz?

Ha eszközre küld üzenetet, az Azure IoT SDK szolgáltatáson keresztül szeretné erre. Továbbra is kell, hogy milyen karakterláncot szeretne küldeni egy adott művelet hívása. Az általános formátum az üzenet elküldése a következőképpen jelenik meg:

```
{"Name" : "", "Parameters" : "" }
```

Két tulajdonságok szerializált JSON objektum küld üzenetet: **a művelet (üzenet) neve** és **paramétereit** határozza meg, hogy a művelet tartalmazza.

Például **SetAirResistance** meghívásához küldhet ezt az üzenetet egy eszközt:

```
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

A művelet neve pontosan egyeznie kell a modell definiált művelet. A paraméterek neve mellé kell lennie. Tartsa szem előtt a kis-és nagybetűk. **Név** és a **Paraméterek** állandóan nagybetűssé. Ellenőrizze, hogy a művelet neve és a paraméterek a modell nagybetűk. Ebben a példában a művelet neve pedig a "SetAirResistance" nem "setairresistance".

Ebben a szakaszban ismertetett mindent kell tudnia, ha küldő események és fogadását az üzeneteket a **szerializáló** tárat. Mielőtt, nézzük néhány a paramétereivel is vannak, amelyek a modell mekkora tárgyalja.

## <a name="macro-configuration"></a>Makró beállítása

A **szerializáló** tár használata során fontos tudatában kell lennie a SDK az azure-c-megosztott-segédprogram tárban található.
Ha az Azure-iot-SDK tárházba GitHub – rekurzív parancs a van klónozva, majd találja a megosztott segédprogram tár itt:

```
.\\c\\azure-c-shared-utility
```

Ha nem a tár példányát klónozták, megtalálhatja a [Itt](https://github.com/Azure/azure-c-shared-utility).

A megosztott segédprogram tárakban találja a következő mappába:

```
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

Ez a mappa neve Visual Studio megoldást **makró\_utils\_h\_generator.sln**:

  ![](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.PNG)

A program a megoldást hoz létre a **makró\_utils.h** fájlt. Alapértelmezett makró van\_a SDK részét képező utils.h fájlt. Ez a megoldás lehetővé teszi néhány paramétereinek módosítása, és hozza létre a következő paraméterek alapján élőfej fájlt.

Az érintett két fő paraméter pedig **nArithmetic** határozzák meg a makró található két sort **nMacroParameters** \_utils.tt:

```
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>

```

Ezeket az értékeket a SDK részét képező alapértelmezett paraméterek. Az egyes paraméterek a következő jelentését foglalja magában:

-   nMacroParameters – vezérlőket is, hogy ki egy DECLARE hány paraméterek\_MODELL makró definíciójának.

-   nArithmetic – szabályozza az adatmodell engedélyezett tagjainak száma.

Fontosak a paramétereket oka az, mert azok szabályozhatja, hogy milyen nagy lehet a modell. Például érdemes megvizsgálni a modell meghatározása:

```
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

Korábban említett **DECLARE\_MODELL** csak a C makrók. A modell nevét, és a **WITH\_adatok** utasítás (még egy másik makró) határozza meg **DECLARE\_MODELL**. **nMacroParameters** határozza meg, hány paraméterek beépíthetők **DECLARE\_MODELL**. Hatékony Ez határozza meg, hány adatok eseményt, és a művelet nyilatkozatokat is. Például az alapértelmezett határértékén 124 Ez azt jelenti, hogy körülbelül 60 műveletek és adatok események modell adhatja meg. Próbál haladja meg ezt a korlátot, kap akkor fordítási hibák, a következőhöz hasonló:

  ![](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.PNG)

A **nArithmetic** paraméter többet az alkalmazás a makró nyelv belső működését.  Meghatározza, hogy az lehet, hogy a modellből, többek között a **DECLARE_STRUCT** makrók tagok száma. Ha elindítja a fordító hibákat, például ezzel jelenik meg, majd meg kell növelje **nArithmetic**:

   ![](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.PNG)

Ha ezek a paraméterek módosítani szeretné, módosíthatja az értékeket a makró\_utils.tt fájlt, a makró fordítani\_utils\_h\_generator.sln megoldást, és futtassa a lefordított program. Amikor új makró így tesz\_létrehozott és helyezett utils.h fájlt a. \\közös\\Inc. címtár.

Az új verzió makró használatához\_utils.h, távolítsa el a **szerializáló** NuGet csomag a megoldás, és vegye fel a **szerializáló** Visual Studio projekt elfoglalt helyét. A kód szemben a szerializáló tár forráskódot összeállításához szolgáltatás lehetővé teszi. Ide tartoznak a frissített makró\_utils.h. Ha azt szeretné, hogy művelet **simplesample\_amqp**, kezdje azzal, hogy a megoldás a szerializáló tár a NuGet csomag eltávolítása:

   ![](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.PNG)

Ehhez a projekthez majd hozzá a Visual Studio megoldás:

> . \\c\\szerializáló\\összeállítása\\windows\\serializer.vcxproj

Amikor elkészült, a megoldás így néz ki:

   ![](media/iot-hub-device-sdk-c-serializer/05-serializer-project.PNG)

Amikor most a a megoldás, a frissített makró összeállítása\_utils.h a bináris szerepel.

Figyelje meg, hogy ezeket az értékeket elég magas növekvő túllépheti a fordító korlátai. Ez a pont a **nMacroParameters** , amellyel érintett a fő paramétert. A C99 specifikáció Itt adhatja meg, hogy a makró-definícióban engedélyezettek 127 paraméterek legalább. A Microsoft fordító pontosan követi a specifikáció (és az 127 legfeljebb), így Ön nem tudja **nMacroParameters** túl az alapértelmezett növeléséhez. Más fordítók bármelyikével előfordulhat, hogy engedélyezi ezt (például a GNU fordító támogatja a magasabb határérték).

Az eddigi azt már szereplő tudnivalók arról, hogy miként kódírás a **szerializáló** tárral mindent. Megkötése, mielőtt vegyük az előző foglalkozó cikkekre, előfordulhat, hogy kell tudni szeretné, hogy a súgótémakörökben látogatnia.

## <a name="the-lower-level-apis"></a>Alsóbb szintű API-khoz

Ez a cikk nagy része a minta-alkalmazás **simplesample\_amqp**. Használja az alábbi példa a magasabb szintű (a nem-"s") API-k események küldhet és fogadhat üzeneteket. Ezek API-k használata esetén a háttérben egy szál fut, amely egyaránt események üzenetek küldése és fogadása gondoskodik. Jó helyen jár a háttérben szál és közvetlen irányítás fölé, amikor Ön események üzenetek küldésére és fogadására a felhőből alsóbb szintű (s) API-khoz is használhatja.

Egy [korábbi cikk](iot-hub-device-sdk-c-iothubclient.md)útmutatását nem függvények csoportja, amely a magasabb szintű API-k áll:

-   IoTHubClient\_CreateFromConnectionString

-   IoTHubClient\_SendEventAsync

-   IoTHubClient\_SetMessageCallback

-   IoTHubClient\_Destroy

Ezeket az API-khoz is mutatni a **simplesample\_amqp**.

Van még egy hasonló alsóbb szintű API-khoz csoportja.

-   IoTHubClient\_s\_CreateFromConnectionString

-   IoTHubClient\_s\_SendEventAsync

-   IoTHubClient\_s\_SetMessageCallback

-   IoTHubClient\_s\_Destroy

Figyelje meg, hogy a alsóbb szintű API-k pontosan a várt módon működik, az előző cikkekben ismertetett módon. Az első készlet az API-khoz is használhatja, ha azt szeretné, hogy a háttér szál küldő események és érkező üzenetek kezelésére. Ha azt szeretné, hogy mikor küldéskor és fogadáskor használandó adatokat IoT központból explicit szabályozható használhatja a második készlete API-khoz. Bármelyik API-khoz munka beállítása a **szerializáló** tárral egyaránt csakúgy.

Példa a alsóbb szintű API-k használata a **szerializáló** tárral, lásd: a **simplesample\_http** alkalmazást.

## <a name="additional-topics"></a>További témakörök

Néhány egyéb témaköröket értékű megemlítése újra tulajdonság kezelése, az alternatív eszköz hitelesítő adatok és a beállítási lehetőségek. Ezek az [előző cikkben](iot-hub-device-sdk-c-iothubclient.md)szereplő összes témaköröket. A fő pont, hogy ezek a funkciók működnek a **szerializáló** tárral ugyanúgy, mind a **IoTHubClient** tárral. Például ha szeretné az esemény tulajdonságai csatolása a modellből, használhatja **IoTHubMessage\_tulajdonságok** és a **térkép**\_**AddorUpdate**, hasonlóan a korábban leírt:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Hogy eseményt hozza létre a **szerializáló** tárból, illetve a **IoTHubClient** tár manuális használatával létrehozott nem számít.

A hitelesítő adatok alternatív eszköz használatával **IoTHubClient\_s\_létrehozása** ugyanolyan jól működik, **IoTHubClient\_CreateFromConnectionString** felosztásának egy **IOTHUB\_ügyfél\_KEZELNI**.

Végül a **szerializáló** tár használata, beállíthatja, hogy a beállítási lehetőségek **IoTHubClient\_s\_SetOption** ugyanúgy, mint a **IoTHubClient** tár használata esetén.

Egy egyedi a **szerializáló** tár szolgáltatást a inicializálni API-khoz is. Mielőtt megkezdené a tárban dolgozik, meg kell hívni **szerializáló\_init**:

```
serializer_init(NULL);
```

Ebben az esetben csak, mielőtt hívná **IoTHubClient\_CreateFromConnectionString**.

Hasonlóképpen, ha végzett a tárban dolgozik, a legutóbbi hívást fogja használni szeretné, hogy **szerializáló\_deinit**:

```
serializer_deinit();
```

Egyéb esetben a fent felsorolt az egyéb funkciók ugyanúgy működik, a **szerializáló** tárban mint a **IoTHubClient** tárban. További információt az alábbi témakörök közül olvassa el a [cikk korábbi](iot-hub-device-sdk-c-iothubclient.md) a sorozat című témakört.

## <a name="next-steps"></a>Következő lépések

Ez a cikk részletesen ismerteti a **szerializáló** tárban található az **Azure IoT eszköz c SDK**egyedi tulajdonságát. A adatokat kapja a van, hogy hogyan modellek segítségével események üzenetek küldése és fogadása IoT központból jól érthető.

Ezzel az három részből álló sorozatot módjáról az **Azure IoT eszköz c SDK**alkalmazások fejlesztői számára is véget ér. Meg kell elegendő információt, nem csak az első lépések, de az API-k működése alapos ismerete ad. További információért vannak néhány minták nem vonatkozik a SDK csomagjában talál. Ellenkező esetben a [dokumentáció SDK csomagjában talál](https://github.com/Azure/azure-iot-sdks) további információt a jó erőforrás értéke.


IoT központi fejlesztésével kapcsolatos további tudnivalókért lásd: a [IoT központi SDK][lnk-sdks].

További feltárása a IoT központi funkcióinak, olvassa el:

- [Egy eszközt a IoT átjáró SDK csomagjában talál, amelyek][lnk-gateway]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
