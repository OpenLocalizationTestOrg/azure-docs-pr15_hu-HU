<properties
    pageTitle="C# Ismerkedés az Azure IoT központi |} Microsoft Azure"
    description="Azure IoT központi és C# intézményi oktatóprogram elindítása. Használjon Azure IoT központi és C# a Microsoft Azure IoT SDK az Internet a dolog, amit megoldás végrehajtásához."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-net"></a>Azure IoT központi .NET használatának első lépései

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Ez az oktatóanyag végén három Windows konzol alkalmazások vannak telepítve:

* **CreateDeviceIdentity**, amely egy eszköz identitás- és a szimulált eszköze társított biztonsági kulcs hoz létre.
* **ReadDeviceToCloudMessages**, amely a szimulált eszköze által küldött telemetriai jeleníti meg.
* **SimulatedDevice**, amely a korábban létrehozott eszköz identitású a IoT-hubhoz csatlakozik, és minden második telemetriai üzenetet küld a AMQP protokoll használatával.

> [AZURE.NOTE] A különböző SDK mindkét alkalmazás futtatásához az eszközén, és a megoldás vissza vége létrehozásához használható kapcsolatos információkért lásd a [IoT központi SDK][lnk-hub-sdks].

Ebben az oktatóanyagban a következőkre lesz szüksége:

+ Microsoft Visual Studio 2015.

+ Azure active fiók. (Nem rendelkeznek fiókkal, ha egy [ingyenes fiókra] hozhat létre[ lnk-free-trial] a mindössze néhány perc.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Most már létrehozott a IoT-központját, és a hostname (állomásnév) és a kapcsolati karakterláncot az oktatóprogram hátralevő részében befejezéséhez szükséges van.

## <a name="create-a-device-identity"></a>Egy eszköz azonosító létrehozása

Ebben a részben hoz létre, amely a IoT-központban identitás beállításjegyzékbeli hoz létre egy eszköz identitás Windows konzol alkalmazást. Egy eszközt IoT központi eszköz identitás beállításjegyzékbeli bejegyzés mindaddig nem tud csatlakozni. További információ című "Eszköz identitás beállításjegyzék" [IoT központi Fejlesztőeszközök útmutató][lnk-devguide-identity]. Az alkalmazás konzol futtatásakor egy egyedi Eszközazonosítót és az eszköz segítségével azonosítja magát, amikor eszköz a felhőbe üzeneteket küld IoT központi kulcsot hoz létre.

1. A Visual Studióban hozzáadása vizuális C# klasszikus Windows asztal projekt az aktuális megoldás a **New** project-sablon segítségével. Győződjön meg arról, hogy a .NET-keretrendszer verzió 4.5.1 vagy újabb verziója. A projekt **CreateDeviceIdentity**neve.

    ![Új képi C# klasszikus Windows asztal projekt][10]

2. Kattintson a jobb gombbal a **CreateDeviceIdentity** projekt megoldás Explorerben, és válassza a **Nuget csomagok kezelése**.

3. A **Nuget csomag kezelő** ablakban válassza a **Tallózás gombra**, **microsoft.azure.devices**keresni, jelölje ki a **telepíteni** a **Microsoft.Azure.Devices** csomag telepítéséhez és fogadja el a használati feltételei. Ez az eljárás letöltések, telepíti, és hozzáadja a [Microsoft Azure IoT szolgáltatás SDK] hivatkozás[ lnk-nuget-service-sdk] Nuget csomag és függőségét.

    ![Nuget csomag kezelőjének ablaka][11]

4. Adja hozzá a következő `using` kimutatások **Program.cs** fájl tetején:

        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;

5. Az alábbi mezők hozzáadása a **Program** osztály. Helyőrző értékét cserélje le a kapcsolati karakterláncot az IoT-központban, az előző részben létrehozott az.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Az alábbi módon adja hozzá a **Program** osztály:

        private static async Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }

    Ez a módszer egy eszköz identitás azonosító **myFirstDevice**hoz létre. (Ha adott Eszközazonosítót már létezik a beállításjegyzékben, a kód egyszerűen olvassa be a meglévő eszköz adatainak.) Az alkalmazás kattintson az elsődleges kulcs az identitás jeleníti meg. Használatával a kulcs az szimulált eszközben a IoT központban összekapcsolása.

7. Végül adja hozzá a következő sort a **fő** módszer:

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();

8. Futtassa az alkalmazást, és jegyezze le a eszköz billentyűt.

    ![Az alkalmazás által létrehozott eszköz billentyűt][12]

> [AZURE.NOTE] A IoT központi identitás beállításjegyzék csak a központi biztonságos hozzáférés engedélyezése eszköz identitások tárolja. Tárolja az azonosítók eszköztől és a hitelesítő adatokat, és engedélyezett/letiltott jelző is használhatja az egyes eszköz hozzáférés letiltása használható billentyűparancsokról. Ha az alkalmazás más eszköz-specifikus metaadatokat tárolhatnak, akkor az alkalmazás-specifikus tárolót kell használni. További információért útmutatójában [IoT központi Fejlesztőeszközök][lnk-devguide-identity].

## <a name="receive-device-to-cloud-messages"></a>Eszköz a felhőbe hibaüzenetek

Ebben a részben hoz létre, amely beolvassa az eszköz a felhőbe üzenetek IoT központból Windows konzol alkalmazást. Egy IoT központi közzététele egy [Azure esemény hubok][lnk-event-hubs-overview]-kompatibilis végpont ahhoz, hogy az eszköz a felhőbe üzenetek olvasása. Ahhoz, hogy egyszerű dolgot, ebben az oktatóprogramban egy egyszerű olvasó, amely nem megfelelő magas átviteli telepítés hoz létre. A Méretezés eszköz a felhőbe üzenetek feldolgozási módjának című témakörben talál a [folyamat eszköz a felhőbe üzenetek] [ lnk-process-d2c-tutorial] oktatóprogram. Az esemény hubok folyamat során használatáról további információt talál az [Első lépések az esemény hubok] [ lnk-eventhubs-tutorial] oktatóprogram. (Ez az oktatóanyag a IoT központi esemény központi-kompatibilis végpontjait alkalmazandó.)

> [AZURE.NOTE] Az esemény központi-kompatibilis végpontot, az eszköz a felhőbe üzenetek olvasása mindig a AMQP protokollt használja.

1. A Visual Studióban hozzáadása vizuális C# klasszikus Windows asztal projekt az aktuális megoldás a **New** project-sablon segítségével. Győződjön meg arról, hogy a .NET-keretrendszer verzió 4.5.1 vagy újabb verziója. A projekt **ReadDeviceToCloudMessages**neve.

    ![Új képi C# klasszikus Windows asztal projekt][10]

2. Kattintson a jobb gombbal a **ReadDeviceToCloudMessages** projekt megoldás Explorerben, és válassza a **Nuget csomagok kezelése**.

3. **Nuget csomag kezelő** ablakban **WindowsAzure.ServiceBus**kereshet, válassza a **telepítés**és fogadja el a használati feltételeket. Ez az eljárás letöltések telepíti és [Azure Service Bus]mutató hivatkozás hozzáadása[lnk-servicebus-nuget], az összes függőségét. A csomag lehetővé teszi, hogy az alkalmazás csatlakozhat az esemény központi-kompatibilis végpontot, kattintson a IoT-központját.

4. Adja hozzá a következő `using` kimutatások **Program.cs** fájl tetején:

        using Microsoft.ServiceBus.Messaging;
        using System.Threading;

5. Az alábbi mezők hozzáadása a **Program** osztály. Helyőrző értékét cserélje le a kapcsolati karakterláncot az az "Egy IoT központi létrehozása" szakaszban létrehozott IoT-központban.

        static string connectionString = "{iothub connection string}";
        static string iotHubD2cEndpoint = "messages/events";
        static EventHubClient eventHubClient;

6. Az alábbi módon adja hozzá a **Program** osztály:

        private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
        {
            var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
            while (true)
            {
                if (ct.IsCancellationRequested) break;
                EventData eventData = await eventHubReceiver.ReceiveAsync();
                if (eventData == null) continue;

                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }

    Ez a módszer fogadására **EventHubReceiver** példány használt összes a felhőből IoT központi eszköz-a-üzenetek fogadására partíciót. Figyelje meg, hogyan sikeres egy `DateTime.Now` paraméter, hogy csak kap után induljon küldött üzeneteket a **EventHubReceiver** objektum létrehozásakor. Ez a szűrő akkor lehet hasznos, tesztkörnyezetben, így láthatja, hogy az üzenetek aktuális készlete. Munkakörnyezetben a kód győződjön meg arról, hogy az összes az üzenetek feldolgozásával. További tudnivalókért lásd: a [IoT központi eszköz a felhőbe üzenetek feldolgozási módjának] [ lnk-process-d2c-tutorial] oktatóprogram.

7. Végül adja hozzá a következő sort a **fő** módszer:

        Console.WriteLine("Receive messages. Ctrl-C to exit.\n");
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);

        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;

        CancellationTokenSource cts = new CancellationTokenSource();

        System.Console.CancelKeyPress += (s, e) =>
        {
          e.Cancel = true;
          cts.Cancel();
          Console.WriteLine("Exiting...");
        };

        var tasks = new List<Task>();
        foreach (string partition in d2cPartitions)
        {
           tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }  
        Task.WaitAll(tasks.ToArray());

## <a name="create-a-simulated-device-app"></a>Szimulált eszközön alkalmazás létrehozása

Ebben a részben hozzon létre egy Windows konzol alkalmazás, amely eszköz a felhőbe üzeneteket küld egy IoT központi eszközt.

1. A Visual Studióban hozzáadása vizuális C# klasszikus Windows asztal projekt az aktuális megoldás a **New** project-sablon segítségével. Győződjön meg arról, hogy a .NET-keretrendszer verzió 4.5.1 vagy újabb verziója. A projekt **SimulatedDevice**neve.

    ![Új képi C# klasszikus Windows asztal projekt][10]

2. Kattintson a jobb gombbal a **SimulatedDevice** projekt megoldás Explorerben, és válassza a **Nuget csomagok kezelése**.

3. A **Nuget csomag kezelő** ablakban válassza a **Tallózás gombra**, **Microsoft.Azure.Devices.Client**keresni, jelölje ki a **telepíteni** a **Microsoft.Azure.Devices.Client** csomag telepítéséhez és fogadja el a használati feltételei. Ez az eljárás letöltések, telepíti, és hozzáadja az [Azure IoT - eszköz SDK Nuget csomag] hivatkozás[ lnk-device-nuget] és függőségét.

4. Adja hozzá a következő `using` utasítás **Program.cs** fájl tetején:

        using Microsoft.Azure.Devices.Client;
        using Newtonsoft.Json;

5. Az alábbi mezők hozzáadása a **Program** osztály. Cserélje le a helyőrző értékeket a IoT központi hostname (állomásnév) a "-IoT központi létrehozása" szakaszban lekért együtt, és olvassa be a "Eszköz megjelenést alakíthat" szakasz a az eszköz billentyűt.

        static DeviceClient deviceClient;
        static string iotHubUri = "{iot hub hostname}";
        static string deviceKey = "{device key}";

6. Az alábbi módon adja hozzá a **Program** osztály:

        private static async void SendDeviceToCloudMessagesAsync()
        {
            double avgWindSpeed = 10; // m/s
            Random rand = new Random();

            while (true)
            {
                double currentWindSpeed = avgWindSpeed + rand.NextDouble() * 4 - 2;

                var telemetryDataPoint = new
                {
                    deviceId = "myFirstDevice",
                    windSpeed = currentWindSpeed
                };
                var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
                var message = new Message(Encoding.ASCII.GetBytes(messageString));

                await deviceClient.SendEventAsync(message);
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

                Task.Delay(1000).Wait();
            }
        }

    Ez a módszer eszköz a felhőbe új üzenetet küld, minden második. Az üzenet egy JSON szerializáltként objektumra, és az Eszközazonosítót tízjegyű szám szél sebesség érzékelő hasonlóan tartalmaz.

7. Végül adja hozzá a következő sort a **fő** módszer:

        Console.WriteLine("Simulated device\n");
        deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

        SendDeviceToCloudMessagesAsync();
        Console.ReadLine();

  Alapértelmezés szerint a **Create** metódus egy AMQP protokollt használó kapcsolatba lépni a IoT központi **DeviceClient** példányt hoz létre. A HTTP protokoll, használja a felülbírálása az **létrehozása** módszer, amely lehetővé teszi, hogy a protokoll megadásához. Ha a HTTP protokollt használja, akkor is fel kell a **Microsoft.AspNet.WebApi.Client** Nuget csomag a projekthez, amelyet fel szeretne venni a **System.Net.Http.Formatting** névtér.

Ebben az oktatóprogramban egy központi IoT eszköz ügyfél létrehozásának lépései végigvezeti Önt. Is használhatja az [Azure IoT elosztóhoz csatlakoztatott szolgáltatás] [ lnk-connected-service] Visual Studio-bővítmény a szükséges kód hozzáadása a eszköz ügyfélalkalmazás.


> [AZURE.NOTE] Ahhoz, hogy egyszerű dolgot, ebben az oktatóanyagban nem valósítja bármely újrapróbálkozási házirend. Gyártási kódban végre kell hajtania újrapróbálkozási házirendek (például egy exponenciális visszalépési), az MSDN-cikk [Tranziens hibafa kezelése]a javasolt[lnk-transient-faults].

## <a name="run-the-applications"></a>Az alkalmazások futtatása

Most már készen áll az alkalmazások futtatásához.

1.  A Visual Studióban, a megoldást Intézőben kattintson a jobb gombbal a megoldás, és kattintson az **Indítás beállítása projektek**. Jelölje ki a **több indítási projekt**, és válassza a **Start** a művelet a **ReadDeviceToCloudMessages** és a **SimulatedDevice** projektekhez.

    ![Indítási projekt tulajdonságai][41]

2.  Nyomja le az **F5** elindítani mindkét operációs rendszert futtató alkalmazásokat. A konzol eredménye a **SimulatedDevice** alkalmazásból látható az üzenetek szimulált eszköze küld a IoT-központját. A **ReadDeviceToCloudMessages** alkalmazásból a konzol eredménye olyan üzenetre, amelyet a IoT központi kap jeleníti meg.

    ![Alkalmazások konzol kimenete][42]

3. A **használati** csempére az [Azure portál] [ lnk-portal] a központi küldött üzenetek számát jeleníti meg:

    ![Azure portál használatát csempe][43]


## <a name="next-steps"></a>Következő lépések

Ebben az oktatóprogramban egy IoT központi konfigurálni az Azure-portálon, és hozza létre a eszköz identitás a központi identitás beállításjegyzékben. Az eszköz identitás használt ahhoz, hogy az eszköz a felhőbe üzeneteket küldeni a központi szimulált eszközön alkalmazás. Az alkalmazás, amely megjeleníti az üzenetek, a központi megkapta is létrehozott. 

Továbbra is az első lépések – IoT központi vagy más IoT esetben feltárása, olvassa el a:

- [Csatlakozás az eszközön][lnk-connect-device]
- [Első lépések a mobileszköz-kezelés][lnk-device-management]
- [Az átjáró IoT SDK – első lépések][lnk-gateway-SDK]

A IoT megoldás kiterjesztése és dolgozza eszköz a felhőbe üzeneteket a méretezés című témakörben talál a [folyamat eszköz a felhőbe üzenetek] [ lnk-process-d2c-tutorial] oktatóprogram.

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp1.png
[11]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp2.png
[12]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp3.png

<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
