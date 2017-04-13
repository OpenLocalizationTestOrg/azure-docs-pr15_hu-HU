<properties
   pageTitle="Logika alkalmazás eset: létrehozása az Azure függvények szolgáltatás Bus eseménykód |} Microsoft Azure"
   description="Hozzon létre egy logikai alkalmazás szolgáltatás Bus eseményindító Azure függvények használatával"
   services="logic-apps,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/23/2016"
   ms.author="jehollan"/>

# <a name="logic-app-scenario-create-an-azure-service-bus-trigger-by-using-azure-functions"></a>Logika alkalmazás eset: Azure Service Bus eseménykód létrehozása az Azure függvények használatával

Azure függvények használhatók bevezetését tervezi a hosszabb ideig futó figyelő vagy a tevékenység kellő összefüggés-alkalmazásokban a kiváltó ok mező létrehozásához. Ha például hozhat létre, amely várólista figyeljen és azonnal fire egy logikai alkalmazás leküldéses kiindulópontként függvény.

## <a name="build-the-logic-app"></a>A logikai alkalmazás összeállítása

Ebben a példában ha operációs rendszert futtató minden logika alkalmazás indítható el kell, hogy a függvény. Első lépésként, amelynek a HTTP-kérés eseménykód logika alkalmazás létrehozása. A függvény végpontra hív meg, amikor egy várólista üzenet jelenik meg.  

1. Hozzon létre egy új logika alkalmazás Jelölje be a **Kézi – Ha a HTTP kérjen érkezik** eseményindító.  
   Tetszés szerint megadhatja a használandó várólista üzenettel például [jsonschema.net](http://jsonschema.net)eszközzel JSON séma. Az eseményindító illessze be a sémában. Ez segít megérteni az alakzat, az adatok tervezője és flow egyszerűen tulajdonságok a munkafolyamaton keresztül.
1. Adja hozzá a kívánt várólista üzenet érkezik után lépnek fel további lépésekre. Ha például küldjön e-mailt az Office 365 keresztül.  
1. A visszahívási URL-CÍMÉT az eseményindító logika alkalmazás létrehozására összefüggés-alkalmazás mentése. A kiváltó ok mező kártya megjelenik az URL-címet.

![A visszahívási URL-cím mely részén jelenjen meg a kiváltó ok mező kártya][1]

## <a name="build-the-function"></a>A függvény felépítése

Ezután létrehozásához szükséges, amely használni kívánt az eseményindító és meghallgatásához a sor függvényt.

1. Az [Azure függvények portálon](https://functions.azure.com/signin)jelölje be az **Új függvénnyel**, és válassza ki a **ServiceBusQueueTrigger - C#** sablont.

    ![Azure függvények portál][2]

2. Állítsa be a kapcsolatot a szolgáltatás Bus várólista (fogja használni, amely az Azure Service Bus SDK `OnMessageReceive()` figyelő).
3. Írja be egy egyszerű függvény, hívja fel a logika alkalmazás végpont (korábban létrehozott) az eseményindító várólista üzenet használatával. Itt látható a teljes függvényt. A példában egy `application/json` webhelytartalom-típus, de is módosítsa, ha szükséges.

   ```
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;

   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";

   public static void Run(string myQueueItem, TraceWriter log)
   {

       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       using (var client = new HttpClient())
       {
           var response = client.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
       }
   }
   ```

Az ellenőrzéshez [Szolgáltatás Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer)hasonló eszköz használatával várólista üzenetet írhat. Lásd: a logika alkalmazás fire rögtön a függvény az üzenetet kap.

<!-- Image References -->
[1]: ./media/app-service-logic-scenario-function-sb-trigger/manualTrigger.PNG
[2]: ./media/app-service-logic-scenario-function-sb-trigger/newQueueTriggerFunction.PNG
