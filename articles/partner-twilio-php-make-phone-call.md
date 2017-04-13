<properties
    pageTitle="Telefonhívás kezdeményezése a Twilio (PHP) hogyan |} Microsoft Azure"
    description="Megtudhatja, hogy miként Telefonhívás kezdeményezése és egy SMS küldése a Twilio API szolgáltatással a Azure. A mintákat PHP alkalmazáshoz."
    documentationCenter="php"
    services=""
    authors="devinrader"
    manager="twilio"
    editor="mollybos"/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="11/25/2014"
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-make-a-phone-call-using-twilio-in-a-php-application-on-azure"></a>Hogyan Twilio használata a PHP-alkalmazások Azure a Telefonhívás kezdeményezése

A következő példa bemutatja, hogyan használhatja Twilio hívással weblapról PHP Azure-ban is. Az eredményül kapott alkalmazás a felhasználó a telefonhívás értékek, kérdezze meg, ahogy az alábbi képernyőfelvételen.

![Azure hívás űrlap Twilio és PHP használatával][twilio_php]

Az alábbi módon használja a ebben a témakörben kell:

1. Szerezheti be egy Twilio fiókkal és a hitelesítési jogkivonat. Az első lépések Twilio, a [http://www.twilio.com/pricing]árak felmérése[twilio_pricing]. Iratkozzon fel a próba-fiók a [https://www.twilio.com/try-twilio][try_twilio]. Twilio által biztosított API-val kapcsolatos további tudnivalókért lásd: [http://www.twilio.com/api][twilio_api].
2. Szerezze be a [Twilio tár PHP-](https://github.com/twilio/twilio-php) vagy KÖRTEFÁK csomag telepítheti. További tudnivalókért lásd: a [Fontos fájlban](https://github.com/twilio/twilio-php/blob/master/README.md).
3. Telepítse az Azure SDK PHP. A SDK és telepítéséhez áttekintése látható [a Azure SDK PHP beállítása][setup_php_sdk].

## <a name="create-a-web-form-for-making-a-call"></a>Telefonhívás kezdeményezése webes űrlap létrehozása

A következő HTML-kód jeleníti meg, hogyan hozhat létre az weblapon (**callform.html**), amely beolvassa a felhasználói adatok, a Telefonhívás kezdeményezése:

    <html>
    <head>
        <title>Automated call form</title>
    </head>
    <body>
    <h1>Automated Call Form</h1>
    <p>Fill in all fields and click <b>Make this call</b>.</p>
    <form action="makecall.php" method="post">
    <table>
        <tr>
            <td>To:</td>
            <td><input type="text" size=50 name="callTo" value=""></td>
        </tr>
        <tr>
            <td>From:</td>
            <td><input type="text" size=50 name="callFrom" value=""></td>
        </tr>
        <tr>
            <td>Call message:</td>
            <td><input type="text" size=100 name="callText" value="Hello. This is the call text. Good bye." /></td>
        </tr>
        <tr>
            <td colspan=2><input type="submit" value="Make this call"></td>
        </tr>
    </table>
    </form>
    <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>A hívás kód létrehozása
A következő kódot jeleníti meg, hogyan hozhat létre az weblapon (**makecall.php**) nevezzük, amikor a felhasználó elküldi az űrlapot, **callform.html**által megjelenített. A kód, alább látható módon a hívás üzenetet hoz létre, és hozza létre a hívást. (Használata helyett a helyőrző értékeket **$sid** és **$token** az alábbi kód rendelt token Twilio fiók és a hitelesítéshez.)

    <html>
    <head><title>Making call...</title></head>
    <body>
    <p>Your call is being made.</p>

    <?php
    require_once 'Services/Twilio.php';

    $sid = "your_account_sid";
    $token = "your_authentication_token";

    $from_number = $_POST['callFrom']; // Calls must be made from a registered Twilio number.
    $to_number = $_POST['callTo'];
    $message = $_POST['callText'];

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    $call = $client->account->calls->create(
        $from_number,
        $to_number,
        'http://twimlets.com/message?Message='.urlencode($message)
    );

    echo "Call status: ".$call->status."<br />";
    echo "URI resource: ".$call->uri."<br />";
    ?>
    </body>
    </html>

Nemcsak a hívást, **makecall.php** jeleníti meg a hívás néhányuk (például az alábbi képernyőképen látható). Hívás metaadatok további információkért lásd a [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].

![Azure hívás válasz Twilio és PHP használatával][twilio_php_response]

## <a name="run-the-application"></a>Futtassa az alkalmazást
A következő lépésként az Azure webhelyek alkalmazás telepítéséhez. Az alábbi cikkekben adatait a webhely létrehozásáról és üzembe helyezése a kód mely számjegy, FTP vagy WebMatrix, (de nem az összes információ minden egyes cikkben nem megfelelő) tartalma:

* [Webhelyeket hozhat létre PHP-MySQL Azure, és üzembe, mely számjegy használatával][website-git]
* [Hozzon létre egy PHP-MySQL Azure webhelyet, és üzembe, FTP használatával][website-ftp]

## <a name="next-steps"></a>Következő lépések
Ez a kód kapott mutatja, hogy Twilio használata a Azure PHP alapvető funkcióit. Mielőtt Azure gyártási, érdemes több hibakezelés és más funkciók. Példa:

* Webes űrlap helyett segítségével tároló Azure BLOB- vagy SQL-adatbázis tárolhatja telefonszámok, és hívja fel a szöveget. Tároló Azure BLOB PHP használatáról további tudnivalókért lásd [Azure tárterületet használ PHP-alkalmazásokkal][howto_blob_storage_php]. SQL-adatbázis PHP használatával kapcsolatos további tudnivalókért lásd [SQL-adatbázis használata PHP-alkalmazásokkal][howto_sql_azure_php].
* A **makecall.php** kódot használ Twilio által biztosított URL-cím ([http://twimlets.com/message][twimlet_message_url]), amely arról értesíti a hívás folytatása módjáról az Twilio Twilio Markup Language (TwiML) választ. A függvény által visszaadott TwiML tartalmazhat például egy `<Say>` igei, amely a szöveg kiejtését, hogy a hívott fél készülékén. Twilio által biztosított URL-címe helyett a saját szolgáltatás Twilio's összehívásra; válasz sikerült összeállítása További információért megtudhatja, [hogy miként Twilio használata a hang-és az SMS-funkciók az PHP][howto_twilio_voice_sms_php]. További információt a TwiML [http://www.twilio.com/docs/api/twiml]találhatók[twiml], és további információt `<Say>` és más Twilio műveletek a következő helyen található [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Olvassa el a Twilio biztonsági irányelvek a [https://www.twilio.com/docs/security][twilio_docs_security].

Twilio kapcsolatos további tudnivalókért lásd: [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Lásd még:
* [Twilio használatáról a hang-és az SMS-funkciók az PHP](partner-twilio-php-how-to-use-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[setup_php_sdk]: http://azurephp.interoperabilitybridges.com/articles/setup-the-windows-azure-sdk-for-php
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[build_php_azure_app]: http://azurephp.interoperabilitybridges.com/articles/build-and-deploy-a-windows-azure-php-application
[howto_twilio_voice_sms_php]: partner-twilio-php-how-to-use-voice-sms.md
[howto_blob_storage_php]: http://azure.microsoft.com/documentation/articles/storage-php-how-to-use-blobs/
[howto_sql_azure_php]: http://azure.microsoft.com/documentation/articles/sql-database-php-how-to-use/
[twilio_call_properties]: https://www.twilio.com/docs/api/rest/call#instance-properties
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_php]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPCallForm.jpg
[twilio_php_response]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPMakeCall.jpg
[website-git]: ./web-sites/web-sites-php-mysql-deploy-use-git.md
[website-ftp]: ./web-sites/web-sites-php-mysql-deploy-use-ftp.md
[twilio_php_github]: https://github.com/twilio/twilio-php
