<properties
    pageTitle="PHP-MySQL webalkalmazást létrehozása az Azure alkalmazás szolgáltatás, és üzembe, mely számjegy használatával"
    description="Amely bemutatja, hogyan MySQL adatokat tároló PHP webalkalmazást létrehozása és használata a telepítés mely számjegy Azure oktatóprogram."
    services="app-service\web"
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="mysql"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a>PHP-MySQL webalkalmazást létrehozása az Azure alkalmazás szolgáltatás, és üzembe, mely számjegy használatával

Ebből az oktatóanyagból megtudhatja, hogyan hozhat létre PHP-MySQL webalkalmazást és hogyan kell [Alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=529714) üzembe mely számjegy használatával. [PHP]használni kívánt[install-php], a MySQL-parancssori eszköz ( [MySQL]része[install-mysql]), és [mely számjegy] [ install-git] telepítve van a számítógépén. Ebben az oktatóprogramban az utasításokat bármely operációs rendszeren, beleértve a Windows, Mac és Linux követheti. Ez az útmutató befejeztével Azure-ban futó PHP/MySQL webalkalmazást lesz.

Ismerkedhet meg:

* Hogyan hozhat létre a megfelelő web App alkalmazásban és az [Azure-portálon]MySQL-adatbázishoz[management-portal]. Mivel PHP alapértelmezés szerint engedélyezve van az [Alkalmazás szolgáltatás webalkalmazásokban](http://go.microsoft.com/fwlink/?LinkId=529714) , semmi speciális a PHP-kód futtatásához szükséges.
* Hogyan lehet közzé, és tegye közzé újra az alkalmazást használja, mely számjegy Azure.
* A tevékenységek hogyan engedélyezhető a zeneszerző kiterjesztés automatizálhatja a zeneszerző minden `git push`.

Ebben az oktatóanyagban követve gyűjt a PHP az egyszerű regisztrációs webalkalmazást. Az alkalmazás fog tárolni a Web Apps alkalmazások. Képernyőkép a kész alkalmazást nem éri el:

![Azure PHP-webhelyen][running-app]

## <a name="set-up-the-development-environment"></a>A fejlesztői környezet beállítása

Ebben az oktatóanyagban feltételezi, hogy van-e [PHP][install-php], a MySQL-parancssori eszköz ( [MySQL]része[install-mysql]), és [mely számjegy] [ install-git] telepítve van a számítógépén.

<a id="create-web-site-and-set-up-git"></a>
## <a name="create-a-web-app-and-set-up-git-publishing"></a>Hozzon létre webalkalmazást, és mely számjegy közzététel beállítása

Hajtsa végre az alábbi lépéseket követve hozzon létre egy web app és a MySQL-adatbázishoz:

1. Jelentkezzen be az [Azure portál][management-portal].
2. Kattintson az **Új** ikonra.

3. Kattintson az **Összes látható** **piactér**mellett. 

4. Kattintson a **webhely + Mobile**, majd **a Web app + MySQL**. Kattintson a **Létrehozás**parancsra.

4. Adjon meg egy érvényes az erőforrás-csoport nevét.

    ![Erőforráscsoport neve beállítása][resource-group]

5. Írja be az értékeket az új webhely számára.

    ![Webalkalmazás létrehozása][new-web-app]

6. Írja be az értékeket az új adatbázisban, többek között, amelyben a jogi feltételeket.

    ![Új MySQL-adatbázis létrehozása][new-mysql-db]

7. A web app létrehozásakor a web app új lap jelenik meg.

7. A **Beállítások** kattintson **Folyamatos**példányban, majd a _Configure szükséges beállításai_.

    ![Mely számjegy történő közzététel beállítása][setup-publishing]

8. Jelölje be a forrás **Helyi mely számjegy tárházba** .

    ![Mely számjegy tárházba beállítása][setup-repository]


9. Ahhoz, hogy mely számjegy közzététel, meg kell adnia egy felhasználónevet és jelszót. Jegyezze le a felhasználónevet és jelszót hoz létre. (Ha van állítva egy mely számjegy tárházba előtt, ebben a lépésben kimarad.)

    ![Közzétételi hitelesítő adatok létrehozása][credentials]


## <a name="get-remote-mysql-connection-information"></a>MySQL-kapcsolat beállításai

A Web Apps alkalmazások, a fog futtató MySQL-adatbázishoz való csatlakozáshoz van szüksége a kapcsolat adatait. Úgy juthat az MySQL-kapcsolati adatait, kövesse az alábbi lépéseket:

1. Az erőforrás csoportjában kattintson az adatbázis:

    ![Jelölje ki az adatbázisban][select-database]

2. Az adatbázist, **Beállítások**válassza a **Tulajdonságok parancsot**.

    ![Válassza a Tulajdonságok parancsot][select-properties]

2. Jegyezze fel az értékei `Database`, `Host`, `User Id`, és `Password`.

    ![Megjegyzés: tulajdonságai][note-properties]

## <a name="build-and-test-your-app-locally"></a>Készítése és az alkalmazás helyileg tesztelése

Most, hogy a megfelelő web App alkalmazásban létrehozott, az alkalmazás helyileg kidolgozása, majd ellenőrzése után telepítse azt.

A regisztráció alkalmazása egy egyszerű PHP-alkalmazás, amely lehetővé teszi, hogy regisztráljon az esemény, mert a nevét és e-mail címét. Előző igénylők információt egy táblázatban jelennek meg. Regisztrációs információ a MySQL-adatbázisban vannak tárolva. Az alkalmazás egy fájl (alatti érhető el a másolás és beillesztés kód) áll:

* **index.php**: regisztráció és a bejegyzés információkat tartalmazó táblában űrlapot jeleníti meg.

Létre, és futtassa az alkalmazást helyi meghajtóra, kövesse az alábbi lépéseket. Figyelje meg, hogy ezek a lépések feltételezik, a PHP és a helyi számítógépre beállítása MySQL-parancssori eszköz (MySQL része) van, és a [MySQL-OEM bővítmény]engedélyezve van[pdo-mysql].

1. Csatlakozás a távoli MySQL-kiszolgáló, az az érték használata `Data Source`, `User Id`, `Password`, és `Database` , amely a korábban lekért:

        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]

2. A MySQL-parancssort jelennek meg:

        mysql>

3. Illessze be a következő `CREATE TABLE` létrehozása parancs a `registration_tbl` az adatbázis táblájához:

        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);

4. Kattintson a helyi alkalmazás mappa gyökerébe **index.php** fájl létrehozása.

5. Nyissa meg a **index.php** fájlt egy szövegszerkesztőben vagy IDE, és hozzá a következő kódot, és fejezze be a szükséges módosításokat jelölt `//TODO:` megjegyzéseket.


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
            // DB connection info
            //TODO: Update the values for $host, $user, $pwd, and $db
            //using the values you retrieved earlier from the Azure Portal.
            $host = "value of Data Source";
            $user = "value of User Id";
            $pwd = "value of Password";
            $db = "value of Database";
            // Connect to database.
            try {
                $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
                $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            }
            catch(Exception $e){
                die(var_dump($e));
            }
            // Insert registration info
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
            // Retrieve data
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
        ?>
        </body>
        </html>

4.  A terminálablakba nyissa meg az alkalmazás mappát, és írja be a következő parancsot:

        php -S localhost:8000

Most már tallózhat **http://localhost:8000 /** tesztelje az alkalmazást.


## <a name="publish-your-app"></a>Az alkalmazás közzététele

Az alkalmazás helyileg tesztelése, után a Web Apps alkalmazások használata a mely számjegy közzéteheti. Ha a helyi mely számjegy tárházba inicializálni, és közzététele az alkalmazás.

> [AZURE.NOTE]
> Ezek az Azure-portálon végén az egy webalkalmazás létrehozása és a fenti mely számjegy közzétételi szakasz feljebb látható ezeket a lépéseket.

1. (Nem kötelező)  Ha elfelejtette vagy elvesztette a mely számjegy távoli repostitory URL-címet, nyissa meg a web app tulajdonságok az Azure-portálon.

1. Nyissa meg a GitBash (vagy a terminált, mely számjegy esetén a `PATH`), váltson a legfelső szintű könyvtárak az alkalmazást, és futtassa az alábbi parancsokat:

        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master

    Kéri a korábban létrehozott jelszót.

    ![Az Azure kezdeti leküldéses, mely számjegy keresztül][git-initial-push]

2. Tallózással keresse meg a **http://[site name].azurewebsites.net/index.php** az alkalmazás (ezt az információt szeretne tárolni a fiók irányítópulton) használatának megkezdéséhez:

    ![Azure PHP-webhelyen][running-app]

Az alkalmazás közzétételét követően kezdődik a változtatásokat, és mely számjegy használható közzéteheti őket.

## <a name="publish-changes-to-your-app"></a>Az alkalmazás módosításainak közzététele

Az alkalmazás módosításainak közzététele, kövesse az alábbi lépéseket:

1. Módosíthatja az alkalmazás helyi meghajtóra.
2. Nyissa meg a GitBash (vagy a terminált, mely számjegy értéke az informatikai a `PATH`), váltson a legfelső szintű könyvtárak az alkalmazást, és futtassa az alábbi parancsokat:

        git add .
        git commit -m "comment describing changes"
        git push azure master

    Kéri a korábban létrehozott jelszót.

    ![Webhely-módosítások terjesztése az Azure mely számjegy keresztül][git-change-push]

3. Tallózással keresse meg a **http://[site name].azurewebsites.net/index.php** az alkalmazás és az Ön által végzett módosításokat:

    ![Azure PHP-webhelyen][running-app]

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók használatbavételéhez Azure alkalmazás szolgáltatás, [Próbálja meg alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban megnyitásához. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

<a name="composer"></a>
## <a name="enable-composer-automation-with-the-composer-extension"></a>A zeneszerző kiterjesztésű zeneszerző automatizálási engedélyezése

Alapértelmezés szerint a mely számjegy telepítési folyamatot App szolgáltatásban még semmit sem tesz composer.json, ha több a PHP projektben. Engedélyezheti feldolgozása közben composer.json `git push` a zeneszerző bővítmény engedélyezésével.

1. A PHP a web app fel az [Azure portál][management-portal], kattintson az **eszközök** > **bővítmények**.

    ![Zeneszerző bővítmény beállításai][composer-extension-settings]

2. Kattintson a **Hozzáadás**gombra, majd kattintson a **Zeneszerző**.

    ![Zeneszerző bővítmény hozzáadása][composer-extension-add]
    
3. Kattintson **az OK gombra** koppintva fogadja el a jogi feltételeket. Kattintson ismét az **OK gombra** a bővítmény hozzáadása.

    A **telepített bővítmények** lap most jelennek meg a zeneszerző bővítmény.  
    ![Zeneszerző bővítmény megtekintése][composer-extension-view]
    
4. Most, hajtsa végre `git add`, `git commit`, és `git push` , mint az előző szakaszban. Most már láthatja, hogy zeneszerző telepítésére van beállítva az composer.json definiált függőségek.

    ![A siker zeneszerző bővítmény][composer-extension-success]

## <a name="next-steps"></a>Következő lépések

További tudnivalókért lásd: a [PHP Developer Center](/develop/php/).

<!-- URL List -->

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[install-mysql]: http://dev.mysql.com/downloads/mysql/
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[management-portal]: https://portal.azure.com
[sql-database-editions]: http://msdn.microsoft.com/library/windowsazure/ee621788.aspx

<!-- IMG List -->

[running-app]: ./media/web-sites-php-mysql-deploy-use-git/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-git/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-git/create_web_mysql.png
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-git/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-git/select_webapp.png
[setup-git-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_git_publishing.png
[credentials]: ./media/web-sites-php-mysql-deploy-use-git/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-git/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-git/create_wa.png
[setup-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_deploy.png
[setup-repository]: ./media/web-sites-php-mysql-deploy-use-git/select_local_git.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-git/select_database.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-git/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-git/note-properties.png

[git-instructions]: ./media/web-sites-php-mysql-deploy-use-git/git-instructions.png
[git-change-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-change-push.png
[git-initial-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-initial-push.png

[composer-extension-settings]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-settings.png
[composer-extension-add]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-add.png
[composer-extension-view]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-view.png
[composer-extension-success]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-success.png
