<properties 
    pageTitle="Lekérdezési hosszan futó műveleteket |} Microsoft Azure" 
    description="Ez a témakör bemutatja, hogyan lekérdezik hosszan futó műveleteket." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="delivering-live-streaming-with-azure-media-services"></a>Az Azure Media Services élő Streaming előadásához

##<a name="overview"></a>– Áttekintés

Microsoft Azure Media Services kínál kérést küld a Media Services műveletek indításához API-k (például: hozzon létre, leállítása vagy és a csatorna törlését). Ezek a műveletek hosszan futó.

A Media Services .NET SDK biztosít, amelyek a kérelem elküldése, és várja meg a művelet elvégzéséhez API-khoz (belső az API-khoz is lekérdezési művelet előrehaladásra néhány időközönként). Ha például csatorna hívásakor. Start(), a módszerrel a csatorna indítása után adja eredményül. Az aszinkron verziójával is használhatja: csatorna kerülve. StartAsync() (aszinkron mintát feladataival kapcsolatos tudnivalókért lásd: [KOPPINTSON a](https://msdn.microsoft.com/library/hh873175(v=vs.110).aspx)). API-k művelet felkérés küldése, és majd lekérdezik az állapotát, a művelet befejeződéséig "lekérdezési módszerek" nevezik. Ezeket a módszereket (különösen az aszinkron verzióval) az ügyfélprogramok alkalmazások, illetve az állapot-nyilvántartó szolgáltatások használata ajánlott.

Vannak olyan esetek, ahol az alkalmazás nem megvárja, amíg egy ideig futó HTTP-kérést, és lekérdezik az művelet végrehajtását kézzel szeretne. Egy tipikus példa állapot nélküli webszolgáltatás használata böngészőben a következő lesz:, amikor a böngésző kéri a csatorna létrehozásához, a webszolgáltatás kezdeményez egy ideig futó művelet, és a böngészőben a művelet Azonosítóját adja vissza. A böngésző sikerült majd kérje meg a webszolgáltatás művelet állapotának az azonosító alapján való A Media Services .NET SDK API-hoz, amelyek hasznos, ha ebben az esetben tartalmaz. Ezek az API-khoz "nem lekérdezési módszerek" nevezik.
A "nem lekérdezési módszerek" rendelkezik az alábbi elnevezési minta: küldése*OperationName*művelet (például SendCreateOperation). Küldés*OperationName*művelet módszerek vissza az **IOperation** objektumot. a visszaadott objektum információkat tartalmaz, a művelet nyomon követésére használható. A Küldés*OperationName*OperationAsync módszerek visszatérési **tevékenység<IOperation>**.

Jelenleg a következő osztályok támogatja a nem lekérdezési módszerek: **csatornát**, a **StreamingEndpoint**és a **Program**.

Lekérdezi a művelet állapot, használja a **GetOperation** módszert a **OperationBaseCollection** osztály. A következő intervallumok használata a művelet állapotának ellenőrzése: **csatorna** és **StreamingEndpoint** műveletekhez, használja a 30 másodperc; a **Program** műveletek 10 másodperc használja.


##<a name="example"></a>Példa

A következő példa **ChannelOperations**nevű osztály határozza meg. A osztály definition web service osztály definíciójának kiindulási pont lehet. Az alábbi példák egyszerűség kedvéért módszerek nem aszinkron változatának használja.

A példában is látható, az ügyfél miként használhat az osztály.

###<a name="channeloperations-class-definition"></a>ChannelOperations osztály meghatározása

    /// <summary> 
    /// The ChannelOperations class only implements 
    /// the Channel’s creation operation. 
    /// </summary> 
    public class ChannelOperations
    {
        // Read values from the App.config file.
        private static readonly string _mediaServicesAccountName =
            ConfigurationManager.AppSettings["MediaServicesAccountName"];
        private static readonly string _mediaServicesAccountKey =
            ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
        // Field for service context.
        private static CloudMediaContext _context = null;
        private static MediaServicesCredentials _cachedCredentials = null;
    
        public ChannelOperations()
        {
                _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
                _context = new CloudMediaContext(_cachedCredentials);    }
    
        /// <summary>  
        /// Initiates the creation of a new channel.  
        /// </summary>  
        /// <param name="channelName">Name to be given to the new channel</param>  
        /// <returns>  
        /// Operation Id for the long running operation being executed by Media Services. 
        /// Use this operation Id to poll for the channel creation status. 
        /// </returns> 
        public string StartChannelCreation(string channelName)
        {
            var operation = _context.Channels.SendCreateOperation(
                new ChannelCreationOptions
                {
                    Name = channelName,
                    Input = CreateChannelInput(),
                    Preview = CreateChannelPreview(),
                    Output = CreateChannelOutput()
                });
    
            return operation.Id;
        }
    
        /// <summary> 
        /// Checks if the operation has been completed. 
        /// If the operation succeeded, the created channel Id is returned in the out parameter.
        /// </summary> 
        /// <param name="operationId">The operation Id.</param> 
        /// <param name="channel">
        /// If the operation succeeded, 
        /// the created channel Id is returned in the out parameter.</param>
        /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
        public bool IsCompleted(string operationId, out string channelId)
        {
            IOperation operation = _context.Operations.GetOperation(operationId);
            bool completed = false;
    
            channelId = null;
    
            switch (operation.State)
            {
                case OperationState.Failed:
                    // Handle the failure. 
                    // For example, throw an exception. 
                    // Use the following information in the exception: operationId, operation.ErrorMessage.
                    break;
                case OperationState.Succeeded:
                    completed = true;
                    channelId = operation.TargetEntityId;
                    break;
                case OperationState.InProgress:
                    completed = false;
                    break;
            }
            return completed;
        }
    
    
        private static ChannelInput CreateChannelInput()
        {
            return new ChannelInput
            {
                StreamingProtocol = StreamingProtocol.RTMP,
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                    {
                        new IPRange
                        {
                            Name = "TestChannelInput001",
                            Address = IPAddress.Parse("0.0.0.0"),
                            SubnetPrefixLength = 0
                        }
                    }
                }
            };
        }
    
        private static ChannelPreview CreateChannelPreview()
        {
            return new ChannelPreview
            {
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                    {
                        new IPRange
                        {
                            Name = "TestChannelPreview001",
                            Address = IPAddress.Parse("0.0.0.0"),
                            SubnetPrefixLength = 0
                        }
                    }
                }
            };
        }
    
        private static ChannelOutput CreateChannelOutput()
        {
            return new ChannelOutput
            {
                Hls = new ChannelOutputHls { FragmentsPerSegment = 1 }
            };
        }
    }

###<a name="the-client-code"></a>Az ügyfél kódot.

    ChannelOperations channelOperations = new ChannelOperations();
    string opId = channelOperations.StartChannelCreation("MyChannel001");
    
    string channelId = null;
    bool isCompleted = false;
    
    while (isCompleted == false)
    {
        System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
        isCompleted = channelOperations.IsCompleted(opId, out channelId);
    }
    
    // If we got here, we should have the newly created channel id.
    Console.WriteLine(channelId);
 


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
