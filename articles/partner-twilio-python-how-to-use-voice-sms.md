<properties
    pageTitle="Twilio használatáról a hang-és SMS (PHP) |} Microsoft Azure"
    description="Megtudhatja, hogy miként Telefonhívás kezdeményezése és egy SMS küldése a Twilio API szolgáltatással a Azure. A mintakódok PHP nyelven íródott."
    services=""
    documentationCenter="python"
    authors="devinrader"
    manager="twilio"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python" 
    ms.topic="article"
    ms.date="02/19/2015"
    ms.author="MicrosoftHelp@twilio.com"/>





# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-php"></a>Twilio használatáról a hang-és az SMS-funkciók az PHP
Ez az útmutató bemutatja, hogyan Azure a Twilio API szolgáltatással programozási alapműveletek végrehajtásához. Az érintett esetek telefonhívások lebonyolítása és rövid szolgáltatás (SMS) az üzenet elküldése. Twilio és a hang- és az SMS funkció használata az alkalmazásokban a további tudnivalókért lásd: a [Következő lépésekkel](#NextSteps) szakaszban.

## <a id="WhatIs"></a>Mi az Twilio?
Twilio van – áttekintés business kommunikáció engedélyezése a fejlesztők számára, hogy a hang, a VoIP-hez és a alkalmazásokba üzenetküldés beágyazása a tervek. Azok a különböző helyekre történő, virtualizálása a felhőalapú, globális környezetben, ki azt a Twilio kommunikáció API-platformon keresztül szükséges összes infrastruktúra. Alkalmazások olyan egyszerű, összeállítása és méretezhető. Fogják túlságosan nagyra értékelni rugalmasság fizetési –,-, lépjen a árak, és összekapcsolhatók felhőbeli megbízhatóságát.

**Twilio Voice** lehetővé teszi, hogy az alkalmazások indíthat és fogadhat telefonhívásokat. **Twilio SMS** lehetővé teszi, hogy az alkalmazás szöveges üzenetek küldéséhez és fogadásához. **Twilio ügyfél** lehetővé teszi a VoIP-hívásokhoz bármilyen telefonról, táblagépen vagy böngészőt, és WebRTC támogatja.

## <a id="Pricing"></a>Árak Twilio és különleges ajánlatok

Azure visszaszorítása [különleges ajánlatot] 10 USD Twilio hitel Twilio fiókja frissítéskor. A Twilio hitelképesség alkalmazhatók bármely Twilio használatát (10 USD követel azonos, akkor 1000 SMS-üzenetek küldése és fogadása a legfeljebb 1000 a bejövő hang perc, a telefon számát, és üzenetet vagy -hívás cél helyétől függően). A Twilio hitelképesség beváltása és az első lépések az: [ahoy.twilio.com/azure].

Twilio egy olyan befizetések szolgáltatás. Vannak nem lehet beállítani díjak és bármikor bezárhatja a fiók. További részleteket találhat a [Twilio árak] [twilio_pricing].

## <a id="Concepts"></a>Fogalmak
A Twilio API, amelybe a hang- és az SMS funkció alkalmazások RESTful API. Ügyfél-tárak érhetők el a több nyelv; listájáért lásd a [Twilio API tárak] [twilio_libraries].

A Twilio API főbb részei a következők: Twilio műveletek és Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio műveletek
Az API tesz Twilio műveletek; Ha például a ** &lt;Ismerkedés&gt; ** utasítás arra utasítja Twilio hallhatóan hívás az üzenet kézbesítését.

Az alábbiakban felsoroljuk Twilio műveletek. Megtudhatja, milyen műveletek és funkciók keresztül [Twilio Markup Language dokumentáció] [http://www.twilio.com/docs/api/twiml].

* ** &lt;Tárcsázási&gt;**: a hívó csatlakozik egy másik telefonra.
* ** &lt;Gyűjtése&gt;**: gyűjti össze a telefon billentyűzeten megadott számjeggyel.
* ** &lt;Vonalbontás&gt;**: hívás befejeződik.
* ** &lt;Lejátszása&gt;**: hangfájl lejátszása.
* ** &lt;Szünet&gt;**: megadott számú másodpercig csendes várakozik.
* ** &lt;Rekord&gt;**: a program a hang, a hívó, illetve adja vissza egy, a felvétel tartalmazó fájl URL-CÍMÉT.
* ** &lt;Átirányítás&gt;**: a hívás vagy SMS irányítása át a TwiML egy másik URL-címen.
* ** &lt;Elvetése&gt;**: a Twilio számra a bejövő hívás elutasítja, számlázás nélkül
* ** &lt;Ismerkedés&gt;**: alakítja Szöveg felolvastatása a hívás végrehajtott.
* ** &lt;Sms&gt;**: az SMS-üzenetet küld.

### <a id="TwiML"></a>TwiML
TwiML egy XML-alapú útmutatást a Twilio műveleteken, amely tájékoztatja, hogy miként hívás feldolgozása Twilio vagy SMS alapján.

Példaként a következő TwiML szeretné a szöveg átalakítása **Helló, világ** beszédfelismerés.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Ha az alkalmazás felhívja a Twilio API-t, az API-paraméterek egyik az URL-címet, amely a TwiML választ ad vissza. Fejlesztési célokra Twilio által biztosított URL-címeit is használhatja az alkalmazások által használt TwiML visszajelzést. Akkor is elhelyezheti a TwiML válaszokat létrehozásához a saját URL-címeit, és egy másik, hogy az **TwiMLResponse** objektumot használja.

Twilio műveletek, attribútumok és TwiML kapcsolatos további tudnivalókért lásd: [TwiML] [twiml]. A Twilio API-val kapcsolatos további információkért lásd a [Twilio API] [twilio_api].

## <a id="CreateAccount"></a>Twilio-fiók létrehozása
Ha készen áll arra, hogy Twilio-fiók beszerzése, regisztráció [Twilio próbálja meg]a [try_twilio]. Kiindulás ingyenes fiók, és később frissíti fiókját.

Amikor regisztrál Twilio fiókot, megjelenhet Fiókazonosítót, és a hitelesítési jogkivonat. Mindkét Twilio API-hívásokhoz lesz szükség. Ezzel az illetéktelen hozzáférést a fiókjához megakadályozására biztonsága a hitelesítési jogkivonat. Az account ID és a hitelesítéshez jogkivonat [Twilio fiókja lapján]megtekinthető [twilio_account], a mezők címkézett **biztonsági AZONOSÍTÓK fiókra** , és **AUTH JOGKIVONAT**, illetve.

## <a id="create_app"></a>PHP-alkalmazás létrehozása
Egy PHP-alkalmazást, amely a Twilio szolgáltatást használja, és Azure fut jelentkezés nem különbözik más PHP-alkalmazás, amely a Twilio szolgáltatást használja. Miközben Twilio szolgáltatások többi-alapú és PHP-ből többféleképpen hívható, ez a cikk Twilio szolgáltatások használatához [a GitHub PHP-Twilio]tárral összpontosít[twilio_php]. A Twilio tár PHP használatával kapcsolatos további tudnivalókért lásd: [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].

Összeállítását, és az Azure Twilio vagy PHP-alkalmazások telepítése vonatkozó részletes útmutatást érhetők el, [hogyan készíthet a Azure PHP alkalmazásban a telefonhívás használatával Twilio][howto_phonecall_php].

## <a id="configure_app"></a>Állítsa be az alkalmazás Twilio tárak használata
Az alkalmazás a Twilio tár használandó PHP kétféle módon adhatja meg:

1. Töltse le a Twilio tárat a GitHub PHP ([https://github.com/twilio/twilio-php][twilio_php]) és az alkalmazás hozzáadása a **szolgáltatások** címtárban.

    – VAGY –

2. Telepítse a Twilio tár PHP KÖRTEFÁK csomag. Az alábbi parancsok telepíthető:

        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

Miután telepítette a Twilio tár PHP, majd adhat hozzá egy **require_once** utasítást a hivatkozást a tár PHP-fájlok tetején:

        require_once 'Services/Twilio.php';

További tudnivalókért lásd: [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].

## <a id="howto_make_call"></a>Útmutató: a kimenő hívás
A következő mutatja, hogy a **Services_Twilio** osztály használatával kimenő hívást. Ez a kód is használja az Twilio által biztosított webhely a Twilio Markup Language (TwiML) választ. A **Kezdő** és **Záró** telefonszámok értékének helyettesítő, és győződjön meg róla, a kód futtatása előtt Twilio fiókjának ellenőriznie kell **a** telefonszámát.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // The number of the phone initiating the the call.
    // (Must be previously validated with Twilio.)
    $from_number = "NNNNNNNNNNN";

    // The number of the phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use the Twilio-provided site for the TwiML response.
    $url = "http://twimlets.com/message";

    // The phone message text.
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make the call.
    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

Említett, a kód használja Twilio által biztosított webhely a TwiML választ. Ehelyett a saját webhely segítségével adja meg a TwiML válasz; További információért megtudhatja, [hogy miként TwiML válaszok adja meg a saját webhelyről](#howto_provide_twiml_responses).


- **Megjegyzés**: az SSL-tanúsítványt az adatérvényesítési hibák elhárításához lásd [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]


## <a id="howto_send_sms"></a>Útmutató: az SMS küldése
A következő mutatja a **Services_Twilio** osztály használatával SMS küldése. **A** szám Twilio próba fiókok SMS-üzeneteket küldhet által biztosított. A kód futtatása előtt Twilio fiókjának ellenőrizni kell **a számát** .

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send the SMS message.
    try
    {
        $client->account->sms_messages->create($from_number, $to_number, $message);
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

## <a id="howto_provide_twiml_responses"></a>Útmutató: TwiML visszajelzést, kattintson a saját webhelyen
Amikor az alkalmazás kezdeményez egy hívás a Twilio API-hoz, Twilio kell visszaadnia TwiML választ URL-cím a kérelem küld. A fenti példában a Twilio által biztosított URL-cím [http://twimlets.com/message][twimlet_message_url]. (Miközben TwiML készült Twilio szerint, megtekintheti az informatikai a böngészőben. Kattintson például [http://twimlets.com/message] [ twimlet_message_url] tekintheti meg egy üres `<Response>` elem; másik példaként, kattintson a [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] megtekintéséhez egy `<Response>` elem, amely tartalmazza a `<Say>` elem.)

Használna Twilio által biztosított URL-CÍMÉT, helyett a saját webhely, amely visszaadja a HTTP-válaszok hozhat létre. A webhely hozhat létre bármely nyelvre adja vissza az XML-válasz; Ez a témakör tartalma feltételezi, hogy fogja használni, akkor PHP a TwiML létrehozásához.

A következő PHP-lapot, amely szerint **Helló, világ** hívás TwiML választ eredményez.

    <?php
        header("content-type: text/xml");
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>
        <Say>Hello world.</Say>
    </Response>

Amint a fenti példában látható, a TwiML válasz a egyszerűen egy XML-dokumentum. PHP Twilio médiatárban TwiML hoz létre, az osztályok. Az alábbi példában a megfelelő választ létrehozza a fent látható, de használja a **szolgáltatások\_Twilio\_Twiml** osztály PHP-a Twilio tárban:

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

TwiML kapcsolatos további tudnivalókért lásd: [https://www.twilio.com/docs/api/twiml][twiml_reference].

Ha befejezte a TwiML visszajelzést beállított PHP lapra, használja a PHP-lap URL-CÍMÉT az URL-cím átadott a `Services_Twilio->account->calls->create` módot. Például **MyTwiML** rendszerbe állított nevű webalkalmazás esetén az Azure üzemeltetett szolgáltatást, és a PHP lap neve **mytwiml.php**, az URL-címet is átadandó **Services_Twilio -> fiók -> hívások -> létrehozása** az alábbi példában látható módon:

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // The phone message text.
    $message = "Hello world.";

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

További információt a Twilio használata PHP az Azure megtudhatja, [hogy miként, hogy a Azure PHP alkalmazásban a telefonhívás használatával Twilio][howto_phonecall_php].

## <a id="AdditionalServices"></a>Útmutató: további Twilio szolgáltatások használata
Mellett az itt ismertetett példák Twilio a webes API-hoz, amelyek segítségével kihasználhatja az Azure-alkalmazásokból további Twilio funkciókat kínál. Teljes részletekért olvassa el a [Twilio API dokumentáció] [twilio_api_documentation].

## <a id="NextSteps"></a>Következő lépések
Most, hogy a Twilio szolgáltatás alapvető van megtanulta, kövesse ezeket a hivatkozásokat, ha többet szeretne:

* [Twilio biztonsági irányelvek] [twilio_security_guidelines]
* [Twilio útmutató segédvonalak és példa kódot.] [twilio_howtos]
* [Twilio quickstart útmutató oktatóanyagok][twilio_quickstarts]
* [A GitHub Twilio] [twilio_on_github]
* [Beszélgetés a Twilio támogatás] [twilio_support]

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-php/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html

[howto_phonecall_php]: http://windowsazure.com/documentation/articles/partner-twilio-php-make-phone-call
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: http://www.twilio.com/pricing

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/api
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
