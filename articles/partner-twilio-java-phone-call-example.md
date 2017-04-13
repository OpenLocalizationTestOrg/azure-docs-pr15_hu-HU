<properties 
    pageTitle="Telefonhívás kezdeményezése a Twilio (Java) hogyan |} Microsoft Azure" 
    description="Megtudhatja, hogy miként Twilio Azure Java-alkalmazások használata weblapról hívni." 
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

# <a name="how-to-make-a-phone-call-using-twilio-in-a-java-application-on-azure"></a>Hogyan kell a Telefonhívás kezdeményezése Twilio Azure Java-alkalmazások használata 

A következő példa bemutatja, hogyan használhatja Twilio hívással Azure-ban tárolt weblapról. Az eredményül kapott alkalmazás a felhasználó a telefonhívás értékek, kérdezze meg, ahogy az alábbi képernyőfelvételen.

![Azure hívás űrlap Twilio és Java használatával][twilio_java]

Az alábbi módon használja a ebben a témakörben kell:

1. Szerezheti be egy Twilio fiókkal és a hitelesítési jogkivonat. Az első lépések Twilio, a [http://www.twilio.com/pricing]árak felmérése[twilio_pricing]. Iratkozzon fel a [https://www.twilio.com/try-twilio][try_twilio]. Twilio által biztosított API-val kapcsolatos további tudnivalókért lásd: [http://www.twilio.com/api][twilio_api].
2. Szerezze be a Twilio üveg. A [https://github.com/twilio/twilio-java][twilio_java_github], töltse le a GitHub források és hozzon létre saját üveg, vagy töltse le egy beépített üveg (vagy anélkül függőségek).
Ez a témakör a kód készült, a beépített TwilioJava-3.3.8-a-függőségek üveg használatával.
3. Adja hozzá a üveg a Java összeállítás elérési útját.
4. Ha Holdas létrehozni a Java alkalmazást használ, vegye fel a Twilio JAR a alkalmazás telepítési fájlba (HÁBORÚ) Holdas féle telepítésének összeállítás funkció használata. Ha nem használ Holdas létrehozni a Java-alkalmazást, győződjön meg róla, a Twilio JAR részét képező belül a Java-alkalmazásként azonos Azure szerepkört, és hozzá az osztályjegyzetfüzet elérési útját az alkalmazás.
5. Győződjön meg róla, a cacerts keystore tartalmazza a Equifax biztonságos hitelesítésszolgáltató tanúsítványát MD5 ujjlenyomat 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (időértékét 35:DE:F4:CF pedig a SHA1 ujjlenyomat D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Ez a [https://api.twilio.com] tanúsítvány hitelesítésszolgáltató tanúsítványát[ twilio_api_service] szolgáltatása, amelynek Twilio API-khoz használatakor nevezik. Ez hitelesítésszolgáltató tanúsítványát a JDK cacert áruházból való felvételéről további tudnivalókért lásd [a Java hitelesítésszolgáltató tanúsítvány tárolásához tanúsítvány hozzáadása][add_ca_cert].

Emellett az információkat a [létrehozása egy helló világ alkalmazás segítségével az Azure eszközkészlete Holdas]ismerős[azure_java_eclipse_hello_world], vagy az egyéb szolgáltatója Java-alkalmazások Azure-ban, ha nem használ Holdas technikák, nagyon ajánlott.

## <a name="create-a-web-form-for-making-a-call"></a>Telefonhívás kezdeményezése webes űrlap létrehozása

A következő kódot megtudhatja, hogy miként kezdeményezhet hívásokat a felhasználói adatok beolvasásához webes űrlap létrehozása. Ez a példa alkalmazásában új dinamikus webes projekt, **TwilioCloud**, nevű jött létre, és **callform.jsp** hozzá lett adva JSP fájlként.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Automated call form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Make this call</b>.</p>
     <br/>
      <form action="makecall.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="callTo" value="" />
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="callFrom" value="" />
           </td>
         </tr>
         <tr>
           <td>Call message:</td>
           <td><input type="text" size=400 name="callText" value="Hello. This is the call text. Good bye." />
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Make this call" />
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>A hívás kód létrehozása
A következő kódot, és a képernyőn megjelenő callform.jsp befejeztével neve, a hívás üzenetet hoz létre, és hozza létre a hívást. Ebben a példában alkalmazásában a JSP fájl **makecall.jsp** neve és a **TwilioCloud** projekthez való hozzáadásának. (Használata helyett a helyőrző értékeket **accountSID** és **authToken** az alábbi kód rendelt token Twilio fiók és a hitelesítéshez.)

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    import="java.util.*"
    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
    pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Call processing happens here</title>
    </head>
    <body>
        <b>This is my make call page.</b><p/>
     <%
    try 
    {
         // Use your account SID and authentication token instead
         // of the placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";
     
         // Instantiate an instance of the Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve the account, used later to retrieve the CallFactory.
         Account account = client.getAccount();

         // Display the client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");
     
         // Display the API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");
    
         // Retrieve the values entered by the user.
         String callTo = request.getParameter("callTo");  
         // The Outgoing Caller ID, used for the From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");
     
         // Replace spaces in the user's text with '%20', 
         // to make the text suitable for a URL.
         userText = userText.replace(" ", "%20");
     
         // Create a URL using the Twilio message and the user-entered text.
         String Url="http://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;
     
         // Display the message URL.
         out.println("<p>");
         out.println("The URL is " + Url);
         out.println("</p>");
    
         // Place the call From, To and URL values into a hash map. 
         HashMap<String, String> params = new HashMap<String, String>();
         params.put("From", callFrom);
         params.put("To", callTo);
         params.put("Url", Url);
     
         CallFactory callFactory = account.getCallFactory();
         Call call = callFactory.create(params);
         out.println("<p>Call status: " + call.getStatus()  + "</p>"); 
    } 
    catch (TwilioRestException e) 
    {
        out.println("<p>TwilioRestException encountered: " + e.getMessage() + "</p>");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    catch (Exception e) 
    {
        out.println("<p>Exception encountered: " + e.getMessage() + "");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    %>
    </body>
    </html>

Nemcsak a hívást, makecall.jsp az Twilio végpontot, API verzió és a hívás állapotát jeleníti meg. Példa az alábbi képernyőfelvételen:

![Azure hívás válasz Twilio és Java használatával][twilio_java_response]

## <a name="run-the-application"></a>Futtassa az alkalmazást
Az alábbiakban a magas szintű lépéseket a az alkalmazásnak a futtatására az alábbi lépéseket a következő helyen található [létrehozása egy helló világ alkalmazás segítségével az Azure eszközkészlete Holdas]részletek[azure_java_eclipse_hello_world].

1. A TwilioCloud HÁBORÚ exportálása a Azure **approot** mappát. 
2. Módosítsa a **startup.cmd** a TwilioCloud HÁBORÚ bonthatók ki.
3. Az alkalmazás számítási irányító fordítása.
4. Indítsa el a üzembe a számítási irányító.
5. Nyissa meg a böngészőt, és futtassa a **http://localhost:8080/TwilioCloud/callform.jsp**.
6. Értékek megadása a képernyőn, és kattintson **a hívás**dátumtípus makecall.jsp a találatokat.

Közvetlenül a telepítéshez használni Azure-telepítéshez a felhőben, recompile Azure telepítése, és a böngészőben futó http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp (helyettesítése a *your_hosted_name*értéket).

## <a name="next-steps"></a>Következő lépések
Ez a kód kapott mutatja, hogy a Java Twilio használata Azure alapvető funkcióit. Mielőtt Azure gyártási, érdemes több hibakezelés és más funkciók. Példa:

* Webes űrlap helyett segítségével tároló Azure BLOB- vagy SQL-adatbázis tárolhatja telefonszámok, és hívja fel a szöveget. Java a tárhely Azure BLOB használatával kapcsolatos további tudnivalókért lásd [Java Blob Tárhelyszolgáltatása használata][howto_blob_storage_java]. Java az SQL-adatbázis használatával kapcsolatos további tudnivalókért lásd [SQL-adatbázis használata a Java][howto_sql_azure_java].
* Ha segítségével **RoleEnvironment.getConfigurationSettings** lekérés a Twilio Fiókazonosítót, és a hitelesítési jogkivonat a üzembe konfigurációs beállítások merevlemez-kódolás makecall.jsp szereplő értékek helyett. A **RoleEnvironment** osztály információt talál [az Azure Service futási idejű tárba JSP használatával] [ azure_runtime_jsp] és az Azure Service futási idejű csomag dokumentációt, a [http://dl.windowsazure.com/javadoc][azure_javadoc].
* A makecall.jsp kódot rendel egy Twilio által biztosított URL-címet, [http://twimlets.com/message][twimlet_message_url], az **URL-cím** változó. Ez az URL-címet, amely arról értesíti a hívás folytatása módjáról az Twilio Twilio Markup Language (TwiML) választ biztosít. A függvény által visszaadott TwiML tartalmazhat például egy ** &lt;Ismerkedés&gt; ** igei, amely a szöveg kiejtését, hogy a hívott fél készülékén. Twilio által biztosított URL-címe helyett a saját szolgáltatás Twilio's összehívásra; válasz sikerült összeállítása További információért megtudhatja, [hogy miként Twilio használata a hang-és az SMS-funkciók az Java][howto_twilio_voice_sms_java]. További információt a TwiML [http://www.twilio.com/docs/api/twiml]találhatók[twiml], és további információt ** &lt;Ismerkedés&gt; ** és más Twilio műveletek a következő helyen található [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Olvassa el a Twilio biztonsági irányelvek a [https://www.twilio.com/docs/security][twilio_docs_security].

Twilio kapcsolatos további tudnivalókért lásd: [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Lásd még:
* [Twilio használatáról a hang-és az SMS-funkciók az Java][howto_twilio_voice_sms_java]
* [Tanúsítvány felvétele az Java hitelesítésszolgáltató tanúsítvány áruházból][add_ca_cert]

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_java_github]: http://github.com/twilio/twilio-java
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[azure_java_eclipse_hello_world]: http://msdn.microsoft.com/library/windowsazure/hh690944.aspx
[howto_twilio_voice_sms_java]: partner-twilio-java-how-to-use-voice-sms.md
[howto_blob_storage_java]: http://www.windowsazure.com/develop/java/how-to-guides/blob-storage/
[howto_sql_azure_java]: http://msdn.microsoft.com/library/windowsazure/hh749029.aspx
[azure_runtime_jsp]: http://msdn.microsoft.com/library/windowsazure/hh690948.aspx
[azure_javadoc]: http://dl.windowsazure.com/javadoc
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[twilio_java]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaCallForm.jpg
[twilio_java_response]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaMakeCall.jpg
