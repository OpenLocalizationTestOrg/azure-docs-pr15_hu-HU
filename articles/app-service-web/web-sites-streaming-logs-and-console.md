<properties 
    pageTitle="A folyamatos átvitelű naplókról és a konzolon" 
    description="A folyamatos átvitelű naplókról és a konzol – áttekintés" 
    authors="btardif" 
    manager="wpickett" 
    editor="" 
    services="app-service\web" 
    documentationCenter=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/12/2016" 
    ms.author="byvinyal"/>

# <a name="streaming-logs-and-the-console"></a>A folyamatos átvitelű naplókról és a konzolon

## <a name="streaming-logs"></a>Adatfolyam naplók

Az **Azure portál** tartalmaz egy beépített adatfolyam napló megjelenítő, amellyel az **Alkalmazás szolgáltatás** alkalmazásokból nyomkövetési események megtekintése valós időben.  

Ez a szolgáltatás beállítása néhány egyszerű lépésből áll:

- Nyomkövetés írja be a kódot.
- Az alkalmazás a **diagnosztikai naplók** alkalmazás engedélyezése
- A beépített **Streaming naplók** a felhasználói felület az adatfolyam megtekintése a **portálon Azure**.

### <a name="how-to-write-traces-in-your-code"></a>Hogyan halad írja be a kódot. ###

Gyerekjáték a nyomkövetési naplók írás be a kódot.  A C# ugyanolyan egyszerű, mint írása a következő kódot:

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

A nyomkövetési osztály abban a System.Diagnostics névtér.

Egy node.js alkalmazásban ugyanazt az eredményt eléréséhez kód írhat:

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-to-enable-and-view-the-streaming-logs"></a>Eseménynaplók engedélyezése és a folyamatos átvitelű megtekintése
![][BrowseSitesScreenshot]Diagnosztikai egy alkalmazás alapon engedélyezett. Először tallózással megkeresi a szeretné, hogy ez a funkció engedélyezése a webhelyen.  
  
![][DiagnosticsLogs]A beállítások menüben görgessen lefelé a **Figyelés** szakaszig, és kattintson **a diagnosztikai naplók (1)**. Majd **Alkalmazás (formázáshoz) naplózás** **engedélyezése (2)** vagy az **Alkalmazás naplózás (blob)** a a **szint** beállítás lehetővé teszi a nyomkövetési információk rögzítése súlyosságát szintjének módosítása. Ha csak próbálja megismerkedhet a szolgáltatást, állítsa a szint **részletes** annak érdekében, hogy az összes nyomkövetési utasításba begyűjtési.

Kattintson a **MENTÉS** elemre a lap tetején, és készen áll a naplók megtekintéséhez.

>[AZURE.NOTE] Minél nagyobb a további erőforrások felhasznált napló és a további halad **súlyosságát szint** darab termék készült. Ellenőrizze, hogy **súlyosságát szint** van beállítva, hogy a helyes részletességi gyártás vagy nagy forgalmat webhelyet. 

![][StreamingLogsScreenshot]A **folyamatos átvitelű naplók** a belül az Azure portál megtekintéséhez kattintson az **(1) napló adatfolyam** is szakaszában **figyelése** a beállítások menüben. Ha az alkalmazás aktív ír nyomkövetési kimutatások, majd meg kell jelennie őket az a **felhasználói felület (2) a folyamatos átvitelű naplózza** közeli valós időben.

## <a name="console"></a>Konzol
Az **Azure portál** az alkalmazás konzol hozzáférést biztosít. Ismerje meg az alkalmazás fájlrendszer és a powershell/cmd parancsfájlok futtatása. A futó alkalmazás kódként beállítása konzol parancsai végrehajtásakor ugyanolyan engedélyekkel is köti. A hozzáférést a védett könyvtárak vagy parancsfájlok, emelt engedélyeket igénylő futtatása le van tiltva.  

![][ConsoleScreenshot]A beállítások menüben a **console (2)** , és görgessen le a **Fejlesztőeszközök** szakasz **Console (1)** parancsra, majd megnyitja, jobbra.

Megismerkedhet a **konzol**, próbálja meg a legalapvetőbb gombokkal, például:

`````````````````````````
dir
`````````````````````````

`````````````````````````
cd
`````````````````````````

<!-- Images. -->
[DiagnosticsLogs]: ./media/web-sites-streaming-logs-and-console/diagnostic-logs.png
[BrowseSitesScreenshot]: ./media/web-sites-streaming-logs-and-console/browse-sites.png
[StreamingLogsScreenshot]: ./media/web-sites-streaming-logs-and-console/streaming-logs.png
[ConsoleScreenshot]: ./media/web-sites-streaming-logs-and-console/console.png
