<properties 
    pageTitle="Azure alkalmazás szolgáltatás Web Apps alkalmazások Java alkalmazás hozzáadása" 
    description="Ebből az oktatóanyagból megtudhatja, hogy hogyan egy lap vagy az Azure alkalmazás szolgáltatás Web Apps alkalmazások használata Java már konfigurált példányára alkalmazás hozzáadása." 
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
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="add-a-java-application-to-azure-app-service-web-apps"></a>Azure alkalmazás szolgáltatás Web Apps alkalmazások Java alkalmazás hozzáadása

Miután inicializálni az [Azure][] -App szolgáltatásban Java-webalkalmazást [létrehozása az Azure alkalmazás szolgáltatás Java webalkalmazást](web-sites-java-get-started.md)ismertetett az alkalmazás feltöltheti a HÁBORÚ helyezze a **webalkalmazás** mappában.

A navigációs mappa elérési útját **webalkalmazás** eltérő hogyan állíthatja be a Web Apps-példány alapján.

- A web app beállítása a Microsoft Azure piactéren használatával, a mappa elérési útját **webalkalmazás** esetén a képernyőn **d:\home\site\wwwroot\bin\application\_server\webapps**, ahol **alkalmazás\_server** neve az application Server gyakorlatilag a Web Apps-példány. 
- A web app beállítása a Azure konfigurációval felhasználói felület, ha az űrlap **d:\home\site\wwwroot\webapps**szerepel a **webalkalmazás** mappa elérési útját. 

Figyelje meg, hogy a verziókövetés tölthet fel, az alkalmazás és a weblapok, beleértve a [folyamatos integrációs forgatókönyvek](app-service-continuous-deployment.md)használhatja. FTP egyben a weblapok vagy az alkalmazás feltöltése egy lehetőséget.

Megjegyzés: Tomcat web Apps alkalmazások: Miután feltöltötte a fájljait HÁBORÚ a **webalkalmazás** mappába, a Tomcat application server észleli, hogy korábban megadott, és automatikusan tölteni az. Fontos tudni, hogy a legfelső szintű címtárhoz másolta a fájlokat (kívül HÁBORÚ fájlok), ha az application server fog újra kell indítani, mielőtt használják ezeket a fájlokat. A autoload funkciókat Azure futó Tomcat Java web Apps alkalmazások új HÁBORÚ fájl hozzá, vagy új fájlokat vagy az **webalkalmazás** mappához könyvtárak alapul. 

## <a name="next-steps"></a>Következő lépések

További tudnivalókért lásd: a [Java Developer Center](/develop/java/).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Azure alkalmazás szolgáltatás]: http://go.microsoft.com/fwlink/?LinkId=529714
