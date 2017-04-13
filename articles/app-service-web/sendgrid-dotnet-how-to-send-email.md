<properties 
    pageTitle="A SendGrid használt e-mail szolgáltatás (.NET) használata |} Microsoft Azure" 
    description="Megtudhatja, hogyan küldhet e-mailben a SendGrid használt e-mail szolgáltatás a Azure. Minták írt C# kód, és a .NET API-t használja." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="thinkingserious" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="01/14/2016" 
    ms.author="team-pi@sendgrid.com"/>





# <a name="how-to-send-email-using-sendgrid-with-azure"></a>Hogyan küldhet e-mailt Azure SendGrid használata


## <a name="overview"></a>– Áttekintés

Ez az útmutató bemutatja, hogyan Azure a SendGrid e-mail szolgáltatással közös programozási különböző művelet végrehajtását. A minták írt C\#
és a .NET API-t használja. Az érintett esetek **megépítése e-mailek**, a **küldenek e-mailt**, a **mellékletek hozzáadása**és a **szűrők használata**. SendGrid és a Küldés e-mailek további tudnivalókért lásd: a [következő lépésekkel][] szakaszban.

## <a name="what-is-the-sendgrid-email-service"></a>Mi az a SendGrid használt e-mail szolgáltatás?

SendGrid egy [felhőalapú e-mail szolgáltatás] megbízható [tranzakció alapú e-mailek kézbesítési], méretezhetőség, és a valós idejű analytics rugalmas API-hoz, amelyekkel egyszerűbbé egyéni integráció együtt. SendGrid használatát esetei a következők:

-   Automatikus küldése a vevők visszaigazolások.
-   Havi az e-szórólapok és különleges ajánlatok küldése a felhasználók felügyelete terjesztési listája.
-   Valós idejű mértékek, például a letiltott e-mailek és ügyfél válaszidő gyűjteni.
-   Trendek azonosításához jelentések létrehozásához.
-   Ügyfél lekérdezésekben továbbítás.
-   Bejövő e-mailek feldolgozása.

További információ a [https://sendgrid.com](https://sendgrid.com) és a [C# tár][sendgrid-csharp] című cikkben talál.

## <a name="create-a-sendgrid-account"></a>SendGrid-fiók létrehozása

[AZURE.INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-net-class-library"></a>Hivatkozás a SendGrid .NET osztálytár

A [csomag SendGrid NuGet](https://www.nuget.org/packages/Sendgrid) legkönnyebben a SendGrid API és állítható be az alkalmazás az összes függőségek. NuGet a Visual Studio-bővítmény a Microsoft Visual Studio 2015, amely megkönnyíti, telepítése, és frissítse a tárakban és eszközök beépített. 

> [AZURE.NOTE] NuGet Ha futtatja a Visual Studio Visual Studio 2015-nél korábbi verziójának telepítéséhez látogasson el a [http://www.nuget.org](http://www.nuget.org), és kattintson a **Telepítés NuGet** gombra.

A SendGrid NuGet szoftvercsomag telepítésekor az alkalmazásban, tegye a következőket:

1.  Új projekt létrehozása.

    ![Új projekt létrehozása][create-new-project]

2.  Válasszon egy sablont.

    ![Sablon kiválasztása][select-a-template]

3.  **Megoldás Explorer**kattintson a jobb gombbal a **hivatkozást**, majd **NuGet csomagok kezelése**gombra.

4.  Keresse meg a **SendGrid** , és az eredmények listájában jelölje ki az **SendGrid** elemet.

    ![SendGrid NuGet csomag][SendGrid-NuGet-package]

5.  Kattintson a **telepítés** gombot a telepítés befejezéséhez, és zárja be a párbeszédpanelt.

SendGrid a .NET osztálytár **SendGridMail**neve. A következő névteret tartalmaz:

-   **SendGridMail** létrehozása és használata az e-mail elemek.
-   **SendGridMail.Transport** az **SMTP-** protokoll vagy a HTTP 1.1 protokollt használata a **Webes/többi**e-mailek küldéséhez.

Adja hozzá a következő kódot névtér nyilatkozatokat bármely C tetején\# , amelyben a SendGrid használt e-mail szolgáltatás programozás útján elérni kívánt fájlt.
**System.Net** és **System.Net.Mail** a .NET-keretrendszer névtér, amelyek szerepelnek, mert gyakran használ, a SendGrid API-k típusok tartalmazzák.

    using System;
    using System.Net;
    using System.Net.Mail;
    using SendGrid;

## <a name="how-to-create-an-email"></a>Hogyan kell: e-mail létrehozása

A **SendGridMessage** objektum segítségével e-mailt. Amikor az üzenet objektum létrejött, beállíthatja, hogy tulajdonságokat és módszerek, beleértve az e-mail feladójának, az e-mail címzett és a téma és az e-mail törzsében.

A következő példa bemutatja, hogyan kell teljesen feltöltött e-mail-objektum létrehozása:

    // Create the email object first, then add the properties.
    var myMessage = new SendGridMessage();

    // Add the message properties.
    myMessage.From = new MailAddress("john@example.com");

    // Add multiple addresses to the To field.
    List<String> recipients = new List<String>
    {
        @"Jeff Smith <jeff@example.com>",
        @"Anna Lidman <anna@example.com>",
        @"Peter Saddow <peter@example.com>"
    };

    myMessage.AddTo(recipients);

    myMessage.Subject = "Testing the SendGrid Library";

    //Add the HTML and Text bodies
    myMessage.Html = "<p>Hello World!</p>";
    myMessage.Text = "Hello World plain text!";

További információt a tulajdonságok és módszerek **SendGrid** típus által támogatott nézze meg [sendgrid-csharp][] GitHub.

## <a name="how-to-send-an-email"></a>Útmutató: a Küldés e-mailben

Miután létrehozta a e-mailt, elküldheti a webes API SendGrid által biztosított. Azt is megteheti akkor előfordulhat, hogy [használatát. A nettó beépített tár](https://sendgrid.com/docs/Code_Examples/csharp.html).

E-mail küldése a SendGrid fiók hitelesítő adatait (felhasználónév és jelszó) vagy a SendGrid API-ja kulcs ellátási szükséges. API kulcsa a használni kívánt módot. Ha API kulcsok beállításáról olvashat, kérjük, keresse fel a [dokumentáció](https://sendgrid.com/docs/Classroom/Send/api_keys.html)

Ezek a hitelesítő adatok tárolhatnak az Azure-portálon keresztül, a beállítás gombra kattintva, és fel a kulcs/érték párokká "alkalmazás beállítások" területen.

 ![Azure alkalmazás beállításainak][azure_app_settings]

 Ezután akkor előfordulhat, hogy elérheti őket, az alábbi képlettel történik: 
    
    var username = System.Environment.GetEnvironmentVariable("SENDGRID_USERNAME"); 
    var pswd = System.Environment.GetEnvironmentVariable("SENDGRID_PASSWORD");
    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");

Hitelesítő adatok használata:
    
    // Create network credentials to access your SendGrid account
    var username = "your_sendgrid_username";
    var pswd = "your_sendgrid_password";

    var credentials = new NetworkCredential(username, pswd);
    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

API billentyűt használhatja:

    var apiKey = "your_sendgrid_api_key";  
    // create a Web transport, using API Key
    var transportWeb = new Web(apiKey);


Az alábbi példák bemutatják, hogyan a webes API üzenet küldése.

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Create credentials, specifying your user name and password.
    var credentials = new NetworkCredential("username", "password");

    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

    // Send the email, which returns an awaitable task.
    transportWeb.DeliverAsync(myMessage);

    // If developing a Console Application, use the following
    // transportWeb.DeliverAsync(mail).Wait();

## <a name="how-to-add-an-attachment"></a>Útmutató: melléklet hozzáadása

Mellékletek is hozzá egy üzenetet a **AddAttachment** metódus meghívása, és adja meg a nevét és elérési útját a csatolni kívánt fájlt.
Több melléklet hívja fel ezt a módszert, amint az egyes fájlok csatolni kívánt is. Az alábbi példa bemutatja, hogy az üzenetekben a melléklet hozzáadása:

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    myMessage.AddAttachment(@"C:\file1.txt");
    
Mellékletek felveheti az adatokat **megjelenítő Értékáram**is. Hívja fel a ugyanazzal a módszerrel, a fenti **AddAttachment**, de az adatok az adatfolyamban átadása tud végezni, és a fájlnév szeretne megjeleníteni, ahogy az üzenetet. Ebben az esetben szüksége lesz a System.IO tár hozzáadása.

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    using (var attachmentFileStream = new FileStream(@"C:\file.txt", FileMode.Open))
    {
        myMessage.AddAttachment(attachmentFileStream, "My Cool File.txt");
    }


## <a name="how-to-use-apps-to-enable-footers-tracking-and-analytics"></a>Útmutató: ahhoz, hogy élőlábak, a nyomon követés és analytics-alkalmazások használata

SendGrid való alkalmazások további e-mail funkciókat tartalmaz. Ezek a beállítások, manuálisan is hozzáadhatók e-mailhez ahhoz, hogy bizonyos funkciókat, például a követés gombra, a Google analytics, az előfizetés nyomon követéséhez, és így tovább. Az összes alkalmazás listájának olvassa el az [Alkalmazás beállításainak][]című témakört.

Alkalmazások **SendGrid** e-mailek végrehajtani a **SendGrid** osztály részeként módszerekkel is alkalmazható.

Az alábbi példák bemutatják az élőláb, és kattintson a követés szűrők:

### <a name="footer"></a>Élőláb

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Add a footer to the message.
    myMessage.EnableFooter("PLAIN TEXT FOOTER", "<p><em>HTML FOOTER</em></p>");

### <a name="click-tracking"></a>A nyomon követés gombra

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Html = "<p><a href=\"http://www.example.com\">Hello World Link!</a></p>";
    myMessage.Text = "Hello World!";
    
    // true indicates that links in plain text portions of the email 
    // should also be overwritten for link tracking purposes. 
    myMessage.EnableClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a>Útmutató: további SendGrid szolgáltatások használata

SendGrid webes API-k és webhooks, amelyek segítségével kihasználhatja az Azure-alkalmazásokból további SendGrid funkciókat kínál. Teljes részletekért olvassa el a [SendGrid API dokumentációt][].

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta a SendGrid használt E-mail szolgáltatás alapjait, kövesse az alábbi hivatkozásokra kattintva további.

*   SendGrid C\# tár repó: [sendgrid-csharp][]
*   SendGrid API-dokumentáció: <https://sendgrid.com/docs>
*   Azure ügyfelek különleges ajánlatot, SendGrid: [https://sendgrid.com](https://sendgrid.com)

  [Következő lépések]: #next-steps
  [What is the SendGrid Email Service?]: #whatis
  [Create a SendGrid Account]: #createaccount
  [Reference the SendGrid .NET Class Library]: #reference
  [How to: Create an Email]: #createemail
  [How to: Send an Email]: #sendemail
  [How to: Add an Attachment]: #addattachment
  [How to: Use Filters to Enable Footers, Tracking, and Analytics]: #usefilters
  [How to: Use Additional SendGrid Services]: #useservices
  
  [special offer]: https://www.sendgrid.com/windowsazure.html
  
  [create-new-project]: ./media/sendgrid-dotnet-how-to-send-email/create_new_project.png
  [select-a-template]: ./media/sendgrid-dotnet-how-to-send-email/select_a_template.png
  [SendGrid-NuGet-package]: ./media/sendgrid-dotnet-how-to-send-email/sendgrid_nuget.png
  [azure_app_settings]: ./media/sendgrid-dotnet-how-to-send-email/app_settings.png
  [sendgrid-csharp]: https://github.com/sendgrid/sendgrid-csharp
  [SMTP vs. Web API]: https://sendgrid.com/docs/Integrate/index.html
  [Alkalmazás beállításainak]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
  [Dokumentáció SendGrid API]: https://sendgrid.com/docs
  
  [felhőalapú e-mail szolgáltatásból]: https://sendgrid.com/email-solutions
  [tranzakció alapú e-mailek kézbesítési]: https://sendgrid.com/transactional-email
 
