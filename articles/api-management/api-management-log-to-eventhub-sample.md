<properties
    pageTitle="Az API-khoz Azure API-kezelés, esemény hubok és Runscope figyelése"
    description="A napló-eventhub házirend összekötő Azure API-kezelés, Azure esemény hubok és HTTP naplózáshoz és a figyeléshez Runscope igazoló minta alkalmazás"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="darrmi"/>

# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a>Az API-khoz Azure API-kezelés, esemény hubok és Runscope figyelése

Az [API alkalmazáskezelési szolgáltatás](api-management-key-concepts.md) tökéletesíthetik HTTP-összehívások a HTTP API a feldolgozása sok szolgáltatásokat nyújt. Azonban a kérések és válaszok megléte olyan ideiglenes (tranziens). A kérés és folyik át a kódmentes API az API Management szolgáltatás keresztül. Az API végrehajtja a kérést, és az API-felhasználónak belül hátra átfolyása választ. Az API alkalmazáskezelési szolgáltatás továbbra is megjeleníthető az API-k néhány fontos statisztikájának a Publisher portál irányítópulton, de túl, hogy nincsenek-e benne a részletek.

A [napló-eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [házirend](api-management-howto-policies.md) az API alkalmazáskezelési szolgáltatás használatával elküldheti a kérést, és a válasz [Azure esemény központi](../event-hubs/event-hubs-what-is-event-hubs.md)olyan elérhetőségi adatait. Vannak olyan számos oka, miért célszerű az API-khoz küldi el a HTTP-üzenetek eseményeket hoznak létre. Könyvvizsgálati frissítések, a használati analitikát, a kivétel riasztási és a 3 fél integrációs, többek között.   

Ez a cikk bemutatja, hogyan rögzítheti a teljes HTTP kérelem és válasz üzenet, küldje el az esemény hubon keresztül csatlakozott, és kattintson a harmadik fél szolgáltatás, ahol a naplózáshoz és a figyeléshez szolgáltatások HTTP az üzenet közvetítése.

## <a name="why-send-from-api-management-service"></a>Miért küldhessenek API alkalmazáskezelési szolgáltatás?
Érdemes lehet, hogy HTTP API keretek rögzítheti a HTTP-összehívások és válaszok, és a hírcsatorna őket naplózáshoz és a figyeléshez rendszerek csatlakoztatható HTTP köztes írni. Mi a hátrányuk, ezt a megközelítést pedig a HTTP köztes kell a háttér API-kiszolgálói lesznek integrálva meg kell egyeznie a platform, a API. Ha több API-khoz mindegyik a köztes kell telepítenie. Gyakran okból miért nem lehet frissíteni kódmentes API-khoz.

Naplózás infrastruktúra integrálása az Azure API-kezelés szolgáltatással központi és a platform beállítástól független megoldást is kínál. Célszerű is méretezhető, részben Azure API-kezelés az [geo replikációs](api-management-howto-deploy-multi-region.md) funkciók miatt.

## <a name="why-send-to-an-azure-event-hub"></a>Miért Azure esemény hubhoz küldeni?
Indokolt egy kérdezze meg, miért hozzon létre egy házirendet, amely az Azure esemény hubok? Vannak olyan hol szeretném előfordulhat, hogy jelentkezzen be a kérések számos más pontjaira. Miért nem csak a kérelem elküldése közvetlenül a záró cél?  Ez egy lehetőséget. Azonban egy API alkalmazáskezelési szolgáltatás naplózás kérései készítésekor szükség fontolja meg, milyen hatással van az üzenetek naplózásához lesz az API teljesítményét. A betöltés fokozatos nő rendszer összetevőit elérhető példányai növelésével, illetve a replikáció geo nyújtotta előnyök kihasználása is kezelhető. Azonban a forgalmat a rövid kiugrásainak megfelelő jelentősen késik, ha naplózási infrastruktúra kérések terhelés alatt lassú kérések okozhatnak.

Azure esemény hubokon bejövő adatok nagyon nagy mennyiségű adat, az események sokkal nagyobb számú kezelése, mint a HTTP-kérések száma a legtöbb API-khoz folyamat kapacitással szolgál. Az esemény-központban működik-e egy összetett puffer a API alkalmazáskezelési szolgáltatás és az infrastruktúra, amely tárolása és feldolgozása az üzenetek között típusú. Ezzel biztosíthatja, hogy az API-teljesítmény nem várható a naplózás infrastruktúra miatt.  

Ha az adatok megőrződnek, és a feldolgozási esemény központi fogyasztói megvárja esemény hubhoz továbbították. Az esemény-központban nem bánja, hogyan dolgozza, akkor csak ügyel gondoskodhat arról, hogy az üzenetek sikeresen kézbesítve.     

Esemény hubok több fogyasztói csoportok eseményeket adatfolyam lehetősége van. Ezzel az teljesen más rendszerek feldolgoztatni események. Ez lehetővé teszi, hogy sok integrációs forgatókönyvek támogatása nem helyezi összeadás késések belül az API alkalmazáskezelési szolgáltatás API-összehívás feldolgozása, csak egy esemény kell jön létre.

## <a name="a-policy-to-send-applicationhttp-messages"></a>Egy házirendet, amely alkalmazás/HTTP-üzenetek küldésére
Egy esemény hubhoz eseményadatok egyszerű karakterláncként fogadja el. A karakterlánc tartalma teljesen, felfelé. Engedélyezni szeretné a HTTP-kérelem felfelé csomag, és küldje esemény hubok szükséges formázandó kérések és válaszok információkkal a karakterlánc. Azokról a helyzetekről jelennek meg ha a meglévő formátum felhasználhatja azt, majd előfordulhat, hogy nincs írni a saját kód elemzése. Az eredetileg lehet figyelembe venni, HTTP-összehívások és válaszok küldése a [HAR](http://www.softwareishard.com/blog/har-12-spec/) verzióval. Ez a formátum azonban a HTTP-kérések sorozata tárolásához alapú JSON formátumban van optimalizálva. Szám kötelező elemek, amelyek az alkalmazási példát, a HTTP-üzenet továbbítása a hálózaton a szükségtelen összetettsége hozzá tartalmazott.  

Egy alternatív lehetőséget volt, használja a `application/http` médiatípus a HTTP-specifikáció [RFC 7230](http://tools.ietf.org/html/rfc7230)leírt módon. A médiatípus használja pontosan ugyanazt a formátumot, amely ténylegesen a hálózaton a HTTP-üzenetek küldésére, de az üzenetnek lehet helyezni egy másik HTTP-összehívás törzsében. Abban az esetben csak fogjuk használni a szervezet az üzenetet szeretne küldeni az esemény hubok. Kényelmesen, akkor egy adott megtalálható a [Microsoft ASP.NET webes API 2.2 ügyfél](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) tárakat, amelyek képesek elemezni, ez a formátum és a natív alakíthatja elemző `HttpRequestMessage` és `HttpResponseMessage` objektumok.

Engedélyezni kell a kihasználhatja a C#, ez az üzenet létrehozása Azure API adatkezelési [házirend kifejezések](https://msdn.microsoft.com/library/azure/dn910913.aspx) alapján. Az alábbiakban a szabályt, amely a HTTP-összehívási üzenetben küld az Azure esemény hubok.

       <log-to-eventhub logger-id="conferencelogger" partition-id="0">
       @{
           var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                       context.Request.Method,
                                                       context.Request.Url.Path + context.Request.Url.QueryString);

           var body = context.Request.Body?.As<string>(true);
           if (body != null && body.Length > 1024)
           {
               body = body.Substring(0, 1024);
           }

           var headers = context.Request.Headers
                                  .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                                  .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                  .ToArray<string>();

           var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

           return "request:"   + context.Variables["message-id"] + "\n"
                               + requestLine + headerString + "\r\n" + body;
       }
       </log-to-eventhub>

### <a name="policy-declaration"></a>Házirend deklaráció
Létezik néhány adott dolog, amit érdemes megemlítése kapcsolatos házirend kifejezés. A napló-eventhub házirend nevére az API alkalmazáskezelési szolgáltatás belül létrehozott naplózó hivatkozik, amely naplózó azonosító nevű attribútum tartalmaz. Egy esemény központi naplózó az API Management szolgáltatás beállítása részleteit a dokumentumban, [hogyan eseménynaplózás Azure esemény hubok Azure API kezelése lapon](api-management-howto-log-event-hubs.md)találhatók. A második attribútum választható paraméter, amely arra utasítja az esemény hubok partition tárolja az üzenet, amelynek. Esemény hubok scalabilty engedélyezése és legalább két kötelezővé tétele partíciót használatával. Üzenetek kézbesítésének rendezett csak a partíciók belül garantált. Ha nem arra utasítja esemény-központját melyik partíciót szeretne helyezni az üzenet, hogy a betöltés terjesztése azt fogja használni egy ciklikus algoritmus. Azonban, hogy jelenhet meg a megfelelő sorrendben feldolgoztatni üzenetek egy része.  

### <a name="partitions"></a>Partíciót
Az üzenetek érkeznek fogyasztói sorrendben, és kihasználhatja a betöltés terjesztési videofunkcióinak partíciót érdekében kiválasztott HTTP kérelem üzeneteket küldeni egy partíciót és a HTTP válaszüzenetek második partíciók. Ezzel biztosíthatja az páros terheléselosztási, és azt is, hogy az összes kérelem fogyni fog sorrendben, és az összes válasz fogyni fog sorrendben biztosítása. Lehetséges, hogy a megfelelő kérelem előtt felhasználandó választ, de ez nem probléma csak akkor van egy másik mechanizmusa használatával történik a kéri, hogy a válaszok és tudjuk, hogy kérések mindig azt megelőző válaszokat.

### <a name="http-payloads"></a>HTTP-tartalmak számára
Létrehozása után a `requestLine` akkor ellenőrizze, hogy ha összehívás törzsébe csonkoltak lehetnek. A program csak 1024 csak összehívás törzsébe. Ez lehet növelni kell, de egyes esemény központi üzeneteket legfeljebb 256KB, így valószínű, hogy bizonyos HTTP üzenet szervezetek nem férnek el a egyetlen üzenetre fog. Naplózás és az elemzést során információk jelentős mennyiségű származhat mindössze a HTTP kérelem sor és a fejlécek. Is, sok API kérelem csak ad vissza kis szerveknek, és így információk értéket nagy méretű szervezetek csonkítása funkcióvesztést meglehetősen minimális az átadás, feldolgozása és tárolási költségek tartani a teljes szervezet tartalom csökkenése összehasonlítva. A szervezet feldolgozási a végleges OneNote, hogy szükség adják át az `true` az as<string>() módszer, mert azt olvasása a szervezet tartalmát, de lett is a kódmentes API segítségével tudja felolvasni a szervezet kívánt. Ez a módszer igaz átadása által azt okozó a szervezetet, hogy meg tudja olvasni másodszori pufferelni. Ez fontos tudatában kell lennie az API, amely rendelkezik, a nagyméretű fájlok feltöltése, vagy használja a hosszú lekérdezési esetén. Ebben az esetben a legjobb, ha a szervezet minden olvasási elkerülése lenne.   

### <a name="http-headers"></a>HTTP-fejlécek
HTTP-fejlécek egyszerűen átirányítható fölé be az üzenet formátuma egyszerű kulcs/érték pár formátumban. Azt választotta ki a hitelesítő adatok szükségtelenül httpserversockethandlers elkerülése érdekében bizonyos biztonsági bizalmas mezők. Valószínű, hogy API kulcsok és egyéb hitelesítő adatok volna használható analytics céljából. Ha azt szeretné, hogy a felhasználó és az adott termék elemzéseket végezni tulajdonosa milyen alkalmazásokat használ, akkor azt olvasható, hogy az a `context` objektum, és vegye fel, amelyek az üzenetbe.     
### <a name="message-metadata"></a>Üzenet-metaadat
A teljes üzenetet küldeni az esemény-központban készítésekor az első sor nincs valójában egy részét a `application/http` üzenetet. Az első sor további metaadatokat tartalmazó, hogy az üzenet egy kérelmet vagy válaszüzenet és egy üzenetazonosító válaszokkal kéréseket összehangolására használt. Az üzenetazonosító, amely a következőképpen néz ki egy másik szabály használatával hozzák létre:

    <set-variable name="message-id" value="@(Guid.NewGuid())" />

Azt is létrehozott a összehívási üzenetben, típusú változóban tárolja, amelyek, amíg a választ adja vissza, egyszerűen csak elküldi a kérelem és a válasz egyetlen üzenetként volt. Azonban a kérelem és a válasz küldése független, és egy üzenetazonosító a két összehangolására, azt megnyithatja egy kicsit nagyobb rugalmasság érdekében az üzenet mérete, az azt jelenti, hogy kihasználhatja a több partíciók, miközben üzenet sorrend és a kérelem fenntartása hamarabb fog megjelenni a naplózás irányítópulton. Is lehetnek bizonyos esetekben, ahol érvényes választ el nem küldött az esemény-hubon keresztül csatlakozott, esetleg az API Management szolgáltatás kritikus kérelem hiba miatt, de azt is lesz egy rekordot a kérést.

A házirendet a HTTP válaszüzenetet küldeni formátumban nagyon hasonlít a kérést, és így a teljes házirendjének beállítása így néz ki:

      <policies>
        <inbound>
            <set-variable name="message-id" value="@(Guid.NewGuid())" />
            <log-to-eventhub logger-id="conferencelogger" partition-id="0">
              @{
                  var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                              context.Request.Method,
                                                              context.Request.Url.Path + context.Request.Url.QueryString);

                  var body = context.Request.Body?.As<string>(true);
                  if (body != null && body.Length > 1024)
                  {
                      body = body.Substring(0, 1024);
                  }

                  var headers = context.Request.Headers
                                       .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                                       .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                       .ToArray<string>();

                  var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

                  return "request:"   + context.Variables["message-id"] + "\n"
                                      + requestLine + headerString + "\r\n" + body;
              }
          </log-to-eventhub>
        </inbound>
        <backend>
            <forward-request follow-redirects="true" />
        </backend>
        <outbound>
            <log-to-eventhub logger-id="conferencelogger" partition-id="1">
              @{
                  var statusLine = string.Format("HTTP/1.1 {0} {1}\r\n",
                                                      context.Response.StatusCode,
                                                      context.Response.StatusReason);

                  var body = context.Response.Body?.As<string>(true);
                  if (body != null && body.Length > 1024)
                  {
                      body = body.Substring(0, 1024);
                  }

                  var headers = context.Response.Headers
                                                  .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                                  .ToArray<string>();

                  var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

                  return "response:"  + context.Variables["message-id"] + "\n"
                                      + statusLine + headerString + "\r\n" + body;
             }
          </log-to-eventhub>
        </outbound>
      </policies>

A `set-variable` házirend létrehoz egy érték, amely szerint egyaránt elérhető a `log-to-eventhub` házirendet a `<inbound>` szakasz és a `<outbound>` szakaszban.  

## <a name="receiving-events-from-event-hubs"></a>Az esemény hubok fogadó események
Azure esemény központból események fogadásának [AMQP protokoll](http://www.amqp.org/)használatával. A Microsoft-szolgáltatás Bus híreit saját ügyfél tárak érhető el, így könnyebben igénybe eseményeket. Kétféleképpen különböző támogatott, egy folyamatban van a *Közvetlen fogyasztói* , és használja a másik a `EventProcessorHost` osztály. Példák a következő két megközelítés az [Esemény hubok programozási útmutató](../event-hubs/event-hubs-programming-guide.md)található. A különbségeket rövid verziója, `Direct Consumer` teljes szabályozható, és a `EventProcessorHost` tartalmaz néhány vízvezetékek munka, de bizonyos feltételezésekkel kapcsolatban, hogyan dolgozza az események számára.  

### <a name="eventprocessorhost"></a>EventProcessorHost
Ez a példa a fogjuk használni a `EventProcessorHost` az egyszerűség kedvéért jó helyen jár, előfordulhat, hogy ebben az esetben nem a legjobb választás. `EventProcessorHost`gondoskodhat arról, hogy nem kell aggódnia amiatt, hogy egy adott esemény processzor osztály belüli problémák threading rengeteg munkája nem. Azonban az esetben azt egyszerűen az üzenet konvertálása más formátumban, és beengedné mentén aszinkron módszerrel másik szolgáltatásra. A nem megosztott állapotát és így kockázata problémák threading frissítése nincs szükség. Ha a legtöbb esetben `EventProcessorHost` valószínűleg a legjobb választás és bizonyára az egyszerűbb lehetőség.     

### <a name="ieventprocessor"></a>IEventProcessor
A központi fogalma használata esetén `EventProcessorHost` hoz létre egy megvalósítása a `IEventProcessor` felület, mely tartalmazza a módszerrel `ProcessEventAsync`. Ez a módszer lényege Itt jelennek meg:

  aszinkron tevékenység IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages) {

           foreach (EventData eventData in messages)
           {
               _Logger.LogInfo(string.Format("Event received from partition: {0} - {1}", context.Lease.PartitionId,eventData.PartitionKey));

               try
               {
                   var httpMessage = HttpMessage.Parse(eventData.GetBodyStream());
                   await _MessageContentProcessor.ProcessHttpMessage(httpMessage);
               }
               catch (Exception ex)
               {
                   _Logger.LogError(ex.Message);
               }
           }
            ... checkpointing code snipped ...
        }

Módszer van átadott EventData objektumok listáját, és azt a listát ismétlés. Mindegyik módszernek az bájt elemzésének HttpMessage objektummá, és az objektum átadott IHttpMessageProcessor egy példánya.

### <a name="httpmessage"></a>HttpMessage
A `HttpMessage` példány három csatolt fájllal tartalmaz:

      public class HttpMessage
       {
           public Guid MessageId { get; set; }
           public bool IsRequest { get; set; }
           public HttpRequestMessage HttpRequestMessage { get; set; }
           public HttpResponseMessage HttpResponseMessage { get; set; }

        ... parsing code snipped ...

      }

A `HttpMessage` példány tartalmaz egy `MessageId` globálisan egyedi azonosítója, amellyel velünk a HTTP-kérés csatlakozni a megfelelő HTTP-válasz és egy logikai érték, amely azonosítja, ha az objektum tartalmaz egy HttpRequestMessage és HttpResponseMessage egy példánya. A HTTP órákra a beépített használatával `System.Net.Http`, e tudott kihasználhatja a `application/http` elemzése a kód `System.Net.Http.Formatting`.  

### <a name="ihttpmessageprocessor"></a>IHttpMessageProcessor
A `HttpMessage` példány továbbítja végrehajtásának `IHttpMessageProcessor` melyiket érdemes lehet hoz létre a receiving és Azure esemény központból az esemény értelmezését és a tényleges feldolgozása decouple felületet.


## <a name="forwarding-the-http-message"></a>A HTTP üzenet továbbítása
Az ebben a példában lehet úgy döntött, a HTTP-kérés leküldéses fölé való [Runscope](http://www.runscope.com)segítségével lenne. Runscope egy felhőalapú szolgáltatás, amely HTTP hibakeresése során, a naplózáshoz és a figyeléshez. Egy ingyenes réteg rendelkeznek, egyszerűen próbálja meg, és lehetővé teszi, hogy nekünk az API-kezelés szolgáltatáson keresztül valós idejű lépés a HTTP-összehívások megtekintéséhez.

A `IHttpMessageProcessor` végrehajtása így néz ki,

      public class RunscopeHttpMessageProcessor : IHttpMessageProcessor
       {
           private HttpClient _HttpClient;
           private ILogger _Logger;
           private string _BucketKey;
           public RunscopeHttpMessageProcessor(HttpClient httpClient, ILogger logger)
           {
               _HttpClient = httpClient;
               var key = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-KEY", EnvironmentVariableTarget.User);
               _HttpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", key);
               _HttpClient.BaseAddress = new Uri("https://api.runscope.com");
               _BucketKey = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-BUCKET", EnvironmentVariableTarget.User);
               _Logger = logger;
           }

           public async Task ProcessHttpMessage(HttpMessage message)
           {
               var runscopeMessage = new RunscopeMessage()
               {
                   UniqueIdentifier = message.MessageId
               };

               if (message.IsRequest)
               {
                   _Logger.LogInfo("Sending HTTP request " + message.MessageId.ToString());
                   runscopeMessage.Request = await RunscopeRequest.CreateFromAsync(message.HttpRequestMessage);
               }
               else
               {
                   _Logger.LogInfo("Sending HTTP response " + message.MessageId.ToString());
                   runscopeMessage.Response = await RunscopeResponse.CreateFromAsync(message.HttpResponseMessage);
               }

               var messagesLink = new MessagesLink() { Method = HttpMethod.Post };
               messagesLink.BucketKey = _BucketKey;
               messagesLink.RunscopeMessage = runscopeMessage;
               var runscopeResponse = await _HttpClient.SendAsync(messagesLink.CreateRequest());
               _Logger.LogDebug("Request sent to Runscope");
           }
       }

Lehet egy [meglévő ügyfél dokumentumtár a Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) megkönnyíti leküldéses hasznát lett `HttpRequestMessage` és `HttpResponseMessage` feljebb helyezés a példányok. Ahhoz, hogy hozzáférjen a Runscope API szüksége lesz a fiók és az API-ja kulcs. Az API-ja kulcs utasításokat az [Alkalmazások létrehozása az Access Runscope API-nak](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) képernyőfelvétel találhatók.

## <a name="complete-sample"></a>Kész példa
A vizsgálatok [Forráskód](https://github.com/darrelmiller/ApimEventProcessor) és a minta Github találhatók. Szüksége lesz az [API alkalmazáskezelési szolgáltatás](api-management-get-started.md) [Egy csatlakoztatott esemény hubhoz](api-management-howto-log-event-hubs.md)és a [Tárterület-fiók](../storage/storage-create-storage-account.md) magának a minta futtatásához.   

A minta egy csak egy egyszerű konzol alkalmazás, amely figyeli az események várhatók esemény központból alakítja át őket egy `HttpRequestMessage` és `HttpResponseMessage` objektumokat, és ezután megjelenítheti a Runscope API továbbítja.

Az alábbi animált képen megtekintheti történik az API-ja a Developer Portal segítségével, a megjelenítő az üzenetet megkapta, feldolgozott és továbbított New, majd a kérelem és válasz jelennek meg a forgalom Runscope a felügyelő a kérelmet.

![A bemutató Runscope továbbítsák kérés](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a>Összefoglalás
Azure API alkalmazáskezelési szolgáltatás, amellyel rögzítheti a HTTP-forgalmat a API-khoz az azonosítók között zajló egy ideális helyet biztosít a. Azure esemény hubok erősen méretezhető, alacsony költség megoldást a forgalom rögzítéséhez és töltését mutató azt be a naplózás másodlagos feldolgozás rendszerek, figyelemmel kísérésére és más kifinomult analytics. Csatlakozás rendszerek néhány dozen kódsorokat Runscope egy egyszerű, mint figyelése 3 fél forgalmat.

## <a name="next-steps"></a>Következő lépések
-   További tudnivalók az Azure esemény hubok
    -   [Azure esemény hubok – első lépések](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [EventProcessorHost üzenetek fogadása](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Esemény hubok programozási útmutató](../event-hubs/event-hubs-programming-guide.md)
-   További tudnivalók az API-kezelési és esemény hubok integrációja
    -   [Hogyan naplózása a Azure esemény hubok Azure API kezelése](api-management-howto-log-event-hubs.md)
    -   [Naplózó Egyed hivatkozása](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [log-eventhub házirend-hivatkozás](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    