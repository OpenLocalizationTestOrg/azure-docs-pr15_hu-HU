<properties 
    pageTitle="Az első Java-webalkalmazást beállítaná Azure öt perc |} Microsoft Azure" 
    description="Megtudhatja, hogy milyen könnyen web Apps alkalmazások futtatásához alkalmazás szolgáltatás üzembe helyezése a minta-at. Indítsa el a valódi fejlesztéséhez gyorsan módon, és azonnal eredmények megtekintéséhez." 
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/13/2016" 
    ms.author="cephalin"
/>
    
# <a name="deploy-your-first-java-web-app-to-azure-in-five-minutes"></a>Az első Java-webalkalmazást beállítaná Azure öt perc

Ebben az oktatóanyagban segít egyszerű Java webes alkalmazások terjesztése [Azure alkalmazás](../app-service/app-service-value-prop-what-is.md)szolgáltatásba.
Alkalmazás szolgáltatással web Apps alkalmazások, [mobilalkalmazás biztonsági végű](/documentation/learning-paths/appservice-mobileapps/)és az [API-alkalmazások](../app-service-api/app-service-api-apps-why-best-platform.md)létrehozásához.

Teendők: 

- Azure alkalmazás szolgáltatás hozzon létre webalkalmazást.
- Minta Java alkalmazások terjesztése.
- Lásd: a kód futó élő gyártási.

## <a name="prerequisites"></a>Előfeltételek

- A GET-FTP/FTPS ügyfélprogram, például [FileZilla](https://filezilla-project.org/).
- Microsoft Azure-fiók beszerzése. Ha nem rendelkeznek fiókkal, akkor [Jelentkezzen be az ingyenes próbaverzióra](/pricing/free-trial/?WT.mc_id=A261C142F) , vagy [a Visual Studio előfizetői előnyeinek aktiválása](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] [Próbálja meg alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751) Azure-fiók nélkül is. Alapszintű alkalmazás létrehozása, és nem szükséges hitelkártya, nincs kötelezettségek akár egy óra – vele az lejátszása.

<a name="create"></a>
## <a name="create-a-web-app"></a>Egy webalkalmazás létrehozása

1. Jelentkezzen be az [Azure portál](https://portal.azure.com) Azure fiókjával.

2. A bal oldali menüben kattintson az **Új** > **webes + Mobile** > **Web App alkalmazásban**.

    ![](./media/app-service-web-get-started-languages/create-web-app-portal.png)

3. Az alkalmazás a Létrehozás lap, a használja az alábbi beállításokat az új számára:

    - **Alkalmazás neve**: írja be egy egyedi nevet.
    - **Erőforráscsoport**: jelölje be az **Új létrehozása** , és adjon nevet az erőforrás-csoportnak.
    - **Alkalmazás terv/SRV**: a gombra kattintva állítsa be, majd kattintson a **Hozzon létre új** nevét, helyét, és az alkalmazás szolgáltatás terv árak szint beállítása. Nyugodtan réteg árak **ingyenes** használja.

    Amikor elkészült, az alkalmazás a Létrehozás lap így néz ki:

    ![](./media/app-service-web-get-started-languages/create-web-app-settings.png)

3. A képernyő alján kattintson a **Létrehozás** gombra. Az **értesítési** ikonra a képernyő tetején a haladás megtekintése kattinthat.

    ![](./media/app-service-web-get-started-languages/create-web-app-started.png)

4. Telepítés befejezésekor, meg kell jelennie a értesítő üzenet. Kattintson a telepítés lap nyissa meg az üzenetet.

    ![](./media/app-service-web-get-started-languages/create-web-app-finished.png)

5. A **sikeres telepítés** lap hivatkozásra az **erőforrás** az új webalkalmazást lap megnyitásához.

    ![](./media/app-service-web-get-started-languages/create-web-app-resource.png)

## <a name="deploy-a-java-app-to-your-web-app"></a>A web App Java alkalmazások terjesztése

Most vegyük Java alkalmazások terjesztése az Azure FTPS használatával.

5. A web app lap görgessen le az **alkalmazás beállításai** vagy a keresést, majd kattintson rá. 

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

6. **Java verzióját**jelölje ki a **Java 8** , és kattintson a **Mentés**gombra.

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

    Ha **frissítése megtörtént a web app beállításai**értesítést kap, nyissa meg a http://*&lt;alkalmazásnév >*. azurewebsites.net a művelet az alapértelmezett JSP servlet megjelenítéséhez.

7. Vissza a web app lap görgessen le a **telepítési hitelesítő adatait** , vagy keressen rá a, majd kattintson rá.

8. A telepítési hitelesítő adatainak beállítása, és kattintson a **Mentés**gombra.

7. Vissza a web app lap, kattintson az **Áttekintés**hivatkozásra. **FTP/telepítési felhasználónév** és a **FTPS hostname (állomásnév)**kattintson a másolandó ezeket az értékeket a **Másolás** gombra.

    ![](./media/app-service-web-get-started-languages/get-ftp-url.png)

    Most már készen a FTPS a Java-alkalmazások terjesztése.

8. Az FTP/FTPS ügyfélprogramban jelentkezzen be az Azure webalkalmazást FTP-kiszolgáló az utolsó lépésben a vágólapra másolt értékekkel. Telepítési, amely a korábban létrehozott jelszót használja.

    Az alábbi képernyőképen látható FileZilla a naplózás.

    ![](./media/app-service-web-get-started-languages/filezilla-login.png)

    Biztonsági figyelmeztetések az Azure unrecognized az SSL-tanúsítvány látható. Jóváhagyást, és továbbra is.

9. Kattintson [erre a hivatkozásra](https://github.com/Azure-Samples/app-service-web-java-get-started/raw/master/webapps/ROOT.war) kattintva töltse le a HÁBORÚ fájlt a helyi számítógépre.

9. Az FTP/FTPS ügyfélprogramban **/site/wwwroot/webapps** a távoli webhelyen keresse meg, és húzza a helyi számítógépre letöltött HÁBORÚ fájl, hogy távoli Directoryban.

    ![](./media/app-service-web-get-started-languages/transfer-war-file.png)

    Kattintson az **OK** felülbírálása az Azure-ban a fájl.

    >[AZURE.NOTE] Megfelelően Tomcat tartozó alapértelmezett működés, /site/wwwroot/webapps **ROOT.war** fájlnév lehetővé teszi a legfelső szintű web app (http://*&lt;alkalmazásnév >*. azurewebsites.net), és a fájlnevet ** * &lt;anyname >*.war** lépve elnevezett webalkalmazást (http://*&lt;alkalmazásnév >*.azurewebsites.net/*&lt;anyname >*).

Az egész! A Java-alkalmazás letöltése fut élő Azure-ban. A böngészőben nyissa meg a http://*&lt;alkalmazásnév >*. azurewebsites.net a funkció működés közben. 

## <a name="make-updates-to-your-app"></a>Végezze el az alkalmazás frissítések

Frissítés szükséges, amikor csak töltse fel az új HÁBORÚ fájl ugyanabban a távoli könyvtárban az FTP/FTPS ügyfélnek.

## <a name="next-steps"></a>Következő lépések

[A Microsoft Azure piactéren található sablonból Java webalkalmazást létrehozása](web-sites-java-get-started.md#marketplace). A saját teljesen testre szabható Tomcat tároló első, és a már jól ismert Manager felhasználói felületének első. 

Az Azure-webalkalmazást, közvetlenül az [IntelliJ](app-service-web-debug-java-web-app-in-intellij.md) vagy [Holdas](app-service-web-debug-java-web-app-in-eclipse.md)hibakeresési.

Másik lehetőségként további műveletek az első webalkalmazást. Példa:

- Próbálja ki az [egyéb módjai az Azure kód telepítése](../app-service-web/web-sites-deploy.md). 
- A következő szintre, hogy az Azure-alkalmazást. A felhasználók hitelesítő. A méretezés igény alapján. Bizonyos teljesítmény értesítések beállítása. Mindezt néhány kattintással. Lásd: [az első webalkalmazást hozzáadása funkciókat](app-service-web-get-started-2.md).

