<properties
    pageTitle="Azure függvények Twilio kötés |} Microsoft Azure"
    description="Megtudhatja, hogyan Azure függvényekkel Twilio kötések használható."
    services="functions"
    documentationCenter="na"
    authors="wesmc7777"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure funkciók esemény feldolgozása, dinamikus számítási, kiszolgáló nélküli architektúráját, függvények"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="10/20/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-twilio-output-binding"></a>Azure függvények Twilio kimeneti kötése

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Ez a cikk ismerteti, hogyan beállítása és használata Twilio kötések Azure funkciókkal. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Azure funkciókat támogatja a kimeneti ahhoz, hogy a kód és a [Twilio](https://www.twilio.com/) fiók néhány vonalakkal együtt az SMS-szöveges üzenetek küldésére függvények kötések Twilio. 
 

## <a name="functionjson-for-azure-notification-hub-output-binding"></a>az Azure értesítés központi Function.JSON kimeneti kötése

A function.json fájlt tartalmazza a következő tulajdonságokat:

- `name`: Változó nevét a kód függvény az Twilio SMS szöveges üzenet.
- `type`: *"twilioSms"*értékre kell állítani.
- `accountSid`: Ez az érték meg kell egy App beállítása a Twilio fiók biztonsági azonosítók betöltő nevét.
- `authToken`: Ez az érték meg kell egy App beállítása a Twilio hitelesítési jogkivonat betöltő nevét.
- `to`: Ez az érték a telefonszámot, amelyen az SMS szöveg küldött értéke.
- `from`: Ez az érték a telefonszámot, amelyen az SMS szöveg küldünk értéke.
- `direction`: *","*értékre kell állítani.
- `body`: Ez az érték használható az SMS-szöveges üzenet végleges kód, ha nem kell dinamikusan állítsa be a kódot a függvény. 

 
Példa function.json:

    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "+1704XXXXXXX",
      "from": "+1425XXXXXXX",
      "direction": "out",
      "body": "Azure Functions Testing"
    }


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a>Példa a C# várólista eseményindító Twilio a kimeneti kötése

#### <a name="synchronous"></a>Szinkronizált

Az Azure tároló várólista eseménykód szinkron példa kód egy kimenő paramétert szöveges üzenet küldése egy ügyfél, aki a megrendelés használja.

    #r "Newtonsoft.Json"
    #r "Twilio.Api"

    using System;
    using Newtonsoft.Json;
    using Twilio;

    public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        message = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        message.Body = msg;
        message.To = order.mobileNumber;
    }

#### <a name="asynchronous"></a>Aszinkron

Az Azure tárolás várólista eseménykód aszinkron példa kód szöveges üzenetet küld egy ügyfél, aki a megrendelés.

    #r "Newtonsoft.Json"
    #r "Twilio.Api"
     
    using System;
    using Newtonsoft.Json;
    using Twilio;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");

        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        SMSMessage smsText = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        smsText.Body = msg;
        smsText.To = order.mobileNumber;
        
        await message.AddAsync(smsText);
    }


## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a>Példa Node.js várólista eseményindító Twilio a kimeneti kötése

Ez a példa Node.js szöveges üzenetet küld egy ügyfél, aki a megrendelés.

    module.exports = function (context, myQueueItem) {
        context.log('Node.js queue trigger function processed work item', myQueueItem);
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        var msg = "Hello " + myQueueItem.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the message binding.
        context.bindings.message = {};
    
        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        context.bindings.message = {
            body : msg,
            to : myQueueItem.mobileNumber
        };
    
        context.done();
    };

## <a name="next-steps"></a>Következő lépések

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
