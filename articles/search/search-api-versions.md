<properties
   pageTitle="Azure keresési API változatának |} Microsoft Azure |} Keresés API"
   description="Azure keresési REST API-k és az ügyfél tárba, a .NET SDK verzió házirend."
   services="search"
   documentationCenter=""
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="dotnet"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="08/16/2016"
   ms.author="brjohnst"/>

# <a name="api-versions-in-azure-search"></a>Azure keresés API-verziók

Azure keresési szolgáltatás frissítések összesíti az rendszeresen. Időnként, de nem mindig alábbi frissítéseket kell nekünk az API új verziójának közzététele a korábbi verziókkal való kompatibilitás megőrzése érdekében. Új verzió közzététele lehetővé teszi szabályozhatja, hogy mikor és hogyan keresési szolgáltatásfrissítések integrálnia be a kódot.

Szabály akkor próbálja meg közzétenni új verzió csak akkor, ha szükséges, óta is tartalmazhat, akkor néhány munkamennyiség frissíteni a kódot, és új API-verziót használja. Új verzió csak azt teszi közzé, ha kell változtatnia néhány szempont a API oly módon, hogy a korábbi verziókkal való kompatibilitás töréspontok. Ez akkor fordulhat elő, meglévő szolgáltatások megoldásai miatt, vagy módosítsa a meglévő API felületének új funkciók miatt.

Azt hajtsa végre ugyanezt a szabályt a SDK frissítések keresése hivatkozásra. Az Azure keresési SDK követi a [szemantikai verziószámozás](http://semver.org/) szabályokat, ami azt jelenti, hogy a verzióját három részből áll: fő alverzió és build száma (például 1.1.0). Azt a módosításokat, amelyeket a korábbi verziókkal való kompatibilitás megszüntetése esetén csak a SDK új főverzióját jelentetünk. Nem törhető szolgáltatás frissítéseit azt növelik a alverzió, és a hibajavítás csak növelik, azt a verziójával.

##<a name="snapshot-of-current-versions"></a>Az aktuális verzióira pillanatfelvétel 

Alatti van az aktuális verziója az összes pillanatképét fejlesztői felületek Azure keresés. Ütemtervben és más adatokat a dokumentum későbbi szakaszában találhatók.

Kapcsolatok|Legutóbbi főverzió|Állapot
----------|-------------------------|------
[.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)|1.1|Általában elérhető, megjelenik az február 2016-ban
[.NET SDK előzetes verzió](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx)|2.0 – előzetes verzió|A minta kiadott augusztus 2016-ban
[Szolgáltatás REST API-val](https://msdn.microsoft.com/library/azure/dn798935.aspx)|2015-02-28|Általában elérhető
[Szolgáltatás REST API-val előzetes verzió](search-api-2015-02-28-preview.md)|2015-02-28 – előzetes verzió|Előzetes verzió
[Adatkezelési REST API-val](https://msdn.microsoft.com/library/azure/dn832684.aspx)|2015-08-19|Általában elérhető

Az a REST API-k, beleértve a `api-version` minden hívás szükség. Ezzel megkönnyíti az egy adott verziójához, például az API előnézetét. Az alábbi példa bemutatja a `api-version` paraméter meg van adva:

    GET https://adventure-works.search.windows.net/indexes/bikes?api-version=2015-02-28

> [AZURE.NOTE] Bár az egyes kérés egy `api-version`, javasoljuk, hogy a megfelelő verziójú használhatja az összes API kérelem. Ez a különösen igaz, ha új API verzióiban bevezetésére, attribútumok és a műveleteket, korábbi verziók nem ismeri fel. API-verziók keverése beállíthatja, hogy a várt következmények és kerülendő.
> 
A szolgáltatás REST API-t és a kezelés REST API-t az verziószámmal, egymástól függetlenül is. Bármely hasonló verziószámai közös járulékos.

Általában elérhető (vagy kiadás) API-khoz gyártási is használható, és Azure szolgáltatás szintű rendelkezést érvényes. Előzetes verzió kísérleti funkciói áttelepítése nem történik mindig Georgia verziójára van. **Ajánlott termelési alkalmazásokat az előnézet API-k használata szemben.**

##<a name="sdk-version-roadmap"></a>SDK verzió – útmutató

A .NET SDK minden verzió célként szolgáltatás REST API-t egy adott verzióját. Szolgáltatások közzétételének a REST API-t az első, és kattintson a SDK szerepelni fog.

A .NET SDK most általában elérhető és munka már folyamatban van a következő verzióját. Az alábbi táblázat a következő lépés milyen újdonságokat arról, hogy a SDK jövőbeli verzióiban formátumban előre.

.NET SDK verziója|REST API-verzió|Szolgáltatások|EURÓPAI
----------------|----------------|--------|---
1.1|2015-02-28|Lucene lekérdezés szintaxisa|Február 2016-ban
2.0 – előzetes verzió|2015-02-28 – előzetes verzió|Egyéni gázelemzők, az Azure Blob és a táblázat indexelő, mező megfeleltetésének, ETags|Augusztus 2016-ban
2.x|Új Georgia API-verzió|Ugyanaz, mint 2.0 – előzetes verzió|Korai 4 2016-ban

##<a name="about-preview-and-generally-available-versions"></a>A körlevél megtekintése és általában elérhető változatairól

Azure keresési mindig előre elengedi az REST API-k kísérleti szolgáltatások először, majd a .NET SDK előzetes verziója között.

Előzetes verzió szolgáltatások nem garantált áttelepítendő Georgia verziójára. Mivel Georgia verziójában szolgáltatások tekinthetők stabil és valószínű, hogy a kis visszamenőlegesen is kompatibilisek javítások és továbbfejlesztett kivételével módosítása, tesztelése és a visszajelzések összegyűjtése a szolgáltatás tervezéséhez és kivitelezéséhez célját kísérletezés előzetes verzió szolgáltatások érhetők el. 

Jó helyen jár mivel előzetes verzió szolgáltatások változhatnak, javasoljuk, hogy kódírás gyártási, hogy mi függőség előzetes verzióján szemben. Ha egy régebbi előzetes verzióját használja, azt javasoljuk, általában elérhető (kiadás) verziójára áttelepítése. 

A .NET SDK az: kód áttérési útmutató a következő helyen található [a .NET SDK frissítése](search-dotnet-sdk-migration.md).

Általános elérhetősége azt jelenti, hogy Azure keresési most csoportban a szolgáltatásiszint-szerződés (SLA). A SLA [Azure keresési szolgáltatás szint rendelkezést](https://azure.microsoft.com/support/legal/sla/search/v1_0/)találhatók.

