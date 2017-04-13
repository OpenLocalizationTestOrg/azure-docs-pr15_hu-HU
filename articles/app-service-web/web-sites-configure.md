<properties 
    pageTitle="Web Apps alkalmazások Azure alkalmazás szolgáltatás beállítása" 
    description="Egy webalkalmazás konfigurálása a Azure alkalmazás szolgáltatások" 
    services="app-service\web" 
    documentationCenter="" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="configure-web-apps-in-azure-app-service"></a>Web Apps alkalmazások Azure alkalmazás szolgáltatás beállítása #

Ebből a témakörből megtudhatja, hogy miként konfigurálható az [Azure-portálon]webalkalmazást.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="application-settings"></a>Az alkalmazásbeállítások

1. Az [Azure-portálon]nyissa meg a lap, a webalkalmazásban.
2. Kattintson **az összes beállításai**parancsra.
3. Kattintson az **alkalmazás beállításait**.

![Az alkalmazásbeállítások][configure01]

Az **alkalmazás beállításai** lap rendszer több kategóriák szerint csoportosítva.

### <a name="general-settings"></a>Általános beállítások megadása

**Framework verziók**. A beállítások megadása, ha az alkalmazást használ, minden e keretek: 

- **.NET-keretrendszer**: a .NET-keretrendszer verzió beállítása. 
- **PHP**: **ki** PHP letiltása vagy PHP-verzió beállítása. 
- **Java**: Jelölje ki a Java verziót vagy **ki** Java letiltása. A **Webes tároló** beállítás használatával Tomcat és rakodóhely közül választhat.
- **Python**: Jelölje ki a Python verzióját vagy a **ki** Python letiltása.

Műszaki okokból Java engedélyezése az alkalmazás letiltja a .NET PHP és Python beállításokat.

<a name="platform"></a>
**Platform**. Kijelöli, hogy a webalkalmazás futtatása 32 bites vagy 64 bites környezetben. A 64 bites környezet Basic vagy szabványos üzemmódban van szükség. Ingyenes, és a megosztott módok mindig futtatása 32 bites környezetben.

**Webes Sockets**. Állítsa **be** ahhoz, hogy a WebSocket protokoll; Ha például a web app [ASP.NET SignalR] vagy [socket.io]használja.

<a name="alwayson"></a>
**Mindig a**. Alapértelmezés szerint web Apps alkalmazások betöltetlen hagyják üresjárati bizonyos ideje. Ezzel a lehetőséggel a rendszer erőforrások megőrzése. Basic vagy szabványos üzemmódban engedélyezheti **a mindig** tartani az alkalmazás betöltött mindig. Ha az alkalmazás folyamatos webes feladatok fut, engedélyeznie kell a **Mindig**vagy a webes feladatok futása nem biztos, hogy.

A **felügyelt folyamat verziója**. Az IIS [folyamat mód]állítja. Hagyja meg az integrált (alapértelmezett) a régi alkalmazás IIS régebbi verzióját igénylő működött.

**Automatikus felcserélése**. Ha automatikus felcserélése engedélyezése egy telepítési tárolóhely, alkalmazás szolgáltatás fog automatikusan felcserélése a web app éles frissítésére, hogy a tárolóhely megnyomása. További tudnivalókért lásd: [Deploy átmeneti tárolására helyek web Apps alkalmazások Azure App szolgáltatásban történő] (webhely-webhelyek-előkészített-publishing.md).

### <a name="debugging"></a>Hibakeresése során

A **távoli hibakereséshez**. Lehetővé teszi, hogy a távoli hibakereséshez. Ha engedélyezve van, a Visual Studióban a távoli debugger is használhatja a csatlakoztassa közvetlenül a web App alkalmazásban. Távoli hibakeresés bekapcsolva marad 48 óra. 

### <a name="app-settings"></a>Alkalmazás beállításainak

Ez a szakasz tartalma: név/érték párokká, akkor a webes alkalmazás indításkor tölti. 

- A .NET-alkalmazásokat, ezekkel a beállításokkal beékelt be a .NET configuration `AppSettings` futásidőben, a meglévő beállítások felülbírálása. 

- Ezeket a beállításokat a környezet változók futásidőben PHP, Python, Java és csomópont alkalmazások érheti el. Minden egyes app beállítása a két környezetben változó létrehozott; egy, az alkalmazás beállítás bejegyzést, és egy másik, a APPSETTING_ előtaggal megadott névvel. Az azonos értékkel is tartalmazza.

### <a name="connection-strings"></a>Kapcsolati karakterlánc

Kapcsolati karakterlánc csatolt erőforrások. 

A .NET-alkalmazásokat, ezek a csatlakozási_karakterlánc beékelt be a .NET configuration `connectionStrings` beállítások futásidőben, ahol megegyezik az a billentyűt a csatolt adatbázis neve meglévő bejegyzések felülbírálása. 

PHP, Python, Java és csomópont-alkalmazásokhoz ezek a beállítások érhetők el környezet változók futásidőben, ha a kapcsolat típusát. A környezet változó prefixumokban a következők: 

- SQL Server:`SQLCONNSTR_`
- MySQL:`MYSQLCONNSTR_`
- SQL-adatbázis:`SQLAZURECONNSTR_`
- Egyéni:`CUSTOMCONNSTR_`

Ha például a MySql-kapcsolati karakterlánc voltak nevű `connectionstring1`, szeretné azok webböngészőn keresztül a környezeti változó `MYSQLCONNSTR_connectionString1`.

### <a name="default-documents"></a>Olyan alapértelmezett dokumentumok

Az alapértelmezett kombináció a weblapot, amelyre jelenik meg a legfelső szintű webhelye URL-CÍMÉT.  A lista első egyező fájlt használja. 

Web Apps alkalmazások modulokat, hogy útvonal alapján URL-címe helyett szolgáló statikus tartalommá, ebben az esetben nem ilyen nincs alapértelmezett dokumentum használhat.    

### <a name="handler-mappings"></a>Eseménykezelő-megfeleltetések

Egyéni parancsfájl processzorok kezelheti az adott fájlkiterjesztések kérelem kövesse ezen a területen. 

- A **bővítmény**. Kezelendő, például *.php vagy handler.fcgi fájlkiterjesztés. 
- **Parancsfájl processzor elérési útját**. Abszolút elérési útját a parancsfájl-feldolgozó. A parancsprogram-feldolgozó fájlokat, amelyek megfelelnek a megadott kérések fog dolgozható fel. Az elérési út `D:\home\site\wwwroot` a legfelső szintű alkalmazástárához hivatkozik.
- **További argumentumokat**. A parancsfájlok processzor választható parancssori argumentumok 


### <a name="virtual-applications-and-directories"></a>Virtuális alkalmazások és -mappák 
 
Virtuális alkalmazások és könyvtárak beállítása, adja meg mindegyik könyvtár és a megfelelő fizikai utat viszonyított legfelső szintű webhelyét. Tetszés szerint kijelölhet az **alkalmazás** jelölőnégyzetet, hogy egy virtuális könyvtár alkalmazásként.


## <a name="enabling-diagnostic-logs"></a>A diagnosztikai naplók engedélyezése

A diagnosztikai naplók engedélyezése:

1. A lap, a webalkalmazásban kattintson az **összes**.
2. Kattintson **a diagnosztikai naplók**. 

A diagnosztikai naplók írása egy webalkalmazás, amely támogatja a naplózási beállítások: 

- A **naplózás alkalmazást**. Ír alkalmazás naplók a fájlrendszerben. A naplózás a 12 órával ideig tart. 

**Szintjét**. Amikor az alkalmazás naplózás engedélyezve van, ezzel a beállítással ellátni kívánt adatok mennyiségét felvett (hiba, figyelmeztetés, információ, vagy a részletes).

A **naplózás webkiszolgálóra**. Naplók a W3C bővített naplófájl fájlformátumban menti. 

**Részletes hibaüzenetek jelennek meg**. A hiba részletes mentése üzenetek .htm fájlokat. A fájlok csoportban /LogFiles/DetailedErrors menti. 

**Hibás kérés nyomkövetési**. Naplók kéréseit az XML-fájlok sikertelen volt. A fájlok menti a program a /LogFiles/W3SVC*xxx*xxx esetén egy egyedi azonosítót. Ez a mappa tartalmaz egy XSL-fájl, és egy vagy több XML-fájlok. Feltétlenül töltse le az XSL-fájl, mert azt szolgáltatásokat nyújt a formázást, és az XML-fájlok tartalma szűrés.

A naplófájlok megtekintése, létre kell hoznia FTP hitelesítő adatait, az alábbi képlettel történik:

1. A lap, a webalkalmazásban kattintson az **összes**.
2. Kattintson a **telepítés hitelesítő adatok**.
3. Írja be a felhasználónevét és jelszavát.
4. Kattintson a **Mentés**gombra.

![Telepítési hitelesítő adatok beállítása][configure03]

A teljes FTP felhasználóneve "app\username" *alkalmazás* esetén a webes alkalmazás nevére. A felhasználónév web app **alapvető tudnivalók**lap jelenik meg.  

![FTP telepítési hitelesítő adatok][configure02]

## <a name="other-configuration-tasks"></a>Egyéb beállítási feladatok

### <a name="ssl"></a>AZ SSL 

Basic vagy szabványos üzemmódban is feltölthet egy egyéni tartományt SSL-tanúsítványok. További tudnivalókért lásd: [engedélyezése HTTPS egy webalkalmazás]. 

A feltöltött tanúsítványok megtekintéséhez kattintson **a minden elérhető beállítás** > **egyéni tartományok és az SSL**.

### <a name="domain-names"></a>A tartománynevek

Vegye fel a webalkalmazás egyéni tartomány nevét. További információ a [konfigurálása Azure App szolgáltatásban webalkalmazást egyéni tartománynév] című témakört.

A tartománynevek megtekintéséhez kattintson **a minden elérhető beállítás** > **egyéni tartományok és az SSL**.

### <a name="deployments"></a>Telepítések

- Folytonos telepítési beállítása. Lásd: [Mely számjegy használatával Web Apps alkalmazások szolgáltatásban Azure alkalmazás telepítéséhez](./web-sites-deploy.md).
- Telepítési helyek. Lásd: [Azure App szolgáltatásban Web Apps alkalmazások fejlesztői környezetben üzembe].


A telepítési helyek megtekintéséhez kattintson **a minden elérhető beállítás** > **telepítési helyek**.

### <a name="monitoring"></a>Figyelése

A Basic vagy szabványos üzemmódban tesztelheti a HTTP- és HTTPS-végpontok legfeljebb három geo elosztott helyekről elérhetőségét. A felügyeleti teszt sikertelen, ha a HTTP-válasz kódot hiba lép fel, (4xx vagy 5xx) vagy a válasz legfeljebb 30 másodperc vesz igénybe. Zárólap érhető el, ha az adott helyekről sikeres felügyeleti tesztek számít. 

További tudnivalókért lásd: [Útmutató: webhely végpont állapot ellenőrzését].

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók kezdéshez Azure alkalmazás szolgáltatással, nyissa meg a [Próbálja alkalmazás szolgáltatás], ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

## <a name="next-steps"></a>Következő lépések

- [Azure alkalmazás szolgáltatás a saját tartománynév beállítása]
- [HTTPS-alkalmazás Azure alkalmazás szolgáltatás engedélyezése]
- [Azure alkalmazás szolgáltatás webalkalmazást méretezése]
- [Web Apps alkalmazások Azure alkalmazás szolgáltatás felügyeleti alapjai]

<!-- URL List -->

[ASP.NET SignalR]: http://www.asp.net/signalr
[Azure portál]: https://portal.azure.com/
[Azure alkalmazás szolgáltatás a saját tartománynév beállítása]: ./web-sites-custom-domain-name.md
[Azure App szolgáltatásban Web Apps alkalmazások fejlesztői környezet telepítése]: ./web-sites-staged-publishing.md
[HTTPS-alkalmazás Azure alkalmazás szolgáltatás engedélyezése]: ./web-sites-configure-ssl-certificate.md
[Útmutató: webhely végpont állapot ellenőrzését]: http://go.microsoft.com/fwLink/?LinkID=279906
[Web Apps alkalmazások Azure alkalmazás szolgáltatás felügyeleti alapjai]: ./web-sites-monitor.md
[tölcsér mód]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Azure alkalmazás szolgáltatás webalkalmazást méretezése]: ./web-sites-scale.md
[Socket.IO]: ./web-sites-nodejs-chat-app-socketio.md
[Próbálja meg az App szolgáltatás]: http://go.microsoft.com/fwlink/?LinkId=523751

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png
