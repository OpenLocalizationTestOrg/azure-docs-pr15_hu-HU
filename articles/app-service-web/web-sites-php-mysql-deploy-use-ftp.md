<properties 
    pageTitle="PHP-MySQL webalkalmazást Azure alkalmazás szolgáltatás hozzon létre és üzembe FTP használatával" 
    description="Egy oktatóprogram, mely szemlélteti, hogyan hozhat létre a MySQL- és használata az Azure FTP-példányban adatokat tároló PHP webalkalmazást." 
    services="app-service\web" 
    documentationCenter="php" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>


#<a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-ftp"></a>PHP-MySQL webalkalmazást Azure alkalmazás szolgáltatás hozzon létre és üzembe FTP használatával

Ebből az oktatóanyagból megtudhatja, hogyan hozhat létre PHP-MySQL webalkalmazást és telepítéséről FTP használatával. Ebben az oktatóanyagban feltételezi, hogy van-e [PHP][install-php], [MySQL][install-mysql], webkiszolgálóra, és az FTP-ügyfél telepítve van a számítógépén. Ebben az oktatóprogramban az utasításokat bármely operációs rendszeren, beleértve a Windows, Mac és Linux követheti. Ez az útmutató befejeztével Azure-ban futó PHP/MySQL webalkalmazást lesz.
 
Ismerkedhet meg:

* Hogyan lehet létrehozni a megfelelő web App alkalmazásban és az Azure-portálon MySQL-adatbázishoz. Mivel PHP alapértelmezés szerint engedélyezve van a Web Apps, semmi speciális a PHP-kód futtatásához szükséges.
* Hogy miként teheti közzé a Azure FTP használatával az alkalmazást.
 
Ebben az oktatóanyagban követve gyűjt a PHP az egyszerű regisztrációs webalkalmazást. Az alkalmazás fog tárolni a webalkalmazást. Képernyőkép a kész alkalmazást nem éri el:

![Azure PHP-webhelyen][running-app]

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az ügyfélhez kezdéshez Azure alkalmazás szolgáltatással, nyissa meg a [Próbálja alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban. Nem kötelező hitelkártyát, nincs nyilatkozatát. 


##<a name="create-a-web-app-and-set-up-ftp-publishing"></a>Hozzon létre webalkalmazást, és állítsa be FTP-közzététel

Hajtsa végre az alábbi lépéseket követve hozzon létre egy web app és a MySQL-adatbázishoz:

1. Jelentkezzen be az [Azure portál][management-portal].
2. Kattintson arra a Azure portál bal a **+ Új** ikonra a képernyő tetején.

    ![Azure új webhely létrehozása][new-website]

3. A Keresés írja be a **Web app + MySQL** , majd kattintson **a Web app + MySQL**.

    ![Egyéni létrehozása új webhely][custom-create]

4. Kattintson a **létrehozása**gombra. Írja be egy egyedi alkalmazás szolgáltatás neve, az erőforráscsoport és az új szolgáltatás csomag érvényes nevét.

    ![Erőforráscsoport neve beállítása][resource-group]


6. Írja be az értékeket az új adatbázisban, többek között, amelyben a jogi feltételeket.

    ![Új MySQL-adatbázis létrehozása][new-mysql-db]
    
7. A web app létrehozásakor az új alkalmazás szolgáltatás lap jelenik meg.


6. Kattintson a **Beállítások**elemre > **telepítési hitelesítő adatait**. 

    ![Telepítési hitelesítő adatok beállítása][set-deployment-credentials]

7. Ahhoz, hogy az FTP-közzététel, meg kell adnia egy felhasználónevet és jelszót. A hitelesítő adatok mentését, és jegyezze fel a felhasználónevet és jelszót hoz létre.

    ![Közzétételi hitelesítő adatok létrehozása][portal-ftp-username-password]

##<a name="build-and-test-your-app-locally"></a>Készítése és az alkalmazás helyileg tesztelése

A regisztráció alkalmazása egy egyszerű PHP-alkalmazás, amely lehetővé teszi, hogy regisztráljon az esemény, mert a nevét és e-mail címét. Előző igénylők információt egy táblázatban jelennek meg. Regisztrációs információ a MySQL-adatbázisban vannak tárolva. Az alkalmazás két fájlok áll:

* **index.php**: regisztráció és a bejegyzés információkat tartalmazó táblában űrlapot jeleníti meg.
* **createtable.php**: az alkalmazás a MySQL-táblázatot hoz létre. Ez a fájl csak egyszer lesz.

Létre, és futtassa az alkalmazást helyi meghajtóra, kövesse az alábbi lépéseket. Megjegyzendő, hogy ezek a lépések feltételezik PHP, MySQL, van, és állítsa be a helyi számítógép zónában, és amelyekhez Önnek a webkiszolgálóra engedélyezte a [MySQL OEM bővítményének][pdo-mysql].

1. Nevű MySQL-adatbázis létrehozása `registration`. Ezt megteheti a MySQL-parancssorából, ezzel a paranccsal:

        mysql> create database registration;

2. A webkiszolgáló legfelső szintű címtárban nevű mappa létrehozása `registration` , és hozzon létre két fájlok azt – egy úgynevezett `createtable.php` és egy úgynevezett `index.php`.

3. Nyissa meg a `createtable.php` fájlt egy szövegszerkesztőben vagy IDE, és adja hozzá a következő kódot. Ez a kód használandó létrehozása a `registration_tbl` tábla a `registration` adatbázis.

        <?php
        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        try{
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            $sql = "CREATE TABLE registration_tbl(
                        id INT NOT NULL AUTO_INCREMENT, 
                        PRIMARY KEY(id),
                        name VARCHAR(30),
                        email VARCHAR(30),
                        date DATE)";
            $conn->query($sql);
        }
        catch(Exception $e){
            die(print_r($e));
        }
        echo "<h3>Table created.</h3>";
        ?>

    > [AZURE.NOTE] 
    > Meg kell frissíteni az értékeket az <code>$user</code> és <code>$pwd</code> helyi MySQL-felhasználónevet és jelszót.

4. Nyisson meg egy webböngészőt, és keresse meg [http://localhost/registration/createtable.php][localhost-createtable]. Ez a művelet létrehoz a `registration_tbl` az adatbázistáblába.

5. Nyissa meg a **index.php** fájlt egy szövegszerkesztőben vagy IDE, és adja hozzá a egyszerű HTML- és CSS kódrészletet (a PHP kód megjelenik az újabb lépések) lap.

        <html>
        <head>
        <Title>Registration Form</Title>
        <style type="text/css">
            body { background-color: #fff; border-top: solid 10px #000;
                color: #333; font-size: .85em; margin: 20; padding: 20;
                font-family: "Segoe UI", Verdana, Helvetica, Sans-Serif;
            }
            h1, h2, h3,{ color: #000; margin-bottom: 0; padding-bottom: 0; }
            h1 { font-size: 2em; }
            h2 { font-size: 1.75em; }
            h3 { font-size: 1.2em; }
            table { margin-top: 0.75em; }
            th { font-size: 1.2em; text-align: left; border: none; padding-left: 0; }
            td { padding: 0.25em 2em 0.25em 0em; border: 0 none; }
        </style>
        </head>
        <body>
        <h1>Register here!</h1>
        <p>Fill in your name and email address, then click <strong>Submit</strong> to register.</p>
        <form method="post" action="index.php" enctype="multipart/form-data" >
              Name  <input type="text" name="name" id="name"/></br>
              Email <input type="text" name="email" id="email"/></br>
              <input type="submit" name="submit" value="Submit" />
        </form>
        <?php

        ?>
        </body>
        </html>

6. Belül a PHP címkék hozzáadása az adatbázishoz való csatlakozáshoz PHP-kódot.

        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect to database.
        try {
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }

    > [AZURE.NOTE]
    > Ismét meg kell frissíteni az értékeket az <code>$user</code> és <code>$pwd</code> helyi MySQL-felhasználónevet és jelszót.

7. Követni a adatbázis-kapcsolatot kódot vegye fel a regisztrációs adatok beszúrása az adatbázisba kódját.

        if(!empty($_POST)) {
        try {
            $name = $_POST['name'];
            $email = $_POST['email'];
            $date = date("Y-m-d");
            // Insert data
            $sql_insert = "INSERT INTO registration_tbl (name, email, date) 
                           VALUES (?,?,?)";
            $stmt = $conn->prepare($sql_insert);
            $stmt->bindValue(1, $name);
            $stmt->bindValue(2, $email);
            $stmt->bindValue(3, $date);
            $stmt->execute();
        }
        catch(Exception $e) {
            die(var_dump($e));
        }
        echo "<h3>Your're registered!</h3>";
        }

8. Végezetül követően a fenti kódot, adja hozzá az adatbázisból származó adatok visszakeresése kódját.

        $sql_select = "SELECT * FROM registration_tbl";
        $stmt = $conn->query($sql_select);
        $registrants = $stmt->fetchAll(); 
        if(count($registrants) > 0) {
            echo "<h2>People who are registered:</h2>";
            echo "<table>";
            echo "<tr><th>Name</th>";
            echo "<th>Email</th>";
            echo "<th>Date</th></tr>";
            foreach($registrants as $registrant) {
                echo "<tr><td>".$registrant['name']."</td>";
                echo "<td>".$registrant['email']."</td>";
                echo "<td>".$registrant['date']."</td></tr>";
            }
            echo "</table>";
        } else {
            echo "<h3>No one is currently registered.</h3>";
        }

Most már tallózással [http://localhost/registration/index.php] [ localhost-index] a alkalmazásban tesztelje.

##<a name="get-mysql-and-ftp-connection-information"></a>MySQL- és FTP-kapcsolat beállításai

A Web Apps alkalmazások, a fog futtató MySQL-adatbázishoz való csatlakozáshoz van szüksége a kapcsolat adatait. Úgy juthat az MySQL-kapcsolati adatait, kövesse az alábbi lépéseket:

1. Az alkalmazás szolgáltatásból web app lap kattintson az erőforrás csoport hivatkozásra:

    ![Erőforrás-csoport kijelölése][select-resourcegroup]

1. Az erőforrás csoportjában kattintson az adatbázis:

    ![Jelölje ki az adatbázisban][select-database]

2. Az összefoglaló adatbázist, válassza a **Beállítások** > **tulajdonságait**.

    ![Válassza a Tulajdonságok parancsot][select-properties]
    
2. Jegyezze fel az értékek `Database`, `Host`, `User Id`, és `Password`.

    ![Megjegyzés: tulajdonságai][note-properties]

3. A web app alkalmazásból hivatkozásra a **letöltési profil közzététele** elemre a lap jobb alsó sarkában látható:

    ![Letöltés profil közzététele][download-publish-profile]

4. Nyissa meg a `.publishsettings` XML-szerkesztő fájlt. 

3. Keresse meg a `<publishProfile >` elem `publishMethod="FTP"` , hogy néz ki:

        <publishProfile publishMethod="FTP" publishUrl="ftp://[mysite].azurewebsites.net/site/wwwroot" ftpPassiveMode="True" userName="[username]" userPWD="[password]" destinationAppUrl="http://[name].antdf0.antares-test.windows-int.net" 
            ...
        </publishProfile>
    
Jegyezze fel a `publishUrl`, `userName`, és `userPWD` attribútumok.

##<a name="publish-your-app"></a>Az alkalmazás közzététele

Az alkalmazás helyileg tesztelése, után közzéteheti az FTP használatával webalkalmazást. Jó helyen jár először kell frissíteni az alkalmazást az adatbázis-kapcsolat beállításai. A következő információkat **is** az adatbázis-kapcsolati adatait, szerezte be korábbi (használata **az első MySQL és FTP kapcsolatadatainak** csoportban) frissítheti a `createdatabase.php` és `index.php` a megfelelő értékeket tartalmazó fájlok:

    // DB connection info
    $host = "value of Data Source";
    $user = "value of User Id";
    $pwd = "value of Password";
    $db = "value of Database";

Most már készen áll az alkalmazás FTP használatával közzé.

1. Nyissa meg az FTP-ügyfél megválasztott.

2. Írja be az *állomásnév* a `publishUrl` be az FTP-ügyfél fentiekben attribútum.

3. Írja be a `userName` és `userPWD` feletti lejegyzett attribútumok be az FTP-ügyfél változatlan marad.

4. Kapcsolat létrehozása.

Csatlakozás után a fájlok feltöltése és letöltése szükség szerint tudni fogja. Ügyeljen arra, hogy vannak-e fájlok feltöltéséről a legfelső szintű címtárhoz, amely `/site/wwwroot`.

Miután feltöltött mindkét `index.php` és `createtable.php`, tallózással keresse meg a **http://[site name].azurewebsites.net/createtable.php** az alkalmazáshoz a MySQL-táblázat létrehozása, majd tallózással keresse meg a **http://[site name].azurewebsites.net/index.php** az alkalmazás használatának megkezdéséhez.
 
## <a name="next-steps"></a>Következő lépések

További tudnivalókért lásd: a [PHP Developer Center](/develop/php/).

[install-php]: http://www.php.net/manual/en/install.php
[install-mysql]: http://dev.mysql.com/doc/refman/5.6/en/installing.html
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[localhost-createtable]: http://localhost/tasklist/createtable.php
[localhost-index]: http://localhost/tasklist/index.php
[running-app]: ./media/web-sites-php-mysql-deploy-use-ftp/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-ftp/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-ftp/create_web_mysql.png
[website-details]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/website_details.jpg
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-ftp/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-ftp/select_webapp.png
[set-deployment-credentials]: ./media/web-sites-php-mysql-deploy-use-ftp/set_credentials.png
[portal-ftp-username-password]: ./media/web-sites-php-mysql-deploy-use-ftp/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-ftp/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-ftp/create_wa.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-ftp/select_database.png
[select-resourcegroup]: ./media/web-sites-php-mysql-deploy-use-ftp/select_resourcegroup.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/note-properties.png

[connection-string-info]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/connection_string_info.png
[management-portal]: https://portal.azure.com
[download-publish-profile]: ./media/web-sites-php-mysql-deploy-use-ftp/download_publish_profile_3.png
 
