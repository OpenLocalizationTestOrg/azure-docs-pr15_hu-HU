<properties 
    pageTitle="A IntelliJ az Azure Java webalkalmazást hibakeresési |} Microsoft Azure" 
    description="Ebből az oktatóanyagból megtudhatja, hogy miként, történő használatát az Azure eszközkészlet IntelliJ az Azure futó Java webalkalmazást." 
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
    ms.date="09/20/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="debug-a-java-web-app-on-azure-in-intellij"></a>A IntelliJ az Azure Java webalkalmazást hibakeresése

Ebből az oktatóanyagból megtudhatja, hogy miként hibakeresési Azure futó az [Azure eszközkészlete IntelliJ]használatával Java webalkalmazást. Az egyszerűség kedvéért ebben az oktatóprogramban egy egyszerű Java kiszolgáló lap (JSP) Példa használni, de a lépéseket a Java servlet hasonló lenne, amikor a Azure hibakeresés alatt.

Ebben az oktatóanyagban befejezése után, az alkalmazás fognak kinézni hasonlóan, mint az alábbi ábra is hibakeresési IntelliJ:

![][01]
 
## <a name="prerequisites"></a>Előfeltételek

* Egy Java Developer Kit (JDK), v 1,8 vagy újabb verziója.
* IntelliJ arról Ultimate Edition. Ez a <https://www.jetbrains.com/idea/download/index.html>lehet letölteni.
* Java-alapú webkiszolgálóra vagy alkalmazáskiszolgáló, például Apache Tomcat vagy rakodóhely megoszlása.
* Egy Azure-előfizetést, amelyhez <https://azure.microsoft.com/en-us/free/> vagy <http://azure.microsoft.com/pricing/purchase-options/>szerezhetők be.
* A IntelliJ Azure eszközkészlet. További tudnivalókért olvassa el a [IntelliJ az Azure eszközkészlete telepítése]című témakört.
* A dinamikus webhely projekt létrehozott és Azure alkalmazás szolgáltatás; rendszerbe Ha például lásd [létrehozása helló világ webalkalmazást IntelliJ az Azure].

## <a name="to-debug-a-java-web-app-on-azure"></a>A Azure Java webalkalmazást hibakeresése

Ebben a szakaszban a lépések elvégzéséhez, használata a dinamikus webhely projekt, amely már telepítette a Azure Java webalkalmazást, töltse le a [Minta dinamikus webes projekt] [létrehozása helló világ webalkalmazást az Azure IntelliJ az] Azure telepítendő lépéseket. 

1. Nyissa meg a Java Web App-projektet, amely a IntelliJ Azure rendszerbe állított.

1. Kattintson a **Futtatás** menüre, és kattintson a **Beállítások szerkesztése**gombra.

    ![][02]

1. Ha a **Futtatás/hibakeresési beállítások** párbeszédpanel megnyitása: 

    1. Jelölje ki az **Azure Web App alkalmazásban**.
    1. Kattintson a **+** hozzáadása egy új konfigurációt.
    1. Adjon **nevet** a konfiguráció.
    1. Fogadja el a hátralévő az alapértelmezett értékeket, amely szerint az Azure eszközkészlet használata javasolt, és kattintson **az OK**gombra.

        ![][03]

1. Válassza ki az Azure Web App hibakeresési konfigurációt jobb felső részén a menü éppen létrehozott, és kattintson a **hibakeresési**

    ![][04]

1. Amikor a rendszer megkérdezi, hogy **távoli hibakeresés engedélyezése a távoli webalkalmazásban most?**, kattintson az **OK gombra**.

1. Amikor a rendszer kéri, hogy **a webalkalmazás ettől kezdve készen áll a távoli hibakereséshez**, kattintson az **OK gombra**.

    ![][05]

1. Válassza ki az Azure Web App hibakeresési konfigurációt jobb felső részén a menü éppen létrehozott, és válassza a **hibakeresési**.

1. Egy Windows parancssor vagy Unix rendszerhéj nyissa meg, és felkészülés szükséges kapcsolat, a hibakereséshez. meg kell várnia, amíg a kapcsolatot a távoli Java Web App befejeződött, a folytatás előtt. Ha Windows használata esetén az alábbi ábrán fog megjelenni:

    ![][06]

1. Egy oldaltörés pont beszúrása a JSP lapon, majd az Java-webalkalmazást a böngészőben nyissa meg az URL-címe:

    1. **Azure Explorer** IntelliJ megnyitása.
    1. Nyissa meg a **Web Apps alkalmazások** és a Java Web App szeretné hibakeresése.
    1. Kattintson a Web App alkalmazásban kattintson a jobb gombbal, és kattintson a **Megnyitás böngészőben**parancsra.
    1. IntelliJ most hibakeresési üzemmódba adja meg.

## <a name="next-steps"></a>Következő lépések

Java Azure használatával kapcsolatos további tudnivalókért olvassa el a az [Azure Java Developer Center]című témakört.

Azure Web Apps alkalmazások létrehozásáról további információt a a [Web Apps alkalmazások – áttekintés]című témakörben találhat.

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure eszközkészlete IntelliJ]: ../azure-toolkit-for-intellij.md
[Az Azure eszközkészlet IntelliJ telepítése]: ../azure-toolkit-for-intellij-installation.md
[Helló világ webalkalmazást IntelliJ az Azure létrehozása]: ./app-service-web-intellij-create-hello-world-web-app.md
[Minta dinamikus webes projekt]: http://go.microsoft.com/fwlink/?LinkId=817337

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Web Apps alkalmazások áttekintése]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-intellij/01-debug-java-web-app-in-intellij.png
[02]: ./media/app-service-web-debug-java-web-app-in-intellij/02-configure-intellij-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-intellij/03-debug-configuration.png
[04]: ./media/app-service-web-debug-java-web-app-in-intellij/04-select-debug.png
[05]: ./media/app-service-web-debug-java-web-app-in-intellij/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-intellij/06-windows-command-prompt-connection-successful-to-remote.png
