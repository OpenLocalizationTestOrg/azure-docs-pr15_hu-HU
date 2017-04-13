## <a name="send-messages-to-event-hubs"></a>Esemény hubok üzeneteket küldeni

Ebben a részben egy Windows konzol alkalmazás, amely események küld az esemény központi fogja írhat.

1. A Visual Studióban vizuális C# asztali alkalmazás a **New** project-sablon használata új projekt létrehozása A projekt **Feladó**neve.

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp1.png)

2. A megoldás Intézőben kattintson a jobb gombbal a megoldást, és kattintson a **Megoldás NuGet csomagok kezelése**. 

3. Kattintson a **Tallózás** fülre, majd keressen `Microsoft Azure Service Bus`. Győződjön meg arról, hogy a projekt nevét (**Feladó**) a **változatokat** mezőben megadott értékkel. Kattintson a **telepítés**gombra, és fogadja el a használati feltételeket. 

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp2.png)

    Visual Studio tölthető le, és a telepítést, és hozzáadja a az [Azure Service Bus tár NuGet csomag](https://www.nuget.org/packages/WindowsAzure.ServiceBus)mutató hivatkozás.

4. Adja hozzá a következő `using` kimutatások **Program.cs** fájl tetején:

    ```
    using System.Threading;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Az alábbi mezők hozzáadása a **Program** osztály, helyettesítése a helyőrző értékeket az előző részben létrehozott az esemény-központban, és a korábban mentett névtér szintű kapcsolattal a nevet.

    ```
    static string eventHubName = "{Event Hub name}";
    static string connectionString = "{send connection string}";
    ```

6. Az alábbi módon adja hozzá a **Program** osztály:

    ```
    static void SendingRandomMessages()
    {
        var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
        while (true)
        {
            try
            {
                var message = Guid.NewGuid().ToString();
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
                eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
                Console.ResetColor();
            }

            Thread.Sleep(200);
        }
    }
    ```

    Ez a módszer események folyamatosan küld a 200-ms késleltetést az esemény-központját.

7. Végül adja hozzá a következő sort a **fő** módszer:

    ```
    Console.WriteLine("Press Ctrl-C to stop the sender process");
    Console.WriteLine("Press Enter to start now");
    Console.ReadLine();
    SendingRandomMessages();
    ```
