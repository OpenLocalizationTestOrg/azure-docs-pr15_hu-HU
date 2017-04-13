<properties
    pageTitle="IoT elosztót használ eszközökön a fájlok feltöltése a |} Microsoft Azure"
    description="Ebből az oktatóanyagból megtudhatja, hogy miként tölthet fel fájlokat Azure IoT elosztót használ a és C# eszközökről kövesse."
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
     ms.date="06/21/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-upload-files-from-devices-to-the-cloud-with-iot-hub"></a>Oktatóprogram: Hogy miként tölthet fel fájlokat a felhőben való IoT központi eszközökről

## <a name="introduction"></a>– Bevezetés

Azure IoT központi egy teljes körű felügyelt szolgáltatás, amely lehetővé teszi a megbízható és biztonságos kétirányú kommunikációt, több millió IoT eszközök és az alkalmazások között biztonsági célból. Előző oktatóanyagok ([IoT központi – első lépések] és [IoT központi Cloud-eszköz üzenet küldése]) ábrázolják a egyszerű eszköz-a-felhőbe, és felhőalapú-eszköz üzenetkezelő IoT központi, és a [folyamat eszköz felhő üzenetek] oktatóprogram funkcióit ismerteti biztos, hogy az eszköz a felhőbe üzenetek tárolását Azure blob-tárolóhoz lehetőséget. Egyes esetekben azonban megfeleltetése nem az eszközén küldése viszonylag kis eszköz a felhőbe által küldött üzenetekről IoT központi fogadja el az adatok egyszerűen. Nagyméretű fájlok, képek, videók, bejárására mintát rezgő adatokat tartalmazó, illetve a előre feldolgozott adatok valamilyen tartalmazó többek között. Ezek a fájlok jellemzően a felhőben, például [Azure Data Factory] vagy a [Hadoop] Papírhalom eszközeivel feldolgozott köteget. Ha olyan eszközről fájlfeltöltések elsődleges-események küldés, biztonság és megbízhatóság funkciók IoT központi továbbra is használhatja.

Ebben az oktatóanyagban épül a [felhőben-eszköz IoT központi üzenet küldése] oktatóprogram során mutatja, hogy a IoT központi fájl feltöltése funkcióinak használatához a kódot. Azt mutatja be biztonságosan küldje eszközt az Azure blob-URI feltöltése egy fájlt, és használatáról a IoT központi fájl feltöltésekről tájékoztató értesítések szeretne elindítani a fájlt az alkalmazás vissza vége a feldolgozása.

Ez az oktatóanyag végén futtatja a Windows-konzol két alkalmazások:

* **SimulatedDevice**, az a [felhő-eszköz IoT központi üzenet küldése] oktatóprogram során, amely feltölt egy fájlt a IoT központi által biztosított Társítások URI használatával tárolóhoz létrehozott alkalmazás módosított verzióját.
* **ReadFileUploadNotification**, amely fájl feltöltésekről tájékoztató értesítések megkapja a IoT központból.

> [AZURE.NOTE] IoT központi Azure IoT eszközön SDK keresztül számos eszközt platformokon és nyelvek (többek között, C, Java és Javascript) támogat. Keresse meg az [Azure IoT Developer Center] for lépésenkénti útmutatást csatlakoztatni az eszközt, ebben az oktatóanyagban kódot, és általában Azure IoT hubon keresztül csatlakozott.

Oktatóprogram elvégzéséhez az alábbiakra van szükség:

+ Microsoft Visual Studio 2015,

+ Azure active fiók. (Nem rendelkeznek fiókkal, ha egy [ingyenes fiókra] hozhat létre[ lnk-free-trial] a mindössze néhány perc.)

## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Rendeljen egy Azure tároló fiókot IoT hubhoz

A szimulált eszközön blob feltölti fájl, mert, a kapcsolódó IoT hubhoz [Azure tároló] fiókkal kell rendelkeznie. Azure tároló fiók társítása egy IoT hubhoz, amikor a központi Társítások URI, amely egy eszköz segítségével biztonságosan fájl feltöltése blob-tárolóhoz miatt. A központi IoT szolgáltatás és az eszköz SDK koordinálása a folyamat, amely a Társítások URI hoz létre, és egy fájl feltöltése használni kívánt eszköz számára elérhetővé teszi azt.

Kövesse a [fájlfeltöltések konfigurálása az Azure portálon] [ lnk-configure-upload] a IoT hubhoz Azure tároló fiók társítani.

## <a name="upload-a-file-from-a-simulated-device"></a>A szimulált eszközről fájl feltöltése

Ebben a részben szimulált eszköz alkalmazása módosíthatja az a felhő-eszköz üzenetek fogadására a IoT központból [IoT központi Cloud-eszköz üzenet küldése] létrehozott.

1. A Visual Studióban kattintson a jobb gombbal a **SimulatedDevice** projektet, kattintson a **Hozzáadás**gombra, és válassza a **Meglévő elemet**. Keresse meg a kép fájl, és adja hozzá a projekthez. Ebben az oktatóanyagban feltételezi, hogy a kép neve `image.jpg`.

2. Kattintson a jobb gombbal a képre, és válassza a **Tulajdonságok parancsot**. Győződjön meg arról, hogy **másolja a vágólapra a kimeneti könyvtár** értéke **mindig másolatot**.

    ![][1]

3. A **Program.cs** fájlban adja hozzá a következő utasítások a fájl tetején:

        using System.IO;

4. Az alábbi módon adja hozzá a **Program** osztály:
         
        private static async void SendToBlobAsync()
        {
            string fileName = "image.jpg";
            Console.WriteLine("Uploading file: {0}", fileName);
            var watch = System.Diagnostics.Stopwatch.StartNew();

            using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
            {
                await deviceClient.UploadToBlobAsync(fileName, sourceData);
            }

            watch.Stop();
            Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
        }

    A `UploadToBlobAsync` módszer vesz fel kell tölteni a fájl nevét és adatfolyam forrásának a fájlt, és a feltöltés tárolóhoz kezeli. A New jeleníti meg a fájl feltöltéséhez szükséges időt.

5. A **fő** módszer esetén az alábbit hozzáadása előtt jobbra a `Console.ReadLine()` sor:

        SendToBlobAsync();

> [AZURE.NOTE] Ebben az oktatóprogramban az egyszerűség 's rizspálinkát, nem hajtja végre bármely újrapróbálkozási házirend. A gyártási kódot végre kell hajtania a újrapróbálkozási házirendek (például exponenciális visszalépési), az [Ideiglenes (tranziens) hiba kezelésének]MSDN-cikk javasolt.

## <a name="receive-a-file-upload-notification"></a>Fájl feltöltése értesítést kapnak

Ebben a részben egy Windows konzol alkalmazás, fájl feltöltése értesítését kapja IoT központból írni.

1. Az aktuális Visual Studio megoldás az új vizuális C# Windows projekt létrehozása a **New** project-sablon használatával. A projekt **ReadFileUploadNotification**neve.

    ![A Visual Studio alkalmazásban új projektet][2]

2. Kattintson a jobb gombbal a **ReadFileUploadNotification** projekt megoldás Explorerben, és válassza a **NuGet csomagok kezelése**.

    Ekkor megjelenik a NuGet csomagok kezelése ablakot.

2. Keressen `Microsoft.Azure.Devices`, kattintson a **telepítés**gombra, és fogadja el a használati feltételeket. 

    A letöltések, telepíti, és a **ReadFileUploadNotification** project összeadja az [Azure IoT - szolgáltatás SDK NuGet csomag] mutató hivatkozás.

3. A **Program.cs** fájlban adja hozzá a következő utasítások a fájl tetején:

        using Microsoft.Azure.Devices;

4. Az alábbi mezők hozzáadása a **Program** osztály. Cserélje le a helyőrző értéket, az [első lépések a IoT központi]IoT központi kapcsolati karakterlánccal:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
        
5. Az alábbi módon adja hozzá a **Program** osztály:
   
        private async static Task ReceiveFileUploadNotificationAsync()
        {
            var notificationReceiver = serviceClient.GetFileNotificationReceiver();

            Console.WriteLine("\nReceiving file upload notification from service");
            while (true)
            {
                var fileUploadNotification = await notificationReceiver.ReceiveAsync();
                if (fileUploadNotification == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received file upload noticiation: {0}", string.Join(", ", fileUploadNotification.BlobName));
                Console.ResetColor();

                await notificationReceiver.CompleteAsync(fileUploadNotification);
            }
        }

    Ne feledje, hogy a fogadás minta azonos azzal a eszköz alkalmazásból cloud-eszköz-üzenetek fogadására használt-e.

6. Végül adja hozzá a következő sort a **fő** módszer:

        Console.WriteLine("Receive file upload notifications\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        ReceiveFileUploadNotificationAsync().Wait();
        Console.ReadLine();

## <a name="run-the-applications"></a>Az alkalmazások futtatása

Most már készen áll az alkalmazások futtatásához.

1. A Visual Studióban kattintson a jobb gombbal a megoldás, és válassza **a projekt beállítása indítási**. Válassza a **több indítási projektet**, majd jelölje ki a **ReadFileUploadNotification** és **SimulatedDevice** **indítása** műveletet.

2. Nyomja le az **F5 billentyűt**. Mindkét alkalmazás kezdetének. Meg kell jelennie a feltöltés befejeződött egy console app és a feltöltés értesítő üzenet, a többi konzol által fogadott. Az [Azure portálja] vagy Visual Studio Server Explorer segítségével a feltöltött fájl jelenlétét keresésének Azure tárterület-fiókjában.

  ![][50]


## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban megtanulta, hogyan használhatja a fájl feltöltése a funkcióinak IoT központi fájlfeltöltések eszközökről egyszerűsítése érdekében. Is feltárása IoT központi szolgáltatások és -esetek, az alábbi cikkekben:

- [Egy IoT központi programozás útján létrehozása][lnk-create-hub]
- [C SDK – bevezetés][lnk-c-sdk]
- [IoT központi SDK][lnk-sdks]

További feltárása a IoT központi funkcióinak, olvassa el:

- [Egy eszközt a IoT átjáró SDK csomagjában talál, amelyek][lnk-gateway]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/create-identity-csharp1.png

<!-- Links -->

[Azure portál]: https://portal.azure.com/

[Azure Data Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[Hadoop]: https://azure.microsoft.com/documentation/services/hdinsight/

[IoT központi Cloud-eszköz üzenet küldése]: iot-hub-csharp-csharp-c2d.md
[A folyamat eszköz a felhőbe üzenetek]: iot-hub-csharp-csharp-process-d2c.md
[Első lépések a IoT központi]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot

[Ideiglenes (tranziens) hibafa kezelése]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure tárhely]: ../storage/storage-create-storage-account.md#create-a-storage-account
[lnk-configure-upload]: iot-hub-configure-file-upload.md
[Azure IoT - szolgáltatás SDK NuGet csomag]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md


