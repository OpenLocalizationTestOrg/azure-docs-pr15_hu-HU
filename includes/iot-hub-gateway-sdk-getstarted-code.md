## <a name="typical-output"></a>Kimenete

Az alábbi példája a kimenetet írja be a naplófájl a Helló, világ minta. Lap és a soremelés karaktereket olvashatóságát adtak:

```
[{
    "time": "Mon Apr 11 13:48:07 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:48:48 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:48:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:01 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:04 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a>Kódtöredékek

Ez a rész ismerteti a kódot a Helló, világ mintában néhány kulcsfontosságú része.

### <a name="gateway-creation"></a>Átjáró létrehozása

A fejlesztő az *átjáró folyamat*kell írni. A program létrehozza a belső infrastruktúra (broker), betölti a modulok és beállítja mindent helyesen fog működni. Az SDK tartalmaz egy JSON fájl átjáró bootstrap teszi lehetővé a **Gateway_Create_From_JSON** függvény. A **Gateway_Create_From_JSON** függvény használatakor meg kell adja át az elérési út egy JSON fájl, amely megadja a betöltendő modulok. 

A kódot a gateway folyamat a [main.c] a Helló, világ mintában található[ lnk-main-c] fájlt. Az alábbi objektumdarab olvashatóságát, rövidített változata az átjáró folyamat kódot jeleníti meg. Ez a program létrehoz egy átjárót, majd megvárja, amíg a felhasználó előtt azt kiváltó lefelé a átjáró, nyomja le az **ENTER** billentyűt. 

```
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed to create the gateway from JSON\n");
    }
    else
    {
        printf("gateway successfully created from JSON\n");
        printf("gateway shall run until ENTER is pressed\n");
        (void)getchar();
        Gateway_LL_Destroy(gateway);
    }
    return 0;
}
```

A JSON fájl betöltése modulok listáját tartalmazza. Minden modul a: kell megadni.

- **module_name**: a modul egy egyedi nevet.
- **module_path**: a a modult tartalmazó könyvtár elérési útvonalát. A Linux egy .so fájl, a Windows rendszer DLL-fájl.
- **argumentumok**: minden konfigurációs adatokat kell a modult.

A JSON fájlt is tartalmaz, amely a munkamenet-átvitelszervező átkerülnek a modulok közötti kapcsolatokat. Egy kapcsolat két tulajdonsága van:
- **forrás**: a modul nevét a `modules` szakasz, vagy "\*".
- **gyűjtő**: a modul nevét a `modules` szakasz

Minden kapcsolat határozza meg, a üzenet útvonalának és irányát. Modul üzenetei `source` kell leszállítani a modulba való `sink`. A `source` lehet "\*", jelezve, hogy a modulok érkező üzenetek által kapott `sink`.

A következő példában a JSON fájl a Helló, világ minta Linux konfigurálásához. Minden üzenet modul által gyártott `hello_world` modul fogyni fog `logger`. E modul szükséges argumentum függ a modul. Ebben a példában a naplózó modul tart egy argumentumot, amely a kimeneti fájl elérési útját, és a Helló, világ modul lép argumentumai:

```
{
    "modules" :
    [ 
        {
            "module name" : "logger",
            "module path" : "./modules/logger/liblogger_hl.so",
            "args" : {"filename":"log.txt"}
        },
        {
            "module name" : "hello_world",
            "module path" : "./modules/hello_world/libhello_world_hl.so",
            "args" : null
        }
    ],
    "links" :
    [
        {
            "source" : "hello_world",
            "sink" : "logger"
        }
    ]
}
```

### <a name="hello-world-module-message-publishing"></a>Helló világ modul üzenet közzététele

Üzenetek közzététele az a ["hello_world.c"] a "Helló, világ" modul által használt kód található[ lnk-helloworld-c] fájlt. Az alábbi objektumdarab további megjegyzéseket és néhány hibakezelési kód olvashatóságát eltűnik a módosított változat mutatja:

```
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);
    
    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set the properties for the message
    msgConfig.sourceProperties = propertiesMap;
    
    // create a message based on the msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of the thread*/
        }
        else
        {
            // publish the message to the broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

### <a name="hello-world-module-message-processing"></a>Helló világ modul üzenetek feldolgozása

A Helló, világ modul soha nem kell-e dolgoznia az üzenetek, a munkamenet-átvitelszervező közzététele más modulokban. Így az üzenet-visszahívás végrehajtása a Helló, világ modul műveletvégzés függvényt.

```
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a>Naplózó modul üzenet közzététele és feldolgozása

A naplózó modul közvetítő üzeneteket kap, és fájlba írja. Üzenetek soha nem teszi közzé. A kódot a naplózó modul, ezért soha nem hívja a **Broker_Publish** függvényt.

A **Logger_Recieve** függvény a [logger.c] [ lnk-logger-c] fájl a visszahívás, a munkamenet-átvitelszervező elindítja a naplózó modul küldött üzenetek kézbesítésekor. Az alábbi objektumdarab további megjegyzéseket és néhány hibakezelési kód olvashatóságát eltűnik a módosított változat mutatja:

```
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get the message properties from the message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert the collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode the message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start the construction of the final string to be logged by adding
    // the timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add the message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add the content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write the formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a>A következő lépések

A IoT átjáró SDK használata című témakörben talál a következő:

- [IoT átjáró SDK – eszköz-felhő üzeneteket küldeni egy szimulált eszköz segítségével Linux][lnk-gateway-simulated].
- [Borzas IoT átjáró SDK] [ lnk-gateway-sdk] a GitHub.

<!-- Links -->
[lnk-main-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/logger/src/logger.c
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-gateway-simulated]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md