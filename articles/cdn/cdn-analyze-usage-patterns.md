<properties
    pageTitle="Azure CDN szokásai elemzése |} Microsoft Azure"
    description="Az a CDN, az alábbi jelentések használata szokásai megtekintése: sávszélesség átvitt adatok, a találatok, gyorsítótár állapotok, gyorsítótár találati arány, IPV4/IPV6 adatátvitel."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="analyze-azure-cdn-usage-patterns"></a>Azure CDN szokásai elemzése

[AZURE.INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

A következő jelentések használata CDN szokásai tekintheti meg:

- A sávszélesség
- Átvitt adatok
- A találatok
- Gyorsítótár állapotok
- Találati arány gyorsítótárban
- Átvitt IPv4/IPV6-adatok

## <a name="accessing-advanced-http-reports"></a>Speciális HTTP-jelentések elérése

1. Az a CDN a profil lap a **kezelés** gombra.

    ![CDN-profil lap kezelése gomb](./media/cdn-reports/cdn-manage-btn.png)

    Ekkor megnyílik a CDN kezelőportálja segítségével.

2. Az **elemzés** lapon mutasson, majd mutasson a **Core jelentések** előugró.  Kattintson a kívánt jelentést, a menüben.

    ![CDN - Core jelentések menü kezelőportálja](./media/cdn-reports/cdn-core-reports.png)


## <a name="bandwidth"></a>A sávszélesség

A sávszélesség-jelentés áll egy graph és az adatok táblázatot, amely tartalmazza a sávszélesség-használat a HTTP- és HTTPS egy adott időszakban. A sávszélesség-használat minden CDN POP vagy az adott POP keresztül is megtekintheti. Ez lehetővé teszi, hogy tekintse meg a forgalom kiugrásainak megfelelő és a terjesztési CDN megjelenik a MB közötti.

- Jelölje ki a forgalmat a csomópontjait megtekintéséhez, vagy a legördülő listából választhat egy adott terület csomópont csomópontjait él.
- Jelölje ki a nyomtatandó dátumtartomány nézet adatok ma a héten/havi, stb vagy adja meg egyéni dátumokat, majd kattintson a "Ugrás" Ellenőrizze, hogy a kijelölés frissül.
- Exportálhatja, és töltse le az adatokat az excel-munkalap "Ugrás" mellett található ikonra kattintva.

A jelentés 5 percenként frissül.

![Sávszélesség-jelentés](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Átvitt adatok

Ez a jelentés, ahol a forgalom használatát jelző HTTP- és HTTPS-KAPCSOLATOKAT az adott időszakban graph és az adatok táblázat. A forgalom használatát minden CDN POP vagy az adott POP keresztül is megtekintheti. Ez lehetővé teszi, hogy a forgalom kiugrásainak megfelelő és a terjesztési megtekintése keresztül CDN megjelenik a GB.

- Jelölje ki az összes feljegyzés érkező forgalmat megtekintéséhez, vagy a legördülő listából választhat egy adott terület csomópont csomópontjait él.
- Jelölje ki a nyomtatandó dátumtartomány nézet adatok ma a héten/havi, stb vagy adja meg egyéni dátumokat, majd válassza az "Ugrás" Ellenőrizze, hogy a kijelölés frissül.
- Exportálhatja, és töltse le az adatokat az excel-munkalap "Ugrás" mellett található ikonra kattintva.

A jelentés 5 percenként frissül.

![Az adatátvitel jelentés](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>A találatok (állapot kódok)

Ez a jelentés a kérelem állapot kódokat a tartalom számára az eloszlás ismerteti. Minden tartalom kérése egy HTTP állapotkód hoz létre. Állapotkód ismerteti, hogy a kérelem él POP kezelésének módját. 2xx állapot kódok például azt jelzik, hogy a kérelem lett sikeresen felszolgált ügyfélnek, miközben 4xx állapotkódot azt jelzi, hiba történt. Ha többet szeretne tudni a HTTP-állapotkód lásd: [állapot kódok](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

- Jelölje ki a nyomtatandó dátumtartomány nézet adatok ma a héten/havi, stb vagy adja meg egyéni dátumokat, majd válassza az "Ugrás" Ellenőrizze, hogy a kijelölés frissül.
- Exportálhatja, és töltse le az adatokat az excel-munkalap "Ugrás" ikonra kattintva.

![A találatok jelentés](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Gyorsítótár állapotok

Ez a jelentés gyorsítótár-találatok és az ügyfél kérése a gyorsítótár mért sikertelen találatok eloszlása ismerteti. Mivel a leggyorsabb teljesítményt gyorsítótár-találatok származik, optimalizálhatja az adatok kézbesítési sebesség gyorsítótár mért sikertelen találatok minimalizálása és a lejárt gyorsítótár-találatok. Gyorsítótár mért sikertelen találatok csökkenthető a forráskiszolgálóval elkerülése érdekében, "nincs-gyorsítótár" válasz fejlécek hozzárendelése konfigurálásával elkerülése érdekében a lekérdezés-karakterlánc gyorsítótárazás kivéve, ha feltétlenül szükséges és nem cacheable válasz kódok elkerülése érdekében. A találatok elkerülhetők legyenek azáltal, hogy az eszköz lejárt gyorsítótár addig, amíg a forráskiszolgálóval kérések számának csökkentése érdekében a lehető adatait a max-életkorát.

![Gyorsítótár állapotok jelentés](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Fő gyorsítótár állapotok a következők:

- TCP_HIT: Szélétől fel. Az objektum a gyorsítótár és volna ne lépjék túl a max életkorát.
- TCP_MISS: Az Origin fel. Az objektum nem volt a gyorsítótár és a válasz vissza az origin.
- TCP_EXPIRED _MISS: az origin ismételt érvényesítése után az origin felszolgált. Az objektum gyorsítótárban volt, de a max kor nagyobb volt. Egy ismételt érvényesítése az origin következtében a gyorsítótár objektum origin az új visszajelzés helyettesíti.
- TCP_EXPIRED _HIT: után az origin ismételt érvényesítése szélétől felszolgált. Az objektum gyorsítótárban volt, de a max kor nagyobb volt. Egy ismételt érvényesítése a forráskiszolgálóval a következtében a gyorsítótár objektum változatlanul alatt.

- Jelölje ki a nyomtatandó dátumtartomány nézet adatok ma a héten/havi, stb vagy adja meg egyéni dátumokat, majd kattintson a "Ugrás" Ellenőrizze, hogy a kijelölés frissül.
- Exportálhatja, és töltse le az adatokat az excel-munkalap "Ugrás" mellett található ikonra kattintva.

### <a name="full-list-of-cache-statuses"></a>Gyorsítótár állapotok teljes listája

- TCP_HIT – ezt az állapotot, amikor egy kérelmet a POP-ről az ügyfél felszolgált jelenteni. Tárgyi eszköz azonnal fel, ha a POP, az ügyfél legközelebb a gyorsítótárban van, és van érvényes time-to-live POP vagy TTL (élettartam). TTL (élettartam) a következő válasz fejlécek határozza meg:

    - Gyorsítótár-vezérlők: s-maxage
    - Gyorsítótár-vezérlők: max-kor
    - Lejár

- TCP_MISS – ezt az állapotot azt jelzi, hogy a kért eszköz gyorsítótárazott verzióját nem található meg a POP, az ügyfél legközelebb. A digitáliseszköz-forráskiszolgálóval vagy a védelem forráskiszolgálóval kérni fogja. Ha a forráskiszolgálóval vagy a védelem forráskiszolgálóval adja eredményül egy eszköz, az ügyfélprogram és az ügyfél és a biztonsági kiszolgálón gyorsítótárazott. Ellenkező esetben nem 200 állapotkód (például 403 mozgást, 404 nem található, stb.) vissza kell.

- TCP_EXPIRED _HIT – ezt az állapotot, amikor egy tárgyi eszköz egy lejárt TTL, ha az eszköz max életkor lejárt, például a célzott kérelem közvetlenül a POP, az ügyfél számára felszolgált jelenteni.

    Lejárt felkérés általában eredményeinek ismételt érvényesítése összehívásban a forráskiszolgálóval. Ahhoz, hogy egy TCP_EXPIRED _HIT fennáll a forráskiszolgálóval kell azt jelzik, hogy az eszköz újabb verzióját nem létezik. A helyzet a következő típusú általában frissül, hogy eszköz gyorsítótár-vezérlők és elévülési fejlécek.

- TCP_EXPIRED _MISS – ezt az állapotot van jelenteni, amikor lejárt gyorsítótárazott tárgyi eszköz újabb verzióját a POP a felszolgált az ügyfél számára. Ez akkor fordul elő, ha a TTL (élettartam), a gyorsítótárban tárolt eszköz lejárt (például lejárt max életkor) és a forráskiszolgálóval eszköz újabb verzióját adja eredményül. Ez az eszköz verzióját szolgáltató helyett a gyorsítótárban lévő verziót az ügyfél számára. Ezenkívül azt a gyorsítótárban tartja a biztonsági kiszolgálójának és az ügyfél.

- CONFIG_NOCACHE – Ez azt jelzi, hogy egy ügyfélnek szól konfigurációt a meg él mentén POP megakadályozza az eszköz gyorsítótárba.

- Nincs – Ez azt jelzi, hogy a gyorsítótár-tartalom frissessége ellenőrzése nem történt meg.

- TCP_ CLIENT_REFRESH _MISS – ezt az állapotot kell jelenteni, amikor egy HTTP-ügyfél (például a böngészőben) kezd a elavult eszköz új verziója beolvasni a forráskiszolgálóval POP él.

    Alapértelmezés szerint kiszolgálóiról megakadályozzák a HTTP-ügyfél az eszköz új verziója beolvasásához a forráskiszolgálóval a peremhálózat-kiszolgálói kényszerítése.

- TCP_ PARTIAL_HIT – ezt az állapotot kell jelenteni, ha egy bájt tartomány kérelmet részben gyorsítótárazott eszköz találat eredményezi. A kért bájt tartományt közvetlenül az ügyfélnek fel a POP.

- UNCACHEABLE – ezt az állapotot van jelenteni, amikor egy digitáliseszköz-gyorsítótár-vezérlők és elévülési fejlécek jelzi, hogy meg kell nem lehet gyorsítótárba POP vagy a HTTP-ügyfél által. Az ilyen típusú kérelmek a forráskiszolgálóval a szolgáltatott

## <a name="cache-hit-ratio"></a>Találati arány gyorsítótárban

Ez a jelentés azt jelzi, hogy közvetlenül a gyorsítótár kombinációja gyorsítótárazott kérések százalékát.

A jelentés tartalmazza az alábbiakat:

- A kívánt tartalmat a POP, az igénylő legközelebb a lett gyorsítótárazott.
- A kérés közvetlenül a hálózatba szélétől lett fel.
- A kérés nem volt szükség a forráskiszolgálóval a ismételt érvényesítése.

A jelentés nem tartalmazza:

- Ország szűrési beállítások miatt van megtagadja összehívások.
- Eszközök fejlécekhez jelzi, hogy azok nem gyorsítótárba kérelem. Gyorsítótár-vezérlők, például: lehetőséget választva személyes, gyorsítótár-vezérlők: nincs-gyorsítótár vagy Pragma: nincs-gyorsítótár fejlécek megakadályozza, hogy az eszköz gyorsítótárba.
- Tartomány kérelem bájt részben gyorsítótárban tárolt tartalmat.

A képlet: (TCP_ TALÁLAT / (TCP_ TALÁLAT + TCP_MISS)) * 100

- Jelölje ki a nyomtatandó dátumtartomány nézet adatok ma a héten/havi, stb vagy adja meg egyéni dátumokat, majd kattintson a "Ugrás" Ellenőrizze, hogy a kijelölés frissül.
- Exportálhatja, és töltse le az adatokat az excel-munkalap "Ugrás" mellett található ikonra kattintva.


![A gyorsítótárban ráta jelentés](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>Átvitt IPv4/IPV6-adatok

Ez a jelentés a forgalom használatát eloszlás IPV4-és az IPV6 mutatja.

![Átvitt IPv4/IPV6-adatok](./media/cdn-reports/cdn-ipv4-ipv6.png)

- Jelölje ki a nyomtatandó dátumtartomány megtekintése a ma a héten/ebben a hónapban adatok, stb, vagy írja be az egyéni dátum.
- Kattintson a "Ugrás" Ellenőrizze, hogy a kijelölés frissül.


## <a name="considerations"></a>Megfontolandó szempontok

Jelentések csak a 18 utolsó hónapon belül hozható létre.
