<properties 
    pageTitle="Twilio használatáról a hang-és SMS (Java) |} Microsoft Azure" 
    description="Megtudhatja, hogy miként Telefonhívás kezdeményezése és egy SMS küldése a Twilio API szolgáltatással a Azure. Java nyelven írt mintakódok." 
    services="" 
    documentationCenter="java" 
    authors="devinrader" 
    manager="twilio" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-java"></a>Twilio használatáról a hang-és az SMS-funkciók az Java

Ez az útmutató bemutatja, hogyan Azure a Twilio API szolgáltatással programozási alapműveletek végrehajtásához. Az érintett esetek telefonhívások lebonyolítása és rövid szolgáltatás (SMS) az üzenet elküldése. Twilio és a hang- és az SMS funkció használata az alkalmazásokban a további tudnivalókért lásd: a [Következő lépésekkel](#NextSteps) szakaszban.

## <a id="WhatIs"></a>Mi az Twilio?
Twilio egy telefonos webszolgáltatás API-val, amely lehetővé teszi a meglévő webes nyelvek és szakértelmek használata a hang- és az SMS-alkalmazások létrehozásához. Twilio egy külső szolgáltatásnak (nem olyan Azure szolgáltatás, és nem Microsoft-termékek).

**Twilio Voice** lehetővé teszi, hogy az alkalmazások indíthat és fogadhat telefonhívásokat. **Twilio SMS** lehetővé teszi, hogy az alkalmazások és az SMS-üzeneteket fogadni. **Twilio ügyfél** lehetővé teszi, hogy az alkalmazások használata Internet meglévő kapcsolatok, például a mobil kapcsolatok hangposta kommunikációhoz.

## <a id="Pricing"></a>Árak Twilio és különleges ajánlatok
Információ a Twilio árak érhető el: [Twilio árak] [twilio_pricing]. Azure egy [különleges ajánlatot]visszaszorítása[special_offer]: 1000 szövegek ingyenes hitelképesség vagy a bejövő perc 1000. Regisztrálás az ezt az ajánlatot, vagy további információk, kérjük, keresse fel [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Fogalmak
A Twilio API, amelybe a hang- és az SMS funkció alkalmazások RESTful API. Ügyfél-tárak érhetők el a több nyelv; listájáért lásd a [Twilio API tárak] [twilio_libraries].

A Twilio API főbb részei a következők: Twilio műveletek és Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio műveletek
Az API tesz Twilio műveletek; Ha például a ** &lt;Ismerkedés&gt; ** utasítás arra utasítja Twilio hallhatóan hívás az üzenet kézbesítését. 

Az alábbiakban felsoroljuk Twilio műveletek.

* ** &lt;Tárcsázási&gt;**: a hívó csatlakozik egy másik telefonra.
* ** &lt;Gyűjtése&gt;**: gyűjti össze a telefon billentyűzeten megadott számjeggyel.
* ** &lt;Vonalbontás&gt;**: hívás befejeződik.
* ** &lt;Lejátszása&gt;**: hangfájl lejátszása.
* ** &lt;Szünet&gt;**: megadott számú másodpercig csendes várakozik.
* ** &lt;Rekord&gt;**: a hívó hangposta rögzíti, és adja vissza egy, a felvétel tartalmazó fájl URL-CÍMÉT.
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
Amikor készen áll arra, hogy Twilio-fiók beszerzése, jelentkezzen a [Próbálja Twilio] [try_twilio]. Kiindulás ingyenes fiók, és később frissíti fiókját.

Amikor regisztrál Twilio fiókot, kapni fog Fiókazonosítót, és a hitelesítési jogkivonat. Mindkét Twilio API-hívásokhoz lesz szükség. Ezzel az illetéktelen hozzáférést a fiókjához megakadályozására biztonsága a hitelesítési jogkivonat. Az account ID és a hitelesítéshez jogkivonat [Twilio fiókja lapján]megtekinthető [twilio_account], a mezők címkézett **biztonsági AZONOSÍTÓK fiókra** , és **AUTH JOGKIVONAT**, illetve.

## <a id="create_app"></a>Java-alkalmazás létrehozása
1. Szerezze be, a Twilio JAR, és vegye fel a Java összeállítás elérési utat és a háborús telepítési összeállítás. A [https://github.com/twilio/twilio-java][twilio_java], töltse le a GitHub források és hozzon létre saját üveg, vagy töltse le egy beépített üveg (vagy anélkül függőségek).
2. Győződjön meg róla, a JDK **cacerts** keystore tartalmazza a Equifax biztonságos hitelesítésszolgáltató tanúsítványát MD5 ujjlenyomat 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (időértékét 35:DE:F4:CF pedig a SHA1 ujjlenyomat D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Ez a [https://api.twilio.com] tanúsítvány hitelesítésszolgáltató tanúsítványát[ twilio_api_service] szolgáltatása, amelynek Twilio API-khoz használatakor nevezik. A JDK **cacerts** biztosítása információt keystore a megfelelő hitelesítésszolgáltató tanúsítványát tartalmaz, [a Java hitelesítésszolgáltató tanúsítvány áruház tanúsítvány felvétele]című témakör tartalmaz[add_ca_cert].

Részletes útmutatást találhat a Twilio ügyfél tár Java érhetők el [, hogy a telefonhívás használatával Twilio az Azure Java-alkalmazások hogyan][howto_phonecall_java].

## <a id="configure_app"></a>Állítsa be az alkalmazás Twilio tárak használata
A kód kimutatások **importálása** a forrásfájlokat Twilio csomagok vagy az alkalmazásban használni kívánt osztályok tetején is hozzáadhat. 

Java forrásfájlokat:

    import com.twilio.*;
    import com.twilio.sdk.*;
    import com.twilio.sdk.resource.factory.*;
    import com.twilio.sdk.resource.instance.*;

Java kiszolgálólapok (JSP): forrásfájlokat:

    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
Attól függően, hogy melyik Twilio csomagok vagy az osztályok használni kívánt az **Importálás** kimutatások eltérő lehet.

## <a id="howto_make_call"></a>Útmutató: a kimenő hívás
A következő mutatja, hogy a **CallFactory** osztály használatával kimenő hívást. Ez a kód is használja az Twilio által biztosított webhely a Twilio Markup Language (TwiML) választ. A **Kezdő** és **Záró** telefonszámok értékének helyettesítő, és győződjön meg róla, a kód futtatása előtt Twilio fiókjának ellenőriznie kell **a** telefonszámát.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the CallFactory.
    Account account = client.getAccount();

    // Use the Twilio-provided site for the TwiML response.
    String Url="http://twimlets.com/message";
    Url = Url + "?Message%5B0%5D=Hello%20World";

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN"); // Use your own value for the second parameter.
    params.put("To", "NNNNNNNNNN");   // Use your own value for the second parameter.
    params.put("Url", Url);

    // Create an instance of the CallFactory class.
    CallFactory callFactory = account.getCallFactory();

    // Make the call.
    Call call = callFactory.create(params);

A paramétereket a **CallFactory.create** módszerrel átadott további információkért lásd a [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].

Említett, a kód használja Twilio által biztosított webhely a TwiML választ. Ehelyett a saját webhely segítségével adja meg a TwiML válasz; További információért megtudhatja, [hogy miként TwiML a válaszok adja meg a Azure Java-alkalmazások](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Útmutató: az SMS küldése
A következő mutatja a **SmsFactory** osztály használatával SMS küldése. Az **a** szám, **4155992671**, az SMS-üzeneteket küldhet próba fiókok Twilio által biztosított. A kód futtatása előtt Twilio fiókjának ellenőrizni kell **a számát** .

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the SmsFactory.
    Account account = client.getAccount();

    // Send an SMS message.
    MessageFactory messageFactory = account.getMessageFactory();
    
    List<NameValuePair> params = new ArrayList<NameValuePair>();
    params.add(new BasicNameValuePair("To", "+14159352345")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("From", "+14158141829")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("Body", "Where's Wallace?"));
    
    Message sms = messageFactory.create(params);
        
A paramétereket a **SmsFactory.create** módszerrel átadott további információkért lásd a [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].

## <a id="howto_provide_twiml_responses"></a>Útmutató: TwiML visszajelzést, kattintson a saját webhelyen
Amikor az alkalmazás kezdeményez egy hívás a Twilio API-hoz, például a **CallFactory.create** módszer keresztül Twilio lesz a kérelmet küldhet kell visszaadnia TwiML választ URL-cím. A fenti példában a Twilio által biztosított URL-cím [http://twimlets.com/message][twimlet_message_url]. (TwiML készült webes szolgáltatásai, miközben megtekintheti a TwiML a böngészőben. Kattintson például [http://twimlets.com/message] [ twimlet_message_url] tekintheti meg egy üres ** &lt;válasz&gt; ** elem; másik példaként, kattintson a [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] megtekintéséhez egy ** &lt;válasz&gt; ** elem, amely tartalmazza a ** &lt;Ismerkedés&gt; ** elem.)

Használna Twilio által biztosított URL-CÍMÉT, helyett a saját URL-CÍMÉT a webhely, amely visszaadja a HTTP-válaszok hozhat létre. A webhely hozhat létre bármely nyelvre, amely visszaadja a HTTP-válaszok; Ez a témakör tartalma feltételezi, hogy fogja az URL-címet az weblapon JSP szolgáltatója.

A következő JSP lapot, amely szerint **Helló, világ** hívás TwiML választ eredményez.

    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello World</Say>
    </Response>

A megjelenő JSP lap szöveghez szerint, több szünet, és a Twilio API-verzió és az Azure szerepkör neve az információ olvasható TwiML választ eredményez.


    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello from Azure</Say>
        <Pause></Pause>
        <Say>The Twilio API version is <%= request.getParameter("ApiVersion") %>.</Say>
        <Say>The Azure role name is <%= System.getenv("RoleName") %>.</Say>
        <Pause></Pause>
        <Say>Good bye.</Say>
    </Response>

A **ApiVersion** paraméter Twilio hangposta-összehívásokban (nem SMS-kérelmek) érhető el. A rendelkezésre álló kérelem paraméterek Twilio hang-és az SMS-kérelmek talál további <https://www.twilio.com/docs/api/twiml/twilio_request> és <https://www.twilio.com/docs/api/twiml/sms/twilio_request>, illetve. A **RoleName** környezeti változó érhető el az Azure telepítés részeként. (Vegye fel az egyéni környezeti változók, hogy azok is felvételre **System.getenv**a szeretné, ha című környezet változók [egyéb szerepkör konfigurációs]beállításait[misc_role_config_settings].)

Ha befejezte a TwiML visszajelzést beállított JSP lapra, használja a JSP lap URL-CÍMÉT az URL-címet a **CallFactory.create** módszer átadott. Például nevű webalkalmazás esetén az Azure rendszerbe állított MyTwiML üzemeltetett szolgáltatást, és a JSP lap neve mytwiml.jsp, az URL-címet is átadandó **CallFactory.create** látható az alábbi módon:

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN");
    params.put("To", "NNNNNNNNNN");
    params.put("Url", "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    CallFactory callFactory = account.getCallFactory();
    Call call = callFactory.create(params);

Másik lehetőségként a TwiML válaszol a **TwiMLResponse** osztály, amely a **com.twilio.sdk.verbs** csomagban érhető található.

További információt a Twilio használata Java Azure megtudhatja, [hogy miként, hogy az Azure Java-alkalmazások a telefonhívás használatával Twilio][howto_phonecall_java].

## <a id="AdditionalServices"></a>Útmutató: további Twilio szolgáltatások használata
Mellett az itt ismertetett példák Twilio a webes API-hoz, amelyek segítségével kihasználhatja az Azure-alkalmazásokból további Twilio funkciókat kínál. Teljes részletekért olvassa el a [Twilio API dokumentáció] [twilio_api_documentation].

## <a id="NextSteps"></a>Következő lépések
Most, hogy megtanulta a Twilio szolgáltatás alapjait, kövesse az alábbi hivatkozásokra kattintva további:

* [Twilio biztonsági irányelvek] [twilio_security_guidelines]
* [Twilio útmutató és példa-kód] [twilio_howtos]
* [Twilio quickstart útmutató oktatóanyagok][twilio_quickstarts] 
* [A GitHub Twilio] [twilio_on_github]
* [Beszélgetés a Twilio támogatás] [twilio_support]

[twilio_java]: https://github.com/twilio/twilio-java
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[howto_phonecall_java]: partner-twilio-java-phone-call-example.md
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls
[twilio_rest_sending_sms]: http://www.twilio.com/docs/api/rest/sending-sms
[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
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
