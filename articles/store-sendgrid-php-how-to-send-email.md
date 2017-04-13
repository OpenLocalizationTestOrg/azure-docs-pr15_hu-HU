<properties 
    pageTitle="A SendGrid használt e-mail szolgáltatás (PHP) használata |} Microsoft Azure" 
    description="Megtudhatja, hogyan küldhet e-mailben a SendGrid használt e-mail szolgáltatás a Azure. A mintakódok PHP nyelven íródott." 
    documentationCenter="php" 
    services="" 
    manager="sendgrid" 
    editor="mollybos" 
    authors="thinkingserious"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork; matt.bernier@sendgrid.com"/>
# <a name="how-to-use-the-sendgrid-email-service-from-php"></a>A PHP SendGrid E-mail szolgáltatás használata

Ez az útmutató bemutatja, hogyan Azure a SendGrid e-mail szolgáltatással közös programozási különböző művelet végrehajtását. A minták PHP írt.
Az érintett esetek **megépítése e-mailek** **küldése e-mailek**és **a mellékletek hozzáadása**. SendGrid és a Küldés e-mailek további tudnivalókért lásd: a [Következő lépésekkel](#next-steps) szakaszban.

## <a name="what-is-the-sendgrid-email-service"></a>Mi az a SendGrid használt E-mail szolgáltatás?

SendGrid egy [felhőalapú e-mail szolgáltatás] megbízható [tranzakció alapú e-mailek kézbesítési], méretezhetőség, és a valós idejű analytics rugalmas API-hoz, amelyekkel egyszerűbbé egyéni integráció együtt. SendGrid használatát esetei a következők:

-   Automatikusan elküldeni visszaigazolások ügyfelek
-   Havi az e-szórólapok és különleges ajánlatok küldése a felhasználók felügyelete a terjesztési listák
-   Valós idejű mértékek, például a letiltott e-mailek és ügyfél válaszidő gyűjtése
-   Trendek azonosításához-jelentések létrehozása
-   Ügyfél lekérdezésekben továbbítás
- Az alkalmazás az e-mail értesítések

További tudnivalókért olvassa el a [https://sendgrid.com][]című témakört.

## <a name="create-a-sendgrid-account"></a>SendGrid-fiók létrehozása

[AZURE.INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="using-sendgrid-from-your-php-application"></a>Az PHP-alkalmazásokból SendGrid használatával

SendGrid Azure PHP alkalmazás használatához nincs speciális konfigurációs vagy kódolási. Mivel SendGrid szolgáltatás, azt is elérhető pontosan megegyező módon egy felhőalapú alkalmazásból egy helyszíni alkalmazásból igénybe.

## <a name="how-to-send-an-email"></a>Útmutató: a Küldés e-mailben

SMTP vagy a webes API SendGrid által biztosított e-mailben elküldheti.

### <a name="smtp-api"></a>SMTP API

A SendGrid SMTP API e-mailek küldéséhez *Swift levelezőprogrammal*, e-mailek küldéséhez PHP-alkalmazásokból összetevő-alapú tár használata A *Swift levelezőprogrammal* tár letölthető [http://swiftmailer.org/download][] v5.3.0 ( [Zeneszerző] Swift levelezőprogrammal telepíteni kell használni). Küldése e-mailben a tár példányának létrehozása magában foglalja a <span class="auto-style2">Swift\_SmtpTransport</span>, <span class="auto-style2">Swift\_levelezőprogrammal</span>, és <span class="auto-style2">Swift\_üzenet</span> osztályok, beállításával a megfelelő beállításokat, és hívja fel a <span class="auto-style2">Swift\_Mailer::send</span> módszert.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */ 
     $text = "Hi!\nHow are you?\n";
     $html = "<html>
           <head></head>
           <body>
               <p>Hi!<br>
                   How are you?<br>
               </p>
           </body>
           </html>";
     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     // Email recipients
     $to = array(
           'john@contoso.com'=>'Destination 1 Name',
           'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';

     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';
     
     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);

     // Create a message (subject)
     $message = new Swift_Message($subject);

     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     
     // send message 
     if ($recipients = $swift->send($message, $failures))
     {
         // This will let us know how many users received this message
         echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
         echo "Something went wrong - ";
         print_r($failures);
     }

### <a name="web-api"></a>Webes API

PHP-féle [curl függvény][] használatával küldhet e-mailt a SendGrid webes API.

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD'; 

     $params = array(
          'api_user' => $user,
          'api_key' => $pass,
          'to' => 'john@contoso.com',
          'subject' => 'testing from curl',
          'html' => 'testing body',
          'text' => 'testing body',
          'from' => 'anna@contoso.com',
       );
       
     $request = $url.'api/mail.send.json';
     
     // Generate curl request
     $session = curl_init($request);
     
     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);
     
     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);
     
     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);
     
     // obtain response
     $response = curl_exec($session);
     curl_close($session);
     
     // print everything out
     print_r($response);

SendGrid a webes API nagyon hasonlít a REST API-t, abban az esetben, ha még nem igazán RESTful API, mert a legtöbb hívásokat, mindkét első és bejegyzés műveletek szót azonos értelemben használható.

## <a name="how-to-add-an-attachment"></a>Útmutató: melléklet hozzáadása

### <a name="smtp-api"></a>SMTP API

A példa parancsfájl Swift levelezőprogrammal tartalmazó e-mail küldésének kód további egysoros a SMTP API melléklet küldése magába foglalja.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */
     $text = "Hi!\nHow are you?\n";
      $html = "<html>
          <head></head>
          <body>
             <p>Hi!<br>
                How are you?<br>
             </p>
          </body>
          </html>";

     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     
     // Email recipients
     $to = array(
          'john@contoso.com'=>'Destination 1 Name',
          'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';
     
     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';
     
     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);
     
     // Create a message (subject)
     $message = new Swift_Message($subject);
     
     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName("file_name"));
     
     // send message 
     if ($recipients = $swift->send($message, $failures))
     {
          // This will let us know how many users received this message
          echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
          echo "Something went wrong - ";
          print_r($failures);
     }

A kód további sora van az alábbi képlettel történik:

     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName('file_name'));

Ebben a sorban a kód felhívja a csatolása módszer a <span class="auto-style2">Swift\_üzenet</span> objektum és statikus módszer <span class="auto-style2">fromPath</span> használja a <span class="auto-style2">Swift\_melléklet</span> osztály első és a fájl csatolása üzenethez.

### <a name="web-api"></a>Webes API

A webes API melléklet küldése nagyon hasonlít a webes API e-mailt küld. Megjegyzendő, hogy a következő példában a paraméter tömb kell tartalmaznia az elem:

    'files['.$fileName.']' => '@'.$filePath.'/'.$fileName

Példa:

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD';
     
     $fileName = 'myfile';
     $filePath = dirname(__FILE__);

     $params = array(
         'api_user' => $user,
         'api_key' => $pass,
         'to' =>'john@contoso.com',
         'subject' => 'test of file sends',
         'html' => '<p> the HTML </p>',
         'text' => 'the plain text',
         'from' => 'anna@contoso.com',
         'files['.$fileName.']' => '@'.$filePath.'/'.$fileName
     );
     
     print_r($params);
     
     $request = $url.'api/mail.send.json';
     
     // Generate curl request
     $session = curl_init($request);
     
     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);
     
     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);
     
     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);
     
     // obtain response
     $response = curl_exec($session);
     curl_close($session);
     
     // print everything out
     print_r($response);

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a>Útmutató: szűrők használatával élőlábak, a nyomon követés és az elemzést engedélyezése

SendGrid "szűrők" való további e-mail funkciókat tartalmaz. Ezek a beállítások, manuálisan is hozzáadhatók e-mailhez ahhoz, hogy bizonyos funkciókat, például engedélyezése a követés gombra, a Google analytics, előfizetés nyomon követése és így tovább.

Szűrők a szűrők tulajdonság segítségével alkalmazható üzenetre. Minden szűrő szűrőre vonatkozó beállításokat tartalmazó kivonat határozza meg. A következő példa lehetővé teszi, hogy az élőláb szűrő, és adja meg a szöveges üzenet, amely hozzáfűzi az e-mailben alján.
Ebben a példában fogjuk használni [sendgrid-php-tárat].
[Zeneszerző] segítségével tár telepítése:
    
    php composer.phar require sendgrid/sendgrid 2.1.1

Példa:    

    <?php
     /*
      * This example is used for sendgrid-php V2.1.1 (https://github.com/sendgrid/sendgrid-php/tree/v2.1.1)
      */
     include "vendor/autoload.php";

     $email = new SendGrid\Email();
     // The list of addresses this message will be sent to
     // [This list is used for sending multiple emails using just ONE request to SendGrid]
     $toList = array('john@contoso.com', 'anna@contoso.com');

     // Specify the names of the recipients
     $nameList = array('Name 1', 'Name 2');

     // Used as an example of variable substitution
     $timeList = array('4 PM', '5 PM');

     // Set all of the above variables
     $email->setTos($toList);
     $email->addSubstitution('-name-', $nameList);
     $email->addSubstitution('-time-', $timeList);

     // Specify that this is an initial contact message
     $email->addCategory("initial");

     // You can optionally setup individual filters here, in this example, we have 
     // enabled the footer filter
     $email->addFilter('footer', 'enable', 1);
     $email->addFilter('footer', "text/plain", "Thank you for your business");
     $email->addFilter('footer', "text/html", "Thank you for your business");

     // The subject of your email
     $subject = 'Example SendGrid Email';

     // Where is this message coming from. For example, this message can be from 
     // support@yourcompany.com, info@yourcompany.com
     $from = 'someone@example.com';

     // If you do not specify a sender list above, you can specifiy the user here. If 
     // a sender list IS specified above, this email address becomes irrelevant.
     $to = 'john@contoso.com';

     # Create the body of the message (a plain-text and an HTML version). 
     # text is your plain-text email 
     # html is your html version of the email
     # if the receiver is able to view html emails then only the html
     # email will be displayed

     /*
      * Note the variable substitution here =)
      */
     $text = "
     Hello -name-,
     Thank you for your interest in our products. We have set up an appointment to call you at -time- EST to discuss your needs in more detail.
     Regards,
     Fred";

     $html = "
     <html> 
     <head></head>
     <body>
     <p>Hello -name-,<br>
     Thank you for your interest in our products. We have set up an appointment
     to call you at -time- EST to discuss your needs in more detail.

     Regards,

     Fred<br>
     </p>
     </body>
     </html>";

     // set subject
     $email->setSubject($subject);

     // attach the body of the email
     $email->setFrom($from);
     $email->setHtml($html);
     $email->addTo($to);
     $email->setText($text);

     // Your SendGrid account credentials
     $username = 'sendgridusername@yourdomain.com';
     $password = 'example';

     // Create SendGrid object
     $sendgrid = new SendGrid($username, $password);

     // send message
     $response = $sendgrid->send($email);

     print_r($response);

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta a SendGrid használt E-mail szolgáltatás alapjait, kövesse az alábbi hivatkozásokra kattintva további.

-   SendGrid dokumentáció: <https://sendgrid.com/docs>
-   SendGrid PHP-tár: <https://github.com/sendgrid/sendgrid-php>
-   Azure ügyfelek különleges ajánlatot, SendGrid: <https://sendgrid.com/windowsazure.html>

További tudnivalókért lásd még: a [PHP Developer Center](/develop/php/).


  [https://sendgrid.com]: https://sendgrid.com
  [https://sendgrid.com/transactional-email/pricing]: https://sendgrid.com/transactional-email/pricing
  [special offer]: https://www.sendgrid.com/windowsazure.html
  [Packaging and Deploying PHP Applications for Azure]: http://msdn.microsoft.com/library/windowsazure/hh674499(v=VS.103).aspx
  [http://swiftmailer.org/download]: http://swiftmailer.org/download
  [curl függvény]: http://php.net/curl
  [felhőalapú e-mail szolgáltatásból]: https://sendgrid.com/email-solutions
  [tranzakció alapú e-mailek kézbesítési]: https://sendgrid.com/transactional-email
  [sendgrid-php-tárban]: https://github.com/sendgrid/sendgrid-php/tree/v2.1.1
  [Zeneszerző]: https://getcomposer.org/download/
