<properties
   pageTitle="Méretezhető webalkalmazás |} Azure hivatkozás architektúra |} Microsoft Azure"
   description="A Microsoft Azure-ban futó webalkalmazás méretezhetőség javításához."
   services="app-service,app-service\web,sql-database"
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/12/2016"
   ms.author="mwasson"/>


# <a name="improving-scalability-in-a-web-application"></a>A webes alkalmazásokban méretezhetőség javításához 

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Ez a cikk bemutatja a javasolt architektúra méretezhetőség és a Microsoft Azure futó webalkalmazás a teljesítmény javítására. A architektúrára épül [Azure hivatkozás architektúra: egyszerű webalkalmazás][basic-web-app]. Ez a felépítés javaslatok és a cikkben szempontok vonatkoznak.

>[AZURE.NOTE] Azure van két különböző telepítési modellek: az erőforrás-kezelő és klasszikus. Ez a cikk az erőforrás-kezelő Microsoft javasolja új telepítési használja.

## <a name="architecture-diagram"></a>Architektúra diagramja

![[0]][0]

Az alábbi összetevőket architektúrája foglalja magában:

- **Erőforráscsoport**. [Erőforráscsoport] [ resource-group] egy logikai tároló Azure erőforrások. 

- ** [Web app] [ app-service-web-app] ** és ** [API-alkalmazás][app-service-api-app]**. Egy tipikus modern alkalmazás egy webhelyen és egy vagy több RESTful webes API-khoz is tartalmazhatnak olyan. A webes API előfordulhat, hogy felhasználandó, AJAX keresztül böngésző ügyfelek, natív ügyfélalkalmazások vagy kiszolgálóoldali alkalmazások. Az kapcsolatos szempontok tervezése webes API-hoz, lásd: az [API tervezési útmutató][api-guidance].    

- **WebJob**. [Azure WebJobs] használata[ webjobs] hosszabb ideig futó feladatok futtatása a háttérben. WebJobs futtatását is lehetővé teszi, ütemezés, változásáig, vagy az eseményindító, például az üzenet elhelyezése egy várólista választ. Egy WebJob fut a háttér folyamat alkalmazás szolgáltatás alkalmazás környezetében. 

- **A várakozási**. Az itt ismertetett architektúrában az alkalmazás sorban várakozó a háttérben futó feladatok Ha elhelyez egy üzenetet az [Azure várólista-tároló] alakzatot[ queue-storage] várólista. Az üzenet elindítja a WebJob a függvény. 

    Másik lehetőségként szolgáltatás Bus sorban várakozó is használhatja. Összehasonlítása érdekében lásd: [Azure sorban várakozó szolgáltatás Bus sorban várakozó - összehasonlítás és ellentétben][queues-compared].

- **Gyorsítótár**. [Azure vgx.dll]gyorsítótárban félig statikus adattárolásra[azure-redis].  

- **CDN-t**. [Azure tartalom kézbesítési hálózatot] használ[ azure-cdn] (CDN) gyorsítótár nyilvánosan elérhető tartalmat az alsó időtartama és tartalom gyorsabb kézbesítve.

- **Adattárolás.** [Azure SQL-adatbázis] használata[ sql-db] relációs adatok. Nem relációs adatok, fontolja meg egy NoSQL mentése, például Azure Táblatárolóhoz vagy [DocumentDB][documentdb].

- **Azure keresése**. [Azure] keresés[ azure-search] keresési funkciót is tartalmaz, beleértve a keresési javaslatok, zavaros keresési és nyelvspecifikus keresési hozzáadni. Azure keresési van jellemzően együtt egy másik adatok mentése, különösen akkor, ha az elsődleges adattár szigorú konzisztencia van szükség. Ezt a megközelítést lenne a adatokat tároló mérvadó adatait, és a keresési index üzembe Azure keresés. Azure keresési is használható összesítése egyetlen keresési index létrehozása a több adatokat tárolja.  

- **E-mailben vagy SMS**. Ha az alkalmazás e-mailben vagy SMS-üzenetek küldése, használja a egy külső szolgáltatásnak például SendGrid vagy Twilio helyett ezt a funkciót létrehozása közvetlenül az alkalmazásba.

## <a name="recommendations"></a>Javaslatok

### <a name="app-service-apps"></a>Alkalmazás szolgáltatás alkalmazások 

Azt javasoljuk, hogy a webes alkalmazás és a webes API létrehozása külön alkalmazás szolgáltatás Apps. A tervezés lehetővé teszi futtatását őket külön App milyen szolgáltatáscsomagok, ami viszont lehetővé teszi az önállóan átméretezheti őket. Ha már nincs szükség az, hogy a méretezhetőség szintjét először telepítheti az alkalmazások ugyanarra a csomagra szóló, és át őket külön csomagok később szükség esetén. (Basic, normál és prémium csomag esetén meg vannak számlát kapni a virtuális előfordulását a csomagban, nem egy alkalmazást. Lásd: az [alkalmazás szolgáltatás árak][app-service-pricing])

Ha azt szeretné, *Könnyen táblák* vagy alkalmazás szolgáltatás Mobile-alkalmazások *Egyszerű API -khoz* szolgáltatásai, külön alkalmazás szolgáltatás alkalmazás létrehozása erre a célra.  Ezek a funkciók egy adott alkalmazás keretrendszer tudjanak támaszkodhat.

### <a name="webjobs"></a>WebJobs

Ha a WebJob erőforrás-igényes folyamat, fontolja meg egy külön alkalmazás szolgáltatás terv dedikált példányok nyújt a WebJob belül üres alkalmazás szolgáltatás alkalmazás való üzembe helyezése. Lásd: a [háttér feladatok útmutatást][webjobs-guidance].  

### <a name="cache"></a>Gyorsítótár

Javíthatja a teljesítmény és méretezhetőség: [Azure vgx.dll gyorsítótár] használatával[ azure-redis] gyorsítótárban néhány adatot. Fontolja meg inkább vgx.dll gyorsítótár beállítása:

- Részben statikus tranzakció adatokat.

- Munkamenet-állapotot.

- HTML-kimenet. Ez lehet hasznos az alkalmazásokat, amelyek összetett HTML kimenet jeleníti meg. 

Részletes útmutatást a gyorsítótár stratégia kialakítása, lásd: [Útmutató gyorsítótárazás][caching-guidance].

### <a name="cdn"></a>CDN 

[Azure CDN] használata[ azure-cdn] gyorsítótár statikus tartalommá. A fő a CDN előnye csökkentheti a késés a felhasználóknak, mivel a tartalom gyorsítótárazott egy *biztonsági kiszolgálójának* közelébe a felhasználó földrajzi van. CDN is csökkentheti az alkalmazás, a betöltés mert adott forgalmat kezeli az alkalmazás nem alatt.

- Ha az alkalmazás főleg statikus lapok áll, fontolja meg CDN gyorsítótárban a teljes alkalmazást. Ismerje meg, [az Azure alkalmazás szolgáltatás Azure CDN][cdn-app-service].

- Egyéb esetben statikus tartalommá, például képek, helyezze a CSS és a HTML fájlok, az Azure-tárhely és CDN-t használja ezeket a fájlokat a gyorsítótárban. [Integráció a tárhely fiókot CDN]látható[cdn-storage-account].

> [AZURE.NOTE] Azure CDN nem szolgálják hitelesítést igénylő tartalmat.

Részletes útmutatást, lásd: a [tartalom kézbesítési hálózati (CDN) útmutatást][cdn-guidance]. 

### <a name="storage"></a>Tárhely

Alkalmazásokat modern gyakran feldolgozni nagy mennyiségű adatot. Annak érdekében, hogy az a felhő méretezéséhez fontos válassza ki a megfelelő tároló típusát. Az alábbiakban néhány eredeti javaslatok.  Részletes útmutatást, lásd: [Felmérése adatokat tároló funkciók Polyglot adatmegőrzési megoldások][polyglot-storage].

Mit szeretne tárolni | Példa | Ajánlott tárolási
--- | --- | ---
Fájlok | Képek, dokumentumok, PDF-fájlok | Azure Blob-tárolóhoz
Kulcs/érték párokká | Felhasználói azonosító által keresett felhasználói profiladatok | Azure Táblatárolója
Rövid üzenetek szánt feldolgozásra elindítása | Rendelés kérések | Azure várólista tárhely, a szolgáltatás Bus várólista vagy a szolgáltatás Bus témakör
Nem relációs adatok, egy rugalmas séma, igénylő egyszerű lekérdezés használata | Termékkatalógus | Dokumentum-adatbázisban, például az Azure DocumentDB, MongoDB vagy Apache CouchDB
Relációs adatok sokoldalúbb lekérdezések támogatását, szigorú séma és/vagy erős konzisztencia megkövetelése | Termék leltár | Azure SQL-adatbázis

## <a name="scalability-considerations"></a>Méretezhetőség kapcsolatos szempontok

### <a name="app-service-app"></a>Alkalmazás szolgáltatás alkalmazás

Ha a megoldás több alkalmazás szolgáltatás alkalmazások tartalmaz, érdemes megfontolni a App milyen szolgáltatáscsomagok elkülönítésére őket. Ezt a megközelítést lehetővé teszi a független, átméretezheti őket, mert a különálló példányai futnak. Méretezés kifelé kapcsolatos további tudnivalókért lásd: a [méretezhetőség szempontok] [ basic-web-app-scalability] szakaszban az [egyszerű webes alkalmazás architektúra][basic-web-app].

Hasonlóképpen fontolja meg egy WebJob üzembe helyezése a saját verzió esetében, hogy a háttérben futó feladatokat az azonos példányok, amely a HTTP-kérelmek kezeléséhez nem futtatni.  

### <a name="sql-database"></a>SQL-adatbázis

SQL-adatbázis méretezhetőség növelése *sharding* az adatbázis &mdash; Ez azt jelenti, hogy szétválasztás vízszintesen az adatbázist. Sharding lehetővé teszi, hogy az adatbázis vízszintesen méretezése [rugalmas Adatbáziseszközök][sql-elastic]. Sharding potenciális előnyei többek között:

- Jobb tranzakció kapacitása.

- Lekérdezések gyorsabban futtatható az adatok egy részhalmazát fölé. 

### <a name="azure-search"></a>Azure keresés

Azure keresési eltávolítja a terhelést a komplex adatok bemutatásához keresések elvégzéséhez az elsődleges adatok áruházból, valamint azt a betöltés kezelésére is méretezhető. Lásd: [a lekérdezés és Azure keresése az indexelő munkaterhelésekből skála erőforrásszintek][azure-search-scaling].

## <a name="security-considerations"></a>Biztonsági megfontolások

### <a name="cross-origin-resource-sharing-cors"></a>Idegen származási erőforrás (CORS) megosztása

Ha egy webhely és a webes API külön Apps hoz létre, a webhely nem hívásokat ügyféloldali AJAX az API-nak kivéve, ha engedélyezi a CORS. 

> [AZURE.NOTE] Böngésző biztonsági megakadályozza, hogy az weblapon AJAX-kérelmek tétele a további tartomány. Ezt a korlátozást az azonos származási házirend neve, és megakadályozhatja, hogy a kártékony webhely sentitive adatok beolvasása más webhelyről. CORS, amely lehetővé teszi, hogy az azonos származási házirend enyhítése, és néhány idegen származási kérések engedélyezése közben elvetésével mások kiszolgáló W3C szabvány.

Alkalmazás szolgáltatások tartalmaz a beépített támogatása CORS, anélkül, hogy minden alkalmazás kódírás. Lásd: [felhasználandó CORS használatáról JavaScript API alkalmazás][cors]. A webhely felvétele engedélyezett forrásokból listája az API. 

### <a name="sql-database-encryption"></a>SQL-adatbázis titkosítás:

[Áttetsző] titkosítással[ sql-encryption] Ha módosítani szeretné az adatokat a többi az adatbázis titkosítása. Ez a funkció anélkül, hogy az alkalmazás módosításainak valós idejű titkosítást és (beleértve a biztonsági mentés és tranzakció naplófájlok), teljes adatbázis visszafejtése hajt végre. Titkosítás hozzáadása néhány a késés, ezért tanácsos külön kell biztonságos saját adatbázisba, és csak az adatbázis titkosítása engedélyezése adatokat.  

## <a name="next-steps"></a>Következő lépések

- Magasabb rendelkezésre állás, egynél több területen az alkalmazás telepítéséhez és [Azure forgalom Manager] használata[ tm] feladatátvételi. További tudnivalókért lásd: [Azure hivatkozás architektúra: a magas rendelkezésre webalkalmazás][web-app-multi-region].    

<!-- links -->

[api-guidance]: ../best-practices-api-design.md
[app-service-web-app]: ../app-service-web/app-service-web-overview.md
[app-service-api-app]: ../app-service-api/app-service-api-apps-why-best-platform.md
[app-service-pricing]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[azure-cdn]: https://azure.microsoft.com/en-us/services/cdn/
[azure-redis]: https://azure.microsoft.com/en-us/services/cache/
[azure-search]: https://azure.microsoft.com/en-us/documentation/services/search/
[azure-search-scaling]: ../search/search-capacity-planning.md
[background-jobs]: ../best-practices-background-jobs.md
[basic-web-app]: guidance-web-apps-basic.md
[basic-web-app-scalability]: guidance-web-apps-basic.md#scalability-considerations 
[caching-guidance]: ../best-practices-caching.md
[cdn-app-service]: ../app-service-web/cdn-websites-with-cdn.md
[cdn-storage-account]: ../cdn/cdn-create-a-storage-account-with-cdn.md
[cdn-guidance]: ../best-practices-cdn.md
[cors]: ../app-service-api/app-service-api-cors-consume-javascript.md
[documentdb]: https://azure.microsoft.com/en-us/documentation/services/documentdb/
[polyglot-storage]: https://github.com/mspnp/azure-guidance/blob/master/Polyglot-Solutions.md
[queue-storage]: ../storage/storage-dotnet-how-to-use-queues.md
[queues-compared]: ../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md
[resource-group]: ../resource-group-overview.md
[sql-db]: https://azure.microsoft.com/en-us/documentation/services/sql-database/
[sql-elastic]: ../sql-database/sql-database-elastic-scale-introduction.md
[sql-encryption]: https://msdn.microsoft.com/en-us/library/dn948096.aspx
[tm]: https://azure.microsoft.com/en-us/services/traffic-manager/
[web-app-multi-region]: ./guidance-web-apps-multi-region.md
[webjobs-guidance]: ../best-practices-background-jobs.md
[webjobs]: ../app-service/app-service-webjobs-readme.md
[0]: ./media/blueprints/paas-web-scalability.png "Webalkalmazás továbbfejlesztett méretezhetőség Azure-ban"