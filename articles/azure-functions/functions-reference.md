<properties
    pageTitle="Azure függvények Fejlesztői segédlet |} Microsoft Azure"
    description="Azure függvények fogalmak és -összetevők, az összes nyelvek és kötések közös megértése"
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure függvények funkciók, esemény feldolgozása, webhooks, dinamikus számítási, kiszolgáló nélküli architektúra"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-developer-reference"></a>Azure függvények Fejlesztői segédlet

Azure függvények oszthat meg néhány alapvető technikai fogalmak és -összetevők, függetlenül attól, a nyelv vagy kötelező használnia. Mielőtt meg egy adott nyelvre vagy kötés adott részletek tanuló ugorhat, feltétlenül olvassa el az Áttekintés mindegyikük alkalmazott.

Ez a cikk azt feltételezi, hogy már olvasott az [Azure függvények áttekintése](functions-overview.md) , és a jól ismert [WebJobs SDK fogalmak például eseményindítók, kötések, és a JobHost futtatókörnyezet](../app-service-web/websites-dotnet-webjobs-sdk.md). Azure függvényeket alapuló a WebJobs SDK csomagjában talál. 


## <a name="function-code"></a>Kód függvény

A *függvény* Azure-függvényekkel elsődleges fogalma. Tetszés szerinti nyelven kódírás függvény és a kód fájlokat és a konfigurációs fájl mentése ugyanabban a mappában. Konfigurációs JSON, és a fájl neve `function.json`. Számos nyelven támogatottak, és egyenként az egy némileg eltérő élmény működnek a legjobban az adott nyelvhez optimalizálva. 

A `function.json` fájl függvény, beleértve annak kötések jellemző konfigurációs tartalmaz. A futtatókörnyezet beolvassa a fájl ki a kiváltó események határozza meg, milyen adatokat, ha meg szeretné jeleníteni a függvényt, és küldje el az adatok hol hívásakor átadott mentén magát a függvény. 

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

Megakadályozhatja, hogy a futtatókörnyezet megadásával a funkció futtatása a `disabled` tulajdonság `true`.

A `bindings` tulajdonság értéke, ahol eseményindítók és a kötések konfigurálása. Minden egyes kötés osztja meg néhány gyakori és beállítások néhány jellemző egy bizonyos típusú kötés. Minden kötés van szükség a következő beállításokat:

|A tulajdonság|Értékek és típusai|Megjegyzések|
|---|-----|------|
|`type`|karakterlánc|Kötés típusát. Ha például `queueTrigger`.
|`direction`|"in" "out"| Azt jelzi, hogy a kötést a függvény az adatok fogadása vagy adatokat küld a függvény.
| `name` | karakterlánc | A függvény az adatokhoz kötött használt név. A C# ez lesz egy argumentum neve; JavaScript-, akkor a kulcsot a kulcs/értéklista.

## <a name="function-app"></a>Alkalmazás függvény

A függvény alkalmazás tevődik egy vagy több egyes funkciók Azure alkalmazás szolgáltatás együttes kezelhetők. A függvények a függvény alkalmazásban az összes oszthat meg a ugyanarra a csomagra árak, folytonos telepítési és runtime verziója. Több nyelven írt függvények összes megoszthatja az azonos függvény alkalmazást. Érdemes a függvény-at, és és a függvények együttesen kezelése lehetőséget. 

## <a name="runtime-script-host-and-web-host"></a>Futási idejű (script host és webes állomás)

A futtatókörnyezet vagy script host, az alapul szolgáló WebJobs SDK állomás hozhat létre, mely események figyeli gyűjt és adatokat küld, és végül futtatja a kódot. 

HTTP indítók megkönnyítésére is van egy webes állomás célja, hogy a script host gyártási helyzetekben elé ülnie. Ezzel az elemzéssel elkülönítése a script host a kezeli a webhely állomáswebhelyén a előtér-forgalmat.

## <a name="folder-structure"></a>Mappastruktúra

[AZURE.INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Ha felállításának egy függvény alkalmazásban Azure alkalmazás szolgáltatás üzembe helyezése a funkciók a projekt, a mappastruktúra is kezelheti a webhely-kód. Használhatja a meglévő eszközök, például a folyamatos integráció és a telepítés, vagy egyéni telepítési parancsfájlok, ezzel az üzembe idő csomag telepítése vagy a transpilation.

>[AZURE.NOTE] A `wwwroot` mappa itt, ahol a fájlok fog üzemelnek szeretne. Azonban nem szerepelnie kell, hogy a mappa a fájlokat telepít, amely esetben a végeredmény `wwwroot\wwwroot`. Ehelyett a `host.json` fájl- és függvény mappák közvetlenül a legfelső szintű, telepíteni kell lennie.

## <a id="fileupdate"></a>Függvény alkalmazás fájlok frissítése

A függvény-szerkesztőben az Azure portál épített lehetővé teszi a frissítése a *function.json* és a kód fájlt függvény. Fel-vagy más fájlok, például *package.json* vagy *project.json* vagy függőségek frissítése, egyéb telepítési módszerrel van.

Függvény alkalmazások kialakításának alkalmazás szolgáltatása, így az összes [rendelkezésre álló szabványos web Apps alkalmazások telepítési lehetőségek](../app-service-web/web-sites-deploy.md) érhetők el, valamint függvény alkalmazások. Íme néhány módszer használhatja fel-vagy függvény alkalmazás fájlok frissítése. 

#### <a name="to-use-app-service-editor"></a>Szolgáltatás-szerkesztő alkalmazás használata

1. Az Azure függvények-portálon kattintson a **függvény app beállításai**parancsra.

2. A **Speciális beállítások** résznél kattintson az **Ugrás a alkalmazás szolgáltatás beállításai**hivatkozásra.

3. Kattintson az **Alkalmazás szolgáltatás szerkesztő** Alkalmazásmenüre navigációs **FEJLESZTŐESZKÖZÖK**alatt.

4.  Kattintson az **Ugrás**gombra.

    Miután alkalmazás szolgáltatás szerkesztő betölti, látni fogja a *wwwroot* *host.json* fájl- és függvény csoportbeli. 

5. Nyissa meg a fájlok ná, illetve húzással a fejlesztés fájlok feltöltése a számítógépről.

#### <a name="to-use-the-function-apps-scm-kudu-endpoint"></a>A függvény alkalmazás SCM (Kudu) végpont használata

1. Nyissa meg azt: `https://<function_app_name>.scm.azurewebsites.net`.

2. Kattintson a **konzol hibakeresési > CMD**.

3. Nyissa meg azt `D:\home\site\wwwroot\` *host.json* frissítése vagy `D:\home\site\wwwroot\<function_name>` egy függvény fájlok frissítéséhez.

4. Húzással történő áthelyezés fel szeretné tölteni a fájlt megjelenítő rácson megfelelő mappába fájl. A fájl rács, ahol húzhatja fájl két területen szerepelnek. A fájlok *.zip* egy jelenik meg a címkét a "Húzza ide tölthet fel, és bontsa ki az." A más fájltípusok engedje el a fájlt megjelenítő rácson, de a "unzip" kívülre.

#### <a name="to-use-ftp"></a>FTP használata

1. Kövesse [az alábbi](../app-service-web/web-sites-deploy.md#ftp) első konfigurált FTP.

2. Kapcsolódik az függvény webhelyét, ha a frissített *host.json* fájl másolása `/site/wwwroot` függvény fájlok másolása vagy `/site/wwwroot/<function_name>`.

#### <a name="to-use-continuous-deployment"></a>Folytonos telepítési használata

Kövesse a következő témakört: [Azure függvények folyamatos telepítésének](functions-continuous-deployment.md)a.

## <a name="parallel-execution"></a>A párhuzamos végrehajtása

Több kiváltó események előfordulásakor gyorsabb, mint egy függvényt egy téma szerint rendezett futtatókörnyezet lehet feldolgozni őket, a futtatókörnyezet hivatkozhat párhuzamosan többször a függvény.  Ha egy függvény alkalmazást használja a [Dinamikus szolgáltatás megtervezése](functions-scale.md#dynamic-service-plan), a függvény alkalmazás sikerült méretezése automatikusan.  A függvény alkalmazás minden példányában az alkalmazás futtat, a dinamikus szolgáltatás megtervezése vagy egy normál [Alkalmazás szolgáltatás tervezi](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), hogy előfordulhat, hogy folyamat egyidejű függvény meghívásához párhuzamosan több szálak használ.  Az egyes függvény alkalmazás példányokban egyidejű függvény meghívásához maximális száma a memória méretét, a függvény alkalmazás függ. 

## <a name="azure-functions-pulse"></a>Azure függvények Pulse  

Pulse, amely mutatja, hogy milyen gyakran a függvény fut, valamint sikeres és sikertelen élő esemény adatfolyam. Figyelheti a átlagos végrehajtási idő is. Azt fogja kell elhelyezni benne további funkciókat és testreszabási, az idő. A **Figyelés** lapon a **Pulse** lapot is elérheti.

## <a name="repositories"></a>Tárházakban

Azure függvények kódját Megnyitás és a tárolt GitHub tárházakban található:

* [Azure függvényeket futási idejű](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Azure függvények portál](https://github.com/projectkudu/AzureFunctionsPortal)
* [Azure függvények sablonok](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/)
* [Azure WebJobs SDK bővítmények](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Kötések

Az alábbiakban az összes támogatott kötések tartalmazó táblázat.

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

## <a name="reporting-issues"></a>Hibák jelentése

[AZURE.INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)] 

## <a name="next-steps"></a>Következő lépések

További információt az alábbi forrásokban talál:

* [Azure függvények C# Fejlesztői segédlet](functions-reference-csharp.md)
* [Azure függvények F # Fejlesztői segédlet](functions-reference-fsharp.md)
* [Azure függvények NodeJS Fejlesztői segédlet](functions-reference-node.md)
* [Azure függvények eseményindítók és kötések](functions-triggers-bindings.md)
* [Azure funkciók: az utazás](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) az Azure alkalmazás szolgáltatás termékcsapat blogjában. Hogyan Azure függvények kifejlesztett előzményeit.





