<properties 
    pageTitle="Twilio használata a hang, a VoIP-hez és a SMS üzenetek Azure-ban" 
    description="Megtudhatja, hogy miként Telefonhívás kezdeményezése és egy SMS küldése a Twilio API szolgáltatással a Azure. A mintakódok Node.js nyelven íródott." 
    services="" 
    documentationCenter="nodejs" 
    authors="devinrader" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="wpickett"/>


# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a>Twilio használata a hang, a VoIP-hez és a SMS üzenetek Azure-ban

Ez az útmutató bemutatja, hogyan lehet kommunikálni Twilio és node.js a Azure-alkalmazások létrehozásához.

<a id="whatis"/>
## <a name="what-is-twilio"></a>Mi az Twilio?

Twilio egy platform, vagyis API-val, amely megkönnyíti a fejlesztők indíthat és telefonhívások fogadása, küldje el és SMS-üzenetek fogadására és beágyazása a VoIP-hívás böngészőalapú és natív mobilalkalmazások be.  Vegyük röviden lépjen fölé, hogy hogyan működik van előtt.

### <a name="receiving-calls-and-text-messages"></a>Hívások és üzenetek fogadása

Twilio lehetővé teszi, hogy a fejlesztők [programozható telefonszámok] vásárlásához[ purchase_phone] küldése és fogadása a hívások és üzenetek is használható.  Amikor Twilio számot kap egy bejövő hívás vagy a szövegre, Twilio küld a webalkalmazás egy HTTP-bejegyzés vagy GET kérés, kéri a hívás vagy szöveg kezelése utasításokat.  A kiszolgáló válaszol Twilio a HTTP-összehívás [TwiML][twiml], XML-címkéket útmutatást kezelheti a hívást, illetve a szöveget tartalmazó egyszerű csoportja.  Akkor jelenik meg TwiML példák a kis türelmet.

### <a name="making-calls-and-sending-text-messages"></a>Hívások és a szöveges üzenetek küldése

HTTP-kérelmeket, így a Twilio webes API-t, a fejlesztők szöveges üzenetek küldésére, vagy kimenő hívásokat kezdeményezzen.  A kimenő hívások csoportban a fejlesztői kell adnia egy URL-címet, amely visszaadja a kimenő hívások kezelése, miután csatlakoztatva van TwiML útmutatóját.

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a>A VoIP-funkciók beágyazni a felhasználói felület kód (JavaScript, az iOS és az Android)

Twilio egy ügyféloldali SDK, amely lehet alakítani bármely asztali böngészőben, az iOS-alkalmazás vagy Android alkalmazást a VoIP-telefon biztosít.  Ez a cikk azt összpontosít használatáról a VoIP-hívás a böngészőben.  Nemcsak a böngészőben futó Twilio JavaScript SDK a kiszolgálóoldali kiegyenlítés (a node.js) "képesség jogkivonat" kibocsátása a JavaScript-ügyfélnek kell használni.  Erről további használatáról a VoIP-hez node.js [a Twilio fejlesztők blog][voipnode].

<a id="signup"/>
## <a name="sign-up-for-twilio-microsoft-discount"></a>Feliratkozás Twilio (Microsoft árengedmény)

Twilio szolgáltatások használata előtt be kell első [fiók regisztrálhat][signup].  Microsoft Azure visszaszorítása speciális árengedményt – [ne felejtse el az alábbi regisztráció][signup]!

<a id="azuresite"/>
## <a name="create-and-deploy-a-nodejs-azure-website"></a>Hozzon létre és telepítsen node.js Azure webhelye

Ezután meg kell Azure futó node.js webhely létrehozása.  [Ezzel a hivatalos dokumentációjában helyezkedik el az alábbi][azure_new_site].  Magas szintű akkor fogja kell az alábbiak:

* Feliratkozna az Azure fiókkal, ha nincs telepítve egyik már
* Új webhely létrehozása az Azure felügyeleti konzoljában használatával
* Adatforrás-vezérlő támogatás (fog feltételezzük, mely számjegy használt) hozzáadása
* Fájl létrehozása `server.js` az egyszerű node.js webalkalmazás
* Azure egyszerű alkalmazás telepítése

<a id="twiliomodule"/>
## <a name="configure-the-twilio-module"></a>A Twilio modul konfigurálása

Ezután azt meg is kezdi írni egy egyszerű node.js alkalmazást, amely a Twilio API-t használja.  Ahhoz, hogy megkezdhesse, hogy konfigurálnia kell a Twilio fiók hitelesítő adatait.  

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a>A rendszer környezeti változói Twilio hitelesítő adatok beállítása

A Twilio háttér ellen hitelesített kérések kezdeményezéséhez szükséges a fiók biztonsági AZONOSÍTÓK és auth jogkivonat, a felhasználónév és jelszó beállítása a Twilio fiók, mely függvény. A legbiztonságosabb úgy állítsa be ezeket használatra a csomópont modul Azure-ban, közvetlenül az Azure felügyeleti konzolban beállíthatja, hogy a rendszer környezeti változói keresztül.

Jelölje ki a node.js webhelyet, és kattintson a "KONFIGURÁLÁSA" hivatkozásra.  Ha egy kicsit lefelé görget, látni fogja a egy területet, ahol beállíthatja, hogy konfiguráció tulajdonságai párbeszédpanel az alkalmazás.  Írja be a Twilio fiók hitelesítő adatait ([a Twilio irányítópulton található][twilio_dashboard]) – lásd ügyeljen arra, hogy a név őket "TWILIO_ACCOUNT_SID" és "TWILIO_AUTH_TOKEN", illetve:

![Azure felügyeleti konzolban][azure-admin-console]

Ha ezek a változók van beállítva, indítsa újra az alkalmazást az Azure konzolban.

### <a name="declaring-the-twilio-module-in-packagejson"></a>Package.json Twilio moduljának deklarálása

Ezután szükség hozzon létre egy package.json kezelése a csomópont modul függőségek [npm]keresztül.  Az azonos szinten a Azure/node.js oktatóanyagban hoztunk "server.js"-fájlként hozzon létre egy "package.json" nevű fájlt.  Ez a fájl belül helyezze az alábbi:

  {"név": "alkalmazás neve", "verziója": "0.0.1", "magánjellegű": igaz, "parancsfájlok": {"indítása": "csomópont kiszolgálója"}, "függőségek": {"express": "3.1.0", "ejs": "*", "twilio": "*"}}

Ez a twilio modul deklarálása a függőséget, valamint a népszerű [express webes keretrendszer] mint[ express] és EJS sablon motor.  Rendben most azt kell tennie semmit, – néhány kódírás vegyük!

<a id="makecall"/>
## <a name="make-an-outbound-call"></a>A kimenő hívás

Most, hogy a program a hívást egy számot válassza azt, egyszerű űrlap létrehozása.  Nyissa meg a server.js, és adja meg a következő kódot.  Megjegyzés: Ha azt észleli, hogy "CHANGE_ME" - van elhelyezése a azure webhely neve:

    // Module dependencies
    var express = require('express'), 
      path = require('path'), 
      http = require('http'), 
      twilio = require('twilio');

    // Create Express web application
    var app = express();

    // Express configuration
    app.configure(function(){
      app.set('port', process.env.PORT || 3000);
      app.set('views', __dirname + '/views');
      app.set('view engine', 'ejs');
      app.use(express.favicon());
      app.use(express.logger('dev'));
      app.use(express.bodyParser());
      app.use(express.methodOverride());
      app.use(app.router);
      app.use(express.static(path.join(__dirname, 'public')));
    });
    app.configure('development', function(){
      app.use(express.errorHandler());
    });

    // Render an HTML user interface for the application's home page
    app.get('/', function(request, response) {
      response.render('index');
    });

    // Handle the form POST to place a call
    app.post('/call', function(request, response) {
      var client = twilio();
      client.makeCall({
          // make a call to this number
          to:request.body.number,

          // Change to a Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // A URL in our app which generates TwiML
          // Change "CHANGE_ME" to your app's name
          url:'https://CHANGE_ME.azurewebsites.net/outbound_call'
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

    // Generate TwiML to handle an outbound call
    app.post('/outbound_call', function(request, response) {
      var twiml = new twilio.TwimlResponse();

      // Say a message to the call's receiver 
      twiml.say('hello - thanks for checking out Twilio and Azure', {
          voice:'woman'
      });

      response.set('Content-Type', 'text/xml');
      response.send(twiml.toString());
    });

    // Start server
    http.createServer(app).listen(app.get('port'), function(){
      console.log("Express server listening on port " + app.get('port'));
    });

Ezután hozzon létre egy könyvtárat "nézetnek" nevezik – ezt a címtárat belül, a következő tartalommal "index.ejs" nevű fájl létrehozása:

    <!DOCTYPE html>
    <html>
    <head>
      <title>Twilio Test</title>
      <style>
        input { height:20px; width:300px; font-size:18px; margin:5px; padding:5px; }
      </style>
    </head>
    <body>
      <h1>Twilio Test</h1>
      <form action="/call" method="POST">
          <input placeholder="Enter a phone number" name="number"/>
          <br/>
          <input type="submit" value="Call the number above"/>
      </form>
    </body>
    </html>

Ezután a webhely telepítse az Azure, nyissa meg a kezdőlap.  Látnia kell, hogy a szöveg mezőbe írja be a telefonszámot, kezdeményezése és fogadása a Twilio szám!

<a id="sendmessage"/>
## <a name="send-an-sms-message"></a>SMS küldése

Most vegyük beállítása a felhasználói felület és a logika kezelése űrlap szöveges üzenetet küldeni.  Nyissa meg a "server.js", és a következő kód hozzáadása a legutóbbi hívást "app.post" után:

    app.post('/sms', function(request, response) {
      var client = twilio();
      client.sendSms({
          // send a text to this number
          to:request.body.number,

          // A Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // The body of the text message
          body: request.body.message
          
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

A "views/index.ejs" hozzáadása csoportban az első alkalommal szám és a szöveges üzenet küldése egy másik űrlap:

    <form action="/sms" method="POST">
      <input placeholder="Enter a phone number" name="number"/>
      <br/>
      <input placeholder="Enter a message to send" name="message"/>
      <br/>
      <input type="submit" value="Send text to the number above"/>
    </form>

Telepítsen újra az Azure alkalmazást, és most láthatja, űrlap és küldése saját magának (vagy bármely legközelebbi ismerősei) szöveges üzenet elküldése!

<a id="nextsteps"/>
## <a name="next-steps"></a>Következő lépések

Most már megtanulta azt az alkalmazások, amely közli a node.js és Twilio használatának alapjai.  De ezek a példák alig felmarására mi mindenre jó Twilio és node.js.  További információt a node.js Twilio használja tanulmányozza az alábbi forrásokat:

* [A dokumentumok hivatalos modul][docs]
* [A VoIP-oktatóanyag node.js alkalmazások][voipnode]
* [Votr – a valós idejű SMS szavazás node.js és CouchDB (három részből) alkalmazással][votr]
* [A böngészőben a node.js pár programozás][pair]

Azt a projektet, közkedvelt node.js és Twilio betörési a Azure!

[purchase_phone]: https://www.twilio.com/user/account/phone-numbers/available/local
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: http://ahoy.twilio.com/azure
[azure_new_site]: /app-service-web/web-sites-nodejs-develop-deploy-mac.md
[twilio_dashboard]: https://www.twilio.com/user/account
[npm]: http://npmjs.org
[express]: http://expressjs.com
[voipnode]: http://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: http://twilio.github.io/twilio-node/
[votr]: http://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: http://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png



