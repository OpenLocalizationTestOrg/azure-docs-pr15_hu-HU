<properties 
    pageTitle="Hogyan hozhat létre a Web App alkalmazás szolgáltatással Linux |} Microsoft Azure" 
    description="Web app létrehozási munkafolyamat Linux alkalmazás szolgáltatás." 
    keywords="Azure alkalmazás szolgáltatás, webalkalmazás, linux, oss"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="create-a-web-app-with-app-service-on-linux"></a>Hozzon létre webalkalmazást Linux alkalmazás szolgáltatással

## <a name="using-the-management-portal-to-create-your-web-app"></a>Az adatkezelési portál használatával hozhat létre a web App alkalmazásban
Elindíthatja a webalkalmazás létrehozása Linux az [adatkezelési portálon](https://portal.azure.com) az alábbi képen látható módon.

![][1]

Miután kiválasztotta az alábbi lehetőségek valamelyikére, jelennek meg a Létrehozás lap az alábbi képen látható módon. 

![][2]

-   Nevezze el a web App alkalmazásban.
-   Válassza a meglévő erőforráscsoport, vagy hozzon létre egy újat. (Lásd: régióban elérhető [korlátozások csoportban](./app-service-linux-intro.md)).
-   Válasszon egy meglévő alkalmazás szolgáltatás tervet, vagy hozzon létre egy új egy (lásd: alkalmazás szolgáltatás terv jegyzetek [korlátozások csoportban](./app-service-linux-intro.md)). 
-   Válassza ki a használni kívánt alkalmazás Papírhalom. Választhat a különböző verzióiban Node.js és PHP fog kapni. 

Ha befejezte a létrehozott alkalmazást, az alkalmazás objektumhalomban a módosíthatja az alkalmazás beállításait az alábbi képen látható módon.

![][3]

## <a name="deploying-your-web-app"></a>A web app telepítése

"Telepítési beállítások" kiválasztása az adatkezelési portálról lehetővé teszi a lehetőség, amellyel helyi mely számjegy tár vagy a tár GitHub az alkalmazás telepítéséhez. A képernyőn megjelenő utasításokat ezt követően hasonlóképpen nem Linux webalkalmazást, és, kövesse az utasításokat a [helyi mely számjegy telepítési](./app-service-deploy-local-git.md) vagy a következő [folyamatos telepítési](./app-service-continuous-deployment.md) cikkünket GitHub.

FTP töltse fel az alkalmazást a webhely is használhatja. A webalkalmazás az FTP-végpontot elérheti a diagnosztikai naplók szakaszában az alábbi képen látható módon.

![][4]


## <a name="next-steps"></a>Következő lépések ##

* [Mi az alkalmazás szolgáltatás Linux?](./app-service-linux-intro.md)
* [A Web Apps alkalmazások Linux Node.js PM2 konfiguráció használata](./app-service-linux-using-nodejs-pm2.md)

<!--Image references-->
[1]: ./media/app-service-linux-how-to-create-a-web-app/top-level-create.png
[2]: ./media/app-service-linux-how-to-create-a-web-app/create-blade.png
[3]: ./media/app-service-linux-how-to-create-a-web-app/application-settings-change-stack.png
[4]: ./media/app-service-linux-how-to-create-a-web-app/diagnostic-logs-ftp.png
