<properties
    pageTitle="IoT központi üzenetek cloud-eszköz |} Microsoft Azure"
    description="Ebből az oktatóanyagból megtudhatja, hogy miként Azure IoT elosztót használ a és C# cloud-eszköz üzenetek küldéséhez kövesse."
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/23/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-net"></a>Oktatóprogram: Hogyan IoT központi és .net cloud-eszköz üzenet küldése

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>– Bevezetés

Azure IoT központi, amely segít teljes körű felügyelt szolgáltatás engedélyezése a megbízható és nem biztonságos kétirányú kommunikációt milliónyi IoT eszközök között, és a Befejezés vissza egy alkalmazást. Az [első lépések a IoT központi] szabhatja hozzon létre egy IoT hubhoz, rajta egy eszköz identitás kiépítése és kód eszköz a felhőbe üzeneteket küld szimulált eszközt.

Ez az oktatóanyag [első]lépések a IoT központi hoz létre. Hogyan azt mutatja, hogy:

- Végéről az alkalmazás felhő vissza üzenetküldés cloud-eszköz IoT hubon keresztül egyetlen eszközre.
- Egy eszközön cloud-eszköz üzeneteket fogadni.
- Az alkalmazás felhőből vége biztonsági, (*Visszajelzés*) kézbesítési visszaigazolás kérése egy eszközt IoT központból küldött üzenetek.

Talál további információt a felhőben-eszköz üzenetek a [IoT központi Developer Guide][IoT Hub Developer Guide - C2D].

Ez az oktatóanyag végén futtatja a Windows-konzol két alkalmazások:

* **SimulatedDevice**, az alkalmazás [IoT központi – első lépések], amelyek a IoT hubhoz csatlakozik, és felhőalapú-eszköz üzenetek fogadása létrehozott módosított verzióját.
* **SendCloudToDevice**, mely cloud-eszköz üzenetet küld a szimulált eszközön IoT hubon keresztül, és ezután kap a kézbesítési visszaigazolás.

> [AZURE.NOTE] IoT hubhoz számos eszközt platformokon és nyelvek (többek között, C, Java és Javascript) keresztül Azure IoT eszköz SDK SDK támogatása. Ebben az oktatóanyagban kód csatlakoztatni az eszközt, és általában Azure IoT hubon keresztül csatlakozott, lásd: az [Azure IoT Developer Center]részletes útmutatást.

Oktatóprogram elvégzéséhez a következőkre lesz szüksége:

+ Microsoft Visual Studio 2015

+ Azure active fiók. (Nem rendelkeznek fiókkal, ha egy [ingyenes fiókra] hozhat létre[ lnk-free-trial] a mindössze néhány perc.)

## <a name="receive-messages-on-the-simulated-device"></a>Hibaüzenetek szimulált az eszközre

Ebben a szakaszban a szimulált eszközön alkalmazás fog módosítása [IoT központi – első lépések] a IoT központból cloud-eszköz-üzenetek fogadására hozta létre.

1. A Visual Studióban a **SimulatedDevice** projekt adja hozzá a **Program** osztály a következő módszerrel.

        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud to device messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();

                await deviceClient.CompleteAsync(receivedMessage);
            }
        }

    A `ReceiveAsync` metódus aszinkron ad vissza, a beérkezett üzenetekben megkapta az eszköz időpontjában. *Null* vissza egy specifiable időtúllépés után (ebben az esetben egy perc az alapértelmezett használt). Amikor ez történik, a kód továbbra is várnia az új üzeneteket. Ennek az oka az `if (receivedMessage == null) continue` sor.

    A hívás `CompleteAsync()` értesíti IoT központi, hogy az üzenet feldolgozott sikeresen megtörtént. Az üzenet biztonságosan az eszköz várólista eltávolítható. Ha valamit, amit történt, hogy az eszköz alkalmazás megakadályozza az üzenet feldolgozása befejezése, IoT központi kézbesít újra. Fontos majd, hogy a üzenet feldolgozás logika a eszköz alkalmazásban *idempotent*, lehet, hogy az azonos üzenetet fogadó többször ugyanazt az eredményt adja. Az alkalmazások átmenetileg is elhagyja egy üzenetet, amely a jövőbeli felhasználás várakozási sorban az üzenet megőrzése IoT központi eredményez. Vagy az alkalmazás elutasíthatja egy üzenetet, amely véglegesen eltávolítja az üzenetet a sorból. A [IoT központi Fejlesztőeszközök útmutató]című témakörben olvashat a felhőben-eszköz üzenet,[IoT Hub Developer Guide - C2D].

    > [AZURE.NOTE] Ha HTTP használatával MQTT vagy AMQP helyett egy átviteli, mint a `ReceiveAsync` módszer azonnal adja eredményül. HTTP cloud-eszköz üzenetek támogatott mintája időnként csatlakoztatott eszközök jelölje be az üzenetek ritkán (legfeljebb 25 percenként). További HTTP kibocsátó megkapja az eredmények kérelmek szabályozásának IoT-központban. MQTT, a AMQP és a HTTP-támogatási és IoT központi szabályozásának közötti különbségekről olvashat, lásd: a [IoT központi Fejlesztőeszközök útmutató][IoT Hub Developer Guide - C2D].

2. A **fő** módszer esetén az alábbit hozzáadása előtt jobbra a `Console.ReadLine()` sor:

        ReceiveC2dAsync();

> [AZURE.NOTE] Ebben az oktatóprogramban az egyszerűség 's rizspálinkát, nem hajtja végre bármely újrapróbálkozási házirend. A gyártási kódot végre kell hajtania a újrapróbálkozási házirendek (például exponenciális visszalépési), az [Ideiglenes (tranziens) hiba kezelésének]MSDN-cikk javasolt.

## <a name="send-a-cloud-to-device-message"></a>Üzenet küldése egy felhőalapú-eszköz

Ebben a részben egy Windows konzol alkalmazás, amely cloud-eszköz üzeneteket küld a szimulált eszközön alkalmazás fog írhat.

1. A jelenlegi Visual Studio megoldás a vizuális C# asztali alkalmazás új projekt létrehozása a **New** project-sablon segítségével. A projekt **SendCloudToDevice**neve.

    ![A Visual Studio alkalmazásban új projektet][20]

2. Megoldás Explorerben kattintson a jobb gombbal a megoldást, és válassza a **NuGet csomagok kezelése... megoldás**. 

    Ekkor megnyílik a **NuGet csomagok kezelése** ablakot.

3. Keressen `Microsoft Azure Devices`, kattintson a **telepítés**gombra, és fogadja el a használati feltételeket. 

    Ez a letöltések, telepíti, és hozzáadja az [Azure IoT - szolgáltatás SDK NuGet csomag]mutató hivatkozás.

4. Adja hozzá a következő `using` utasítás **Program.cs** fájl tetején:

        using Microsoft.Azure.Devices;

5. Az alábbi mezők hozzáadása a **Program** osztály. Cserélje le a helyőrző értéket, az [első lépések a IoT központi]IoT központi kapcsolati karakterlánccal:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";

6. Az alábbi módon adja hozzá a **Program** osztály:

        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud to device message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }

    Ez a módszer új cloud-eszköz üzenetet küld az eszköz a azonosítójú `myFirstDevice`. A paraméter abban az esetben, ha módosítani szeretne használni az [első lépések a IoT központi]ennek megfelelően módosíthatja.

7. Végül adja hozzá a következő sort a **fő** módszer:

        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);

        Console.WriteLine("Press any key to send a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();

8. A Visual Studio kattintson a jobb gombbal a megoldás, és válassza az **Indítás beállítása projektek...**parancsra. Válassza a **több indítási projektet**, majd jelölje ki a **ProcessDeviceToCloudMessages** **SimulatedDevice**és **SendCloudToDevice** **indítása** műveletet.

9.  Nyomja le az **F5 billentyűt**. Mindhárom alkalmazás kezdetének. Jelölje ki a **SendCloudToDevice** windows, és nyomja le az **ENTER billentyűt**. Meg kell jelennie a a szimulált által éppen kapott üzenetet.

    ![Alkalmazás érkező üzenetek][21]

## <a name="receive-delivery-feedback"></a>Kézbesítési visszajelzéseket
Lehetőség a kérelem kézbesítési (vagy a lejárat) nyugták IoT központból minden cloud-eszköz üzenet. Ezzel a felhőben háttér egyszerűen tájékoztatására ismét vagy támogatás összefüggés. A [IoT központi Fejlesztőeszközök útmutató]című témakörben további információt a felhőben-eszköz visszajelzés[IoT Hub Developer Guide - C2D].

Ebben a szakaszban a visszajelzés kérése, és IoT központból kapta meg a **SendCloudToDevice** alkalmazásban módosítja.

1. A Visual Studióban a **SendCloudToDevice** projekt adja hozzá a **Program** osztály a következő módszerrel.
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();

            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();

                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }

    Ne feledje, hogy a fogadás minta azonos azzal a eszköz alkalmazásból cloud-eszköz-üzenetek fogadására használt-e.

2. A **fő** módszer esetén az alábbit hozzáadása után közvetlenül a `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` sor:

        ReceiveFeedbackAsync();

3. Ha visszajelzést szeretne kérni az a felhő-eszköz üzenet kézbesítésének, akkor adjon meg egy tulajdonságot a **SendCloudToDeviceMessageAsync** módszer. A következő sor hozzáadása után közvetlenül a `var commandMessage = new Message(...);` sor:

        commandMessage.Ack = DeliveryAcknowledgement.Full;

4.  Futtassa az alkalmazások **F5**billentyű lenyomásával. Meg kell jelennie mindhárom alkalmazás indítása. Jelölje ki a **SendCloudToDevice** windows, és nyomja le az **ENTER billentyűt**. Meg kell jelennie a fogadott a szimulált alkalmazást, és néhány másodperc után a visszajelzés üzenet a **SendCloudToDevice** alkalmazás fogadott üzenet.

    ![Alkalmazás érkező üzenetek][22]

> [AZURE.NOTE] Ebben az oktatóprogramban az egyszerűség 's rizspálinkát, nem hajtja végre bármely újrapróbálkozási házirend. A gyártási kódot végre kell hajtania a újrapróbálkozási házirendek (például exponenciális visszalépési), az [Ideiglenes (tranziens) hiba kezelésének]MSDN-cikk javasolt.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban cloud-eszköz üzenetek küldése és fogadása hogyan megtanulta azt. 

Példák a teljes végpont megoldások, amelyek IoT központi talál további [Azure IoT csomagot].

IoT központi megoldások fejlesztésével kapcsolatos további információért lásd: a [IoT központi Fejlesztőeszközök útmutató].

<!-- Images -->
[20]: ./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png
[21]: ./media/iot-hub-csharp-csharp-c2d/sendc2d1.png
[22]: ./media/iot-hub-csharp-csharp-c2d/sendc2d2.png

<!-- Links -->

[Azure IoT - szolgáltatás SDK NuGet csomag]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[Ideiglenes (tranziens) hibafa kezelése]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md

[IoT központi Fejlesztőeszközök útmutató]: iot-hub-devguide.md
[Első lépések a IoT központi]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure IoT programcsomagban]: https://azure.microsoft.com/documentation/suites/iot-suite/