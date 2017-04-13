<properties
    pageTitle="Választhatja a folyamat, logika alkalmazások, funkciók és WebJobs |} Microsoft Azure"
    description="Compare alkalmazással és a kontraszt a felhő integráció a Microsoft-szolgáltatások és döntse el, mely szolgáltatásokat kell használnia."
    services="functions,app-service\logic"
    documentationCenter="na"
    authors="cephalin"
    manager="wpickett"
    tags=""
    keywords="a Microsoft folyamat, folyamat, logika alkalmazások, azure funkciók függvények azure webjobs, webjobs, esemény feldolgozása, dinamikus számítási, kiszolgáló nélküli architektúra"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="chrande; glenga"/>

# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Folyamatábra, logika alkalmazások, funkciók és WebJobs közül választhat

Ez a cikk hasonlítja össze, és kiemeli a a Microsoft felhőben, amely az összes orvosolható integrációs problémák és az üzleti folyamatok automatizálása az alábbi szolgáltatásokat:

- [A Microsoft továbbításához](https://flow.microsoft.com/)
- [Azure összefüggés-alkalmazások](https://azure.microsoft.com/services/logic-apps/)
- [Azure függvények](https://azure.microsoft.com/services/functions/)
- [Azure alkalmazás szolgáltatás WebJobs](../app-service-web/web-sites-create-web-jobs.md)

Ezek a szolgáltatások hasznosak, ha a "kapcsolása" közös elemezve rendszerek. Azok az összes definiálhatók beviteli, a műveletek, a feltételeket és a kimeneti. Ütemezés vagy az eseményindító mindegyiket futtathatók. Azonban egyes szolgáltatások hozzáadása a szülőwebhelyhez értéket, és összehasonlítani nem található a"melyik szolgáltatás a legjobb?" kérdés de egyik "melyik szolgáltatás leginkább alkalmas Ez a helyzet?" Az alábbi szolgáltatások kombinációi gyakran a legegyszerűbben úgy, hogy gyorsan készíthet egy méretezhető, teljes kiemelt integrációs megoldás.

<a name="flow"></a>
## <a name="flow-vs-logic-apps"></a>Munkafolyamat összehasonlítása összefüggés-alkalmazások

Is témákat ölelik fel a Microsoft Flow és Azure logika alkalmazások közös mert túllépik mindkét *konfiguráció – első* integrációs szolgáltatások, ami megkönnyíti a folyamatokat és munkafolyamatokat összeállítása és integráció a különböző szoftver és vállalati alkalmazások. 

- Logika alkalmazások épülő munkafolyamat
- Az ugyanazon a munkafolyamat-Tervező rendelkeznek
- Munka valamelyik megbízhatja a másik [összekötők](../connectors/apis-list.md)

Flow felhatalmazza bármelyik office dolgozó az egyszerű integrációs műveletek (pl. beszerzése SMS fontos e-mailek) anélkül, hogy a fejlesztők keresztül vagy informatikai. Logika alkalmazások azonban, engedélyezheti a speciális vagy kulcsfontosságú integrációs (pl. B2B folyamatok) ahol vállalati szintű DevOps és biztonsági eljárások szükség. Projektvezetői összetettsége túlóra az üzleti munkafolyamat tipikus. Ennek megfelelően indítása az első folyamat, majd átalakíthatja a logikájának alkalmazásba, szükség szerint.

Az alábbi táblázat segítségével állapítsa meg, hogy folyamatot vagy logikai alkalmazások egy adott integrációs ajánlott.

|               | Folyamatábra                                                                             | Logika alkalmazások                                                                                          |
|---------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| A célközönség      | az Office dolgozók, a vállalati verzió                                                   | Az informatikai szakemberek, fejlesztők számára                                                                                 |
| Felhasználási területei     | Önkiszolgáló                                                                     | Kritikus fontosságú                                                                                    |
| Webhelytervező eszköz   | A-böngészőben, csak a felhasználói felület                                                              | A böngészőben és a [Visual Studio](../app-service/logic/app-service-logic-deploy-from-vs.md)és az elérhető [kód megtekintése](../app-service-logic/app-service-logic-author-definitions.md) |
| DevOps        | Alkalmi, gyártási kidolgozása                                                    | adatforrás-vezérlő, tesztelés, támogatását, és automatizálási és [Azure az erőforrás-kezelés](../app-service-logic/app-service-logic-arm-provision.md) kezelhetősége|
| Rendszergazdai élmény| [https://flow.microsoft.com](https://flow.microsoft.com)                       | [https://Portal.Azure.com](https://portal.azure.com)                                                |
| Biztonsági      | Szabványos eljárások: [adatszuverenitás](https://wikipedia.org/wiki/Technological_Sovereignty), [a többi titkosítási](https://wikipedia.org/wiki/Data_at_rest#Encryption) bizalmas adatokat stb. | Az Azure Security garancia: [Azure biztonsági](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Biztonság otthon](https://azure.microsoft.com/services/security-center/), [Naplók](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/)és az egyéb. |

<a name="function"></a>
## <a name="functions-vs-webjobs"></a>Függvények összehasonlítása WebJobs

Azt is vitafórum-témakörök Azure funkciók és Azure alkalmazás szolgáltatás WebJobs közös mert mindkét *kód – első* integrációs szolgáltatások, és a fejlesztők számára készült. Lehetővé teszik a különböző eseményeket, például [Tároló BLOB-új](functions-bindings-storage.md) vagy [egy kérése WebHook](functions-bindings-http-webhook.md)válaszul parancsfájlt vagy valamilyen kód futtatásához. Az alábbiakban azok hasonlóságokat: 

- [Azure alkalmazás](../app-service/app-service-value-prop-what-is.md) szolgáltatása kialakításának mind fogják túlságosan nagyra értékelni a szolgáltatások, például [verziókövetést](../app-service-web/app-service-continuous-deployment.md) [hitelesítési](../app-service/app-service-authentication-overview.md)és [figyelése](../app-service-web/web-sites-monitor.md).
- Fejlesztőeszközök fókuszáló szolgáltatások.
- A szokásos parancsfájlok támogatási mind programozási nyelven.
- NuGet és támogatási NPM is.

Függvények WebJobs természetes alakulása, abban, hogy a legjobb dolog, amit WebJobs kapcsolatos és javítja őket. Fejlesztések: 

- Egyszerűsített fejlesztők, tesztelése és az elérhető kód, közvetlenül a böngészőben.
- További Azure és 3rd külső szolgáltatásokat, például [GitHub WebHooks](https://developer.github.com/webhooks/creating/)beépített integrációja.
- Fizetési-használati, nem kell fizetni az [Alkalmazás szolgáltatás megtervezése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
- Automatikus, [dinamikus méretezését](functions-scale.md).
- A meglévő ügyfelek alkalmazás szolgáltatás, futó alkalmazás szolgáltatáscsomagja továbbra is lehetséges (kihasználhatja a kihasználtság erőforrásokat).
- Integráció a összefüggés-alkalmazások használatát.

Az alábbi táblázat összefoglalja a függvények és WebJobs közötti különbségeket:

|                        | Függvények                                                                                                                                                                | WebJobs                            |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------|
| Méretezés                | Configurationless méretezése                                                                                                                                                | az alkalmazás szolgáltatáscsomagja méretezése        |
| Árak                | Használati fizetési vagy egy részének alkalmazás szolgáltatáscsomagja                                                                                                                                  | Alkalmazás szolgáltatáscsomagja része           |
| Futtatás-típus               | elindul, az ütemezett (időzítő eseményindító)                                                                                                                                  | indítóval kezdeményezett, folytonos, ütemezett   |
| Kiváltó események         | [az időzítőszolgáltatás](functions-bindings-timer.md), [Azure DocumentDB](functions-bindings-documentdb.md), [Azure esemény hubok](functions-bindings-event-hubs), [HTTP/WebHook (GitHub, tartalékidő)](functions-bindings-http-webhook.md), [Azure alkalmazás szolgáltatás Mobile-alkalmazások](functions-bindings-mobile-apps.md), [Azure értesítés hubok](functions-bindings-notification-hubs.md), [Azure Service Bus](functions-bindings-service-bus.md), [Azure-tárolóhoz](articles/functions-bindings-storage.md) | [Azure tárhely](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), [Azure Service Bus](websites-dotnet-webjobs-sdk-service-bus.md)         |
| A böngészőben fejlesztési | x                                                                                                                                                                        |                                    |
| A parancsfájlok ablak       | kísérleti                                                                                                                                                             | x                                  |
| A PowerShell             | kísérleti                                                                                                                                                             | x                                  |
| C#                     | x                                                                                                                                                                        | x                                  |
| F#                     | x                                                                                                                                                                        |                                    |
| Bash                   | kísérleti                                                                                                                                                             | x                                  |
| PHP                    | kísérleti                                                                                                                                                             | x                                  |
| Python                 | kísérleti                                                                                                                                                             | x                                  |
| A JavaScript             | x                                                                                                                                                                        | x                                  |
| Java                   | kísérleti                                                                                                                                                             | x                                  |

Kíván-e használni függvényekről vagy WebJobs végül függ, hogy milyen máris alkalmazás szolgáltatásával. Ha egy alkalmazás szolgáltatás alkalmazás futtatásához a kódtöredék kívánt, és kezelésére őket közösen ugyanazon DevOps környezetben, WebJobs kell használnia. Ha a használni kívánt egyéb Azure szolgáltatások és még 3rd felektől származó alkalmazások kódtöredék, vagy ha szeretné kezelni a integrációs kódtöredék külön-külön, az alkalmazás szolgáltatás alkalmazásokból, vagy ha fel szeretne hívni a kódtöredék logika alkalmazásból, meg kell kihasználhatja az összes javítása a függvények.  

<a name="together"></a>
## <a name="flow-logic-apps-and-functions-together"></a>Áramlás, összefüggés-alkalmazások és a függvények együtt

A korábban említett melyik szolgáltatás a legmegfelelőbb, akkor a helyzettől függ. 

- Egyszerű üzleti optimalizálás majd használja továbbításához.
- Ha a integrációs forgatókönyvek túl van speciális továbbításhoz, vagy DevOps funkciók és biztonsági megfelelőségi van szüksége, majd használja a logika alkalmazások.
- Ha a integrációs forgatókönyvek olyan lépés van szüksége, rendkívül egyéni átalakítása vagy speciális kódot, majd írja be a függvény-at, és majd elindítani a függvény a logikájának alkalmazásban műveletet.

Egy folyamat egy logikai alkalmazást is felhívhatja. Függvény egy érték, és egy logikai alkalmazást a függvényben is felhívhatja. Folyamatábra, összefüggés-alkalmazások és a függvények integrációja túlóra javítása továbbra is. Összeállítása valamit, amit egy szolgáltatásban, és a más szolgáltatásokhoz használni. Bármely, így a következő három technológiák befektetés ezért egyeztetni.

## <a name="next-steps"></a>Következő lépések

Első lépések az egyes szolgáltatások az első munkafolyamat, logika alkalmazást, függvény alkalmazásban, illetve WebJob létrehozásával. Kattintson az alábbi hivatkozások egyikére:

- [A Microsoft Flow – első lépések](https://flow.microsoft.com/en-us/documentation/getting-started/)
- [Logika alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Az első Azure függvény létrehozása](../azure-functions/functions-create-first-azure-function.md)
- [Visual Studio segítségével WebJobs terjesztése](../app-service-web/websites-dotnet-deploy-webjobs.md)

Vagy többet is megtudhat az alábbi integrációs szolgáltatások a következő hivatkozásokkal:

- [Az Azure funkciók és Azure alkalmazás szolgáltatás által Christopher Anderson integrációs forgatókönyvek](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
- [Egyszerű tett Charles Lamanna integrációs eszközök](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
- [Logika alkalmazások élő adás](http://aka.ms/logicappslive)
- [A Microsoft Flow gyakran ismételt kérdések](https://flow.microsoft.com/documentation/frequently-asked-questions/)
- [Azure WebJobs dokumentáció erőforrások](../app-service-web/websites-webjobs-resources.md)
