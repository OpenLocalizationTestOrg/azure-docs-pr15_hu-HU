<properties 
    pageTitle="A Holdas az Azure Java webalkalmazást hibakeresési |} Microsoft Azure" 
    description="Ebből az oktatóanyagból megtudhatja, hogy miként, történő használatát az Azure eszközkészlet Holdas az Azure futó Java webalkalmazást." 
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

# <a name="debug-a-java-web-app-on-azure-in-eclipse"></a>A Holdas az Azure Java webalkalmazást hibakeresése

Ebből az oktatóanyagból megtudhatja, hogy miként hibakeresési Azure futó az [Azure eszközkészlete Holdas]használatával Java webalkalmazást. Az egyszerűség kedvéért ebben az oktatóprogramban egy egyszerű Java kiszolgáló lap (JSP) Példa használni, de a lépéseket a Java servlet hasonló lenne, amikor a Azure hibakeresés alatt.

Ebben az oktatóanyagban befejezése után, az alkalmazás fognak kinézni hasonlóan, mint az alábbi ábra is hibakeresési Holdas:

![][01]
 
## <a name="prerequisites"></a>Előfeltételek

* Egy Java Developer Kit (JDK), v 1,8 vagy újabb verziója.
* IDE Holdas Java EE fejlesztőknek Indigo vagy újabb verziója. Ez a <http://www.eclipse.org/downloads/>lehet letölteni.
* Java-alapú webkiszolgálóra vagy alkalmazáskiszolgáló, például Apache Tomcat vagy rakodóhely megoszlása.
* Egy Azure-előfizetést, amelyhez <https://azure.microsoft.com/en-us/free/> vagy <http://azure.microsoft.com/pricing/purchase-options/>szerezhetők be.
* A Holdas Azure eszközkészlet. További tudnivalókért olvassa el a [Holdas az Azure eszközkészlete telepítése]című témakört.
* A dinamikus webhely projekt létrehozott és Azure alkalmazás szolgáltatás; rendszerbe Ha például lásd [létrehozása helló világ webalkalmazást Holdas az Azure].

## <a name="to-debug-a-java-web-app-on-azure"></a>A Azure Java webalkalmazást hibakeresése

Ebben a szakaszban a lépések elvégzéséhez, használata a dinamikus webhely projekt, amely már telepítette a Azure Java webalkalmazást, töltse le a [Minta dinamikus webes projekt] [létrehozása helló világ webalkalmazást az Azure Holdas az] Azure telepítendő lépéseket. 

1. Nyissa meg a Holdas.

1. Állítsa be a távoli hibakereséshez adatokon való műveletvégzés:

    1. A Holdas **Windows** menüjét, és kattintson a **Beállítások**gombra.
    1. Bontsa ki a **Java** csomópontot, majd jelölje be a **hibakeresési**.
    1. Beállításainak mind a **Debugger időtúllépése (ms)** , és **Indítsa el időtúllépése (ms)** `120000`.

        ![][02]

    1. Kattintson az **OK gombra** a **Beállítások** párbeszédpanel bezárásához.

1. Holdas a Project Explorer nézetben kattintson a jobb gombbal a dinamikus webes projektet, amely Azure telepítette. Amikor megjelenik a helyi menü, válassza a **Hibakeresési**, és válassza az **Azure Web App alkalmazásban**.

    ![][03]

1. Ha az első alkalommal a dinamikus webhely projekt hibakeresés alatt, a **Konfigurációk hibakeresési** párbeszédpanel megnyílik; elfogadhatja az alapértelmezett értékeket, amelyek a **Csatlakozás** lapon az eszközkészlet által megadott. Az **adatforrások** lapon kattintson a **Hozzáadás gombra**, majd a **Java projekt**, jelölje be a **Dinamikus webhely projekt**, és kattintson **az OK**gombra. Miután elvégezte ezeket a lépéseket, kattintson a **hibakeresési**.

    ![][04]

1. Amikor a rendszer megkérdezi, hogy **távoli hibakeresés engedélyezése a távoli webalkalmazásban most?**, kattintson az **OK gombra**.

1. Amikor a rendszer kéri, hogy **a web App alkalmazásban a távoli hibakereséshez készen áll**, kattintson az **OK gombra**.

    ![][05]

1. Amikor megjelenik a **Hibakeresési beállítások** párbeszédpanel, kattintson a **hibakeresési**.

1. Egy Windows parancssor vagy Unix rendszerhéj nyissa meg, és felkészülés szükséges kapcsolat, a hibakereséshez. meg kell várnia, amíg a kapcsolatot a távoli Java Web App befejeződött, a folytatás előtt. Ha a Windows rendszer használata esetén az alábbi ábrán lenne.

    ![][06]

1. Egy oldaltörés pont beszúrása a JSP lapon, majd az Java-webalkalmazást a böngészőben nyissa meg az URL-címe:

    1. **Azure Explorer** Holdas megnyitása.
    1. Nyissa meg a **Web Apps alkalmazások** és a Java Web App szeretné hibakeresése.
    1. Kattintson a Web App alkalmazásban kattintson a jobb gombbal, és kattintson a **Megnyitás böngészőben**parancsra.
    1. Holdas most hibakeresési üzemmódba adja meg.

## <a name="next-steps"></a>Következő lépések

Java Azure használatával kapcsolatos további tudnivalókért olvassa el a az [Azure Java Developer Center]című témakört.

Azure Web Apps alkalmazások létrehozásáról további információt a a [Web Apps alkalmazások – áttekintés]című témakörben találhat.

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure eszközkészlete Holdas]: ../azure-toolkit-for-eclipse.md
[Az Azure eszközkészlet Holdas telepítése]: ../azure-toolkit-for-eclipse-installation.md
[Helló világ webalkalmazást Holdas az Azure létrehozása]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Minta dinamikus webes projekt]: http://go.microsoft.com/fwlink/?LinkId=817337

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Web Apps alkalmazások áttekintése]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-eclipse/01-debug-java-web-app-in-eclipse.png
[02]: ./media/app-service-web-debug-java-web-app-in-eclipse/02-configure-eclipse-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-eclipse/03-debug-as.png
[04]: ./media/app-service-web-debug-java-web-app-in-eclipse/04-debug-configurations.png
[05]: ./media/app-service-web-debug-java-web-app-in-eclipse/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-eclipse/06-windows-command-prompt-connection-successful-to-remote.png
