<properties
    pageTitle="Idegen származási erőforrás megosztása (CORS) támogatási |} Microsoft Azure"
    description="Megtudhatja, hogy miként CORS támogatás engedélyezése a Microsoft Azure tároló szolgáltatást."
    services="storage"
    documentationCenter=".net"
    authors="cbrooks"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="cbrooks"/>

# <a name="cross-origin-resource-sharing-cors-support-for-the-azure-storage-services"></a>Idegen származási erőforrás megosztása a Azure tároló szolgáltatást (CORS) támogatása

2013-08-15 verzió kezdve a Azure tároló szolgáltatások támogatása határokon származási erőforrások megosztása (CORS) a Blob, tábla, várólista és fájl szolgáltatások. CORS fiókfunkció HTTP, amely lehetővé teszi, hogy a webalkalmazás futtató egyik tartományt egy másik tartományban erőforrások eléréséhez. Böngészők megakadályozza, hogy az weblapon egy másik tartományban; hívó API- [azonos származási házirend](http://www.w3.org/Security/wiki/Same_Origin_Policy) neve biztonsági korlátozások megvalósítása Egy tartomány (az origin) fel szeretne hívni egy másik tartomány API-k engedélyezése biztonságos CORS segítségével. Nézze meg a részletek [CORS specifikációja](http://www.w3.org/TR/cors/) CORS.

Beállíthatja, hogy CORS szabályok egyenként az egyes a tároló szolgáltatást, hívja fel a [Blob-tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452235.aspx) [a várakozási szolgáltatás tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452232.aspx)és [Táblázat szolgáltatás tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452240.aspx). Amikor a CORS szabályokat, a szolgáltatás, majd a másik tartományt szemben a szolgáltatás megfelelően hitelesített kérelme kiértékelésre kerül határozza meg, hogy a megadott szabályoknak megfelelő engedélyezett.

>[AZURE.NOTE] Ne feledje, hogy CORS nem hitelesítési módszer. Kérését erőforrás tárolás ellen CORS engedélyezésekor kell lennie a megfelelő hitelesítési aláírás, illetve nyilvános erőforrás ellen kell elvégezni.

## <a name="understanding-cors-requests"></a>CORS kérelmek ismertetése

Az origin tartomány CORS kérése két külön kérések állhat:

- Ellenőrzés kérése, amely a szolgáltatás CORS korlátozásai lekérdezi. Az ellenőrzés kérelem szükség, kivéve, ha a kérelem módszer az [egyszerű módszer](http://www.w3.org/TR/cors/), ami azt jelenti, GET, a fej vagy a bejegyzés.

- A tényleges kérelem a kívánt erőforrás ellen.

### <a name="preflight-request"></a>Ellenőrzés kérés

Az ellenőrzés kérelem lekérdezések a fióktulajdonos a tárhelyszolgáltatáshoz létrehoztak CORS korlátozások. A webböngésző (vagy más felhasználói ügynököt) küld egy beállítások, amely tartalmazza a kérelem fejlécek, módot és origin tartományt. A tároló szolgáltatás kiértékeli a tervezett művelet alapján előre beállított CORS szabályok, amelyek meghatározzák, hogy melyik origin tartományok, kérések és kérelem fejlécek adható meg a tényleges felkérés erőforrás tárolás ellen.

Ha CORS engedélyezve van a szolgáltatás, és az ellenőrzés kérelem CORS szabályt, a szolgáltatás reagál állapotkód 200 (OK), és a szükséges hozzáférési fejlécek szerepel a kérdésre adott választ.

Ha a szolgáltatás nincs engedélyezve a CORS, vagy nincs CORS szabálynak az ellenőrzés kérést, a szolgáltatás válaszol, állapotkód 403 (tiltott).

Ha a beállítások kérelem nem tartalmazza a szükséges CORS fejlécek (az Origin és az Access-vezérlőelem-kérelmek-módja fejlécek), a szolgáltatás válaszol, állapotkód 400 (hibás kérés).

Figyelje meg, hogy a szolgáltatás (Blob várólista és táblázatot) és nem a kért erőforrás elleni kiértékelt ellenőrzés kérést. A fióktulajdonos CORS ahhoz, hogy az értekezlet-összehívást sikeres fiók szolgáltatás tulajdonságainak részeként engedélyezve kell rendelkeznie.

### <a name="actual-request"></a>Tényleges kérés

Miután a ellenőrzés felkérés elfogadása és a választ ad vissza, a böngészőben fog megkeresést tényleges a tárhely erőforrás szemben. A böngészőben a tényleges megtagadja azonnal kérése, ha a ellenőrzés kérelmet elutasították.

A tényleges kérelem szemben a tárhelyszolgáltatáshoz normál kérelem kell kezelni. Az Origin fejlécén jelenléti azt jelzi, hogy a kérelem CORS kérést, és ellenőrzi, hogy a szolgáltatás a megfelelő CORS szabályokat. Ha van találat, a hozzáférés fejlécek a kérdésre adott választ adott hozzá, és küldött az ügyfélnek. Ha nincs találat, a hozzáférés CORS fejlécek nem lehet megjeleníteni.

## <a name="enabling-cors-for-the-azure-storage-services"></a>Az Azure tárolása CORS engedélyezése

CORS szabályok szolgáltatás szintre, de vannak beállítva, így Önnek kell engedélyezése vagy letiltása a CORS az egyes szolgáltatásokhoz (Blob, várólista és táblázatot) külön-külön. Alapértelmezés szerint CORS le van tiltva, az egyes szolgáltatásokhoz. Ahhoz, hogy a CORS, kell beállítania a megfelelő szolgáltatás tulajdonságai verziójával 2013-08-15 vagy újabb, és CORS szabályokat adni a szolgáltatás tulajdonságait. És részletes tudnivalókat engedélyezése és letiltása CORS szolgáltatás CORS szabályok beállításáról olvassa el [Blob-tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452235.aspx), [Várólista szolgáltatást tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452232.aspx)és [Táblázat szolgáltatás tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452240.aspx).

Íme egy mintája szerepel egy egyetlen CORS, a megadott szabály keresztül egy szolgáltatás tulajdonságainak beállítása műveletet:

    <Cors>    
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
            <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
            <MaxAgeInSeconds>200</MaxAgeInSeconds>
        </CorsRule>
    <Cors>

Az egyes elemek szerepelnek a CORS szabály leírása alább:

- **AllowedOrigins**: A origin szemben a via CORS tárhelyszolgáltatáshoz kérelmének engedélyezett tartományok. Az origin tartománya a tartományt, amelyből a kérelem származik. Figyelje meg, hogy az origin kell lennie az origin, amelyek a felhasználói kora a szolgáltatás elküldi a kis-és nagybetűket pontos egyezést. Is használhatja a helyettesítő karakter "*" engedélyezni, hogy a via CORS kérések tartományai origin. A fenti példában a tartományok [http://www.contoso.com](http://www.contoso.com) és [http://www.fabrikam.com](http://www.fabrikam.com) teheti kérések CORS szolgáltatás szemben.

- **AllowedMethods**: az origin tartomány használhat egy CORS kérelmet módszerek (HTTP-kérés műveletek). A fenti példában csak helyezése és GET kérelmek kijelölésének engedélyezése

- **AllowedHeaders**: A kérelem fejlécek, amely az origin tartományt is megadhat a CORS kérésének megfelelően. A fenti példában az x-ms-metaadatok, x-ms-metaadat-tároló és az x-ms-metaadat-abc kezdődő összes metaadatok élőfej kijelölésének engedélyezése Megjegyzendő, hogy a helyettesítő karakter "*" jelzi, hogy engedélyezve van-e minden élőfej kezdődő a megadott előtagot.

- **ExposedHeaders**: A válasz fejlécek, amely a CORS kérelem válaszul és a böngésző a kérelem kibocsátó által elérhetővé tett. A fenti példában a böngészőben az jelenítik meg az x-ms-metaadatok minden élőfej kezdődő utasítást.

- **MaxAgeInSeconds**: A maximális időt, hogy a böngésző gyorsítótár-e az ellenőrzési beállítások kérése.

Az Azure tároló services támogatja a **AllowedHeaders** -és **ExposedHeaders** előre rögzített fejlécek megadása. Ha engedélyezni szeretné a fejlécek kategória, egy közös előtagot, amelyet a kategóriákba tartozó is megadhat. Például az *x-ms-metaadatok*megadása *, előre rögzített élőfej hoz létre egy szabályt, amely az összes élőfej, x-ms-metaadatok kezdődő meg fognak egyezni.

Az alábbi korlátozások vonatkoznak a CORS szabályok vonatkoznak:

- Megadhatja, hogy legfeljebb öt CORS szabályok per tárhelyszolgáltatáshoz (Blob tábla és várólista).
- Az összes CORS szabályok beállítást a kérelmet, XML-címkéket, kivéve a maximális hossza nem haladhatja meg a 2 KB.
- Az engedélyezett fejléc, a elérhető élőfej vagy az engedélyezett origin hossza nem haladhatja meg a 256 karaktert.
- Fejlécek engedélyezett és elérhető fejlécek lehetnek:
  - A pontos névvel amennyiben a, például az **x-ms-metaadat-feldolgozott**konstans fejlécei. Legfeljebb 64 konstans fejlécek a kérésre adható meg.
  - Élőfejek, a fejléc előtaggal amennyiben a, például az **x-ms-metaadatok**előtaggal *. Egy előtagot tartalmazó ily módon lehetővé teszi, vagy bármely fejléc, az adott előtaggal kezdődő közzététele. A kérés legfeljebb két előre rögzített fejlécek adható meg.
- A módszerek (vagy a HTTP-műveletek) az **AllowedMethods** elemet a megadott meg kell felelnie a módszerek Azure tárhelyszolgáltatáshoz API-k által támogatott. Támogatott módszerek az, hogy törlés, GET, vezetője, EGYESÍTÉS, bejegyzés, BEÁLLÍTÁSAINAK és helyezése.

## <a name="understanding-cors-rule-evaluation-logic"></a>CORS szabály kiértékelési logika ismertetése

Ha egy tárhelyszolgáltatáshoz ellenőrzés vagy a tényleges kérelem érkezik, a szolgáltatás a megfelelő szolgáltatás tulajdonságainak beállítása műveletet keresztül létrehozott CORS alapján kérelem értékeli ki. CORS szabályok kiértékelése abban a sorrendben, amelyben azokat meg a szolgáltatás tulajdonságainak beállítása műveletet összehívás törzsében.

CORS szabályok értékeli ki az alábbi képlettel történik:

1. A kérelem origin tartomány először be van jelölve, a **AllowedOrigins** elem felsorolt tartományok szemben. Ha az origin tartomány szerepel a listában, vagy a helyettesítő karaktert engedélyezett tartományok "*", majd a kiértékelés bevétel szabályok. Ha az origin tartomány nem tartalmaz, majd a kérelem sikertelen lesz.

2. Ezután a kérelem módszer (vagy HTTP igei) be van jelölve **AllowedMethods** eleme felsorolt módszerek szemben. Ha a módszerrel szerepel a listában, majd szabályok kiértékelés folytatódik; Ha nem, majd a kérelem sikertelen.

3. A kérelem megegyezik egy szabályt, a származási tartományban és a saját módszer, ha ez a szabály a kérést, és nincsenek további szabályok kiértékelt folyamatban van jelölve. A kérés is sikeresek, mielőtt azonban bármelyik a kérésre megadott fejlécek veti össze a fejlécek szerepel az **AllowedHeaders** elemet. Ha a fejlécek küldött nem egyezik meg a megengedett fejlécek, a kérelem sikertelen lesz.

A szabályok feldolgozása a sorrend összehívás törzsében jelen, mivel gyakorlati tanácsokat javasoljuk, hogy a legszigorúbb szabályokat részletez forrásokból először adni a listában, hogy ezeket először értékelni. Adja meg a szabályok, amelyek kevésbé korlátozó – például egy szabályt, amely lehetővé teszi az összes forrásokból – a lista végére.

### <a name="example--cors-rules-evaluation"></a>Példa – CORS szabályok értékelése

A következő példa bemutatja a tároló szolgáltatást CORS szabályok beállítása a művelet a részleges kérelmet törzsébe. A kérés megépítése kapcsolatos részletekért lásd: [Blob-tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452235.aspx), [Várólista szolgáltatás tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452232.aspx)és [Táblázat szolgáltatás tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452240.aspx) .

    <Cors>
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
            <AllowedMethods>PUT,HEAD</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
        </CorsRule>
        <CorsRule>
            <AllowedOrigins>*</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
        </CorsRule>
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
            <AllowedMethods>GET</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-client-request-id</AllowedHeaders>
        </CorsRule>
    </Cors>

Ezt követően fontolja meg a következő CORS kérések:

Kérés||| Válasz||
---|---|---|---|---
**A módszer** |**Származás** |**Fejlécek kérése** |**A szabály hol.van függvénnyel** |**Eredmény**
**HELYEZÉSE** | http://www.contoso.com |x – az ms-blob-tartalomtípus | Első szabály |A siker
**GET** | http://www.contoso.com| x – az ms-blob-tartalomtípus | Második szabály |A siker
**GET** | http://www.contoso.com| x – az ms-blob-tartalomtípus | Második szabály | Hiba

Az első kérés megegyezik az első szabály – az origin tartomány megegyezik az engedélyezett forrásokból, a módszerrel megegyezik az engedélyezett módszerek és a fejlécet megegyezik az engedélyezett fejlécek – és így sikerül.

A második kérés nem egyezik meg az első szabály, mert a módszerrel nem egyezik meg az engedélyezett módszerek. Azt, azonban felel meg a második szabály, így sikerrel.

A harmadik kérelem origin tartomány és módszer, a második szabály felel meg, nincs további szabályok értékeli ki. Az *x-ms-ügyfél-kérés-id élőfej* azonban nem engedélyezett a második szabály, így a kérelem nem sikerül, annak ellenére, hogy a harmadik szabály szemantikáját lehetővé tette volna, hogy sikertelen volt.

>[AZURE.NOTE] Bár ebben a példában a kevésbé korlátozó szabályok előtt erősebb korlátozásokat alkalmaz egy, az általános célszerű, először a legszigorúbb szabályok listáját.

## <a name="understanding-how-the-vary-header-is-set"></a>A változó fejléc beállításuk ismertetése

A *változó* fejléc a szabványos HTTP/1.1 élőfejbe kérelem élőfej mezők, amelyen leírja, hogy a böngészőben vagy a felhasználói ügynökök a feltételek a kiszolgáló által kijelölt feldolgozni a kérelmet, amely tartalmazza. A *változó* fejléc proxyk, böngészők és CDN határozza meg, hogyan a kérdésre adott választ gyorsítótárba helyezhető használó gyorsítótárazás főként szolgál. Részletekért olvassa el az a [változó fejléc](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)beállítások.

Amikor a böngésző vagy egy másik felhasználói ügynököt a gyorsítótárban CORS összehívásokban a válasz, a származási tartomány gyorsítótárazott engedélyezett származási. Egy másik tartomány tároló erőforrás kérésben problémák, amíg a gyorsítótár működik, ha az a felhasználói ügynököt a gyorsítótárban tárolt származási tartományt ad vissza. A második tartomány nem egyezik meg a gyorsítótárban tárolt tartományt, így a kérelem sikertelen, amikor az egyéb járnak. Bizonyos esetekben Azure tárhely a változó fejléc **Origin** kérje meg a felhasználói ügynököt a későbbi CORS kérelem küld a szolgáltatást, amikor a kérő tartomány eltér a gyorsítótárban tárolt origin állítja be.

Azure tároló állítja be a *változó* fejléc **Origin** tényleges GET/vezetője kérelmek a következő esetekben:

- Ha a kérelem origin pontosan egyezik a megengedett origin CORS szabály által meghatározott. Ahhoz, hogy pontosan egyező, a CORS szabály nem tartalmazhatnak meg helyettesítő karakter "*" karakter.

- Nincs egyező a kérelem származási szabály, de a tárhelyszolgáltatáshoz CORS engedélyezve van.

Abban az esetben, ha a GET/vezetője kérelmének megegyezik olyan CORS szabályt, amely lehetővé teszi, hogy az összes forrásokból a válasz azt jelzi, hogy minden forrásokból áll rendelkezésre, a felhasználói ügynökök gyorsítótár lehetővé teszi, hogy ezután valaki bármely origin tartományban lévő aktív a gyorsítótár állapotában.

Figyelje meg, hogy iránti kérelmek módszerekkel eltérő GET/vezetője, a tárhely szolgáltatások nem állít be a változó fejléc, mivel ezeket a módszereket adott válaszok nem a felhasználói ügynökök gyorsítótárba.

Az alábbi táblázat azt jelzi, hogyan Azure tároló válaszol GET/vezetője kérő a korábban már említettük esetekben alapján:

Kérés|Fiók beállítása és a szabály kiértékelésének eredménye|||Válasz|||
---|---|---|---|---|---|---|---|---
**Bemutató kérésre origin élőfej** | **Ez a szolgáltatás megadott CORS szabály(ok)** | **Egyező szabály létezik, amely lehetővé teszi, hogy az összes forrásokból (*)** | **Létezik megfelelő szabály származási pontos egyezés** | **Válasz tartalmaz változó élőfej származási beállítása** | **Válasz Access vezérlőelem-engedélyezett-származási tartalmaz: "*"** | **Válasz tartalmazza az Access-vezérlő-elérhetővé tett-fejlécek**
nem|nem|nem|nem|nem|nem|nem
nem|igen|nem|nem|igen|nem|nem
nem|igen|igen|nem|nem|igen|igen
igen|nem|nem|nem|nem|nem|nem
igen|igen|nem|igen|igen|nem|igen
igen|igen|nem|nem|igen|nem|nem
igen|igen|igen|nem|nem|igen|igen


## <a name="billing-for-cors-requests"></a>Számlázás CORS kérések

Ha engedélyezte a tárterület-szolgáltatások a fiók valamelyike CORS (meghívásával [Blob-tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452235.aspx), [Várólista szolgáltatás tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452232.aspx)vagy [Táblázat szolgáltatás tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452240.aspx)), a számlát kapni az ellenőrzés sikeres kérelmek. Költségek minimalizálásához fontolja meg a beállítása a **MaxAgeInSeconds** elem nagy értékre CORS szabályokat, hogy a felhasználói ügynököt a gyorsítótárban a kérést.

Sikertelen ellenőrzés kérések nem fognak számlát kapni.

## <a name="next-steps"></a>Következő lépések

[Blob-szolgáltatás tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452235.aspx)

[A várakozási szolgáltatás tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452232.aspx)

[Táblázat szolgáltatás tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452240.aspx)

[W3C határokon származási erőforrás specifikációja megosztása](http://www.w3.org/TR/cors/)
