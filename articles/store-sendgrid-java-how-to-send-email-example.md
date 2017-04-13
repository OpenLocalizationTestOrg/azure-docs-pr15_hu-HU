<properties 
    pageTitle="Store-sendgrid-Java-How-to-send-email-example" 
    description="Hogyan küldhet e-mailt érkező Java SendGrid az Azure környezetben" 
    services="" 
    documentationCenter="java" 
    authors="thinkingserious" 
    manager="sendgrid" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="vibhork;dominic.may@sendgrid.com;elmer.thomas@sendgrid.com"/>

# <a name="how-to-send-email-using-sendgrid-from-java-in-an-azure-deployment"></a>Hogyan küldhet e-mailt érkező Java SendGrid az Azure környezetben

A következő példa bemutatja, hogyan használhatja SendGrid Azure-ban tárolt internetes weblapról származó e-maileket küldhet. Az eredményül kapott alkalmazás a felhasználó e-mail az értékek, kérdezze meg, ahogy az alábbi képernyőfelvételen.

![E-mailek űrlap][emailform]

Az eredményül kapott e-mailt az alábbi képernyőfelvételen hasonlóan fog kinézni.

![E-mail üzenet][emailsent]

Az alábbi módon használja a ebben a témakörben kell:

1. Szerezze be a javax.mail kancsó például <http://www.oracle.com/technetwork/java/javamail/index.html>.
2. Adja hozzá a kancsó a Java összeállítás elérési útját.
3. Holdas létrehozni a Java alkalmazást használ, ha a SendGrid tárak alkalmazás telepítési fájlját (HÁBORÚ) Holdas féle telepítésének összeállítási funkcióval is. Ha nem használ Holdas létrehozni a Java-alkalmazást, győződjön meg róla, a tárak tartoznak a Java-alkalmazásként korábbi Azure szerepkörét és hozzáadva az osztályjegyzetfüzet elérési útját az alkalmazás.


A saját SendGrid felhasználónevével és jelszavával, láthatja az e-mailt küldeni kell rendelkeznie. Az első lépések SendGrid, megtudhatja, [hogy miként küldhet e-mailt a Java SendGrid használatával](store-sendgrid-java-how-to-send-email.md).

Emellett a [egy helló világ alkalmazás létrehozása az Azure Holdas az](http://msdn.microsoft.com/library/windowsazure/hh690944)információkat, vagy más technikákkal elhelyezésére Java-alkalmazások Azure-ban, ha nem használ Holdas, ismerete ajánlott.

## <a name="create-a-web-form-for-sending-email"></a>E-mailek küldéséhez webes űrlap létrehozása

A következő kódot mutatja-e-mailek küldéséhez felhasználói adatok beolvasásához webes űrlap létrehozása. A tartalom alkalmazásában a JSP fájl neve **emailform.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Send this email</b>.</p>
     <br/>
      <form action="sendemail.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="emailTo">
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="emailFrom">
           </td>
         </tr>
         <tr>
           <td>Subject:</td>
           <td><input type="text" size=100 name="emailSubject" value="My email subject">
           </td>
         </tr>
         <tr>
           <td>Text:</td>
           <td><input type="text" size=400 name="emailText" value="Hello,<p>This is my message.</p>Thank you." />
           </td>
         </tr>
         <tr>
           <td>SendGrid user name:</td>
           <td><input type="text" name="sendGridUser">
           </td>
         </tr>
         <tr>
           <td>SendGrid password:</td>
           <td><input type="password" name="sendGridPassword">
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Send this email">
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-send-the-email"></a>Az e-mailt küldeni a kód létrehozása

A következő kódot, vagyis amikor emailform.jsp az űrlapra befejezte, hoz létre az e-mailben, és elküldi azt. A tartalom alkalmazásában a JSP fájl neve **sendemail.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" import="javax.activation.*, javax.mail.*, javax.mail.internet.*, java.util.Date, java.util.Properties" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email processing happens here</title>
    </head>
    <body>
        <b>This is my send mail page.</b><p/>
     <%
     
     final String sendGridUser = request.getParameter("sendGridUser");
     final String sendGridPassword = request.getParameter("sendGridPassword");
     
     class SMTPAuthenticator extends Authenticator
     {
       public PasswordAuthentication getPasswordAuthentication()
       {
            String username = sendGridUser;
            String password = sendGridPassword;
          
            return new PasswordAuthentication(username, password);   
       }
     }
     try
     {
         
         // The SendGrid SMTP server.
         String SMTP_HOST_NAME = "smtp.sendgrid.net";
    
         Properties properties;
        
         properties = new Properties();
         
         // Specify SMTP values.
         properties.put("mail.transport.protocol", "smtp");
         properties.put("mail.smtp.host", SMTP_HOST_NAME);
         properties.put("mail.smtp.port", 587);
         properties.put("mail.smtp.auth", "true");
         
         // Display the email fields entered by the user. 
         out.println("Value entered for email Subject: " + request.getParameter("emailSubject") + "<br/>");        
         out.println("Value entered for email      To: " + request.getParameter("emailTo") + "<br/>");
         out.println("Value entered for email    From: " + request.getParameter("emailFrom") + "<br/>");
         out.println("Value entered for email    Text: " + "<br/>" + request.getParameter("emailText") + "<br/>");
    
         // Create the authenticator object.
         Authenticator authenticator = new SMTPAuthenticator();
         
         // Create the mail session object.
         Session mailSession;
         mailSession = Session.getDefaultInstance(properties, authenticator);
         
         // Display debug information to stdout, useful when using the
         // compute emulator during development.
         mailSession.setDebug(true);
    
         // Create the message and message part objects.
         MimeMessage message;
         Multipart multipart;
         MimeBodyPart messagePart; 
         
         message = new MimeMessage(mailSession);
         
         multipart = new MimeMultipart("alternative");
         messagePart = new MimeBodyPart();
         messagePart.setContent(request.getParameter("emailText"), "text/html");
         multipart.addBodyPart(messagePart);            
    
         // Specify the email To, From, Subject and Content. 
         message.setFrom(new InternetAddress(request.getParameter("emailFrom")));
         message.addRecipient(Message.RecipientType.TO, new InternetAddress(request.getParameter("emailTo")));
         message.setSubject(request.getParameter("emailSubject")); 
         message.setContent(multipart);
         
         // Uncomment the following if you want to add a footer.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"footer\": {\"settings\": {\"enable\":1,\"text/html\": \"<html>This is my <b>email footer</b>.</html>\"}}}}");
    
         // Uncomment the following if you want to enable click tracking.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"clicktrack\": {\"settings\": {\"enable\":1}}}}");
         
         Transport transport;
         transport = mailSession.getTransport();
         // Connect the transport object.
         transport.connect();
         // Send the message.
         transport.sendMessage(message,  message.getRecipients(Message.RecipientType.TO));
         // Close the connection.
         transport.close();
     
        out.println("<p>Email processing completed.</p>");
         
     }
     catch (Exception e)
     {
         out.println("<p>Exception encountered: " + 
                            e.getMessage()     +
                            "</p>");   
     }
    %>
    
    </body>
    </html>

Az e-mailt küld, mellett emailform.jsp eredményt biztosít a felhasználó; Példa az alábbi képernyőfelvételen:

![Küldje el a levelek eredménye][emailresult]

## <a name="next-steps"></a>Következő lépések

Telepíteni az alkalmazást a számítási irányító és webböngészőből futtatása emailform.jsp, adja meg az értékeket a képernyőn, kattintson **az e-mail küldése**és dátumtípus sendemail.jsp eredményez.

Ez a kód mutatja, hogy a Java SendGrid használatáról Azure adta meg. Mielőtt Azure gyártási, érdemes több hibakezelés és más funkciók. Példa: 

* Tárterület Azure BLOB- vagy SQL-adatbázis kihasználva tárolni, e-mail címét és az e-mailek használata a webes űrlap helyett. Java a tárhely Azure BLOB használatával kapcsolatos információkért olvassa el a [Java Blob Tárhelyszolgáltatása használata](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/)című témakört. Java az SQL-adatbázis használatával kapcsolatos információkért olvassa el a [SQL-adatbázis használata Java-ban](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/)című témakört.
* Használhatja is `RoleEnvironment.getConfigurationSettings` SendGrid felhasználónév és jelszó beolvasni a üzembe konfigurációs beállítások helyett a webes űrlap használatával történő ezeket az értékeket. Információt a `RoleEnvironment` osztály [az Azure Service futtatókörnyezet tárba JSP használ](http://msdn.microsoft.com/library/windowsazure/hh690948) , és a <http://dl.windowsazure.com/javadoc>az Azure Service futtatókörnyezet csomag dokumentációjában találhat.
* Java-ban, SendGrid használatával kapcsolatos további tudnivalókért megtudhatja, [hogy miként küldhet e-mailt a Java SendGrid használatával](store-sendgrid-java-how-to-send-email.md).

[emailform]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailform.jpg
[emailsent]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailSent.jpg
[emailresult]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaResult.jpg
