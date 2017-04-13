<properties 
    pageTitle="Hogyan naplózása a Azure esemény hubok Azure API-kezelés |} Microsoft Azure" 
    description="Megtudhatja, hogy miként naplózása a Azure esemény hubok Azure API-kezelés." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Hogyan naplózása a Azure esemény hubok Azure API kezelése

Azure esemény hubok erősen méretezhető adatok bejövő adatok szolgáltatás, amely is ingest másodpercenként események milliónyi, hogy a folyamat, és elemzése a nagy mennyiségű adatot a csatlakoztatott eszközök és alkalmazások által gyártott. Esemény hubok működik-e az "első ajtó" az egy esemény folyamat, és adatgyűjtés esemény-elosztóhoz, miután konvertálhatók, illetve tárolt bármely valós idejű analytics-szolgáltatója vagy a kötegelés/tároló kártyák segítségével. Esemény hubok különválasztja a felhasználás események, az események adatfolyam előállítására, így esemény fogyasztói is elérheti az események saját ütemezés.

Ez a cikk a kereső az [Azure API kezelés integráció esemény hubok a](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) videót, és leírja, hogy miként API-kezelés eseménynaplózás használatával Azure esemény hubok.

## <a name="create-an-azure-event-hub"></a>Hozzon létre egy Azure esemény hubhoz

Hozzon létre egy új esemény hubon keresztül csatlakozott, bejelentkezés az [Azure klasszikus portálra](https://manage.windowsazure.com) , és kattintson az **Új**->**Alkalmazás szolgáltatások**->**Szolgáltatás Bus**->**Esemény központi**->**Gyors létrehozásához**. Adja meg az esemény központi nevét, terület, jelöljön ki egy előfizetést, és jelöljön ki egy névteret. Ha még nem korábban létrehozott névteret létrehozhat egyet név beírása a **Namespace** mezőben lévő értéket. Miután az összes tulajdonság be van állítva, kattintson az esemény-központban **létrehozása egy új esemény-hubhoz** .

![Esemény központi létrehozása][create-event-hub]

Ezután lépjen a **beállítás** lapon az új esemény központi, és hozzon létre két **megosztott hozzáférési**. Nevezze el az első egy **küldése** , és tegyen **Küldés** engedélyt.

![Házirend küldése][sending-policy]

Név, a másodiké pedig **fogadása**, tegyen **meghallgatása** engedélyek, és kattintson a **Mentés**gombra.

![Házirend fogadása][receiving-policy]

Egyes átengedése a házirenddel küldeni és fogadni, mind pedig az esemény-központban események alkalmazások. A kapcsolat karakterláncok, ezek a házirendek az eléréséhez nyissa meg az esemény-központban **Irányítópult** lapja, és kattintson a **kapcsolat adatait**.

![Kapcsolati karakterlánc][event-hub-dashboard]

A **Sending** kapcsolati karakterlánc események bejelentkezéskor használja, és a **beérkező** kapcsolati karakterlánc böngészik események letöltése az esemény-központban.

![Kapcsolati karakterlánc][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a>Hozzon létre egy API-kezelés naplózó

Most, hogy van egy esemény hubhoz, következő lépésként az API alkalmazáskezelési szolgáltatás konfigurálása a [naplózó](https://msdn.microsoft.com/library/azure/mt592020.aspx) , hogy az események bejelentkezhetnek az esemény-központban.

API-kezelés naplózó programok állíthatók be az [API Management REST API -t](http://aka.ms/smapi). Előtt először a REST API-t használ, tekintse át a [vonatkozó követelmények](https://msdn.microsoft.com/library/azure/dn776326.aspx#Prerequisites) , és ellenőrizze, hogy [engedélyezve van a REST API -val való hozzáférést](https://msdn.microsoft.com/library/azure/dn776326.aspx#EnableRESTAPI).

Hozzon létre egy naplózó, HTTP ELHELYEZNI felkérés a következő URL-cím használata érdekében.

    https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview

-   Csere `{your service}` és az API Management szolgáltatás példány nevét.
-   Csere `{new logger name}` és az új naplózó a kívánt nevet. Ez a név fog hivatkozni, a [naplófájl-eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) házirend konfigurálásakor

A következő fejlécek hozzáadása a kérést.

-   -Tartalomtípust: alkalmazás/json
-   Engedélyezés: SharedAccessSignature uid =...
    -   Generálni útmutatást a `SharedAccessSignature` lásd: [Azure API kezelése REST API-hitelesítés](https://msdn.microsoft.com/library/azure/dn798668.aspx).

Adja meg a következő sablonnal összehívás törzsébe.

    {
      "type" : "AzureEventHub",
      "description" : "Sample logger description",
      "credentials" : {
        "name" : "Name of the Event Hub from the Azure Classic Portal",
        "connectionString" : "Endpoint=Event Hub Sender connection string"
        }
    }

-   `type`meg kell `AzureEventHub`.
-   `description`a naplózó szerint egy leírást tartalmaz, illetve nulla hosszúságú karakterláncok lehet, ha szükségesnek látja.
-   `credentials`tartalmazza a `name` és `connectionString` , a Azure esemény-központját.

Ha a naplózó készül állapotkódot mikor ellenőrizze a kérelmet, `201 Created` értéket adja vissza. 

>[AZURE.NOTE] Más lehetséges visszatérési kódok és azok okok című témakörben talál [egy naplózó létrehozása](https://msdn.microsoft.com/library/azure/mt592020.aspx#PUT). Hogy milyen műveleteket más például listák és frissítés, törlés, a dokumentációjában [naplózó](https://msdn.microsoft.com/library/azure/mt592020.aspx) entitás.

## <a name="configure-log-to-eventhubs-policies"></a>Log-eventhubs házirendek beállítása

Miután a naplózó API-kezelés van beállítva, beállíthatja a kívánt naplózása a napló-eventhubs házirendek. A napló-eventhubs házirend használható vagy a bejövő házirend szakaszban, vagy a kimenő házirend szakaszban.

Bejelentkezés az [Azure klasszikus portált](https://manage.windowsazure.com)a házirendek beállítása nyissa meg a API alkalmazáskezelési szolgáltatás, majd **a publisher-portálon** vagy a **kezelése** a publisher portál elérése.

![Publisher-portál][publisher-portal]

**Házirendek** kattintson a bal oldali menüjében API-kezelés, jelölje ki a kívánt termék és az API-val, és kattintson a **Hozzáadás házirend**. Ez a példa azt ad hozzá egy házirendet a **Visszhang API** a **korlátlan** termék.

![Házirend beállítása][add-policy]

Vigye a kurzort a a `inbound` házirend szakaszt, és jelölje ki az **EventHub napló** házirendet szeretne beszúrni a `log-to-eventhub` házirend kimutatás sablon.

![Csoportházirend-szerkesztőben][event-hub-policy]

    <log-to-eventhub logger-id ='logger-id'>
      @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
    </log-to-eventhub>

Csere `logger-id` az API-kezelés naplózó konfigurált az előző lépésben a nevet.

Bármely kifejezés, amely dátumaként karakterláncot ad vissza is használhatja a `log-to-eventhub` elemet. Ebben a példában be van jelentkezve a dátum és idő, szolgáltatás neve, kérelem azonosítója, kérelem IP-cím és művelet nevét tartalmazó karakterlánc.

Kattintson a **Mentés** mentheti a frissített házirendjének beállítása. Amint oda menti a program a házirend aktív, és eseményeket naplózza a kijelölt esemény hubon keresztül csatlakozott.

## <a name="next-steps"></a>Következő lépések

-   További tudnivalók az Azure esemény hubok
    -   [Azure esemény hubok – első lépések](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [EventProcessorHost üzenetek fogadása](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Esemény hubok programozási útmutató](../event-hubs/event-hubs-programming-guide.md)
-   További tudnivalók az API-kezelési és esemény hubok integrációja
    -   [Naplózó Egyed hivatkozása](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [log-eventhub házirend-hivatkozás](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    -   [Az API-khoz Azure API-kezelés, esemény hubok és Runscope figyelése](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a>Megtekintés az videó útmutató

> [AZURE.VIDEO integrate-azure-api-management-with-event-hubs]


[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png






