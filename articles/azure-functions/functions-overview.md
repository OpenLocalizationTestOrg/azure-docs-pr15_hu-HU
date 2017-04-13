<properties
   pageTitle="Azure funkciók áttekintése |} Microsoft Azure"
   description="Megtudhatja, hogyan Azure függvénnyel történő optimalizálásához aszinkron munkaterhelésekből perc alatt."
   services="functions"
   documentationCenter="na"
   authors="mattchenderson"
   manager="erikre"
   editor=""
   tags=""
   keywords="Azure függvények funkciók, esemény feldolgozása, webhooks, dinamikus számítási, kiszolgáló nélküli architektúra"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/29/2016"
   ms.author="cfowler;mahender;glenga"/>
   
   
# <a name="azure-functions-overview"></a>Azure függvények áttekintése

Azure függvények egyszerűen futó kódot, vagy a "függvények," kis darabokra megoldást megtalálható a felhőben. Csak a kód anélkül, hogy a teljes alkalmazást vagy az infrastruktúra futtatásához szükséges kéznél, a probléma is írhat. A fel, hogy fejlesztési még hatékonyabb, és a választási lehetőségek, például a C#, F #, Node.js, Python vagy PHP fejlesztési nyelvének is használhatja. Fizetni csak a futtatásakor a kód és Azure adatvédelmi Ha át kívánja méretezni, szükség szerint.

Ez a témakör a magas szintű Azure funkciók áttekintése. Ha szeretne a belevesse és első lépések az Azure függvények, kezdje a [létrehozása az Azure első függvény](functions-create-first-azure-function.md). Ha függvények technikai információt keres, olvassa el a a [Fejlesztői segédlet](functions-reference.md)című témakört.

## <a name="features"></a>Szolgáltatások

Íme néhány fontosabb funkciói Azure funkciók:
    
* **Választási lehetőségek, a nyelv** - C#, F #, Node.js, Python, PHP, köteg, bash, Java vagy bármely végrehajtható írási függvények.
* **Használati fizetési árak modell** – csak a kód futtatását töltött idő a fizetés. A dinamikus alkalmazás szolgáltatás megtervezése beállítás a [árak rész](#pricing) alatt jelenik meg.  
* **Jelenítse meg a saját függőségek** - funkciókat támogatja a NuGet és NPM, hogy használni tudja a kedvenc tárakhoz.  
* **Integrált adatvédelem** – például az Azure Active Directory, a Facebook, a Google, a Twitteren és a Microsoft-Account OAuth szolgáltatók függvények védelme HTTP indított.  
* **Egyszerűsített integrációs** – egyszerűen kihasználhatja az Azure-szolgáltatások és a szoftver-mint-a-(szoftver) szolgáltatásajánlatok. Olvassa el a [integrációs rész](#integrations) alatt néhány példát talál.  
* **Rugalmas fejlesztői** - kód a függvények jobbra a portálon vagy folyamatos integráció beállítása és a kód GitHub, a Visual Studio Team Services és a más [Fejlesztőeszközök támogatott](../app-service-web/web-sites-deploy.md#deploy-using-an-ide)üzembe.  
* **Megnyitás-adatforrás** - a függvényeket futási idejű, a Megnyitás-forrás- és [a GitHub érhető el](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>Mire van lehetőségem azokat a funkciókat?

Azure funkciók integrálása a rendszerek, az internet-a-dolgot (IoT) használata és létrehozása egyszerű API-k és microservices feldolgozás adatok esetén kiváló megoldás. Fontolja meg a függvények, például kép vagy rendelés feldolgozása, fájl karbantartás, hosszabb ideig futó feladatok futtatása a háttérben egy szál kívánt tevékenységek, vagy bármely tevékenységek ütemezett futtatni kívánt. 

Funkciókat nyújt az első lépések az esetek, többek között az alábbi sablonok:

* **BlobTrigger** - folyamat Azure tároló BLOB tárolók való hozzáadásakor. A kép átméretezése használható.
* Az Azure esemény központi kézbesítve **EventHubTrigger** - események válaszolni. Az alkalmazás műszerezettségi, felhasználói felület vagy a munkafolyamat feldolgozása és dolog Internet (IoT) esetek különösen hasznos.
* **Általános webhook** - folyamat webhook HTTP bármely szolgáltatás, amely támogatja az webhooks kér.
* **GitHub webhook** – a GitHub tárházakban található események válaszolni. Példát olvassa el a [létrehozása egy webhook vagy API függvény](functions-create-a-web-hook-or-api-function.md)című témakört.
* **HTTPTrigger** - hatására a kód használatával a HTTP-kérelem végrehajtását.
* **QueueTrigger** - válasz a leveleket, hogy a beérkező az Azure tárolás várakozási sorában. Egy példa című témakörben [létrehozása egy Azure függvény, amely az Azure szolgáltatás kötődik](functions-create-an-azure-connected-function.md).
* **ServiceBusQueueTrigger** - csatlakozzon a kód más Azure szolgáltatások vagy helyszíni szolgáltatások hallgat üzenet sorok szerint. 
* **ServiceBusTopicTrigger** - csatlakozzon a kód más Azure szolgáltatások vagy helyszíni szolgáltatások témakörökre mutató feliratkozással. 
* **TimerTrigger** - karbantartási vagy más köteg feladatok végrehajtása előre meghatározott időközönként. Példát olvassa el a [feldolgozása függvény esemény létrehozása](functions-create-an-event-processing-function.md)című témakört.

Azure funkciókat támogatja a *Eseményindítók*, amelyek a kód végrehajtását megkezdése, és a *kötések*, amelyek kódolásának bemeneti és kimeneti adatok egyszerűsítése módjai. Eseményindítók és kötés, ahol Azure függvények részletes leírása olvassa el a [Azure függvények eseményindítók és kötések Fejlesztői segédlet](functions-triggers-bindings.md)című témakört.


## <a name="integrations"></a>Integrációs eszközök

Azure függvények integrálódik a különféle Azure és 3rd külső szolgáltatásokra. Használhatja ezeket a funkció, és indítsa el a végrehajtását vagy lesz a bemeneti és kimeneti a kód. A következő szolgáltatás integrációs Azure függvények által támogatott. 

* Azure DocumentDB
* Azure esemény hubok 
* Azure Mobile-alkalmazások (táblák)
* Azure értesítés hubok
* Azure Service Bus (sorban várakozó és témakörök)
* Azure tároló (blob, sorok és táblázatok) 
* GitHub (webhooks)
* Helyszíni (használ szolgáltatási Bus)

## <a name="pricing"></a>Mennyibe kerül a költség működik?

Azure függvények kétféle csomagok árak rendelkezik, válassza a eszközt, amely a legjobban megfelel az igényeinek: 

* **Dinamikus szolgáltatója terv** – a funkció futtatásakor, a Azure ad meg, az összes szükséges számítási erőforrás. Nem kell aggódnia amiatt, hogy az erőforrás-kezelés, vagy ha csak a kód futtató alkalommal. Teljes árak részletek [lap függvények árak](/pricing/details/functions)érhetők el. 

* **Alkalmazás szolgáltatáscsomagja** – a Funkciók, mint a webes, a mobile és az API-alkalmazások futtatásához. Ha a többi alkalmazást az App szolgáltatás már használ, futtathatja a függvények ugyanarra a csomagra külön költség nélkül. Minden részlet [alkalmazás szolgáltatás árak lapon](/pricing/details/app-service/)találhatók.

További információt a méretezés a Funkciók, megtudhatja, [hogy miként méretezheti Azure függvények](functions-scale.md).

##<a name="next-steps"></a>Következő lépések

+ [Az első Azure függvény létrehozása](functions-create-first-azure-function.md)  
Ugrás jobbra, és létrehozása az Azure függvények quickstart útmutató az első funkció. 
+ [Azure függvények Fejlesztői segédlet](functions-reference.md)  
Az Azure függvényeket futási idejű és a hivatkozások technikai információt tartalmaz a függvények kódolási és eseményindítók és kötések definiálása.
+ [Azure függvények tesztelése](functions-test-a-function.md)  
Különböző eszközök és a függvények teszteléshez technikákat ismerteti.
+ [Hogyan méretezheti Azure függvények](functions-scale.md)  
Ismerteti, hogy milyen szolgáltatáscsomagok Azure-függvényekkel, például a dinamikus szolgáltatás csomagot, és válassza ki a megfelelő csomagot számára érhető el. 
+ [További tudnivalók az Azure alkalmazás szolgáltatás](../app-service/app-service-value-prop-what-is.md)  
Azure függvények az Azure alkalmazás szolgáltatás platform az alapvető funkcionalitást, például a telepítések, környezeti változók és diagnosztika használja. 