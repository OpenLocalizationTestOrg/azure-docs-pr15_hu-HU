<properties 
    pageTitle="Azure App szolgáltatásban webalkalmazást kezelése" 
    description="Azure App szolgáltatásban webalkalmazást kezelésével kapcsolatos erőforrásokra mutató hivatkozásokat." 
    services="app-service\web" 
    documentationCenter="" 
    authors="erikre" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="rachelap"/>

# <a name="manage-a-web-app-in-azure-app-service"></a>Azure App szolgáltatásban webalkalmazást kezelése

Ez a témakör az [Azure App](http://go.microsoft.com/fwlink/?LinkId=529714)szolgáltatásban webalkalmazást projektvezetési forrásokra mutató hivatkozásokat. Adatkezelési összes feladatokat a zökkenőmentes webalkalmazás megtartása tartalmazza. 

Webalkalmazást élettartama során különböző kezelési feladatok, a kezdeti telepítés rendes működését, karbantartási és frissítések mozgatásakor végez meg.

Sok web app teendőkről hajthatják az Azure-portálon.

## <a name="before-you-deploy-your-web-app-to-production"></a>Mielőtt beállítaná a web app előállítása

### <a name="choose-a-tier"></a>Válassza ki az egy réteg

Azure alkalmazás szolgáltatás az öt rétegek kínálják: szabad, a megosztott, Basic, normál és prémium verzióban. Információkat talál a és az egyes réteg árak olvassa el a [Részletek árak](/pricing/details/app-service/)című témakört. 

- [App milyen szolgáltatáscsomagok](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) adhatja meg a csoport több web Apps alkalmazások csoportban az egy réteg.
- Bármikor [válthat a réteg](web-sites-scale.md) a webalkalmazás létrehozása után.

### <a name="configuration"></a>Konfiguráció

Az [Azure Portal](https://portal.azure.com/) segítségével különböző konfigurációs beállítások megadása. A részletekért olvassa [konfigurálása web Apps alkalmazások Azure App szolgáltatásban](web-sites-configure.md). Íme egy rövid Feladatlista:

- Jelölje be a **futtatókörnyezet verzióinak** .NET, PHP, Java vagy Python, ha szükséges.
- Engedélyezze a **WebSockets** lehetőséget, ha a web App alkalmazásban a WebSocket protokollt használja. (Ideértendők [ASP.NET SignalR](http://www.asp.net/signalr) vagy [socket.io](web-sites-nodejs-chat-app-socketio.md)használó alkalmazások.)
- Folytonos webes feladatok fut? Ha igen, engedélyezze a **Mindig a**.
- Beállíthatja az **alapértelmezett dokumentum**, például index.html.

Nemcsak a ezeket alapkonfiguráció a beállításokat érdemes adja meg az alábbi beállításokat:

- **Secure Sockets Layer (SSL)** titkosítás. Az egyéni tartománynevet használja az SSL, SSL-tanúsítvány beszerzése, és állítsa be a web app használatához. Lásd: [HTTPS engedélyezése egy webalkalmazás Azure App szolgáltatásban](web-sites-configure-ssl-certificate.md).
- **Egyéni tartomány neve.** A web app automatikusan megkapja a azurewebsites.net altartományt. Társíthat egyéni tartománynevet (például contoso.com). [Egyéni tartománynevet az Azure alkalmazás szolgáltatás konfigurálása](web-sites-custom-domain-name.md)című témakört.

Nyelvspecifikus konfigurálása:

- **PHP**: [állítsa be az Azure alkalmazás szolgáltatás webalkalmazásokban PHP](web-sites-php-configure.md).
- **Python**: [Python az Azure alkalmazás szolgáltatás Web Apps alkalmazások beállítása](web-sites-python-configure.md)


## <a name="while-your-web-app-is-running"></a>A web app futása közben

A web app futása közben ellenőrizze, hogy érhető el, és hogy méretezés felhasználói forgalom alkalmazást szeretné. Is előfordulhat kapcsolatos hibák elhárítása.

### <a name="monitoring"></a>Figyelése

- Az Azure portálon keresztül segítségével [teljesítménymutatók hozzáadása](web-sites-monitor.md) például processzorhasználata és az ügyfél-összehívások.
- [Méretezés a web app](web-sites-scale.md) válaszként forgalmat. Attól függően, hogy a réteg méretezheti VMs számát, illetve a virtuális példányok méretét. A szokásos és prémium rétegek is állíthat be autoscaling, így a web app automatikusan, méretezze át, meghatározott időközönként vagy válasz való betöltéséhez.  
 
### <a name="backups"></a>Biztonsági másolatok

- [Automatikus biztonsági mentést](web-sites-backup.md) , a web App beállítása. További tudnivalók [Ebben](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/)a videóban biztonsági mentést.
- Tudjon meg többet a beállításokat, az [adatbázis-helyreállítás](../sql-database/sql-database-business-continuity.md) Azure SQL-adatbázisban.

### <a name="troubleshooting"></a>Hibaelhárítás

- Ha valamilyen hiba, azt is megteheti, a [Visual Studio kapcsolatos hibák elhárítása](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)a diagnosztikai naplók és élő hibakeresése során a felhőben. 
- Kívül Visual Studióban az alábbi diagnosztikai naplók összegyűjtéséhez különböző módon. Lásd: a [diagnosztikai naplózás web Apps alkalmazások Azure alkalmazás szolgáltatás engedélyezése](web-sites-enable-diagnostic-log.md).
- Node.js alkalmazásokhoz elolvashatja, [hogyan szeretné hibakeresése Azure App szolgáltatásban Node.js webalkalmazást](web-sites-nodejs-debug.md).

### <a name="restoring-data"></a>Adatok visszaállítása

- [Visszaállítás](web-sites-restore.md) a korábban mentett webalkalmazást.


## <a name="when-you-update-your-web-app"></a>Amikor frissíti a web App alkalmazásban

Ha nem engedélyezte a automatikus biztonsági mentést, létrehozhat egy [kézi biztonsági mentést](web-sites-backup.md).

Fontolja meg inkább [telepítési előkészített](web-sites-staged-publishing.md). Ezzel a beállítással frissítések közzétehet egy átmeneti tárolásra szolgáló példányhoz egymás mellett futó a éles üzemi együtt. 

Ha a Visual Studio csapat szolgáltatást használ, beállíthatja a folyamatos telepítési az adatforrás-vezérlő:

- [Verziókövetés a Team Foundation (TFVC) használata](../cloud-services/cloud-services-continuous-delivery-use-vso.md) 
- [Mely számjegy használatával](../cloud-services/cloud-services-continuous-delivery-use-vso-git.md)
 
<!-- Anchors. -->

[Before you deploy your site to production]: #before-you-deploy-your-site-to-production
[While your website is running]: #while-your-website-is-running
[When you update your website]: #when-you-update-your-website

  
