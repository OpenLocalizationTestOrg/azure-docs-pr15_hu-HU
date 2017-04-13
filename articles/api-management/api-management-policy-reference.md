<properties 
    pageTitle="Azure API adatkezelési házirend-hivatkozás" 
    description="Tudjon meg többet a konfigurálásához API-kezelési házirendek." 
    services="api-management" 
    documentationCenter="" 
    authors="vladvino" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="apimpm"/>

# <a name="azure-api-management-policy-reference"></a>Azure API adatkezelési házirend-hivatkozás

Ez a témakör index a házirendek az [API adatkezelési házirend hivatkozást][]. Információt meg felvételével és konfigurálásával házirendek című témakörben talál [az API-kezelési házirendek][].

Házirend-kifejezések is alkalmazható attribútum vagy szöveges értékek közül az API-kezelési házirendek kivéve, ha a házirend mást ad meg. Néhány irányelvet, például a [Control flow][] és [-változó beállítása][] házirendek házirend kifejezések alapulnak. További tudnivalókért lásd: a [Speciális házirendek][] és a [házirend-kifejezések][]

## <a name="policy-reference-index"></a>Házirend-hivatkozás index

-   [Hozzáférés-korlátozás házirendek][]
    -   [Jelölje be a HTTP-fejléc][] - kényszeríti jelenléte és/vagy a HTTP-fejléc értékét.
    -   [Hívás arányt korlát előfizetés][] - megakadályozza, hogy az API felülettel hívás ráta egy előfizetés alapon korlátozásával napra.
    -   [Korlát hívás ráta billentyű](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) - megakadályozza, hogy az API felülettel hívás ráta / kulcs alapon korlátozásával napra.
    -   [Korlátozott hívó IP-címei][] - szűrők (lehetővé teszi, hogy/megtagadja) hívások az adott IP-címek és/vagy címtartományok.
    -   [Előfizetés keretében használati kvóta beállítása][] - lehetővé teszi a hivatkozási megújuló vagy élettartam hívás hangerejét, illetve a sávszélesség kvóta, egy előfizetés alapon.
    -   [Billentyű használati kvóta beállítása](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) - lehetővé teszi a hivatkozási megújuló vagy élettartam hívás hangerejét, illetve a sávszélesség kvóta, / kulcs alapon.
    -   [Az érvényesítés JWT][] - kényszeríti megléte és egy megadott HTTP fejléc vagy a megadott lekérdezési paraméter kinyert JWT érvényességét.
-   [Speciális házirendek][]
    -   [Control flow][] - feltételesen vonatkozik, a logikai [kifejezés][]kiértékelésétől eredményén alapuló házirendi.
    -   [Előre kérést][] - továbbítja az értekezlet-összehívást a kódmentes szolgáltatást.
    -   [Log esemény-hubon keresztül csatlakozott][] – a megadott formátumban, egy üzenet célba egy [naplózó](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) személy által meghatározott küld üzeneteket.
    -   [Újra](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) - zárt házirendi újrapróbálkozások végrehajtását Ha, amíg a feltétel nem teljesül. Végrehajtási ismételje meg a megadott időközönként, és a felfelé a megadott darabszám újra.
    -   [Válasz visszatérési](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) - megszakítja folyamat végrehajtás, és adja eredményül a megadott választ közvetlenül a hívó.
    -   [Egyirányú-összehívás küldése](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) – egy kérést küld a megadott URL-cím Várakozás a válaszra nélkül.
    -   A [kérelem küldése](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) – kérést küld a megadott URL-cím.
    -   [Set kérelem metódus](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) - kérés HTTP módjának módosítása teszi lehetővé.
    -   A megadott érték, [állapot beállítása](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) - a HTTP-állapotkód változik.
    -   [-Változó beállítása][] - marad meg egy újabb access elnevezett [helyi][] változó érték.
    -   A [nyomkövetés](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) - be az [API felügyelő](../api-management/api-management-howto-api-inspector.md) kimeneti összeadja a karakterlánc.
    -   Zárt küldés kérés Get mezőbe írja be a gyorsítótár vagy a Control flow házirendek elvégzéséhez a folytatás előtt [várja](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) - várakozik.
-   [Hitelesítési házirendek][]
    -   [A Basic hitelesítés][] - hitelesítés, alapszintű hitelesítés használatával kódmentes szolgáltatást.
    -   [Hitelesítés ügyfél tanúsítvány][] - hitelesítés ügyfél tanúsítványokkal kódmentes szolgáltatást.
-   [Gyorsítótár-házirendek][] 
    -   [Beolvasása a gyorsítótár][] - gyorsítótár végrehajtása keresheti meg, és vissza egy érvényes gyorsítótárazott választ, ha lehetséges.
    -   [Tár gyorsítótár][] - gyorsítótárát válasz, a megadott gyorsítótár-ellenőrzési konfiguráció megfelelően.
    -   [Érték gyorsítótárból](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) - lekérés billentyű egy gyorsítótárazott elemre.
    -   [Tároló érték a gyorsítótár](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) - tár egy kulcsot a gyorsítótár elemre.
    -   [Eltávolítása a gyorsítótárból érték](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) - eltávolítása egy kulcsot a gyorsítótár elemre.
-   [Tartomány házirendek Cross][] 
    -   [A tartományok közötti hívások][] - teszi az API Adobe Flash és a Microsoft Silverlight böngészőalapú ügyfelektől érkező.
    -   [CORS][] - megosztása (CORS) támogatási művelet és az API-t lehetővé teszi a tartományok közötti hívások böngészőalapú ügyfelektől érkező határokon származási erőforrás hozzáadása
    -   [JSONP][] - engedélyezni a JavaScript böngészőalapú ügyfelektől tartományok közötti hívások felveszi JSON művelet belső margók (JSONP) támogatásával vagy egy API-t.
-   [Átalakítási házirendek][] 
    -   [JSON konvertálása XML][] - alakítja kérések és válaszok szervezet JSON az XML-fájlba.
    -   [JSON konvertálása XML][] - értékké alakul kérés vagy választ XML-ből szervezet, JSON számára.
    -   [Keresése és cseréje a szervezet a karakterlánc][] - karakterláncot keres egy kérések és válaszok karakterlánc, és kicseréli egy másik karakterlánc.
    -   [Tartalom URL-címek maszk][] - hivatkozások (maszkok) újra beírja a válaszban szervezet, hogy azok mutasson az átjáró keresztül egyenértékű hivatkozásra.
    -   A bejövő felkérés kódmentes szolgáltatást, [kódmentes szolgáltatás set][] - változik.
    -   Az üzenet törzsébe, a bejövő és kimenő kérelmek [beállítása a szervezet][] - állítja be.
    -   [Állítsa a HTTP-fejléc][] - hozzárendel egy értéket egy meglévő választ, illetve kérés fejléce, vagy új választ, illetve a kérelem élőfejbe felveszi.
    -   A [lekérdezési karakterlánc paraméter értéke][] - veszi fel, cseréli értéke, vagy törli kérése a lekérdezési karakterlánc paraméter.
    -   [Átírásának URL][] - alakítja a kérelem URL-címe nyilvános formájába a képernyőn a webszolgáltatás által elvárt.

## <a name="next-steps"></a>Következő lépések

Házirend kifejezésekre további tudnivalókért lásd: az alábbi videóban.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Hozzáférés-korlátozás házirendek]: https://msdn.microsoft.com/library/azure/dn894078.aspx
[Jelölje be a HTTP-fejléc]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader
[Előfizetés hívás arányt a korlát]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate
[Korlátozza a hívó IP-címei]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs
[Előfizetés keretében set-használati kvóta]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota
[JWT ellenőrzése]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT

[Speciális házirendek]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Változó beállítása]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[a kifejezések]: https://msdn.microsoft.com/library/azure/dn910913.aspx
[környezet]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables
[Előre kérést]: https://msdn.microsoft.com/library/azure/dn894085.aspx#ForwardRequest
[Esemény-hubon keresztül csatlakozott napló]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[Hitelesítési házirendek]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[A Basic hitelesítő]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[Ügyfél-tanúsítványt a hitelesítéshez]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[Gyorsítótár-házirendek]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[Ismerkedés a gyorsítótárból]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[Tárolni gyorsítótárba]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[Tartomány házirendek Cross]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[Lehetővé teszi a tartományok közötti hívások]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[Átalakítási házirendek]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[JSON konvertálása XML]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[XML JSON konvertálása]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
[Keresés és csere törzsébe karakterlánc]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody
[A tartalom maszkot URL-címei]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent
[Kódmentes szolgáltatás beállítása]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService
[A szervezet beállítása]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody
[HTTP-fejléc beállítása]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader
[Karakterlánc lekérdezésparaméter beállítása]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter
[Átírás URL-címe]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL



[Az API-kezelési házirendek]: api-management-howto-policies.md
[API adatkezelési házirend-hivatkozás]: https://msdn.microsoft.com/library/azure/dn894081.aspx

[Házirend-kifejezések]: https://msdn.microsoft.com/library/azure/dn910913.aspx

 
