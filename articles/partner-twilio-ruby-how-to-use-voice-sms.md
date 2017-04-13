<properties 
    pageTitle="Twilio használatáról a hang-és SMS (fonetikus) |} Microsoft Azure" 
    description="Megtudhatja, hogy miként Telefonhívás kezdeményezése és egy SMS küldése a Twilio API szolgáltatással a Azure. A mintakódok fonetikus nyelven íródott." 
    services="" 
    documentationCenter="ruby" 
    authors="devinrader" 
    manager="twilio" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ruby" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="MicrosoftHelp@twilio.com"/>





# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-ruby"></a>Twilio használatáról a hang-és az SMS-funkciók az fonetikus
Ez az útmutató bemutatja, hogyan Azure a Twilio API szolgáltatással programozási alapműveletek végrehajtásához. Az érintett esetek telefonhívások lebonyolítása és rövid szolgáltatás (SMS) az üzenet elküldése. Twilio és a hang- és az SMS funkció használata az alkalmazásokban a további tudnivalókért lásd: a [Következő lépésekkel](#NextSteps) szakaszban.

## <a id="WhatIs"></a>Mi az Twilio?
Twilio egy telefonos webszolgáltatás API-val, amely lehetővé teszi a meglévő webes nyelvek és szakértelmek használata a hang- és az SMS-alkalmazások létrehozásához. Twilio egy külső szolgáltatásnak (nem olyan Azure szolgáltatás, és nem Microsoft-termékek).

**Twilio Voice** lehetővé teszi, hogy az alkalmazások indíthat és fogadhat telefonhívásokat. **Twilio SMS** lehetővé teszi, hogy az alkalmazások és az SMS-üzeneteket fogadni. **Twilio ügyfél** lehetővé teszi, hogy az alkalmazások használata Internet meglévő kapcsolatok, például a mobil kapcsolatok hangposta kommunikációhoz.

## <a id="Pricing"></a>Árak Twilio és különleges ajánlatok
Információ a Twilio árak érhető el: [Twilio árak] [twilio_pricing]. Azure egy [különleges ajánlatot]visszaszorítása[special_offer]: 1000 szövegek ingyenes hitelképesség vagy a bejövő perc 1000. Regisztrálás az ezt az ajánlatot, vagy további információk, kérjük, keresse fel [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Fogalmak
A Twilio API, amelybe a hang- és az SMS funkció alkalmazások RESTful API. Ügyfél-tárak érhetők el a több nyelv; listájáért lásd a [Twilio API tárak] [twilio_libraries].

### <a id="TwiML"></a>TwiML
TwiML egy XML-alapú utasításokat, amely tájékoztatja, hogy miként hívás feldolgozása Twilio vagy SMS.

Példaként a következő TwiML szeretné a szöveg átalakítása **Helló, világ** beszédfelismerés.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Az összes TwiML dokumentumoknál `<Response>` a legfelső szintű elemként. Itt használatával Twilio műveletek megadása az alkalmazás viselkedését.

### <a id="Verbs"></a>TwiML műveletek
Twilio műveletek, amelyek alapján a Twilio Mi a **teendő**XML-címkéket. Ha például a ** &lt;Ismerkedés&gt; ** utasítás arra utasítja Twilio hallhatóan hívás az üzenet kézbesítését. 

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

Twilio műveletek, attribútumok és TwiML kapcsolatos további tudnivalókért lásd: [TwiML] [twiml]. A Twilio API-val kapcsolatos további információkért lásd a [Twilio API] [twilio_api].

## <a id="CreateAccount"></a>Twilio-fiók létrehozása
Amikor készen áll arra, hogy Twilio-fiók beszerzése, jelentkezzen a [Próbálja Twilio] [try_twilio]. Kiindulás ingyenes fiók, és később frissíti fiókját.

Amikor regisztrál Twilio fiókot, vissza az alkalmazás ingyenes telefonszámot. Biztonsági AZONOSÍTÓK fiók és egy auth jogkivonat is jelenik. Mindkét Twilio API-hívásokhoz lesz szükség. Ezzel az illetéktelen hozzáférést a fiókjához megakadályozására biztonsága a hitelesítési jogkivonat. A fiók biztonsági AZONOSÍTÓK és auth token [Twilio fiókja lapján]megtekinthető[twilio_account], a mezők címkézett **biztonsági AZONOSÍTÓK fiókra** , és **AUTH JOGKIVONAT**, illetve.

### <a id="VerifyPhoneNumbers"></a>Telefonszámok ellenőrzése
A telefonszám mellett az Twilio, képlettel is ellenőrizheti, hogy Ön vezérlő (azaz a mobilról vagy otthoni telefonszámát) számok használata az alkalmazás. 

Telefonszám ellenőrzése a további tudnivalókért lásd [Kezelése számok] [verify_phone].

## <a id="create_app"></a>Fonetikus-alkalmazás létrehozása
A fonetikus alkalmazás, amely a Twilio szolgáltatást használja, és Azure fut jelentkezés nem különbözik más fonetikus alkalmazás, amely a Twilio szolgáltatást használja. Miközben Twilio szolgáltatások RESTful és a fonetikus többféleképpen is nevezik, ez a cikk Twilio szolgáltatások használata [Twilio segítő dokumentumtár a fonetikus]összpontosít[twilio_ruby].

Először [egy új Azure Linux virtuális beállítási] [ azure_vm_setup] használni kívánt a szolgáltató az új fonetikus webes alkalmazáshoz. A ugorja át érintő sínek alkalmazást, csak lehet beállítani a virtuális létrehozását. Ellenőrizze, hogy zárólap hozzon létre egy külső port 80 és 5000 egy belső portot.

Az alábbi példában, akkor fogja használni [Sinatra][sinatra], fonetikus rendkívül egyszerű webes keretét. De biztosan használhatja a Twilio segítő tárat a fonetikus bármely más webes keretrendszer, például a fonetikus sínen.

Az új virtuális be SSH és az új alkalmazás könyvtár létrehozása. Belüli könyvtárra hozzon létre egy Gemfile nevű fájlt, és másolja azt a következő kódot:

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

A Futtatás parancsot a `bundle install`. Ez a fenti függőségeket telepíti. Ezután hozzon létre egy nevű fájlt `web.rb`. Ez lesz, ahol a kódot a webalkalmazás él. Illessze be a következő kódot azt:

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

Ezen a ponton látnia kell a Futtatás a parancs `ruby web.rb -p 5000`. Ez lesz léptetőnyíl felfelé port 5000 kis webkiszolgálóra. Látnia kell tallózással keresse meg a böngészőben az alkalmazás megtalálhatók az URL-cím meg lehet beállítani az Azure virtuális. Érheti el a web app a böngészőben, miután készen áll a Twilio alkalmazás felépítésének megkezdéséhez.

## <a id="configure_app"></a>Az alkalmazás használatához Twilio konfigurálása
A web App alkalmazásban a Twilio könyvtár használata frissítésével adhatja a `Gemfile` ebben a sorban felvenni:

    gem 'twilio-ruby'

A parancssorból futtatható `bundle install`. Most már nyissa meg a `web.rb` , beleértve a felső sor:

    require 'twilio-ruby'

Amikor most már készen a Twilio segítő tár használandó fonetikus a webalkalmazásban.

## <a id="howto_make_call"></a>Útmutató: a kimenő hívás
A következő mutatja, hogy a kimenő hívást. Alapfogalmak közé tartozik, használja az Twilio segítő tárat fonetikus REST API-hívásokhoz, és a megjeleníthető TwiML. A **Kezdő** és **Záró** telefonszámok értékének helyettesítő, és győződjön meg róla, a kód futtatása előtt Twilio fiókjának ellenőriznie kell **a** telefonszámát.

Ezt a funkciót hozzáadása `web.md`:

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # The number of the phone initiating the the call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";

    # The number of the phone receiving call.
    to = "NNNNNNNNNNN";

    # Use the Twilio-provided site for the TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";
      
    get '/make_call' do
      # Create the call client.
      client = Twilio::REST::Client.new(sid, token);
      
      # Make the call
      client.account.calls.create(to: to, from: from, url: url)
    end

    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end
    
Ha Ön Megnyitás felfelé `http://yourdomain.cloudapp.net/make_call` a böngészőben, amely elindítja a hívást a Twilio API-hoz a telefonhívás. Az első két paraméterei `client.account.calls.create` magától értetődő meglehetősen: a hívás a szám nem `from` , és a számot a hívás `to`. 

A harmadik paraméter (`url`) Twilio kérő útmutatást szeretne kapni a Mi a teendő, miután a hívás létrejött URL-címe. Ebben az esetben azt beállítási URL-cím (`http://yourdomain.cloudapp.net`), amely egy egyszerű TwiML dokumentum adja eredményül, és használja a `<Say>` igei néhány szövegfelolvasás és fel, hogy a "Helló, Majom" szeretné a hívást fogadó személy számára.

## <a id="howto_recieve_sms"></a>Útmutató: fogadás SMS-üzenet
Az előző példában azt telefon **kimenő** hívást kezdeményezni. Ez alkalommal, amikor, a telefonszámot, amelyen Twilio rendelte us során előfizetési használata egy **bejövő** SMS-üzenetet feldolgozása.

Első, a naplófájl az-az [Irányítópult Twilio][twilio_account]. A felső navigációs "Számok" kattintson, és válassza a megadott voltak Twilio számára. Két URL-címeit, konfigurálható tulajdonsággal talál. Hangposta kérelem URL-címe és SMS kérése az URL-CÍMÉT. Ezek az URL-címeit, amely Twilio hív meg, valahányszor hívni lett vagy SMS a szolgáltatás elküldi a számot. Az URL-címeit is nevezik "webes horgok".

Azt szeretné bejövő SMS-üzenetek feldolgozása, így érdemes frissítése az URL-címe `http://yourdomain.cloudapp.net/sms_url`. Jóváhagyást, és kattintson a módosítások mentése az oldal alján. Ezután ismét `web.rb` kezelésére, ez az alkalmazás vegyük program:

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for the ping! Twilio and Azure rock!</Message>
       </Response>"
    end

A módosítások végrehajtása után győződjön meg róla, hogy indítsa újra a web App alkalmazásban. Most vegyük ki a telefon, és SMS küldése a Twilio szám. Azonnal szerezheti be az SMS-választ, amely szerint az "Hey, a ping Köszönöm! Twilio és Azure rock! ".

## <a id="additional_services"></a>Útmutató: további Twilio szolgáltatások használata
Mellett az itt ismertetett példák Twilio a webes API-hoz, amelyek segítségével kihasználhatja az Azure-alkalmazásokból további Twilio funkciókat kínál. Teljes részletekért olvassa el a [Twilio API dokumentáció] [twilio_api_documentation].

### <a id="NextSteps"></a>Következő lépések
Most, hogy megtanulta a Twilio szolgáltatás alapjait, kövesse az alábbi hivatkozásokra kattintva további:

* [Twilio biztonsági irányelvek] [twilio_security_guidelines]
* [Twilio HowTos és példa-kód] [twilio_howtos]
* [Twilio quickstart útmutató oktatóanyagok][twilio_quickstarts] 
* [A GitHub Twilio] [twilio_on_github]
* [Beszélgetés a Twilio támogatás] [twilio_support]

[twilio_ruby]: https://www.twilio.com/docs/ruby/install





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
[sinatra]: http://www.sinatrarb.com/
[azure_vm_setup]: http://www.windowsazure.com/develop/ruby/tutorials/web-app-with-linux-vm/
