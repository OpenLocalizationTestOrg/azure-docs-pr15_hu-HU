<properties
    pageTitle="Ajánlott eljárások és Azure Web Apps alkalmazások csomópont hibaelhárítási útmutató"
    description="Ismerje meg az ajánlott eljárások és Azure Web Apps alkalmazások a csomópont-alkalmazásokhoz hibaelhárítási lépéseket."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="ranjithr"
    manager="wadeh"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="06/06/2016"
    ms.author="ranjithr;wadeh"/>
    
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>Ajánlott eljárások és Azure Web Apps alkalmazások csomópont hibaelhárítási útmutató

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Ebben a cikkben megismerheti a gyakorlati tanácsok és Azure webalkalmazás futó (a [iisnode](https://github.com/azure/iisnode)) [csomópontot alkalmazások](app-service-web-nodejs-get-started.md) hibaelhárítási lépéseket.

>[AZURE.WARNING] Járjon hibaelhárítási lépésekkel a termelési webhelyen. Ajánlási, hogy például az átmeneti tárolásra szolgáló tárolóhely egy tesztkörnyezetben beállítása az alkalmazás hibaelhárítása, és ha a probléma megoldódott, felcserélése az átmeneti tárolásra szolgáló tárolóhely és a gyártási tárolóhely.

## <a name="iisnode-configuration"></a>IISNODE konfigurálása

A [sémafájl](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) mutatja az iisnode konfigurált beállításokat. A beállításokat, az alkalmazás hasznosnak a következők:

* nodeProcessCountPerApplication

    Ez a beállítás, amely az IIS alkalmazást egy indítja csomópont folyamatok száma szabályozza. Alapértelmezett érték 1. Elindíthatja az annyi node.exe a virtuális core száma szerint beállításával, ez pedig 0. Ajánlott értéke 0 a legtöbb alkalmazáshoz, használhat az összes a magmintákat a számítógépre. NODE.exe egyetlen összefűzött, így egy node.exe felhasználja legfeljebb 1 core és ki a kívánt összes mag csatlakozást csomópont alkalmazásból maximális teljesítménye érdekében.

* nodeProcessCommandLine

    Ez a beállítás elérési útját a node.exe szabályozza. Beállíthatja, hogy ezt az értéket, mutasson a node.exe verzióra.

* maxConcurrentRequestsPerProcess

    Ez a beállítás szabályozza minden node.exe iisnode által küldött egyidejű kérelmek maximális száma. Azure webalkalmazás, kattintson a végtelen esetén az alapértelmezett érték. Nem kell aggódnia amiatt, hogy ezt a beállítást. Azure webalkalmazás, kívül az alapértelmezett érték 1024. Előfordulhat, hogy be szeretné állítani a attól függően, hogy hány kéri az alkalmazás kap, és hogy milyen gyorsan az alkalmazás minden kérést dolgoz fel.

* maxNamedPipeConnectionRetry

    Ez a beállítás a megengedett számú alkalommal iisnode újra próbálkozik a névvel ellátott függőleges vonás a kérelmet küldhet fölé node.exe a projekten kapcsolat szabályozza. Ez a beállítás namedPipeConnectionRetryDelay együtt határozza meg, hogy az egyes kérelmek iisnode belül a teljes időtúllépés. Alapértelmezett értéke 200 Azure webalkalmazás található. Teljes időtúllépés a másodpercben megadott = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

* namedPipeConnectionRetryDelay

    Ez a beállítás vezérli idő (ms) iisnode mennyiségét megvárja között az egyes kísérletek kérelmet küldhet node.exe a névvel ellátott függőleges vonás fölé. Alapértelmezett értéke 250ms.
    Teljes időtúllépése a másodpercben megadott = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

    Alapértelmezés szerint ki az összes időtúllépés azure webalkalmazás a iisnode 200 \* 250ms = 50 másodperc.

* logDirectory

    Ez a beállítás a címtárban iisnode hol jelentkezzen be stdout/stderr szabályozza. Alapértelmezett értéke, amely a fő parancsfájl könyvtár (ahol fő server.js megtalálható könyvtár) viszonyított iisnode

* debuggerExtensionDll

    Ez a beállítás szabályozza iisnode a felügyelő csomópont melyik verzióját fogja használni a csomópont-alkalmazásban. Iisnode-felügyelő-0.7.3.dll és iisnode-inspector.dll jelenleg a beállítás csak 2 érvényes értékeket. Alapértelmezett értéke iisnode-felügyelő-0.7.3.dll. iisnode-felügyelő-0.7.3.dll verzióját használja csomópont-felügyelő-0.7.3, valamint websockets, így az azure webappban verzióját fogja használni a websockets engedélyezni kell. Nézze meg <http://www.ranjithr.com/?p=98> további információt az új csomópont felügyelő használandó iisnode konfigurálása.

* flushResponse

    Az alapértelmezett működés az IIS, hogy pufferek-e válasz adatok mentése 4 MB kiürítése előtt, vagy a válasz, a végéig attól véget előbb. iisnode kínál a beállítások, ez a működési módjának felülbírálására: ürítése egy kódrészletet válasz törzsében, amint iisnode kap a node.exe, kell beállítania a iisnode/@flushResponse web.config "true" attribútum:
    
    ```
    <configuration>    
        <system.webServer>    
            <!-- ... -->    
            <iisnode flushResponse="true" />    
        </system.webServer>    
    </configuration>
    ```

    Teljesítményét terhelést, amely a teljesítmény, a rendszer csökkenti (kezdve v0.1.13), a ~ 5 %-kal engedélyezése minden részlet, a válasz törzsét a könyvelési hozzáadja, a legjobb, ha ezt a beállítást csak a folyamatos átvitelű választ igénylő végpontok hatókör (például a <location> elem a Web.config)

    Mindezeken kívül a folyamatos átvitelű alkalmazások, meg kell is beállíthatja a iisnode kezelő a responseBufferLimit 0.
    
    ```
    <handlers>    
        <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>    
    </handlers>
    ```

* watchedFiles

    Ez a fájlokat, amelyek megváltozott fog kell vizsgált pontosvesszővel elválasztott listája. Egy fájl módosításának hatására az alkalmazás Lomtár. Mindegyik listaelem-választható könyvtár nevét, valamint a szükséges fájl nevét, amelyek a címtárban, ahol a fő alkalmazás belépési pontjához található viszonyított áll. A fájl neve rész csak helyettesítő karakterek használhatók. Alapértelmezett értéke "\*. js;web.config"

* recycleSignalEnabled

    Alapértelmezett értéke hamis. Ha engedélyezve van, a csomópont-alkalmazás csatlakozhat egy névvel ellátott függőleges vonal (környezeti változó IISNODE\_vezérlő\_függőleges vonás), és küldje el a "Lomtár" üzenet. Ennek hatására a w3wp, hogy biztonságosan Lomtár.

* idlePageOutTimePeriod

    Alapértelmezett értéke 0, ami azt jelenti, hogy ez a szolgáltatás nem érhető el. Ha értéke valamilyen érték 0-nál nagyobb, iisnode fog oldal ki az összes alárendelt folyamat minden "idlePageOutTimePeriod" ezredmásodperc. Milyen azt jelenti, oldal megértéséhez, olvassa el ezt a [dokumentációt](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx). Ez a beállítás, amely sok memóriát igényelnek, és szeretné pageout memória lemezre alkalmanként szabadítson fel néhány RAM alkalmazások esetében hasznos lesz.

>[AZURE.WARNING] Legyen óvatos, ha engedélyezi a következő beállítások gyártási alkalmazások. Javaslat, hogy nem engedélyezi őket a éles alkalmazásokat.

* debugHeaderEnabled

    Az alapértelmezett értéket pedig hamis. Ha a értéke igaz, iisnode hozzáadja egy HTTP válasz élőfej iisnode-hibakeresési minden HTTP-válasz küld iisnode-hibakeresési élőfej értéke egy URL-CÍMÉT. Diagnosztikai adatok adott hardvereszközöket is összegyűjtött, megjeleníti az URL-cím részlet, de sokkal jobban adatábrázolás érhető el a böngészőben nyissa meg az URL-címet.

* loggingEnabled

    Ez a beállítás a iisnode által a naplózás stdout és stderr szabályozza. Iisnode stdout/stderr csomópont folyamatok azt elindítja a rögzítés, és a megadott a "logDirectory" beállítással könyvtár írni. Ez az engedélyezés, az alkalmazás fogja, hogy ír naplók a fájlrendszer és attól függően, hogy az alkalmazás által elvégzett naplózás mennyiségét, teljesítményre lehet.

* devErrorsEnabled

    Alapértelmezett értéke hamis. Ha értéke igaz, iisnode jelennek meg a HTTP-állapotkód és Win32 kódszámú hiba jelenik meg a böngészőben. A win32 kódot hasznos, a hibakereséshez problémák bizonyos típusú lesz.
    
* debuggingEnabled (nem engedélyezi a éles webhelyen)

    Ez a beállítás hibakeresési funkciója szabályozza. Iisnode a felügyelő csomópont integrálva van. Mivel ez a beállítás, lehetősége van engedélyezni a csomópont-alkalmazás hibakeresése során. Ez a beállítás engedélyezve van, miután iisnode fog elrendezés az csomópont alkalmazás első hibakeresési kérés lévő "debuggerVirtualDir" címtár szükséges csomópont-felügyelő fájlokat. A csomópont-felügyelő egy kérést küld a http://yoursite/server.js/debug betöltheti. Megadhatja, hogy a hibakeresési URL-cím szakaszában az "debuggerPathSegment" beállítással. Alapértelmezett debuggerPathSegment által = "hibakeresési. Beállíthatja, hogy ez egy GUID például, hogy nehezebben lehet a mások által talált.

    Jelölje be ezt a [hivatkozást](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) a hibakeresése során további információt.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Forgatókönyvek és javaslatok és hibaelhárítás

### <a name="my-node-application-is-making-too-many-outbound-calls"></a>A csomópont-alkalmazás lehetővé teszi a kimenő hívások túl sok.

Sok alkalmazás ellenőriznie, hogy a kimenő kapcsolatokat a normál során. Például ha kérelem érkezik, a csomópont-alkalmazás szeretne lépjen kapcsolatba a REST API-val máshol, és bizonyos információk az összehívás feldolgozása. Szeretné használni kívánt egy megőrzése életben ügynököt, ha a http- és https-hívások. Például használhatja is a agentkeepalive modul a megőrzése életben ügynökök, amikor a kimenő hívások. Ezzel biztosíthatja, hogy a sockets újrafelhasználható vannak-e a az azure webalkalmazás virtuális és az általános létrehozása az összes kimenő kérés új sockets csökkentése. Is így arról, hogy kevesebb sockets számát, hogy hány kimenő kérelem használ, és ezért meg nem haladhatja meg a maxsocket, amely egy virtuális van rendelve. Azure webalkalmazás ajánlást értéke agentKeepAlive maxsocket egy virtuális 160 sockets összesen lenne. Ez azt jelenti, hogy a virtuális futó 4 node.exe esetén be szeretné szeretné állítani a agentKeepAlive maxsocket 40, amely egy virtuális teljes 160 node.exe per.

Példa agentKeepALive konfigurálása:

```
var keepaliveAgent = new Agent({    
    maxSockets: 40,    
    maxFreeSockets: 10,    
    timeout: 60000,    
    keepAliveTimeout: 300000    
});
```

Ez a példa feltételezi, hogy van-e a virtuális futó 4 node.exe. Ha a virtuális futó node.exe különféle, be kell módosítani a maxsocket beállítása lehetőséget.

### <a name="my-node-application-is-consuming-too-much-cpu"></a>A csomópont-alkalmazás túl sok Processzor van felhasználás.

A magas processzor felhasználási kapcsolatban a portálon az Azure webalkalmazás valószínűleg kaphat ajánlást. Beállíthatja az egyes [Mértékek](web-sites-monitor.md)megtekintés monitorok is. Az [Azure portál irányítópult](../application-insights/app-insights-web-monitor-performance.md)processzorhasználata ellenőrzésekor hátha a maximális értékek Processzor, hogy ne maradjak le ki a csúcs értékeket.
Ha úgy gondolja, hogy az alkalmazás használata van más túl sok Processzor, és nem bemutatják, hogy miért esetben szüksége lesz a csomópont-alkalmazás profil.

### 

#### <a name="profiling-your-node-application-on-azure-webapps-with-v8-profiler"></a>A csomópont alkalmazás V8-Profilert az azure webalkalmazás adatainak összegyűjtése

Ha például mondjuk lehetővé teszi, hogy egy helló világ alkalmazás használni kívánt profilra alább látható módon:

```
var http = require('http');    
function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    WriteConsoleLog();    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Nyissa meg a scm webhely https://yoursite.scm.azurewebsites.net/DebugConsole

Ekkor megjelenik egy parancssort, alább látható módon. Válassza a webhely/wwwroot címtárában

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

Futtassa a parancsot a "npm telepítés v8-profilert"

Ez célszerű telepíteni v8-profilert csomópont alatt\_modulok könyvtár és az összes függőségét.
Most módosítsa a server.js, hogy az alkalmazás profil.

```
var http = require('http');    
var profiler = require('v8-profiler');    
var fs = require('fs');

function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    profiler.startProfiling('HandleRequest');    
    WriteConsoleLog();    
    fs.writeFileSync('profile.cpuprofile', JSON.stringify(profiler.stopProfiling('HandleRequest')));    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

A fenti módosítások profil a WriteConsoleLog függvényt, és a jegyezze meg a profil kimenet csoportban a webhely wwwroot "profile.cpuprofile" fájlt. Az alkalmazás-összehívásokkal. A webhely wwwroot alapján létrehozott "profile.cpuprofile" fájl jelenik meg.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Töltse le ezt a fájlt, és nyissa meg a fájlt a Chrome F12 eszközökkel kell. Találatok az F12 billentyűt a chrome, majd a lapon kattintson a "profil". Kattintson a "Terhelés" gombra. Jelölje ki az imént letöltött profile.cpuprofile fájlt. Kattintson a profil csak betöltött.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

Láthatja, hogy az idő 95 % felhasznált WriteConsoleLog függvény alább látható módon. Ez is megjelennek a pontos sorok számát és a problémát okozó forrásfájlok.

### <a name="my-node-application-is-consuming-too-much-memory"></a>A csomópont-alkalmazás túl sok memória van felhasználás.

A portál magas memóriahasználat kapcsolatban az Azure webalkalmazás valószínűleg kaphat ajánlást. Beállíthatja az egyes [Mértékek](web-sites-monitor.md)megtekintés monitorok is. A memóriahasználat, az [Azure portál irányítópult](../application-insights/app-insights-web-monitor-performance.md)ellenőrzésekor ellenőrizze a memória maximális értékének, hogy ne maradjak le a maximális érték.

#### <a name="leak-detection-and-heap-diffing-for-nodejs"></a>Észlelését és halommemória Diffing node.js számára 

[Csomópont-memwatch](https://github.com/lloyd/node-memwatch) segítségével is a könnyebb azonosítás memória.
Hasonlóan v8-profilert memwatch telepítésével és a kód rögzítése és az élőláb szerkesztése a memória azonosítása halmok elveszít az alkalmazásban.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>A node.exe vannak első véletlenszerűen levágni 

Néhány oka miért Ez lehet történik:

1.  Az alkalmazás van nem kivételek – értesítő, kérjük, jelölőnégyzet d:\\otthoni\\naplófájlok\\alkalmazás\\a kivételhiba lépett fel a részletes naplózás-errors.txt fájlt. Ez a fájl a Papírhalom követés tartoznak, így az javításához válassza az alkalmazás ez alapján.

2.  Az alkalmazás használata van más túl sok memória, amely az első lépések más folyamatok gyengíti. Ha a teljes virtuális memória 100 %-os közelébe, a node.exe sikerült levágni, a folyamat menedzser, hogy más folyamatok első néhány munkavégzés lehetőséget. A hiba kijavításához győződjön meg arról, hogy az alkalmazás nem memóriavesztést, vagy ha, alkalmazás valójában memória sok használ, kérjük, méretezhető egy nagyobb virtuális sokkal több RAM a.

### <a name="my-node-application-does-not-start"></a>A csomópont-alkalmazás nem indul el

Ha az alkalmazás indításakor 500 hibát ad vissza, néhány oka lehet:

1.  NODE.exe nem szerepel a a megfelelő helyen. Jelölje be a nodeProcessCommandLine beállítást.

2.  Fő parancsprogram nem szerepel a a megfelelő helyen. Jelölje be a web.config, és ellenőrizze, hogy a fő parancsfájl kezelők szakaszában neve megegyezik a fő parancsprogram.

3.  Web.config esetén nem beállítás megfelelő – jelölje be a nevek/értékeit.

4.  Hideg indításkor – az alkalmazás túl sokáig tart az indítási. Ha az alkalmazás tovább tart, mint (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000 másodperc, iisnode 500 hibát ad eredményül. Növelje meg ezeket a beállításokat, az alkalmazás kezdetének, hogy időzítését, és a 500 hibát visszaadó iisnode megfelelően az értékeket.

### <a name="my-node-application-crashed"></a>Lefagyott az csomópont-alkalmazás

Az alkalmazás van nem kivételek – értesítő, kérjük, jelölőnégyzet d:\\otthoni\\naplófájlok\\alkalmazás\\a kivételhiba lépett fel a részletes naplózás-errors.txt fájlt. Ez a fájl a Papírhalom követés tartoznak, így az javításához válassza az alkalmazás ez alapján.

### <a name="my-node-application-takes-too-much-time-to-startup-cold-start"></a>A csomópont-alkalmazás szükséges túl sok idő indítási (hideg indítás)

Leggyakoribb ennek oka, hogy az alkalmazás rengeteg a csomópont fájlok\_modulok és az alkalmazás próbálja meg ezeket a fájlokat a legtöbb indításkor betöltött. Alapértelmezés szerint a fájlok állnak a hálózati megosztás Azure webalkalmazás, mivel sok fájlok betöltése időt vehet igénybe néhány.
Néhány Ez legyen gyorsabban megoldást a következők:

1.  Győződjön meg arról, hogy egy egyszerű, strukturálatlan függőség szerkezetűek, és nem ismétlődő függőségek a modul telepítése npm3 használatával.

2.  Lusta megpróbálja a csomópont betöltése\_modulok és nem töltődik be minden indításakor a modulokat. Ez azt jelenti, hogy a hívást require('module') ténylegesen szükség esetén a próbálja használni a modul függvényen belül kell elvégezni.

3.  Azure webalkalmazás egy helyi gyorsítótár nevű funkció kínál. Ez a funkció a hálózati megosztásról a tartalom másolása a helyi lemezen a virtuális meg. Mivel a fájlok helyi, csomópont betöltése idején\_modulok sokkal gyorsabban esetén. – Ez a [dokumentáció](../app-service/app-service-local-cache.md) helyi gyorsítótár használata részletesen ismerteti.

## <a name="iisnode-http-status-and-substatus"></a>IISNODE http állapot és részállapot

A [forrásfájl](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) megjeleníti a lehetséges állapot/részállapot kombináció iisnode hiba esetén térhet vissza.

Az alkalmazás a win32 hibakód megtekintéséhez FREB engedélyezése (Győződjön meg arról, hogy csak a teljesítmény okok miatt nem éles webhelyek FREB engedélyezése).

| HTTP-állapot | HTTP részállapot | Lehetséges oka?                                                                                                                                                                                            
|-------------|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------
| 500         | 1000           | Néhány az értekezlet-összehívást IISNODE elküldési probléma történt – ellenőrzése, ha node.exe indított-e. NODE.exe sikerült összeomlott indításkor. Jelölje be a web.config konfigurációs hibákat.                                                                                                                                                                                                                                                                                     |
| 500         | az 1001           | -Win32Error 0x2 - alkalmazás nem válaszol az URL-címet. Jelölje be az URL-cím átírása szabályok, vagy ha az express alkalmazást tartalmaz-e a helyes útvonalak definiált. -Túlterhelt Win32Error 0x6d – elnevezett függőleges vonás – Node.exe nem fogad kéréseket mivel a függőleges vonás elfoglalt. Jelölje be a processzort. -Más hibákat – ellenőrizze, hogy ha node.exe összeomlott.
| 500         | 1002           | NODE.exe összeomlott – d: ellenőrzése\\otthoni\\naplófájlok\\naplózás-errors.txt és Papírhalom nyomkövetési.                                                                                                                                                                                                                                                                                                                                                                                        |
| 500         | 1003           | Konfigurációs probléma pipe – kell soha nem jelenik meg, de ha nem tesz, az elnevezett függőleges vonás konfiguráció nem megfelelő.                                                                                                                                                                                                                                                                                                                                                          |
| 500         | 1004-1018      | A kérelem küld, vagy a válasz vagy a node.exe feldolgozása közben valamilyen hiba történt. Ha node.exe lefagyott jelölőnégyzetet. d: ellenőrzése\\otthoni\\naplófájlok\\naplózás-errors.txt és Papírhalom nyomkövetési.                                                                                                                                                                                                                                                                                    |
| 503         | 1000           | Nem elegendő memória további elnevezett függőleges vonás kapcsolatok. Miért az alkalmazás használata más annyira memória jelölőnégyzetet. Jelölje be a maxConcurrentRequestsPerProcess beállítás értékét. Ha nem végtelen, és rendelkezik, kérelmeket, növelje ezt az értéket, hogy ezt a hibát.                                                                                                                                                                                                                                                                                                                  |
| 503         | az 1001           | Kérés nem sikerült node.exe továbbítani, mert az alkalmazás újrafelhasználás van. Az alkalmazás újrahasznosított rendelkezik, miután kérelmek a szokásos módon felszolgált kell.                                                                                                                                                                                                                                                                                                               |
| 503         | 1002           | Nem sikerült jelölőnégyzet win32 hibakód tényleges okból – kérelem továbbítani szeretné egy node.exe.                                                                                                                                                                                                                                                                                                                                                                               |
| 503         | 1003           | Elnevezett függőleges vonás túlterhelt – Ha csomópont használata Processzor sok más ellenőrzése                                                                                                                                                                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         
Egy beállítás nevű CSOMÓPONTOT NODE.exe belül\_váró\_függőleges vonás\_példányok. Azure webalkalmazás kívüli alapértelmezés szerint az értéke 4. Ez azt jelenti, hogy node.exe csak 4 kéréseket fogadhasson a névvel ellátott függőleges vonás egy időben. A Azure webalkalmazás Ez az érték 5000 van állítva, és ezt az értéket kell elég jó, a legtöbb azure webalkalmazás futó csomópont-alkalmazásokhoz. Nem kellene látnia 503.1003 azure webalkalmazás meg, mert a magas érték van a csomópont\_váró\_függőleges vonás\_példányok.  |

## <a name="more-resources"></a>További források

Kövesse ezeket a hivatkozásokat, ha többet szeretne tudni az Azure alkalmazás szolgáltatás node.js-alkalmazásokkal.

* [Azure App szolgáltatásban Node.js web Apps alkalmazások – első lépések](app-service-web-nodejs-get-started.md)
* [Hogyan szeretné hibakeresése Node.js webalkalmazást Azure App szolgáltatásban](web-sites-nodejs-debug.md)
* [Azure alkalmazások Node.js modulok használata](../nodejs-use-node-modules-azure-apps.md)
* [Azure alkalmazás szolgáltatás Web Apps alkalmazások: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [NODE.js Developer Center](../nodejs-use-node-modules-azure-apps.md)
* [A kiemelt titkos Kudu hibakeresési konzol felfedezése](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)