<properties
    pageTitle="Azure IoT eszköz SDK C - IoTHubClient |} Microsoft Azure"
    description="Többet szeretne tudni az Azure IoT eszköz SDK IoTHubClient tárba, a C használata"
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

# <a name="microsoft-azure-iot-device-sdk-for-c--more-about-iothubclient"></a>Microsoft Azure IoT eszköz SDK c – további információ: IoTHubClient

A sorozat [első a cikk](iot-hub-device-sdk-c-intro.md) a **Microsoft Azure IoT eszköz c SDK**jelent meg. E cikk magyarázata, hogy vannak-e két építészeti rétegek SDK csomagjában talál. Az alap van a **IoTHubClient** tárban, amely közvetlenül a IoT központi való kommunikáció kezeli. Hozza létre a felül a azaz szerializálási szolgáltatások **szerializáló** tárban is van. Ebben a cikkben biztosítjuk fogja további információt a **IoTHubClient** tárat.

Az előző témakör leírt használata a **IoTHubClient** tár IoT-hubon keresztül csatlakozott a események küldhet és fogadhat üzeneteket. Ez a cikk a vitafórum, hogy miként pontosan a *Ha* küld vagy fogad, adatok bemutatása, az **alsó szintű API-k**kezelése terjed ki. Azt fogja magyarázatot tulajdonságok csatolása események (, valamint az üzenetek) a tulajdonságot a **IoTHubClient** tárban szolgáltatások kezelése. Végezetül azt fogja magyarázatot további IoT központból Beérkezett üzenetek kezelése eltérő módon.

A cikk néhány egyéb témaköröket, például több eszköz hitelesítő adatok és a beállítási lehetőségek között a **IoTHubClient** viselkedésének módosítása kiterjed köt.

A **IoTHubClient** SDK minták használjuk az alábbi témakörök elmagyarázni. Ha követni kívánt, olvassa el a **iothub\_ügyfél\_minta\_http** és **iothub\_ügyfél\_minta\_amqp** alkalmazások, a c mindent az alábbi szakaszokban ismertetett e minták van igazolni az Azure IoT eszközben SDK szereplő.

Az **Azure IoT eszköz c SDK** keresse meg a [Microsoft Azure IoT SDK](https://github.com/Azure/azure-iot-sdks) GitHub adattárban, és az API részleteinek megtekintése a [C API hivatkozást](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html).

## <a name="the-lower-level-apis"></a>Alsóbb szintű API-khoz

Az előző cikkben leírt a **IotHubClient** a környezetén belül az alapműveletek a **iothub\_ügyfél\_minta\_amqp** alkalmazást. Például a tárba, a kód inicializálni hogyan magyarázata azt.

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Is használja a funkcióval hívás események üzenetküldés leírt módon.

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

A cikk is üzenetek fogadására regisztrálja a visszahívási függvény hogyan leírt módon.

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

A cikk az ingyenes források, például a következő kód használatával hogyan is kiderült.

```
IoTHubClient_Destroy(iotHubClientHandle);
```

Vannak azonban companion függvények minden e API-hoz:

-   IoTHubClient\_s\_CreateFromConnectionString

-   IoTHubClient\_s\_SendEventAsync

-   IoTHubClient\_s\_SetMessageCallback

-   IoTHubClient\_s\_Destroy


Ezek a függvények az összes "S" API-név bele. Kívül, hogy az egyes függvényekhez a paraméterek megegyeznek a nem s mint. Azonban ezeket a funkciókat viselkedését eltér fontos módon.

Amikor felhívja **IoTHubClient\_CreateFromConnectionString**, az alapul szolgáló tárak, hozzon létre egy új szál, a háttérben futó. A szál küld eseményeket, és fogadja üzeneteket központból, IoT. Nincs ilyen szál jön létre, amikor a "s" API-k használata. A háttér szál létrehozását a könnyebb a fejlesztők. Nem kell aggódnia amiatt, hogy kifejezetten események üzenetek küldése és fogadása IoT központból – automatikusan történik a háttérben. Viszont "S" API-khoz ad IoT-központban való kommunikáció explicit szabályozható, ha szüksége lenne rá.

Ez jobb megértéséhez, lássunk egy példát:

Amikor felhívja **IoTHubClient\_SendEventAsync**, mi ténylegesen csinál van illusztráló az esemény egy pufferelési. A háttér szál hívásakor létrehozott **IoTHubClient\_CreateFromConnectionString** folyamatosan figyeli a puffer, és értesítést küld a benne lévő adatokat IoT-hubon keresztül csatlakozott. Ez történik a háttérben fut, egy időben, hogy a fő szál más munka hajt végre.

Hasonlóképpen, amikor regisztrál az üzenetek visszahívása függvényének **IoTHubClient\_SetMessageCallback**, modulban esetén a szeretné, hogy a háttér szál meghívása a visszahívás függvény, amikor egy üzenetet a beérkezett, a fő szál független nem a SDK csomagjában talál.

A "s" API-khoz nem hoz létre a háttérben egy szál. Ehelyett egy új API kifejezetten működőképessé tehető adatok IoT központból meg kell hívni. Ez a következő példában az igazolni.

A **iothub\_ügyfél\_minta\_http** alkalmazást, amelyben szerepel a SDK bemutatja az alsóbb szintű API-khoz. Ezt a minta elküldjük események IoT-hubon keresztül csatlakozott, például a következő kódot:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

Az első három vonalak, hozzon létre az üzenetet, és az utolsó sor elküldi az eseményt. Azonban korábban említett, "Küldés" az eseményt, az azt jelenti, hogy az adatokat egy pufferelési egyszerűen kerül. Nem történik adatátvitel a hálózaton, hogy a hívás **IoTHubClient\_s\_SendEventAsync**. Sorrendben, a ténylegesen bejövő adatok az adatok IoT-hubon keresztül csatlakozott, meg kell hívni **IoTHubClient\_s\_DoWork**, az alábbi példában:

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Kód (a a **iothub\_ügyfél\_minta\_http** alkalmazás) többször hív **IoTHubClient\_s\_DoWork**. Minden alkalommal, amikor **IoTHubClient\_s\_DoWork** van neve, bizonyos eseményeket a puffer IoT központi által ismert és elküldése az eszközre az aszinkron üzenet olvassa be. Az utóbbi esetben azt jelenti, hogy ha az üzenetek visszahívása függvény regisztrálva azt, majd a visszahívás meghívott (feltételezve, hogy minden sorban állnak). Azt szeretné regisztrált visszahívási függvény például a következő kódot:

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

Az OK, amely **IoTHubClient\_s\_DoWork** ismétlődve úgynevezett, amely minden alkalommal, amikor ennek neve, *néhány* pufferelt esemény küld IoT központi, és olvassa be *a következő* üzenet várakozik az eszközt. Minden hívás küldése az összes pufferelt események nincs garantált vagy összes beolvasásához aszinkron üzenet. Ha összes eseményének küldése a puffer szeretne készíteni, majd folytassa a más feldolgozás lecserélheti a leállításig például a következő kódot:

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Ez a kód felhívja **IoTHubClient\_s\_DoWork** mindaddig, amíg az összes eseményének puffer az elküldött IoT-hubon keresztül csatlakozott. Megjegyzés: Ez nem is jelenti, hogy érkezett-e a várakozó üzeneteket. A hiba okát részét képezi, hogy nem "az összes" üzenetek keresése a mérvadó művelet. Mi történik, ha beolvasni a "minden" üzenetet, de a másik elküldi az eszközre közvetlenül azután, hogy? Hatékonyabb módszerre kezelésére, amely egy programozott időkorlát. Például a visszahívási üzenet függvény lehetett alaphelyzetbe állítani egy időzítő minden alkalommal, amikor ezt hívják. Logika feldolgozás továbbra is, ha például nincs üzenet nem érkezett utolsó *X* másodpercben majd írhat.

Amikor elkészült ingressing események éppen, és az üzenetek fogadására ügyeljen arra, hogy hívja fel a megfelelő függvénnyel karbantartása: erőforrások.

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

Alapvetően nem küldhet és fogadhat API-k, mely ugyanazt a háttérben szál nélkül a háttérben egy szál és más adatokat API-k csak egy csoportja. A fejlesztők sok előfordulhat, hogy inkább a nem - s API-k, de az alsóbb szintű API-k hasznosak, ha a Fejlesztőeszközök hálózati átvitelek explicit szabályozni szeretné. Ha például egyes eszközök esetén adatgyűjtés időt, és csak bejövő adatok események (például egy óra egyszer vagy egyszer) megadott időközönként. Alsóbb szintű API-khoz lehetőséget kínálunk explicit módon szabályozhatja amikor küldése és fogadása adatok IoT központból. Mások fog egyszerűen inkább az egyszerűség, amelyekkel az alsóbb szintű API-khoz. Minden egyes munka jelet a háttérben fut, hanem a fő szál fordul elő.

Bármelyik lehetőséget választja, modell ne felejtse el a melyik API-hoz használt konzisztens. Ha kezd meghívásával **IoTHubClient\_s\_CreateFromConnectionString**, lehet, hogy csak a megfelelő alsóbb szintű API-et követési munkáját:

-   IoTHubClient\_s\_SendEventAsync

-   IoTHubClient\_s\_SetMessageCallback

-   IoTHubClient\_s\_Destroy

-   IoTHubClient\_s\_DoWork

Ellenkező is igaz. Ha készítésénél kiindulhat **IoTHubClient\_CreateFromConnectionString**, majd használja a nem - s API-k bármilyen további feldolgozásra.

Az Azure IoT eszköz SDK c, olvassa el a **iothub\_ügyfél\_minta\_http** a alsóbb szintű API-k egy teljes például alkalmazást. A **iothub\_ügyfél\_minta\_amqp** alkalmazás hivatkozhat egy teljes például olyan a nem - s API-t.

## <a name="property-handling"></a>A tulajdonság kezelése

Az eddigi azt által bemutatott adatokat küldő, amikor azt már lett hivatkozó az üzenet törzsébe. Például érdemes megvizsgálni a kódot:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

Ez a példa egy üzenetet küld IoT központi szöveget tartalmazó "Helló, világ." Azonban IoT központi is lehetővé teszi, hogy minden üzenethez csatolandó tulajdonságait. Tulajdonságait név/érték összekapcsolásával, az üzenet csatolható. Például az előző kódot, amelyet egy tulajdonságot az üzenethez csatolni módosíthatja azt:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Akkor indítsa el, hívja fel **IoTHubMessage\_tulajdonságok** és az üzenet a leíró beengedné. Mi azt visszatérhet van egy **térkép\_KEZELNI** hivatkozását, amely lehetővé teszi, hogy arra, hogy tulajdonságokat el nekünk. Hívja fel elvégezni **térkép\_AddOrUpdate**, Térképre mutató hivatkozás, amely veszi\_FOGÓPONTRA, a tulajdonság neve és a tulajdonság értékét. Ez az API-val azt annyi tulajdonságokat amilyen azt is hozzáadhat.

Az **Esemény hubok**elolvasásáról az eseményt, a címzett tulajdonságainak felsorolása, és beolvasni a megfelelő értékeket. Például a .NET Ez esetben lehet megvalósítani [EventData-objektum tulajdonságok gyűjtemény](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx)megnyitása.

Az előző példában azt esetén csatolása a tulajdonságok egy eseményt, IoT hubhoz elküldjük. Tulajdonságok is kapcsolható IoT központból Beérkezett üzenetek. Tulajdonságok lekérhetők egy üzenet szeretnénk, ábrázolásakor például a következő kódot az üzenet visszahívása függvény:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from the message
    MAP_HANDLE mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                printf("Message Properties:\r\n");
                for (size_t index = 0; index < propertyCount; index++)
                {
                    printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                printf("\r\n");
            }
        }
    }

    . . .
}
```

A hívást **IoTHubMessage\_tulajdonságok** adja eredményül a **térkép\_KEZELNI** hivatkozást. Azt, majd át, hogy hivatkozás **térkép\_GetInternals** beszerzése a név/érték összekapcsolásával (csakúgy, mint az a tulajdonságokat száma) tömb mutató hivatkozás. Ezen a ponton eléréséhez kattintson az értékek szeretnénk tulajdonságainak felsorolása egyszerű kérdése.

Nem kell tulajdonságainak használata az alkalmazásban. Jó helyen jár Ha kell azokat meg események vagy üzenetek lekérése őket, a **IoTHubClient** tár egyszerűen.

## <a name="message-handling"></a>Üzenet kezelése

Üzenetek érkezésekor IoT központból korábban jelzett módon válaszol a **IoTHubClient** tár meghívása regisztrált visszahívási függvény. Van, hogy néhány további magyarázat érdemel függvény visszatérési paraméter. Az alábbiakban a visszahívás függvény egy szövegrészt a **iothub\_ügyfél\_minta\_http** alkalmazás minta:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Megjegyzendő, hogy a visszatérési típus **IOTHUBMESSAGE\_Törlés\_eredmény** és az adott esetben azt visszatérési **IOTHUBMESSAGE\_elfogadott**. Azt is visszaadhat, ez a funkció át más értékek, módosítsa, hogyan a **IoTHubClient** tár reagál az üzenet visszahívása is. Az alábbiakban a beállításokat.

-   **IOTHUBMESSAGE\_elfogadott** – az üzenet feldolgozása sikeresen megtörtént. A **IoTHubClient** tár nem meghívják a visszahívás függvényt ismét ugyanazt az üzenetet.

-   **IOTHUBMESSAGE\_elutasítva** – az üzenet feldolgozása nem, és nem a jövőben ehhez irányuló nem. A **IoTHubClient** tár kell nem indítható el a visszahívás függvényt ismét ugyanazt az üzenetet.

-   **IOTHUBMESSAGE\_ABANDONED** – az üzenet nem feldolgozása sikeresen megtörtént, de a **IoTHubClient** tár kell elindítani a visszahívás függvényt ismét ugyanazt az üzenetet.

Az első két visszatérési kódokat az a **IoTHubClient** tár üzenetet küld, IoT-hubon keresztül csatlakozott, hogy az üzenetet a eszköz sorból törölt kell-e, és nem kézbesítve újra jelző. A nettó hatást azonos (az üzenet törlődnek az eszközről sorból), de továbbra is átirányítása e az üzenet elfogadták vagy visszautasították.  Felvétel a különbséget akkor hasznos, a feladóknak az üzenet is meghallgatása visszajelzést, és ismerje meg, ha egy eszközt elfogadták vagy visszautasították egy adott üzenet.

Az utolsó jelenik meg üzenet is elküldése IoT-hubon keresztül csatlakozott, de azt jelzi, hogy az üzenet kell-e újra. A szokásos üzenet fog elhagyja, ha hibát néhány, de szeretne kipróbálni feldolgozni ismét az üzenetet. Viszont üzenet visszautasítása nem megfelelő helyrehozhatatlan hibát tapasztal, amikor (vagy ha egyszerűen csak nem szeretné az üzenet feldolgozása).

Minden esetben a felhasználónevek a különböző visszatérési kódokat, hogy akkor is kér a **IoTHubClient** tárból kívánt viselkedését.

## <a name="alternate-device-credentials"></a>Alternatív eszköz hitelesítő adatok

Című cikkben ismertetett módon korábban,-e a legfontosabb dolog a teendő, ha a **IoTHubClient** tárral dolgozó beszerzése a **IOTHUB\_ügyfél\_KEZELNI** a hívást, például a következőket:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Az argumentumok **IoTHubClient\_CreateFromConnectionString** vannak a kapcsolati karakterláncot az eszköz és a protokoll használatával kommunikál a IoT központi használjuk jelző paraméter. A kapcsolati karakterlánc formátuma a következőképpen jelenik meg:

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

Vannak olyan az információk ebben a karakterláncban négy darab: IoT központi IoT központi utótag, Eszközazonosítót és, valamint a megosztott hívóbetű. Fog kapni a teljes tartománynevét (FQDN) egy IoT hubhoz, amikor hozza létre a IoT központi az Azure-portálon – ezzel kap IoT központi nevét (a teljes Tartománynevét első része) és a IoT központi utótag (a többi a FQDN). Amikor regisztrál az eszköz a IoT központi (mint az [előző cikkben](iot-hub-device-sdk-c-intro.md)ismertetett) az Eszközazonosítót és a megosztott hívóbetű kattint.

**IoTHubClient\_CreateFromConnectionString** egyik módja a tár inicializálni lehetővé teszi. Ha azt szeretné, akkor hozhat létre egy új **IOTHUB\_ügyfél\_KEZELNI** a kapcsolati karakterláncot, hanem a egyes paraméterek használatával. Ez a következő kódot tartalmazó érhető el:

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

Ez a teljesít ugyanaz, mint **IoTHubClient\_CreateFromConnectionString**.

Tűnhet, hogy szeretné használni kívánt egyértelmű **IoTHubClient\_CreateFromConnectionString** helyett a inicializálni részletesebb metódusát. Ne feledje, azonban, hogy amikor regisztrál az eszköz IoT-központban tudja elkezdeni pedig egy Eszközazonosítót eszköz kulcs (nem a kapcsolati karakterlánc). A **Eszközkezelő** SDK jelent meg az [Előző témakör](iot-hub-device-sdk-c-intro.md) használja az **Azure IoT szolgáltatás SDK** tárakat a kapcsolati karakterlánc létrehozása a Eszközazonosítót, az eszköz billentyűt, és a IoT központi állomásnév. Így hívása **IoTHubClient\_s\_létrehozása** helyett azért lehet, mert ez a lépés a kapcsolati karakterlánc létrehozása takaríthat meg. Használja az kényelmes felöltési módot.

## <a name="configuration-options"></a>Beállítási lehetőségek

Az eddigi minden információ a **IoTHubClient** tár működésének leírt tükrözi, az alapértelmezett működés. Vannak azonban olyan való működésének beállítása a tárban beállíthatja, hogy néhány beállításokat. Ez megvalósítani feljebb helyezése a **IoTHubClient\_s\_SetOption** API-val. Fontolja meg ebben a példában:

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

Számos gyakran használt beállítások:

-   **SetBatching** (logikai) – Ha **Igaz**, akkor IoT központi küldött adatok elküldése egy csoportba. Ha a **hamis**, akkor üzeneteket küld egyenként. Az alapértelmezett érték **hamis**. Figyelje meg, hogy a **SetBatching** beállítás csak akkor alkalmazható a HTTP-jegyzőkönyv, nem pedig a MQTT vagy AMQP protokollok.

-   **Határidő** (alá nem írt int) – ezredmásodpercben jeleníti meg ezt az értéket. Ha HTTP felkérés vagy kap választ a időnél tovább tart, majd a kapcsolat időtúllépés történik.

Fontos a eljárásnak lehetőséget. Alapértelmezés szerint a tár ingresses események egyenként (egyetlen olyan esemény, függetlenül a sikeres kattintva **IoTHubClient\_s\_SendEventAsync**). A eljárásnak beállítás értéke **Igaz**, ha az a tár annyi események (felfelé az üzenetek maximális mérete, amely IoT központi akkor fogad el) a puffer a igénybe gyűjti össze.  Az esemény köteg IoT központi telefonál egyetlen HTTP (az egyes események egy JSON tömböt be vannak csoportosítva) küldi. A szokásos kötegelés engedélyezése eredményez nagy teljesítmény nyereség óta esetén csökkentése a hálózat állapotát a körbejárásokhoz. Is jelentősen csökkenti a sávszélesség, mivel szeretné elküldeni a HTTP-fejlécek egy esemény köteget, az egyik készlete helyett a fejlécek minden egyes esemény beállítása. Másképp valamiért működött általában célszerű kötegelés engedélyezése.

## <a name="next-steps"></a>Következő lépések

Ez a cikk részletesen ismerteti a **IoTHubClient** tárban található az **Azure IoT eszköz c SDK**viselkedését. Ezekkel az információkkal az funkciók a **IoTHubClient** tár jól érthető kell rendelkeznie. A [következő cikk](iot-hub-device-sdk-c-serializer.md) Ez a témakör a **szerializáló** tár hasonló részletesen.

IoT központi fejlesztésével kapcsolatos további tudnivalókért lásd: a [IoT központi SDK][lnk-sdks].

További feltárása a IoT központi funkcióinak, olvassa el:

- [Egy eszközt a IoT átjáró SDK csomagjában talál, amelyek][lnk-gateway]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
