<properties 
    pageTitle="Alkalmazás szolgáltatás API-alkalmazás indítók |} Microsoft Azure" 
    description="Eseményindítók megvalósításáról Azure alkalmazás szolgáltatás egy API-alkalmazásban" 
    services="logic-apps" 
    documentationCenter=".net" 
    authors="guangyang"
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="na" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="rachelap"/>

# <a name="azure-app-service-api-app-triggers"></a>Azure alkalmazás szolgáltatáshoz tartozó API-alkalmazás indítók

>[AZURE.NOTE] Ez a cikk verziójának API alkalmazások 2014-es – 12-01-séma verziót vonatkozik.


## <a name="overview"></a>– Áttekintés

Ez a cikk ismerteti, hogyan API-alkalmazás indítók végrehajtásához, és felhasználja őket logika alkalmazásból.

Az összes a kódtöredék ebben a témakörben a [FileWatcher API-alkalmazás kód minta](http://go.microsoft.com/fwlink/?LinkId=534802)másolja. 

Megjegyzendő, hogy a kód, a jelen cikkben létre, és futtassa a következő nuget csomag letöltése kell: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).

## <a name="what-are-api-app-triggers"></a>Mik azok a API-alkalmazás indítók?

A leggyakoribb helyzet az API alkalmazások egy eseményt, hogy az ügyfélprogramok az API-alkalmazás vehet a megfelelő műveletet reagálni az esemény. A REST API-alapú mechanizmusa, amely támogatja az ebben az esetben API-alkalmazás eseménykód neve. 

Ha például tegyük fel, hogy az ügyfél-kódot a [Twitter összekötő API alkalmazást](../app-service-logic/app-service-logic-connector-twitter.md) használ, és a kódot kell egy adott szavakat tartalmazó új twitterre alapján műveletet. Ebben az esetben előfordulhat, hogy beállítása a szavazás vagy leküldéses eseményindító elősegítésére e szükség.

## <a name="poll-trigger-versus-push-trigger"></a>A szavazás eseményindító és a leküldéses eseményindító

Jelenleg támogatott indítók két típusa:

- A szavazás eseményindító - ügyfél lekérdezi egy eseményt, amelynek formázás változásokra figyelmeztető üzenet az API-alkalmazás 
- Leküldéses eseményindító - ügyfél értesíti az API-alkalmazás esemény akkor indul el, ha 

### <a name="poll-trigger"></a>A szavazás eseményindító

A szavazás eseményindító történik, mint normál REST API-t, és ügyfelei (például egy logikai alkalmazást) várhatóan lekérdezi azt annak érdekében, hogy értesítést kapni. Az ügyfél előfordulhat, hogy állapotban, magát a szavazás az eseményindító állapot nélküli is. 

A következő információkat a kérelem és válasz csomagok kapcsolatos néhány főbb tényezők a szavazás eseményindító szerződés szemlélteti:

- Kérés
    - HTTP-metódus: GET
    - Paraméterek
        - triggerState – a választható paraméter lehetővé teszi az ügyfelek állapotukban, hogy a szavazás eseményindító megfelelően eldöntheti, vagy nem ad vissza a értesítés alapján a megadott megadásához.
        - API-specifikus paraméterei
- Válasz
    - Állapot kód **200** - kérés érvényes, és a kiváltó ok mező alapján értesítést. Az értesítés tartalmát a válasz szervezet lesz. A válasz "Újrapróbálkozási utáni" fejlécének, hogy további értesítési adatokat kell lehet beolvasni a későbbi kérelem hívást.
    - Állapot kód **202** - kérés érvényes, de nem az eseményindító az új értesítés van.
    - Állapot kód **4xx** - kérés nem érvényes. Az ügyfél nem kell újra a kérést.
    - Állapot kód **5xx** - kérés vezetett egy belső kiszolgálóhiba és/vagy a ideiglenes hiba. Az ügyfél meg kell ismételni a kérést.

A következő kódrészletet példája egy szavazás eseményindító megvalósításáról.

    // Implement a poll trigger.
    [HttpGet]
    [Route("api/files/poll/TouchedFiles")]
    public HttpResponseMessage TouchedFilesPollTrigger(
        // triggerState is a UTC timestamp
        string triggerState,
        // Additional parameters
        string searchPattern = "*")
    {
        // Check to see whether there is any file touched after the timestamp.
        var lastTriggerTimeUtc = DateTime.Parse(triggerState).ToUniversalTime();
        var touchedFiles = Directory.EnumerateFiles(rootPath, searchPattern, SearchOption.AllDirectories)
            .Select(f => FileInfoWrapper.FromFileInfo(new FileInfo(f)))
            .Where(fi => fi.LastAccessTimeUtc > lastTriggerTimeUtc);

        // If there are files touched after the timestamp, return their information.
        if (touchedFiles != null && touchedFiles.Count() != 0)
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventTriggered(new { files = touchedFiles });
        }
        // If there are no files touched after the timestamp, tell the caller to poll again after 1 mintue.
        else
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventWaitPoll(new TimeSpan(0, 1, 0));
        }
    }

A szavazás eseményindító kipróbálni, kövesse az alábbi lépéseket:

1. Telepítse az API-alkalmazást egy **nyilvános névtelen**hitelesítés beállítás.
2. Hívja fel az **érintéses** művelet érintse meg egy fájlt. Az alábbi képen látható egy minta kérelmet Postman keresztül.
   ![Hívja fel a via Postman érintésvezérelt művelet](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
3. A szavazás eseményindító felhívhatja a **triggerState** paraméter előtt lépés #2 időbélyeget. Az alábbi képen látható, a minta kérelem Postman keresztül.
   ![Hívás a szavazás eseményindító Postman keresztül](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)

### <a name="push-trigger"></a>Leküldéses eseményindító

Leküldéses eseményindító akkor lép érvénybe, mint egy normál REST API-t, amely verembe küldi értesítések ügyfelek számára, aki be kell jelenteni, amikor az adott események fire regisztrálta.

A következő információkat a kérelem és válasz csomagok kapcsolatos néhány főbb tényezők a leküldéses eseményindító szerződés szemléltetése

- Kérés
    - HTTP-metódus: helyezése
    - Paraméterek
        - eseményindító azonosítója: kötelező – átlátszó karakterlánc (például egy globálisan egyedi azonosítója), amely a regisztráció leküldéses eseményindító.
        - callbackUrl: kötelező - meghívni, ha az esemény akkor indul el, a visszahívás URL-CÍMÉT. A meghívási egyszerű bejegyzés HTTP hívás.
        - API-specifikus paraméterei
- Válasz
    - Állapot kód **200** - ügyfél sikeres regisztrálhatja kérelem.
    - Állapot kód **4xx** - kérés nem érvényes. Az ügyfél nem kell újra a kérést.
    - Állapot kód **5xx** - kérés vezetett egy belső kiszolgálóhiba és/vagy a ideiglenes hiba. Az ügyfél meg kell ismételni a kérést.
- A visszahívás
    - HTTP-metódus: BEJEGYZÉSBEN
    - Szervezet kérése: értesítés tartalmat.

A következő kódrészletet leküldéses eseményindító megvalósításáról példája:

    // Implement a push trigger.
    [HttpPut]
    [Route("api/files/push/TouchedFiles/{triggerId}")]
    public HttpResponseMessage TouchedFilesPushTrigger(
        // triggerId is an opaque string.
        string triggerId,
        // A helper class provided by the AppService service SDK.
        // Here it defines the input of the push trigger is a string and the output to the callback is a FileInfoWrapper object.
        [FromBody]TriggerInput<string, FileInfoWrapper> triggerInput)
    {
        // Register the trigger to some trigger store.
        triggerStore.RegisterTrigger(triggerId, rootPath, triggerInput);

        // Extension method provided by the AppService service SDK indicating the registration is completed.
        return this.Request.PushTriggerRegistered(triggerInput.GetCallback());
    }

    // A simple in-memory trigger store.
    public class InMemoryTriggerStore
    {
        private static InMemoryTriggerStore instance;

        private IDictionary<string, FileSystemWatcher> _store;

        private InMemoryTriggerStore()
        {
            _store = new Dictionary<string, FileSystemWatcher>();
        }

        public static InMemoryTriggerStore Instance
        {
            get
            {
                if (instance == null)
                {
                    instance = new InMemoryTriggerStore();
                }
                return instance;
            }
        }

        // Register a push trigger.
        public void RegisterTrigger(string triggerId, string rootPath,
            TriggerInput<string, FileInfoWrapper> triggerInput)
        {
            // Use FileSystemWatcher to listen to file change event.
            var filter = string.IsNullOrEmpty(triggerInput.inputs) ? "*" : triggerInput.inputs;
            var watcher = new FileSystemWatcher(rootPath, filter);
            watcher.IncludeSubdirectories = true;
            watcher.EnableRaisingEvents = true;
            watcher.NotifyFilter = NotifyFilters.LastAccess;

            // When some file is changed, fire the push trigger.
            watcher.Changed +=
                (sender, e) => watcher_Changed(sender, e,
                    Runtime.FromAppSettings(),
                    triggerInput.GetCallback());

            // Assoicate the FileSystemWatcher object with the triggerId.
            _store[triggerId] = watcher;

        }

        // Fire the assoicated push trigger when some file is changed.
        void watcher_Changed(object sender, FileSystemEventArgs e,
            // AppService runtime object needed to invoke the callback.
            Runtime runtime,
            // The callback to invoke.
            ClientTriggerCallback<FileInfoWrapper> callback)
        {
            // Helper method provided by AppService service SDK to invoke a push trigger callback.
            callback.InvokeAsync(runtime, FileInfoWrapper.FromFileInfo(new FileInfo(e.FullPath)));
        }
    }

A szavazás eseményindító ellenőrzéséhez kövesse az alábbi lépéseket:

1. Telepítse az API-alkalmazást egy **nyilvános névtelen**hitelesítés beállítás.
2. Tallózással keresse meg a [http://requestb.in/](http://requestb.in/) hozhat létre egy RequestBin, amely a visszahívás URL-címe lesz.
3. Hívja fel a leküldéses eseményindító GUID **eseményindító azonosítója** , és a **callbackUrl**RequestBin URL-cím.
   ![Hívás leküldéses eseményindító Postman keresztül](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)
4. Hívja fel az **érintéses** művelet érintse meg egy fájlt. Az alábbi képen látható egy minta kérelmet Postman keresztül.
   ![Hívja fel a via Postman érintésvezérelt művelet](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
5. Jelölje be a RequestBin, hogy a leküldéses eseményindító visszahívás tulajdonság kimeneti a meghívott megerősítéséhez.
   ![Hívás a szavazás eseményindító Postman keresztül](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)

### <a name="describe-triggers-in-api-definition"></a>Eseményindítók API-definícióban leírása

Végrehajtási indítók, és az API-alkalmazás bevezetéshez Azure után az **API-definíciós** fel az Azure előzetes portálon nyissa meg, és láthatja, hogy indítók automatikusan felismeri felhasználói felület, amely jellemzi az API-alkalmazást a 2.0-s API Swagger meghatározása.

![API a definíció lap](./media/app-service-api-dotnet-triggers/apidefinitionblade.PNG)

Ha a **Swagger letöltése** gombra, és nyissa meg a JSON-fájlt, az alábbihoz hasonló eredmény jelenik meg:

    "/api/files/poll/TouchedFiles": {
      "get": {
        "operationId": "Files_TouchedFilesPollTrigger",
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    },
    "/api/files/push/TouchedFiles/{triggerId}": {
      "put": {
        "operationId": "Files_TouchedFilesPushTrigger",
        ...
        "x-ms-scheduler-trigger": "push"
      }
    }

A kiterjesztés tulajdonság **x-ms-schedular-indító** hogyan indítók az API-definíciós, és az automatikus hozza létre az API-alkalmazás átjáró keresztül az átjáró API meghatározása kérésekor, ha az értekezlet-összehívást az alábbi feltételek közül. (Is felveheti ezt a tulajdonságot manuális.)

- A szavazás eseményindító
    - Ha a HTTP-metódus **első**.
    - Ha a **operationId** tulajdonság tartalmazza a karakterlánc **eseményindító**.
    - Ha a **Paraméterek** tulajdonság paraméter egy **név** tulajdonság beállítása **triggerState**is tartalmaz.
- Leküldéses eseményindító
    - Ha a HTTP-metódus nem **HELYEZI el**.
    - Ha a **operationId** tulajdonság tartalmazza a karakterlánc **eseményindító**.
    - Ha a **Paraméterek** tulajdonság tartalmazza a paraméter egy **név** tulajdonság beállítása **eseményindító azonosítója**.

## <a name="use-api-app-triggers-in-logic-apps"></a>API-alkalmazás indítók logika alkalmazások használata

### <a name="list-and-configure-api-app-triggers-in-the-logic-apps-designer"></a>Listára, és az API-alkalmazás indítók konfigurálása a logika alkalmazások tervezőben

Ha a erőforrás csoporton belül az API-App hoz létre logika alkalmazást, lesz egyszerűen kattintva vegye fel a Lekérdezéstervező vászon. Az alábbi képek bemutatják, hogy ez:

![Eseményindítók logika alkalmazás tervezőben](./media/app-service-api-dotnet-triggers/triggersinlogicappdesigner.PNG)

![Logika alkalmazás Designer szavazás eseményindító beállítása](./media/app-service-api-dotnet-triggers/configurepolltriggerinlogicappdesigner.PNG)

![Logika alkalmazás Designer leküldéses eseményindító beállítása](./media/app-service-api-dotnet-triggers/configurepushtriggerinlogicappdesigner.PNG)

## <a name="optimize-api-app-triggers-for-logic-apps"></a>Alkalmazás indítók API-logika alkalmazások optimalizálása

Eseményindítók felveszi az API-alkalmazásokban, dolgot néhány megteheti, hogy tökéletesíthesse a működését, egy logikai alkalmazásban az API-alkalmazás használatakor.

Például a **triggerState** paraméter szavazás eseményindítók kell hozni az alábbi kifejezés a logika alkalmazásban. Ez a kifejezés kell a logika alkalmazásból az eseményindító a legutóbbi hívás értékelése, és lépjen vissza az adott értéket.  

    @coalesce(triggers()?.outputs?.body?['triggerState'], '')

Megjegyzés: A függvényt a kifejezésben használt szakaszok egyenként ismertetik, tekintse át a [Logika alkalmazás munkafolyamat Definition Language](https://msdn.microsoft.com/library/azure/dn948512.aspx)dokumentációja.

Logika app felhasználói kellene adja meg a fenti kifejezést **triggerState** paraméter az eseményindító használata közben. Akkor lehet, hogy az előre definiált kiterjesztés tulajdonság **x-ms-ütemezőt-ajánlási**keresztül logika alkalmazás készítője ezt az értéket.  Az **x-ms-láthatóság** kiterjesztés tulajdonság a *belső* értékre beállíthatók úgy, hogy a paraméter magát nem jelenik meg a tervező.  A következő kódtöredékének, amely mutatja be.

    "/api/Messages/poll": {
      "get": {
        "operationId": "Messages_NewMessageTrigger",
        "parameters": [
          {
            "name": "triggerState",
            "in": "query",
            "required": true,
            "x-ms-visibility": "internal",
            "x-ms-scheduler-recommendation": "@coalesce(triggers()?.outputs?.body?['triggerState'], '')",
            "type": "string"
          }
        ]
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    }

Leküldéses eseményindítók az **eseményindító azonosítója** paraméter egyedileg azonosítania kell a logika alkalmazást. A javasolt célszerű, beállítandó tulajdonság annak a munkafolyamatnak a nevére kattintva az alábbi kifejezés használatával:

    @workflow().name

Az **x-ms-ütemező-ajánlási** és **a láthatóság ms x** bővítmény tulajdonságai az API definiton használ, az API-alkalmazás is továbbítja a logika alkalmazás Tervező való automatikusan beállítja a felhasználó a kifejezésben.

        "parameters":[  
          {  
            "name":"triggerId",
            "in":"path",
            "required":true,
            "x-ms-visibility":"internal",
            "x-ms-scheduler-recommendation":"@workflow().name",
            "type":"string"
          },


### <a name="add-extension-properties-in-api-defintion"></a>Bővítmény tulajdonságai API attribútumdefiníciós hozzáadása

További metaadatok – például a bővítmény tulajdonságok **x-ms-ütemezőt-ajánlási** és **a láthatóság ms x** - lehet hozzáadni a API attribútumdefiníciós két módszer egyikével: statikus és dinamikus.

Statikus metaadatok közvetlenül is a */metadata/apiDefinition.swagger.json* fájl szerkesztése a projekt és manuálisan adja meg az a tulajdonságokat.

Az API-alkalmazások használata a dinamikus metaadatok módosíthatja a SwaggerConfig.cs fájlt egy művelet szűrő, amely is hozzáadhat, ezek a bővítmények hozzáadása.

    GlobalConfiguration.Configuration 
        .EnableSwagger(c =>
            {
                ...
                c.OperationFilter<TriggerStateFilter>();
                ...
            }


Az alábbi képen hogyan az osztály valósítható-e a dinamikus metaadatok forgatókönyv megkönnyítése érdekében.

    // Add extension properties on the triggerState parameter
    public class TriggerStateFilter : IOperationFilter
    {

        public void Apply(Operation operation, SchemaRegistry schemaRegistry, System.Web.Http.Description.ApiDescription apiDescription)
        {
            if (operation.operationId.IndexOf("Trigger", StringComparison.InvariantCultureIgnoreCase) >= 0)
            {
                // this is a possible trigger
                var triggerStateParam = operation.parameters.FirstOrDefault(x => x.name.Equals("triggerState"));
                if (triggerStateParam != null)
                {
                    if (triggerStateParam.vendorExtensions == null)
                    {
                        triggerStateParam.vendorExtensions = new Dictionary<string, object>();
                    }

                    // add 2 vendor extensions
                    // x-ms-visibility: set to 'internal' to signify this is an internal field
                    // x-ms-scheduler-recommendation: set to a value that logic app can use
                    triggerStateParam.vendorExtensions.Add("x-ms-visibility", "internal");
                    triggerStateParam.vendorExtensions.Add("x-ms-scheduler-recommendation",
                                                           "@coalesce(triggers()?.outputs?.body?['triggerState'], '')");
                }
            }
        }
    }
 
