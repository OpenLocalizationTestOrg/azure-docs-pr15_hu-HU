<properties 
    pageTitle="Helló világ webalkalmazást létrehozása az Azure a Holdas |} Microsoft Azure" 
    description="Ebből az oktatóanyagból megtudhatja, hogy miként, Helló világ webalkalmazást az Azure létrehozása az Azure eszközkészlete Holdas használatával." 
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
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-eclipse"></a>Helló világ webalkalmazást Holdas az Azure létrehozása

Ebből az oktatóanyagból megtudhatja, hogy miként készíthetnek és helyezhetnek üzembe a egy egyszerű Helló, világ alkalmazás Azure a Web App használatával az [Azure eszközkészlete Holdas]. Egy egyszerű JSP példa az egyszerűség, de nagyon hasonló lépéseket kell egy Java servlet Azure környezetben illeti.

Ebben az oktatóanyagban befejezése után, az alkalmazás fognak kinézni hasonlóan, mint az alábbi ábrán egy webböngészőben való megtekintéséhez:

![Előnézete helló világ alkalmazás][01]
 
## <a name="prerequisites"></a>Előfeltételek

* Egy Java Developer Kit (JDK), v 1,8 vagy újabb verziója.
* IDE Holdas a Java EE fejlesztők számára Luna vagy újabb verziójában. Ez a <http://www.eclipse.org/downloads/>lehet letölteni.
* Java-alapú webkiszolgálóra vagy alkalmazáskiszolgáló, például Apache Tomcat vagy rakodóhely megoszlása.
* Egy Azure-előfizetést, amelyhez <https://azure.microsoft.com/free/> vagy <http://azure.microsoft.com/pricing/purchase-options/>szerezhetők be.
* A Holdas Azure eszközkészlet. További tudnivalókért olvassa el a [Holdas az Azure eszközkészlete telepítése]című témakört.

## <a name="to-create-a-hello-world-application"></a>Egy helló világ-alkalmazás létrehozása

Először lássuk először Java projekt létrehozásával.

1. Holdas, indítsa el és a menüt, kattintson a **fájl**, kattintson az **Új**gombra, és válassza a **Dinamikus a Project Web**. (Ha nem látja a **fájlt** , és az **Új**parancsra kattintást követően a rendelkezésre álló projektként felsorolt **Dinamikus webes projekt** , majd tegye a következőket: kattintson a **fájl**, kattintson az **Új**, kattintson a **projekt...**, bontsa ki a **webes**, kattintson a **Dinamikus webhely projekt**, kattintson a **Tovább**gombra.)

1. Ebben az oktatóanyagban céljából adjon nevet a projekt **SajátWebalk**. A képernyő jelenik meg az alábbihoz hasonló:

    ![A dinamikus webhely új projekt létrehozása][02]

1. Kattintson a **Befejezés gombra**.

1. Belül Holdas a Project Explorer nézetben bontsa ki a **SajátWebalk**. Kattintson a jobb gombbal a **Web tartalma**, kattintson az **Új**gombra, és válassza a **Fájl JSP**.

1. Az **Új JSP fájlt** párbeszédpanelen **index.jsp**fájl nevét a szülőmappát, mint **SajátWebalk/Web tartalma**megőrzése, és kattintson a **Tovább gombra**.

1. **JSP sablon kiválasztása** párbeszédpanelen alkalmazásában ebben az oktatóanyagban jelölje be az **Új JSP fájl (html)**, és kattintson a **Befejezés gombra**.

1. A szöveg dinamikusan hozzáadása a index.jsp fájl megnyitásakor a Holdas **Helló, világ!** a meglévő belül `<body>` elemet. A frissített `<body>` tartalmat a következő példa kell hasonló:

    `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Mentse a index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Az alkalmazás-Azure Web App tárolóhoz terjesztése

Többféle módon Azure Java webalkalmazás-központilag. Ebben az oktatóanyagban ismerteti a legegyszerűbb közül: egy Azure Web App tároló telepítik az alkalmazás - nincs speciális projekttípus, sem a további eszközök van szükség. A JDK és a webes tároló szoftver biztosítja meg Azure, ezért nincs szükség töltse fel a saját; szükség a Java Web App alkalmazásban. Emiatt a közzétételi folyamat az alkalmazás megnyílik a nem perc, másodperc.

1. A Project Explorer Holdas meg kattintson a jobb gombbal **SajátWebalk**.

1. A helyi menüben válassza az **Azure**, majd kattintson a **Közzététel Azure Web App …**

    ![Az Azure webalkalmazás közzététele][03]
   
    Azt is megteheti amíg a webes alkalmazás projektet a Project Explorer ki van jelölve, azt is megteheti kattintson a **Közzététel** legördülő gombra az eszköztáron és **Azure Web App közzététel** innen:
   
    ![Az Azure webalkalmazás közzététele][14]
   
1. Ha még nem jelentkezett be Holdas az Azure, akkor a rendszer kéri, jelentkezzen be az Azure-fiókjába a:

    ![Azure bejelentkezés párbeszédpanel][04]
   
    Több Azure-fiók esetén a képernyőn megjelenő utasításokat a bejelentkezési folyamat során részét is megjeleníthető többször, még akkor is, ha jelennek meg kell egyeznie. Amikor ez történik, folytassa a következő útmutatást a bejelentkezési.

1. Miután sikeresen aláírták az Azure-fiókjába, az **Előfizetések kezelése** párbeszédpanel a hitelesítő adatok társított előfizetések listáját jeleníti meg. Ha több előfizetéssel szerepel a listában, és csak meghatározott részhalmazát őket dolgozni szeretne, akkor előfordulhat, hogy tetszés szerint törölje a jelet a használni kívánt típusokra. Az előfizetések be van jelölve, kattintson a **Bezárás**gombra.

    ![Az előfizetések párbeszédpanel kezelése][05]
   
1. A **központi telepítés Azure Web App tárolóhoz** párbeszédpanel megjelenésekor jeleníti meg, amely a korábban létrehozott; Web App tárolót Ha bármelyik tárolók nem hozott létre, a lista üres lesz.

    ![Azure Web App tároló párbeszédpanel terjesztése][06]
   
1. Ha nem hozott létre az Azure Web App tároló előtt, vagy ha szeretné, hogy az alkalmazás új tárolóhoz közzététele, kövesse az alábbi lépéseket. Ellenkező esetben válassza a létező Web App tárolót és ugorjon a 7 alatt.

    1. Kattintson a **Új...**

        ![Azure Web App tároló párbeszédpanel terjesztése][15]

    1. Az **Új Web App tároló** párbeszédpanel jelenik meg:

        ![Új Web App tároló párbeszédpanel][07a]

    1. Írja be a **DNS-címke** a Web App tároló; a levél DNS-címke a webalkalmazás az állomás URL-cím képezik Azure-ban. (Tájékoztatjuk, hogy a név érhető el kell, és Azure Web App elnevezési követelmények felelnek meg.)

    1. A **Webes tároló** legördülő menüben jelölje ki a megfelelő szoftver az alkalmazás.

        Jelenleg Tomcat 8 Tomcat 7 és a 9-es rakodóhely közül választhat. A kijelölt szoftver legutóbbi eloszlás Azure szolgáltatja, és a legutóbbi eloszlás JDK 8 Oracle által létrehozott és Azure által biztosított fog futni.

    1. Az **előfizetés** legördülő menüjében válassza azt az előfizetést, a telepítéshez használni kívánt.

    1. **Erőforráscsoport** legördülő menüben jelölje ki az erőforráscsoport szeretné társítani a Web App alkalmazásban. (Azure erőforráscsoport teszi kapcsolódó források egy csoportba, így például törölhetők közös.)

        Jelölje ki egy meglévő erőforráscsoport (Ha bármilyen) és Ugrás g, az alábbi lépések, vagy használja az alábbi erőforrás új csoport létrehozása az alábbi lépéseket:

        * Kattintson a **Új...**

        * Az **Új erőforráscsoport** párbeszédpanel jelenik meg:

            ![Új erőforráscsoport párbeszédpanel][08]

        * Kattintson a **neve** mezőbe adjon meg egy nevet az új erőforráscsoport.

        * Kattintson a **régió** legördülő menüben válassza a megfelelő Azure adatokat középre igazítása az erőforráscsoport helyét.

        * Nem kötelező: Alapértelmezés szerint Java 8 legutóbbi eloszlásának telepítik Azure által automatikusan a web app tároló, a JVM. Jó helyen jár akkor beállíthatja egy másik verzióját, és a JVM megoszlása a Web App megkívánja. A Web App alkalmazásban a JDK megadásához kattintson a **JDK** fülre, és válasszon az alábbi lehetőségek közül:

            * **Az alapértelmezett Azure Web Apps alkalmazások szolgáltatás által kínált JDK Deploy**: ezt a lehetőséget a legutóbbi elosztása Java 8 telepíteni fog.

            * **Egy 3rd fél JDK Azure a Deploy**: Ezzel a beállítással, amely a Microsoft Azure által nyújtott JDKs listából választhatja ki.

            * **A saját JDK erről a helyről letöltési Deploy**: Ezzel a beállítással megadhatja saját JDK eloszlás, amely kell csomagolt ZIP-fájlként, és a nyilvánosan elérhető letöltési helyére vagy egy Azure tárterület-fiókot, amelyhez hozzáférése van feltöltve.

            ![Új Web App tároló párbeszédpanel][07b]

    * Kattintson az **OK gombra**.

    1. Az **Alkalmazás szolgáltatás terv** legördülő menü az alkalmazás a kijelölt erőforráscsoport társított milyen szolgáltatáscsomagok sorolja fel. (App milyen szolgáltatáscsomagok kitöltését például a hely a Web App alkalmazásban, a árak réteg és a számítási példány méretét. Egy egyetlen alkalmazás szolgáltatás megtervezése használható több Web Apps alkalmazások, ezért azt az egy adott Web App-telepítés külön-külön megmarad.)

        Jelölje ki egy meglévő alkalmazás szolgáltatás terv (Ha bármilyen) és Ugrás h, az alábbi lépések, vagy használja az alábbi hozhat létre egy új alkalmazás szolgáltatás megtervezése alábbi lépéseket:

        * Kattintson a **Új...**

        * Az **Új alkalmazás szolgáltatás megtervezése** párbeszédpanel jelenik meg:

            ![Új alkalmazás szolgáltatás tervezése párbeszédpanelen][09]

        * Kattintson a **neve** mezőbe adjon meg egy nevet az új alkalmazás szolgáltatás megtervezése.

        * Az a **hely** legördülő menüjében válassza a megfelelő Azure adatokat középre a terv helyét.

        * Az a **Szint árak** legördülő menüben jelölje ki a megfelelő árak, a terv. Tesztelés céljából megadhatja az **ingyenes**.

        * Az a a **Példány méret** legördülő menü, jelölje be a megfelelő példányt a terv mérete. Tesztelés céljából **kis**is választhat.

    1. Miután befejeződött a fenti lépéseket, a az új Web App tároló párbeszédpanel az alábbi ábrán kell hasonló:

        ![Új Web App tároló párbeszédpanel][10]

    1. Kattintson az **OK** fejezheti be az új Web App-tároló.

        Várjon néhány másodpercig, frissíteni kell a Web App tárolók listáját, és az újonnan létrehozott web app tároló kijelölődik a listában.

1. Most már készen áll a Web App Azure a kezdeti telepítése:

    ![Azure Web App tároló párbeszédpanel terjesztése][11]

    Kattintson az **OK gombra** a a kijelölt webalkalmazás tárolóhoz Java-alkalmazás telepítéséhez.

    Alapértelmezés szerint az alkalmazás telepítését az application Server alkönyvtáraként kell. Ha azt szeretné, hogy a legfelső szintű alkalmazásként telepíthető, jelölje be a **legfelső szintű Deploy** jelölőnégyzet előtt kattintson az **OK gombra**.

1. Következő lépésként meg kell jelennie az **Azure tevékenységnapló** nézetben, amely jelzi a Web App telepítési állapotának.

    ![Azure tevékenységnapló][12]

    A Web App bevezetéshez Azure folyamata csak néhány másodpercbe kell vennie. Amikor az alkalmazás azonnal megjelenik az **állapot** oszlopban a **közzétett** nevű hivatkozást. A hivatkozásra kattint, megnyílik, a telepített webalkalmazás kezdőlapjára.

## <a name="updating-your-web-app"></a>A webes alkalmazás frissítése

Frissítése egy meglévő Azure Web App alkalmazást futtató egy gyors és egyszerű eljárás, és frissítése két lehetőség közül választhat:

* Frissítheti az egy meglévő Java Web App telepítését.
* A Web App azonos tároló további Java alkalmazás közzéteheti.

Bármelyik lehetőséget választja a folyamat azonos, és csak néhány másodpercet vesz igénybe:

1. A Holdas Projektböngésző kattintson a jobb gombbal a Java-alkalmazás frissítése, illetve egy meglévő Web App tároló felvenni kívánt.

1. Amikor megjelenik a helyi menü, válassza az **Azure** , majd a **Közzététel Azure Web App …**

1. Mivel meg van már bejelentkezett korábbi, láthatja a meglévő webalkalmazás tárolók listáját. Jelölje ki a kívánt közzététele vagy tegye közzé újra az Java alkalmazást, és kattintson az **OK gombra**.

Néhány másodpercet később **Azure tevékenységnapló** látható **közzétéve** , a frissített üzembe, és nem fogja ellenőrizheti a frissített alkalmazás egy webböngészőben.

## <a name="stopping-an-existing-web-app"></a>Egy meglévő webalkalmazás leállítása

Le szeretné állítani egy meglévő Azure Web App tároló, (beleértve az összes telepített Java-alkalmazások meg), az **Azure-tallózó** nézet is használhatja.

Ha még nincs megnyitva az **Azure-tallózó** nézet, majd Holdas, az **ablak** menüre kattintva nyissa meg, majd kattintson a **Nézet megjelenítése** **más...**, majd a **Azure**, és válassza az **Azure Explorer**. Ha korábban már nem jelentkezik, hogy kérni fogja erre.

Az **Azure-tallózó** nézet jelenik meg, amikor a használata kövesse az alábbi lépéseket a Web App leállítása: 

1. Bontsa ki az **Azure** csomópontot.

1. Bontsa ki a **Web Apps alkalmazások** csomópontot. 

1. Kattintson a jobb gombbal a kívánt Web App alkalmazásban.

1. Amikor megjelenik a helyi menü, kattintson a **Leállítás**gombra.

    ![Egy meglévő webalkalmazás leállítása][13]

## <a name="next-steps"></a>Következő lépések

Java IDEs az Azure eszközök gazdag kapcsolatban további információt az alábbi helyeken találhat:

- [Azure eszközkészlete Holdas]
  - [Az Azure eszközkészlet Holdas telepítése]
  - *Helló világ webalkalmazást létrehozása az Azure a Holdas (Ez a cikk)*
  - [Az Azure eszközkészlete Holdas újdonságai]
- [Azure eszközkészlete IntelliJ]
  - [Az Azure eszközkészlet IntelliJ telepítése]
  - [Helló világ webalkalmazást IntelliJ az Azure létrehozása]
  - [Az Azure eszközkészlete IntelliJ újdonságai]

Java Azure használatával kapcsolatos további tudnivalókért olvassa el a az [Azure Java Developer Center]című témakört.

Azure Web Apps alkalmazások létrehozásáról további információt a a [Web Apps alkalmazások – áttekintés]című témakörben találhat.

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure eszközkészlete Holdas]: ../azure-toolkit-for-eclipse.md
[Azure eszközkészlete IntelliJ]: ../azure-toolkit-for-intellij.md
[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Helló világ webalkalmazást IntelliJ az Azure létrehozása]: ./app-service-web-intellij-create-hello-world-web-app.md
[Az Azure eszközkészlet Holdas telepítése]: ../azure-toolkit-for-eclipse-installation.md
[Az Azure eszközkészlet IntelliJ telepítése]: ../azure-toolkit-for-intellij-installation.md
[Az Azure eszközkészlete Holdas újdonságai]: ../azure-toolkit-for-eclipse-whats-new.md
[Az Azure eszközkészlete IntelliJ újdonságai]: ../azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Web Apps alkalmazások áttekintése]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-eclipse-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-eclipse-create-hello-world-web-app/02-Dynamic-Web-Project.png
[03]: ./media/app-service-web-eclipse-create-hello-world-web-app/03-Context-Menu.png
[04]: ./media/app-service-web-eclipse-create-hello-world-web-app/04-Log-In-Dialog.png
[05]: ./media/app-service-web-eclipse-create-hello-world-web-app/05-Manage-Subscriptions-Dialog.png
[06]: ./media/app-service-web-eclipse-create-hello-world-web-app/06-Deploy-To-Azure-Web-Container.png
[07a]: ./media/app-service-web-eclipse-create-hello-world-web-app/07a-New-Web-App-Container-Dialog.png
[07b]: ./media/app-service-web-eclipse-create-hello-world-web-app/07b-New-Web-App-Container-Dialog.png
[08]: ./media/app-service-web-eclipse-create-hello-world-web-app/08-New-Resource-Group-Dialog.png
[09]: ./media/app-service-web-eclipse-create-hello-world-web-app/09-New-Service-Plan-Dialog.png
[10]: ./media/app-service-web-eclipse-create-hello-world-web-app/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/app-service-web-eclipse-create-hello-world-web-app/11-Completed-Deploy-Dialog.png
[12]: ./media/app-service-web-eclipse-create-hello-world-web-app/12-Activity-Log-View.png
[13]: ./media/app-service-web-eclipse-create-hello-world-web-app/13-Azure-Explorer-Web-App.png
[14]: ./media/app-service-web-eclipse-create-hello-world-web-app/14-publishDropdownButton.png
[15]: ./media/app-service-web-eclipse-create-hello-world-web-app/15-New-Azure-Web-Container.png
