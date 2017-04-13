<properties
    pageTitle="Java webalkalmazást létrehozása az Azure alkalmazás szolgáltatás |} Microsoft Azure"
    description="Ebből az oktatóanyagból megtudhatja, hogy hogyan Java webes alkalmazások terjesztése Azure alkalmazás szolgáltatásba."
    services="app-service\web"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="get-started-article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-java-web-app-in-azure-app-service"></a>Java webalkalmazást létrehozása az Azure alkalmazás szolgáltatás

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Ebből az oktatóanyagból megtudhatja, hogy miként Java [Azure App szolgáltatásban webalkalmazás] létrehozása az [Azure Portal]segítségével. Az Azure-portálon egy webes felület az Azure erőforrások kezelésére használható.

> [AZURE.NOTE] Oktatóprogram elvégzéséhez a Microsoft Azure-fiókra van szüksége. Ha nem rendelkeznek fiókkal, akkor [aktiválása a Visual Studio előfizetői juttatások] , vagy [regisztráció az ingyenes próbaverziót].
>
> Ha szeretné az első lépések Azure alkalmazás szolgáltatás, mielőtt regisztrál az Azure-fiók, nyissa meg a [Alkalmazás szolgáltatás próbálja meg]. Azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban, Nincs hitelkártya szükség, és nincs nyilatkozatát.

## <a name="java-application-options"></a>Java-alkalmazás beállításai

Számos módon állíthat be egy alkalmazás szolgáltatás webalkalmazásban Java-alkalmazások. 

1. -Alkalmazás létrehozása, és kattintson az **alkalmazás beállításainak**konfigurálása.

    Alkalmazás szolgáltatás Ez a cikk számos Tomcat és rakodóhely, az alapértelmezett beállítás. Szolgáltatója, az alkalmazás egy beépített verziók működnek, ha ezt a módszert webes tároló beállítása a legcélszerűbb, és tökéletes akkor, ha az összes műveletek háború fájl feltöltése a tároló webhelyen. Az ezzel a módszerrel-alkalmazás létrehozása az Azure-portálon, és kattintson a válassza ki a kívánt Java webes tároló együtt Java verziójának az alkalmazás **beállításai** lap. Ha ezt a módszert Java- és a webes tároló vannak futtassa a Program Files. Más módszerek a webes tároló és esetleg a JVM elhelyezése a szabad lemezterület. Ez a modell használata esetén az access-fájlokat szeretne szerkeszteni a fájlrendszer ezen részében nincs. Ez azt jelenti, például nem hajthatók végre konfigurálja az *server.xml* fájlt, illetve a dokumentumtár fájljainak helyezze a */lib* mappában. További tudnivalókért lásd: a [létrehozása és konfigurálása a Java webalkalmazást](#appsettings) ebben az oktatóanyagban című szakaszában.
    
2. A Microsoft Azure piactéren lévő sablonnal.

    Az Azure piactéren elérhető sablonok, amelyek automatikusan létrehozása és konfigurálása a Java web Apps alkalmazások Tomcat vagy rakodóhely webes jegyzettárolót tartalmaz. A webes tárolók, amely a sablonok létrehozása is beállítható. További tudnivalókért olvassa el a az ebben az oktatóanyagban [használata a Microsoft Azure piactéren lévő Java sablon](#marketplace) szakaszát.
  
3. Az alkalmazás, és másoljon manuálisan fájlok létrehozására és szerkesztésére konfigurálása 

    Érdemes lehet egyéni Java-alkalmazások nem telepíthető az alkalmazás szolgáltatás által megadott web tárolók bármelyikét tárolni. Példa:
    
    * Java-alkalmazások Tomcat vagy rakodóhely, amely nem szolgáltatás alkalmazás által támogatott vagy közvetlenül a gyűjteményben megadott verziója szükséges.
    * Java-alkalmazások HTTP kéréseket fogad, és nem üzembe egy HÁBORÚ megegyezik egy már meglévő webes tárolóba.
    * Állítsa be a webes tároló nulláról, saját maga szeretné. 
    * Java, az alkalmazás szolgáltatás nem támogatott verziójának használata és a feltöltendő saját maga szeretné.

    Például ilyen esetben létrehozása az Azure-portálon alkalmazást, és közölje manuálisan a megfelelő futtatókörnyezet fájlokat. Ebben az esetben a fájlok lesznek megszámlálva a hely tárhelykvótáinak alkalmazás szolgáltatás csomagja szemben. További tudnivalókért olvassa el a [feltöltése egy egyéni Java web app Azure]című témakört.

## <a name="portal"></a>Létrehozása és konfigurálása a megfelelő Java web App alkalmazásban

Ez a szakasz mutatja egy webalkalmazás létrehozása és konfigurálása, Java az **alkalmazás beállításai** lap a portál használatával.

1. Jelentkezzen be az [Azure-portálon].

2. Kattintson a **Új > webes + mobil > Web App**.

    ![Új Web App alkalmazásban][newwebapp]

4. Írja be egy nevet a web app a **Web app** mezőbe.

    Ezt a nevet a azurewebsites.net tartományban egyedinek kell lennie, mert a web app URL-címe lesz {nevű}. azurewebsites.net. Ha a név nem egyedi, piros felkiáltójel jelenik meg, a szövegmezőbe.

5. Jelöljön ki egy **Erőforráscsoport** , vagy hozzon létre egy újat.

    Erőforráscsoport kapcsolatos további tudnivalókért olvassa el [az Azure-portálon az Azure erőforrások kezelése]című témakört.

6. Jelölje be az **Alkalmazás szolgáltatás terv/helyét** , vagy hozzon létre egy újat.

    Többet szeretne tudni az App milyen szolgáltatáscsomagok [Azure alkalmazás szolgáltatás csomagok áttekintése]című témakörben találhat.

7. Kattintson a **létrehozása**gombra.

    ![Webalkalmazás létrehozása][newwebapp2]
 
8. Kattintson a web app létrehozásakor **Web Apps alkalmazások > {a webalkalmazás}**.
 
    ![Jelölje ki a Web App alkalmazásban][selectwebapp]

9. A **Web App alkalmazásban** a lap kattintson a **Beállítások**gombra.

10. Kattintson az **alkalmazás beállításait**.

11. Válassza ki a kívánt **Java verziója**. 

12. Válassza ki a kívánt **Java alverzió**. Ha **a legújabbtól**lehetőséget választja, az alkalmazás a legújabb alverzió elérhető App szolgáltatásban az adott Java főverzió használja. A **legújabbtól** elem egyedi a **Alkalmazásbeállítások**létrehozott Java-alkalmazás között. A gyűjteményből az Java-alkalmazás hoz létre, majd esetén kezelheti saját tároló és JVM módosításokat. 

12. Válassza ki a kívánt **tároló webhelyen**. Ha bejelöli **a legújabbtól**kezdődő tároló nevét, az alkalmazás az adott webhely tároló főverzió App szolgáltatásban elérhető legújabb verzióját kell tartani. 

    ![Webes tároló verziók][versions]

13. Kattintson a **Mentés**gombra.

    Belül egy darabig a webalkalmazásban Java-alapú és a webes tároló kijelölt konfigurált válnak.

14. Kattintson a **Web Apps alkalmazások > {az új webalkalmazást}**.

15. Kattintson az **URL-cím** tallózással keresse meg az új webhelyre.

    Az weblapon megerősíti, hogy létrehozott Java-alapú webalkalmazást.

## <a name="marketplace"></a>A Microsoft Azure piactéren lévő Java sablon használata

Ebben a részben használatáról a Microsoft Azure piactéren Java webes alkalmazás létrehozása céljából. Az azonos általános menetrendjét is használható Java-alapú mobile vagy API-alkalmazás létrehozása. 

1. Jelentkezzen be az [Azure portál]

2. Kattintson a **Új > Marketplace**.

    ![Új piactér][newmarketplace]

3. Kattintson a **webhely + Mobile**gombra.

    Előfordulhat, hogy kell görgetnie balra a **piactér** lap választhatja ki a **webes + Mobile**.

4. A keresett szöveg mezőbe írja be a **Apache Tomcat** vagy **rakodóhely**, például egy Java alkalmazás kiszolgáló nevét, és nyomja le az ENTER billentyűt.

5. A keresési eredmények között kattintson a Java application server.

    ![Webes mobil rakodóhely][webmobilejetty]

6. Az első **Apache Tomcat** vagy **rakodóhely** lap kattintson a **Létrehozás**gombra.

    ![A lap homokfogóból portál][jettyblade]

7. A következő **Apache Tomcat** vagy **rakodóhely** lap adja meg egy nevet a web app a **Web app** mezőben.

    Ezt a nevet a azurewebsites.net tartományban egyedinek kell lennie, mert a web app URL-címe lesz {nevű}. azurewebsites.net. Ha a név nem egyedi, piros felkiáltójel jelenik meg, a szövegmezőbe.

8. Jelöljön ki egy **Erőforráscsoport** , vagy hozzon létre egy újat.

    Erőforráscsoport kapcsolatos további tudnivalókért olvassa el [az Azure-portálon az Azure erőforrások kezelése]című témakört.

9. Jelölje be az **Alkalmazás szolgáltatás terv/helyét** , vagy hozzon létre egy újat.

    Többet szeretne tudni az App milyen szolgáltatáscsomagok [Azure alkalmazás szolgáltatás csomagok áttekintése]című témakörben találhat.

10. Kattintson a **létrehozása**gombra.

    ![Homokfogóból portál létrehozása][jettyportalcreate2]

    Rövid idő általában kisebb, mint egy perc Azure fejeződik be az új webalkalmazás létrehozása.

11. Kattintson a **Web Apps alkalmazások > {az új webalkalmazást}**.

12. Kattintson az **URL-cím** tallózással keresse meg az új webhelyre.

    ![Homokfogóból URL-címe][jettyurl]

    Tomcat szállított alapértelmezés szerinti, oldal; Ha úgy döntött, hogy Tomcat, a következőhöz hasonló oldal volna jelenik meg.

    ![Webalkalmazás Apache Tomcat használatával][tomcat]

    Ha úgy döntött, hogy rakodóhely, a következőhöz hasonló oldal volna jelenik meg. Rakodóhely nem tartozik egy alapértelmezett lapon beállított, így itt újra az azonos JSP, amellyel egy üres Java-webhelyre.

    ![Webalkalmazás rakodóhely használatával][jetty]

Most, hogy a web app-alkalmazás tároló létrehozott, a információk arról, hogy miként tölthet fel a a web app alkalmazás a [következő lépések](#next-steps) szakaszban olvashat.

## <a name="next-steps"></a>Következő lépések

Ezen a ponton Ha az Azure-App szolgáltatásban webalkalmazásban Java alkalmazás kiszolgáló. Telepítéshez használni a saját kód a web App alkalmazásban, olvassa el a [alkalmazás hozzáadása vagy az weblapon a Java web App alkalmazásban]című témakört.

Azure-ban Java-alkalmazások fejlesztésével kapcsolatos további tudnivalókért lásd: a [Java Developer Center].

<!-- URL List -->

[Adja hozzá az adott alkalmazás vagy az weblapon a Java web App alkalmazásban]: ./web-sites-java-add-app.md
[Azure alkalmazás szolgáltatás csomagok – áttekintés]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[Azure portál]: https://portal.azure.com/
[a Visual Studio előfizetői juttatások aktiválása]: http://go.microsoft.com/fwlink/?LinkId=623901
[Regisztráljon az ingyenes próbaverzióra]: http://go.microsoft.com/fwlink/?LinkId=623901
[Próbálja meg az App szolgáltatás]: http://go.microsoft.com/fwlink/?LinkId=523751
[webalkalmazás Azure App szolgáltatásban]: http://go.microsoft.com/fwlink/?LinkId=529714
[Java Developer Center]: /develop/java/
[Az Azure portál használatával az Azure erőforrások kezelése]: ../azure-portal/resource-group-portal.md
[Töltse fel a Java egyéni webalkalmazás Azure]: ./web-sites-java-custom-upload.md

<!-- IMG List -->

[newwebapp]: ./media/web-sites-java-get-started/newwebapp.png
[newwebapp2]: ./media/web-sites-java-get-started/newwebapp2.png
[selectwebapp]: ./media/web-sites-java-get-started/selectwebapp.png
[versions]: ./media/web-sites-java-get-started/versions.png
[newmarketplace]: ./media/web-sites-java-get-started/newmarketplace.png
[webmobilejetty]: ./media/web-sites-java-get-started/webmobilejetty.png
[jettyblade]: ./media/web-sites-java-get-started/jettyblade.png
[jettyportalcreate2]: ./media/web-sites-java-get-started/jettyportalcreate2.png
[jettyurl]: ./media/web-sites-java-get-started/jettyurl.png
[tomcat]: ./media/web-sites-java-get-started/tomcat.png
[jetty]: ./media/web-sites-java-get-started/jetty.png
