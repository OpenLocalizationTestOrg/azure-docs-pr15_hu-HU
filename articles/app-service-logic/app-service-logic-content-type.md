<properties
   pageTitle="Kezelése írja be a tartalom összefüggés-alkalmazások |} Microsoft Azure"
   description="Hogyan logika alkalmazások foglalkozik, tartalomtípusok tervezés és runtime bemutatása"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-content-type-handling"></a>Logika alkalmazások tartalomtípust kezelése

Vannak olyan tartalom, keresztül összefüggés-alkalmazás – beleértve a JSON, XML, egyszerű fájlok és a bináris adatokat is flow számos különböző típusú.  Amíg az összes tartalomtípusok támogatottak, néhány natív módon értelmezni az logika alkalmazások motor, és mások megkövetelheti döntő vagy Dokumentumkonvertálás szükség szerint.  A következő cikk leírja, hogyan kezeli az motor a különböző tartalomtípusok, és hogyan azokat is megfelelően kezelhető szükség szerint.

## <a name="content-type-header"></a>Tartalomtípus-fejléc

Egyszerű kezdéséhez tekintse meg a két `Content-Types` , amely nem igényel, átváltási vagy egy összefüggés - alkalmazásokban használandó döntő `application/json` és `text/plain`.

### <a name="applicationjson"></a>Alkalmazás/json

A munkafolyamat-motor támaszkodik a `Content-Type` HTTP élőfej felhívja a megfelelő kezelési.  A webhelytartalom-típus kérésének `application/json` tárolása és JSON objektumként kezeli.  Ezeken kívül JSON tartalom elemezhető alapértelmezés szerint minden döntő őt.  Igen, amelynek a tartalomtípus-fejléc kérelem `application/json ` ki:

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

egy kifejezés, például az munkafolyamatokban elemezhető `@body('myAction')['foo'][0]` beolvasása a tartományvarázslóval (ebben az esetben `bar`).  Nincsenek további döntő van szükség.  JSON, de nem volt telepítve a megadott élőfej adatokat tartalmazó dolgozik, ha meg is manuálisan típusra, JSON használata a `@json()` függvényt (például: `@json(triggerBody())['foo']`).

### <a name="textplain"></a>Egyszerű szöveg

Hasonló `application/json`, a HTTP-üzenetek a beérkezett a `Content-Type` fejlécének `text/plain` formában annak nyers szeretne tárolni.  Ezenkívül ha felvette a további műveletek bármely döntő nélkül a kérelem fog nyissa a `Content-Type`: `text/plain` fejléc.  Ha például ha strukturálatlan fájlhoz dolgozik jelenhet meg a következő HTTP-tartalom:

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

as `text/plain`.  Ha a következő műveletsor küldte el azt egy másik kérés szövegtörzseként (`@body('flatfile')`), a kérelem szeretné, hogy egy `text/plain` tartalomtípus-fejléc.  Egyszerű szöveg, de nem volt telepítve a megadott élőfej adatokat tartalmazó dolgozik, ha meg is manuálisan leadott azt szöveg használata a `@string()` függvényt (például: `@string(triggerBody())`)

### <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a>Alkalmazás/xml és az alkalmazás/nyolcas-adatfolyam és konverter függvények

A logikai alkalmazás motor mindig megőrzi a `Content-Type` , amely a HTTP-kérés vagy a válasz érkezett.  Ez azt jelenti, ha a tartalom érkezik `Content-Type` a `application/octet-stream`, a kimenő felkérés eredményez, beleértve a további műveletek nem döntő `Content-Type`: `application/octet-stream`.  Ezzel a módszerrel a motor is guaruntee adatok nem elvesznek, Ugrás a munkafolyamat során.  Azonban a művelet állapota (a ráfordítások és a kimeneti értékeket) tárolja a JSON-objektum, folyik át a munkafolyamat során.  Ez azt jelenti, hogy annak érdekében, hogy az egyes adattípusok megőrzése, a motor konvertálja a tartalmat egy bináris base64 kódolt karakterláncot megfelelő metaadatok megőrizve mindkét `$content` és `$content-type` -amelyek automatikusan konvertálja.  Tartalom-típusok között kézzel is alakíthatja beépített konverter függvények használatával:

* `@json()`-adatok árnyékot`application/json`
* `@xml()`-adatok árnyékot`application/xml`
* `@binary()`-adatok árnyékot`application/octet-stream`
* `@string()`-adatok árnyékot`text/plain`
* `@base64()`-tartalom base64 karakterlánc alakít át.
* `@base64toString()`-base64-címként kódolt karakterláncot át.`text/plain`
* `@base64toBinary()`-base64-címként kódolt karakterláncot át.`application/octet-stream`
* `@encodeDataUri()`-karakterlánc dataUri bájt tömbként kódolja
* `@decodeDataUri()`-a dataUri visszafejti be egy bájt tömb

Ha például a HTTP-kérelem kapott `Content-Type`: `application/xml` a:

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

Képes leadott és az alábbihoz hasonló későbbi felhasználás `@xml(triggerBody())`, vagy például egy függvényen belül `@xpath(xml(triggerBody()), '/CustomerName')`.

### <a name="other-content-types"></a>Egyéb webhelytartalom-típusok

Más típusú tartalmat támogatottak, és egy logikai alkalmazással fog működni, de manuálisan lekérdezése az üzenet törzsébe által dekódolását megkövetelheti a `$content`.  Ha például e voltak kiváltó ki egy `application/x-www-url-formencoded` -összehívásba, amelyet a következő hasonlított:

```
CustomerName=Frank&Address=123+Avenue
```

mivel ez nem a egyszerű szöveg vagy JSON kíván tárolni a műveletben szerint:

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

Ha `$content` a tartalom megőrzése érdekében az összes adat base64 karakterláncként encoded.  Mivel jelenleg nem natív függvény az űrlap-adatokat, sikerült továbbra is használni ezeket az adatokat a munkafolyamaton belül az adatokat, például egy funkcióval manuálisan elérésével `@string(body('formdataAction'))`.  E volna, hogy a kimenő kérelem is telepítve van a `application/x-www-url-formencoded` tartalomtípus-fejléc, sikerült csak felvenni, a művelet szervezet bármely döntő például nélkül `@body('formdataAction')`.  Azonban ez csak működnek, ha törzse a csak paramétert a `body` beviteli.  Ha megpróbálja tegye `@body('formdataAction')` belül a egy `application/json` kérése egy futási idejű hiba jelenik meg, elküldi az kódolt törzsébe.
