<properties 
    pageTitle="Helló világ webalkalmazást létrehozása az Azure a IntelliJ |} Microsoft Azure" 
    description="Ebből az oktatóanyagból megtudhatja, hogy miként, Helló világ webalkalmazást az Azure létrehozása az Azure eszközkészlete IntelliJ használatával." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="selvasingh" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-intellij"></a>Helló világ webalkalmazást IntelliJ az Azure létrehozása

Ebből az oktatóanyagból megtudhatja, hogy miként készíthetnek és helyezhetnek üzembe a egy egyszerű Helló, világ alkalmazás Azure a Web App használatával az [Azure eszközkészlete IntelliJ]. Egy egyszerű JSP példa az egyszerűség, de nagyon hasonló lépéseket kell egy Java servlet Azure környezetben illeti.

Ebben az oktatóanyagban befejezése után, az alkalmazás fognak kinézni hasonlóan, mint az alábbi ábrán egy webböngészőben való megtekintéséhez:

![][01]
 
## <a name="prerequisites"></a>Előfeltételek

* Egy Java Developer Kit (JDK), v 1,8 vagy újabb verziója.
* IntelliJ arról Ultimate Edition. Ez a <https://www.jetbrains.com/idea/download/index.html>lehet letölteni.
* Java-alapú webkiszolgálóra vagy alkalmazáskiszolgáló, például Apache Tomcat vagy rakodóhely megoszlása.
* Egy Azure-előfizetést, amelyhez <https://azure.microsoft.com/free/> vagy <http://azure.microsoft.com/pricing/purchase-options/>szerezhetők be.
* A IntelliJ Azure eszközkészlet. További tudnivalókért olvassa el a [IntelliJ az Azure eszközkészlete telepítése]című témakört.

## <a name="to-create-a-hello-world-application"></a>Egy helló világ-alkalmazás létrehozása

Először lássuk először Java projekt létrehozásával.

1. Indítsa el a IntelliJ, és a menüben kattintson a **fájl**, majd az **Új**gombra, és kattintson a **Projekt**.

   ![][02]

1. Új projekt párbeszédpanelen jelölje ki a **Java**, majd **Webalkalmazást**, és kattintson a **Tovább gombra**.

   ![][03a]

   Ha nem rendelt SDK folytatásához arra kéri, kattintson az **Igen**gombra.

   ![][03b]

1. Ebben az oktatóanyagban alkalmazásában a projekt **Java-Web-alkalmazás – a-Azure**nevet, és kattintson a **Befejezés gombra**.

   ![][04]

1. IntelliJ a Project Explorer nézetben bontsa ki a **Java-Web-alkalmazás – a-Azure**, és bontsa ki a **webes**, és kattintson duplán a **index.jsp**parancsra.

   ![][05]

1. A szöveg dinamikusan hozzáadása a index.jsp fájl megnyitásakor a IntelliJ **Helló, világ!** a meglévő belül `<body>` elemet. A frissített `<body>` tartalmat a következő példa kell hasonló:

   `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Mentse a index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Az alkalmazás-Azure Web App tárolóhoz terjesztése

Többféle módon Azure Java webalkalmazás-központilag. Ebben az oktatóanyagban ismerteti a legegyszerűbb közül: egy Azure Web App tároló telepítik az alkalmazás - nincs speciális projekttípus, sem a további eszközök van szükség. A JDK és a webes tároló szoftver biztosítja meg Azure, ezért nincs szükség töltse fel a saját; szükség a Java Web App alkalmazásban. Emiatt a közzétételi folyamat az alkalmazás megnyílik a nem perc, másodperc.

1. A Project Explorer IntelliJ meg kattintson a jobb gombbal a **Java-Web-alkalmazás – a-Azure** projekt. Amikor megjelenik a helyi menü, válassza az **Azure**, és válassza a **Közzététel Azure Web App …**

   ![][06]

1. Ha még nem jelentkezett be IntelliJ az Azure, akkor a rendszer kéri, jelentkezzen be az Azure-fiókjába a:

   ![][07]

   Megjegyzés: Több Azure-fiók esetén a képernyőn megjelenő utasításokat a bejelentkezési folyamat során részét is megjeleníthető többször, még akkor is, ha jelennek meg kell egyeznie. Amikor ez történik, folytassa a következő útmutatást a bejelentkezési.

1. Miután sikeresen aláírták az Azure-fiókjába, az **Előfizetések kezelése** párbeszédpanel a hitelesítő adatok társított előfizetések listáját jeleníti meg. Ha több előfizetéssel szerepel a listában, és csak meghatározott részhalmazát őket dolgozni szeretne, akkor előfordulhat, hogy tetszés szerint törölje a jelet a használni kívánt típusokra. Az előfizetések be van jelölve, kattintson a **Bezárás**gombra.

   ![][08]

1. A **központi telepítés Azure Web App tárolóhoz** párbeszédpanel megjelenésekor jeleníti meg, amely a korábban létrehozott; Web App tárolót Ha bármelyik tárolók nem hozott létre, a lista üres lesz.   

   ![][09]

1. Ha nem hozott létre az Azure Web App tároló előtt, vagy ha szeretné, hogy az alkalmazás új tárolóhoz közzététele, kövesse az alábbi lépéseket. Ellenkező esetben válassza a létező Web App tárolót és ugorjon a 6 alatt.

  1. Kattintson a**+**

        ![][10]

  1. Az **Új Web App tároló** párbeszédpanel jelenik meg, a következő több lépésekkel használandó.

        ![][11]

  1. Írja be a **DNS-címke** a Web App tároló; a levél DNS-címke a webalkalmazás az állomás URL-cím képezik Azure-ban. Megjegyzés: A név elérhető legyen, és Azure Web App elnevezési követelmények felel meg.

  1. A **Webes tároló** legördülő menüben jelölje ki a megfelelő szoftver az alkalmazás.

        Jelenleg Tomcat 8 Tomcat 7 és a 9-es rakodóhely közül választhat. A kijelölt szoftver legutóbbi eloszlás Azure szolgáltatja, és a legutóbbi eloszlás JDK 8 Oracle által létrehozott és Azure által biztosított fog futni.

  1. Az **előfizetés** legördülő menüjében válassza azt az előfizetést, a telepítéshez használni kívánt.

  1. **Erőforráscsoport** legördülő menüben jelölje ki az erőforráscsoport szeretné társítani a Web App alkalmazásban.

        Megjegyzés: Azure erőforráscsoport teszi kapcsolódó források egy csoportba, így például törölhetők együtt.

        Jelölje ki egy meglévő erőforráscsoport (Ha bármilyen) és Ugrás g, az alábbi lépések, vagy használja az alábbi erőforrás új csoport létrehozása az alábbi lépéseket:

      * Kattintson a **Új...**

      * Az **Új erőforráscsoport** párbeszédpanel jelenik meg:

            ![][12]

      * Kattintson a **neve** mezőbe adjon meg egy nevet az új erőforráscsoport.

      * Kattintson a **régió** legördülő menüben válassza a megfelelő Azure adatokat középre igazítása az erőforráscsoport helyét.

      * Kattintson az **OK gombra**.

  1. Az **Alkalmazás szolgáltatás terv** legördülő menü az alkalmazás a kijelölt erőforráscsoport társított milyen szolgáltatáscsomagok sorolja fel.

        Megjegyzés: Egy alkalmazás szolgáltatás megtervezése adja meg az információkat, például a hely a Web App alkalmazásban, a árak réteg és a számítási példány méretét. Egy egyetlen alkalmazás szolgáltatás megtervezése több Web Apps alkalmazások, ezért azt az egy adott Web App-telepítés külön-külön megmarad használható.

        Jelölje ki egy meglévő alkalmazás szolgáltatás terv (Ha bármilyen) és Ugrás h, az alábbi lépések, vagy használja az alábbi hozhat létre egy új alkalmazás szolgáltatás megtervezése alábbi lépéseket:

      * Kattintson a **Új...**

      * Az **Új alkalmazás szolgáltatás megtervezése** párbeszédpanel jelenik meg:

            ![][13]

      * Kattintson a **neve** mezőbe adjon meg egy nevet az új alkalmazás szolgáltatás megtervezése.

      * Az a **hely** legördülő menüjében válassza a megfelelő Azure adatokat középre a terv helyét.

      * Az a **Szint árak** legördülő menüben jelölje ki a megfelelő árak, a terv. Tesztelés céljából megadhatja az **ingyenes**.

      * Az a a **Példány méret** legördülő menü, jelölje be a megfelelő példányt a terv mérete. Tesztelés céljából **kis**is választhat.

  1. Miután befejeződött a fenti lépéseket, a az új Web App tároló párbeszédpanel az alábbi ábrán kell hasonló:

        ![][14]

  1. Kattintson az **OK** fejezheti be az új Web App-tároló.

        Várjon néhány másodpercig, frissíteni kell a Web App tárolók listáját, és az újonnan létrehozott web app tároló kijelölődik a listában.

1. Most már készen áll a webes alkalmazás Azure; a kezdeti telepítés befejezéséhez Kattintson az **OK gombra** a a kijelölt webalkalmazás tárolóhoz Java-alkalmazás telepítéséhez.

    ![][15]

    Megjegyzés: Alapértelmezés szerint az alkalmazás fog telepíthető az application Server alkönyvtáraként. Ha azt szeretné, hogy a legfelső szintű alkalmazásként telepíthető, jelölje be a **legfelső szintű Deploy** jelölőnégyzet előtt kattintson az **OK gombra**.

1. Következő lépésként meg kell jelennie az **Azure tevékenységnapló** nézetben, amely jelzi a Web App telepítési állapotának.

    ![][16]

    A Web App bevezetéshez Azure folyamata csak néhány másodpercbe kell vennie. Amikor az alkalmazás azonnal megjelenik az **állapot** oszlopban a **közzétett** nevű hivatkozást. A hivatkozásra, megnyílik a telepített webalkalmazás kezdőlapra, vagy használhatja a lépéseket a következő szakaszban keresse meg a web App alkalmazásban.

## <a name="browsing-to-your-web-app-on-azure"></a>A Web App alkalmazásban a Azure böngészés

A Web App alkalmazásban a Azure brows, hogy az **Azure-tallózó** nézet is használhatja.

Ha még nincs megnyitva az **Azure-tallózó** nézet, **Nézet** menüjének IntelliJ, majd kattintással nyissa meg, majd kattintson az **Eszköz Windows**, és kattintson a **Szolgáltatás Intéző**. Ha korábban már nem jelentkezik, hogy kérni fogja erre.

Az **Azure-tallózó** nézet jelenik meg, amikor a használata kövesse az alábbi lépéseket a Web App leállítása: 

1. Bontsa ki az **Azure** csomópontot.

1. Bontsa ki a **Web Apps alkalmazások** csomópontot. 

1. Kattintson a jobb gombbal a kívánt Web App alkalmazásban.

1. Amikor megjelenik a helyi menü, kattintson a **Megnyitás böngészőben**.

    ![][17]

## <a name="updating-your-web-app"></a>A webes alkalmazás frissítése

Frissítése egy meglévő Azure Web App alkalmazást futtató egy gyors és egyszerű eljárás, és frissítése két lehetőség közül választhat:

* Frissítheti az egy meglévő Java Web App telepítését.
* A Web App azonos tároló további Java alkalmazás közzéteheti.

Bármelyik lehetőséget választja a folyamat azonos, és csak néhány másodpercet vesz igénybe:

1. A IntelliJ Projektböngésző kattintson a jobb gombbal a Java-alkalmazás frissítése, illetve egy meglévő Web App tároló felvenni kívánt.

1. Amikor megjelenik a helyi menü, válassza az **Azure** , majd a **Közzététel Azure Web App …**

1. Mivel meg van már bejelentkezett korábbi, láthatja a meglévő webalkalmazás tárolók listáját. Jelölje ki a kívánt közzététele vagy tegye közzé újra az Java alkalmazást, és kattintson az **OK gombra**.

Néhány másodpercet később **Azure tevékenységnapló** látható **közzétéve** , a frissített üzembe, és nem fogja ellenőrizheti a frissített alkalmazás egy webböngészőben.

## <a name="starting-or-stopping-an-existing-web-app"></a>Elindítása és leállítása egy meglévő Web App alkalmazásban

Indítsa el, illetve függeszthetem fel a meglévő Azure Web App tároló, (beleértve az összes telepített Java-alkalmazások meg), az **Azure-tallózó** nézet is használhatja.

Ha még nincs megnyitva az **Azure-tallózó** nézet, **Nézet** menüjének IntelliJ, majd kattintással nyissa meg, majd kattintson az **Eszköz Windows**, és kattintson a **Szolgáltatás Intéző**. Ha korábban már nem jelentkezik, hogy kérni fogja erre.

Az **Azure-tallózó** nézet jelenik meg, amikor a használata tegye a következőket elindítása és leállítása a Web App alkalmazásban: 

1. Bontsa ki az **Azure** csomópontot.

1. Bontsa ki a **Web Apps alkalmazások** csomópontot. 

1. Kattintson a jobb gombbal a kívánt Web App alkalmazásban.

1. Amikor megjelenik a helyi menü, kattintson a **indítása** és **leállítása**. Ne feledje, hogy a menüelemek helyi-et, hogy csak egy futó webalkalmazás leállítása vagy indítsa el a megfelelő web App alkalmazásban, amely jelenleg nem működik.

    ![][18]

## <a name="next-steps"></a>Következő lépések

Java IDEs az Azure eszközök gazdag kapcsolatban további információt az alábbi helyeken találhat:

- [Azure eszközkészlete Holdas]
  - [Az Azure eszközkészlet Holdas telepítése]
  - [Helló világ webalkalmazást Holdas az Azure létrehozása]
  - [Az Azure eszközkészlete Holdas újdonságai]
- [Azure eszközkészlete IntelliJ]
  - [Az Azure eszközkészlet IntelliJ telepítése]
  - *Helló világ webalkalmazást létrehozása az Azure a IntelliJ (Ez a cikk)*
  - [Az Azure eszközkészlete IntelliJ újdonságai]

Java Azure használatával kapcsolatos további tudnivalókért olvassa el a az [Azure Java Developer Center]című témakört.

Azure Web Apps alkalmazások létrehozásáról további információt a a [Web Apps alkalmazások – áttekintés]című témakörben találhat.

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure eszközkészlete Holdas]: ../azure-toolkit-for-eclipse.md
[Azure eszközkészlete IntelliJ]: ../azure-toolkit-for-intellij.md
[Helló világ webalkalmazást Holdas az Azure létrehozása]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Az Azure eszközkészlet Holdas telepítése]: ../azure-toolkit-for-eclipse-installation.md
[Az Azure eszközkészlet IntelliJ telepítése]: ../azure-toolkit-for-intellij-installation.md
[Az Azure eszközkészlete Holdas újdonságai]: ../azure-toolkit-for-eclipse-whats-new.md
[Az Azure eszközkészlete IntelliJ újdonságai]: ../azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Web Apps alkalmazások áttekintése]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-intellij-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-intellij-create-hello-world-web-app/02-File-New-Project.png
[03a]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-Dialog.png
[03b]: ./media/app-service-web-intellij-create-hello-world-web-app/03-No-SDK-Specified.png
[04]: ./media/app-service-web-intellij-create-hello-world-web-app/04-New-Project-Dialog.png
[05]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Index-Page.png
[06]: ./media/app-service-web-intellij-create-hello-world-web-app/06-Azure-Publish-Context-Menu.png
[07]: ./media/app-service-web-intellij-create-hello-world-web-app/07-Azure-Log-In-Dialog.png
[08]: ./media/app-service-web-intellij-create-hello-world-web-app/08-Manage-Subscriptions.png
[09]: ./media/app-service-web-intellij-create-hello-world-web-app/09-App-Containers.png
[10]: ./media/app-service-web-intellij-create-hello-world-web-app/10-Add-App-Container.png
[11]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container.png
[12]: ./media/app-service-web-intellij-create-hello-world-web-app/12-New-Resource-Group.png
[13]: ./media/app-service-web-intellij-create-hello-world-web-app/13-New-App-Service-Plan.png
[14]: ./media/app-service-web-intellij-create-hello-world-web-app/14-New-App-Container.png
[15]: ./media/app-service-web-intellij-create-hello-world-web-app/15-Deploy-To-Azure.png
[16]: ./media/app-service-web-intellij-create-hello-world-web-app/16-Progress-Indicator.png
[17]: ./media/app-service-web-intellij-create-hello-world-web-app/17-Browse-Web-App.png
[18]: ./media/app-service-web-intellij-create-hello-world-web-app/18-Stop-Web-App.png
