<properties
    pageTitle="Hozzon létre, és csatlakozzon az Azure-ban MySQL-adatbázishoz"
    description="Megtudhatja, hogy miként az Azure portal segítségével MySQL-adatbázis létrehozása, és csatlakoztassa rá PHP-webalkalmazásból Azure-ban."
    documentationCenter="php"
    services="app-service\web"
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="mysql"/>

<tags
    ms.service="multiple"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm;cephalin"/>

# <a name="create-and-connect-to-a-mysql-database-in-azure"></a>Hozzon létre, és csatlakozzon az Azure-ban MySQL-adatbázishoz

Ebből az oktatóanyagból megtudhatja, hogy miként MySQL-adatbázis létrehozása az [Azure portal](https://portal.azure.com) (szolgáltató a [ClearDB](http://www.cleardb.com/)), és hogyan szeretne kapcsolódni egy [Azure alkalmazás szolgáltatást](./app-service/app-service-value-prop-what-is.md)futtató PHP-webalkalmazásból. 

> [AZURE.NOTE] [Piactér alkalmazássablon](./app-service-web/app-service-web-create-web-app-from-marketplace.md)részeként MySQL-adatbázishoz is létrehozhat.

## <a name="create-a-mysql-database-in-azure-portal"></a>MySQL-adatbázis létrehozása az Azure-portálon

A MySQL-adatbázis létrehozása az Azure-portálon, tegye a következőket:

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).

2. A bal oldali menüben kattintson az **Új** > **adatok + tárhely** > **MySQL-adatbázishoz**.

    ![MySQL-adatbázis létrehozása az Azure - indítása](./media/store-php-create-mysql-database/create-db-1-start.png)

2. A MySQL-adatbázis [lap](azure-portal-overview.md), állítsa be az új MySQL-adatbázis következőképpen (*lap*: a portáloldalon a megnyíló vízszintesen):

    - **Az adatbázis neve**: írja be egy egyedi módon azonosító neve
    - **Előfizetés**: válassza az előfizetés használata
    - **Adatbázis-típusa**: jelölje be a **megosztott** az alacsony költség vagy ingyenes rétegek vagy **Dedicated** dedikált erőforrás veheti. 
    - **Erőforráscsoport**: a MySQL-adatbázis ad hozzá egy meglévő [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) vagy helyezze egy újat. Az azonos csoportba könnyen kezelhető együtt.
    - **Hely**: Válasszon egy helyet, közelébe. Meglévő erőforráscsoport való hozzáadásakor a által zárolt az erőforráscsoport helyre.
    - **Réteg árak**: **Réteg árak**gombra, majd jelöljön ki egy árak beállítást (**higany** réteg ingyenes), és kattintson a **Jelölje ki**. 
    - **Jogi feltételek**: kattintson a **Jogi kifejezéseket**, olvassa el a vételi adatait és kattintson a **vásárlás**gombra.
    - **PIN-kód irányítópult**: akkor válassza, ha azt szeretné, hogy közvetlenül az irányítópult eléréséhez. Ez akkor hasznos, ha nem ismeri a portál navigációs még.
    
    Az alábbi képernyőképen látható csak egy hogyan konfigurálható a MySQL-adatbázishoz.  
    ![A MySQL-adatbázis létrehozása az Azure - konfigurálása](./media/store-php-create-mysql-database/create-db-2-configure.png)

3. Ha végzett a beállításához kattintson a **Létrehozás**gombra.

    ![MySQL-adatbázis létrehozása az Azure - létrehozása](./media/store-php-create-mysql-database/create-db-3-create.png)

    Ekkor megjelenik egy előugró ablakokat bérbeadására tudja, hogy telepítési munkatársa.

    ![A MySQL-adatbázis létrehozása az Azure - folyamatban](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    Kaphat egy másik előugró sikeresen befejeződött a telepítés után. A portál is a MySQL-adatbázis lap automatikusan megnyíljon.

<a name="connect"></a>
## <a name="connect-to-your-mysql-database"></a>Csatlakozás MySQL-adatbázishoz

Ha látni szeretné a kapcsolat adatait az új MySQL-adatbázis, csak kattintson a **Tulajdonságok** gombra a webalkalmazás lap.
    
![A MySQL-adatbázis létrehozása az Azure - MySQL-adatbázis lap](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

A kapcsolat adatait a bármely web app most már használhatja. Mintaként szolgáló, amely bemutatja, hogyan használhatja a kapcsolat adatait egy egyszerű PHP-alkalmazásból érhető el [az alábbi](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).

## <a name="connect-a-laravel-web-app-from-the-php-get-started-tutorial"></a>Csatlakozás egy Laravel web app (az PHP-első lépések oktatóprogram)

Tegyük fel, hogy csak végzett az oktatóprogram [létrehozása, adja meg, és egy Azure PHP web app telepítése](./app-service-web/app-service-web-php-get-started.md) , és az Azure-ban futó [Laravel](https://www.laravel.com/) webalkalmazást. Adatbázis-funkciók könnyen hozzáadása az Laravel alkalmazást. Csak kövesse az alábbi lépéseket:

>[AZURE.NOTE] A következő lépések feltételezik, hogy végzett az oktatóprogram [létrehozása, adja meg, és az Azure PHP webes alkalmazások terjesztése](./app-service-web/app-service-web-php-get-started.md).

1. Állítsa be a Laravel alkalmazás a helyi fejlesztői környezet, mutasson a MySQL-adatbázishoz. Ehhez nyissa meg a `.env` az Laravel-alkalmazás legfelső szintű könyvtár és a MySQL-adatbázis beállításainak konfigurálása.

        DB_CONNECTION=mysql
        DB_HOST=<HOSTNAME_from_properties_blade>
        DB_PORT=<PORT_from_properties_blade>
        DB_DATABASE=<see_note_below>
        DB_USERNAME=<USERNAME_from_properties_blade>
        DB_PASSWORD=<PASSWORD_from_properties_blade>

    >[AZURE.NOTE] A **Tulajdonságok** lap a MySQL-adatbázis neve előfordulhat, hogy vagy nem lehet az **Adatbázis neve** mezőben látható. Akkor célszerűbb jelölje be az adatbázis paraméter a **KAPCSOLATI karakterlánc** mezőben. 
    >
    >![A MySQL-adatbázis létrehozása az Azure - folyamatban](./media/store-php-create-mysql-database/connect-db-1-database-name.png)

2. A leggyorsabban ellenőrizheti, hogy MySQL access most [Laravel tartozó alapértelmezett hitelesítési állványon](https://laravel.com/docs/5.2/authentication#authentication-quickstart)környezetbe. A parancssori terminálablakba a következő parancsokat a Laravel alkalmazás legfelső szintű címtárból:

         php artisan migrate
         php artisan make:auth

    Az első parancs hoz létre a táblák alapján az előre definiált áttelepítések Azure-ban a `database/migrations` könyvtárra, és a második parancs az egyszerű nézetek és a felhasználói regisztráció és a hitelesítéshez útvonalak scaffolds.

3. A fejlesztői kiszolgáló futtatása:

        php artisan serve

4. A böngészőben nyissa meg azt a http://localhost:8000, és regisztráljon az új felhasználó, ahogy azt:

    ![Csatlakozás MySQL-adatbázishoz Azure-ban,-felhasználói regisztráció](./media/store-php-create-mysql-database/connect-db-2-development-server.png)

    Kövesse a teljes felhasználói felület kérdésre a regisztráció. Ha befejeződött a regisztrációs, naplózza.
    
    ![Csatlakozás MySQL-adatbázishoz Azure-ban,-felhasználói regisztráció](./media/store-php-create-mysql-database/connect-db-3-registered-user.png)

    Ha a MySQL-adatbázis Azure-ban az alkalmazás letöltése fejlesztéséhez.

5. Most, egyszerűen való replikáció a `.env` beállításokat, hogy az Azure webalkalmazást. A következő Azure CLI parancsokat:

        azure site appsetting add DB_CONNECTION=mysql
        azure site appsetting add DB_HOST=<HOSTNAME_from_properties_blade>
        azure site appsetting add DB_PORT=<PORT_from_properties_blade>
        azure site appsetting add DB_DATABASE=<Database_param_from_CONNECTION_INFO_from_properties_blade>
        azure site appsetting add DB_USERNAME=<USERNAME_from_properties_blade>
        azure site appsetting add DB_PASSWORD=<PASSWORD_from_properties_blade>

    Megtudhatja, hogy hogyan működik a [beállítás a Azure web app](./app-service-web/app-service-web-php-get-started.md#configure).

6. Ezután véglegesítése és a helyi módosítások korábbi futtatása közben az Azure leküldéses `php artisan make:auth`.

        git add .
        git commit -m "scaffold auth views and routes"
        git push azure master

7. Tallózással keresse meg a Azure web App alkalmazásban.

        azure site browse

8. Jelentkezzen be a korábban létrehozott felhasználói hitelesítő adatokat.

    ![Csatlakozás MySQL-adatbázis Azure - ban nyissa meg azt az Azure web App alkalmazásban](./media/store-php-create-mysql-database/connect-db-4-browse-azure-webapp.png)

    Bejelentkezve az, meg kell jelennie a rövid utáni bejelentkezési képernyője.
    
    ![Csatlakozás MySQL-adatbázishoz az Azure - e jelentkezve](./media/store-php-create-mysql-database/connect-db-5-logged-in.png)

    Gratulálunk, az Azure-ban PHP-webalkalmazást van most eléréséhez a MySQL-adatbázisból. 

## <a name="next-steps"></a>Következő lépések

További tudnivalókért lásd: a [PHP Developer Center](/develop/php/).
