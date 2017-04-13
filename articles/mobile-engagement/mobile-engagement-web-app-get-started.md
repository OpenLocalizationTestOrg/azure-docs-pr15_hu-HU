<properties
    pageTitle="Azure Mobile tetszés szerint elmélyedhet Web Apps használatának első lépései |} Microsoft Azure"
    description="Megtudhatja, hogy miként analitikai és a leküldéses értesítések Web Apps alkalmazások használata a Azure Mobile részvételét."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="js"
    ms.topic="hero-article"
    ms.date="06/01/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-web-apps"></a>Azure Mobile tetszés szerint elmélyedhet Web Apps használatának első lépései

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ez a témakör bemutatja, hogyan Azure Mobile tetszés szerint elmélyedhet használatával megérteni a Web App használatát.

Ebben az oktatóprogramban az alábbiakra van szükség:

+ Visual Studio 2015 vagy bármely más szerkesztővel a
+ [Webes SDK](http://aka.ms/P7b453) 

A Web SDK előzetes verzióban, és csak Analytics támogatja pillanatában és egyelőre nem támogatja a küldő böngészőben vagy az alkalmazás leküldéses értesítéseket. 

> [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez, az aktív Azure fiókkal kell rendelkezniük. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).

##<a name="setup-mobile-engagement-for-your-web-app"></a>Mobil tetszés szerint elmélyedhet a Web App beállítása

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Az alkalmazás csatlakoztatása a Mobile tetszés szerint elmélyedhet kódmentes

Ebben az oktatóprogramban egy "egyszerű integráció," amely a minimálisan szükséges adatokat szeretne gyűjteni jeleníti meg.

A Visual Studio keresztül mutatjuk integrációja, minden más webes alkalmazáshoz kívül Visual Studio is létrehozott, kövesse a lépéseket, ha azt hoz létre egy egyszerű web App alkalmazásban. 

###<a name="create-a-new-web-app"></a>Hozzon létre egy új Web App alkalmazásban

A következő lépések feltételezik a Visual Studio 2015 használatát, mintha lépések hasonlóak a Visual Studio korábbi verzióiban. 

1. Indítsa el a Visual Studióban, és a **Kezdőlap** képernyőn jelölje be az **Új projekt**.

2. Az előugró ablakban, jelölje be a **webes** -> **ASP.Net webalkalmazást**. Töltse ki a **helyet** , és a **megoldás nevét**, az alkalmazás **nevét**, és kattintson **az OK**gombra.

3. **Válasszon egy sablont** előugró **üres** **ASP.Net 4.5** -sablonok kiválasztása, és kattintson az **OK gombra**. 

Most már létrehozott egy új üres Web App projekt amelybe azt fogja integrálása az Azure Mobile tetszés szerint elmélyedhet webes SDK.

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Az alkalmazás csatlakoztatása Mobile tetszés szerint elmélyedhet kódmentes

1. A megoldás **javascript** nevű új mappa létrehozása és a webes SDK JS fájl **azure-engagement.js** beszúrhat. 

2. Adja hozzá a következő kódot a javascript mappa **main.js** nevű új fájl. Ellenőrizze, hogy a kapcsolati karakterlánc frissítéséhez. Ez `azureEngagement` objektum használandó Web SDK módszerek eléréséhez. 

        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };

    ![Visual Studio js-fájlokkal][1]

##<a name="enable-real-time-monitoring"></a>Valós idejű ellenőrzése engedélyezése

Adatok küldése, és annak biztosítására, hogy a felhasználók aktív indításához legalább egy tevékenység kell küldenie a Mobile tetszés szerint elmélyedhet kódmentes. Egy tevékenység webalkalmazást környezetében egy olyan weblap. 

1. A megoldás **home.html** nevű új lap létrehozása, és állítsa be a kezdő oldalon, a webalkalmazásban. 
2. A két JavaScript-kódok ezen az oldalon a korábban a következő belül a szervezet címke hozzáadásával jelöltük tartalmazzák. 

        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>

3. A szervezet címke hívja fel a EngagementAgent frissítése `startActivity` módszer
        
        <body onload="engagement.agent.startActivity('Home')">

4. Íme, hogyan fognak kinézni a **home.html**
        
        <html>
        <head>
            ...
        </head>
        <body onload="engagement.agent.startActivity('Home')">
            <script type="text/javascript" src="javascript/main.js"></script>
            <script type="text/javascript" src="javascript/azure-engagement.js"></script>
        </body>
        </html>

##<a name="connect-app-with-real-time-monitoring"></a>Alkalmazás kapcsolatba valós idejű ellenőrzése

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

![][2]

##<a name="extend-analytics"></a>Kijelölés méretének növelése a analytics

Íme a Web analytics-használható SDK jelenleg minden módszer:

1. Tevékenységek és a weblapok:

        engagement.agent.startActivity(name);
        engagement.agent.endActivity();

2. Események
        
        engagement.agent.sendEvent(name, extras);

3. Hibák

        engagement.agent.sendError(name, extras);

4. Feladatok

        engagement.agent.startJob(name);
        engagement.agent.endJob(name);

<!-- Images. -->
[1]: ./media/mobile-engagement-web-app-get-started/visual-studio-solution-js.png
[2]: ./media/mobile-engagement-web-app-get-started/session.png

