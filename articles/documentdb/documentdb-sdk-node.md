<properties
    pageTitle="DocumentDB Node.js API-val és a SDK |} Microsoft Azure"
    description="Megismerheti az összes a Node.js API-val és a fontos dátumok, elavulása dátumok és a DocumentDB Node.js SDK minden verzió közötti végrehajtott módosítások SDK csomagjában talál."
    services="documentdb"
    documentationCenter="nodejs"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB API-k és SDK

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [NODE.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [TÖBBI](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

##<a name="documentdb-nodejs-api-and-sdk"></a>DocumentDB Node.js API-val és a SDK

<table>
<tr><td>**SDK letöltése**</td><td>[NPM](https://www.npmjs.com/package/documentdb)</td></tr>
<tr><td>**API dokumentációja**</td><td>[NODE.js API hivatkozási dokumentáció](http://azure.github.io/azure-documentdb-node/DocumentClient.html)</td></tr>
<tr><td>**SDK csomagjában talál utasításokat.**</td><td>[Telepítési útmutatója](http://azure.github.io/azure-documentdb-node/)</td></tr>
<tr><td>**Közreműködés SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-node/tree/master/source)</td></tr>
<tr><td>**A minták**</td><td>[NODE.js mintakódok](documentdb-nodejs-samples.md)</td></tr>
<tr><td>**Első lépések oktatóprogram**</td><td>[A Node.js SDK – első lépések](documentdb-nodejs-get-started.md)</td></tr>
<tr><td>**Web app oktatóprogram**</td><td>[Node.js webalkalmazás használatával DocumentDB összeállítása](documentdb-nodejs-application.md)</td></tr>
<tr><td>**Aktuális támogatott platform**</td><td>[NODE.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/)<br/>[NODE.js v0.12](https://nodejs.org/en/blog/release/v0.12.0/)<br/>[NODE.js v4.2.0](https://nodejs.org/en/blog/release/v4.2.0/)</td></tr>
</table></br>

##<a name="release-notes"></a>Kibocsátási megjegyzések

###<a name="1.10.0"/>1.10.0</a>

- Verziónak támogatnia kell az tartományok partíciót a párhuzamos lekérdezések.
- Verziónak támogatnia kell az particionált gyűjtemények felső/ORDER BY lekérdezések.

###<a name="1.9.0"/>1.9.0</a>

- Csökkentett kérések hozzáadott újrapróbálkozási házirend támogatása. (Csökkentett kérések kap egy kérés ráta túl nagy a kivétel, 429 kódszámú hiba jelenik meg.) Alapértelmezés szerint DocumentDB próbálkozások kilenc időpontok minden kérelme 429 kódszámú hiba jelenik meg akkor fordul elő, amikor a retryAfter időpontot, a válasz fejlécében vállalta. A rögzített újrapróbálkozási időintervallum most beállítható a RetryOptions tulajdonság részeként a ConnectionPolicy objektum, ha a próbálkozások közötti kiszolgálótól kapott retryAfter idő mellőzni szeretne. Legfeljebb 30 másodperc minden egyes vár kéri, hogy most DocumentDB folyamatban van folyamatban (függetlenül a kísérletek száma), és 429 kódszámú hiba jelenik meg a választ adja eredményül. Ez esetben is felüldefiniálni ConnectionPolicy objektum RetryOptions tulajdonságban.

- DocumentDB most eredményez, x-ms-szabályozási-újrapróbálkozási-darab és a x-ms-throttle-retry-wait-time-ms, a válasz fejlécek, a szabályozási jelölésére minden összehívásban próbálkozzon újra a darab és a kérelem megvárta a próbálkozások közötti cummulative idő.

- A RetryOptions osztály lett hozzáadva, az egyes az alapértelmezett beállítások felülbírálása használható ConnectionPolicy osztály RetryOptions tulajdonsága ki.

###<a name="1.8.0"/>1.8.0</a>

 - Adja hozzá az adatbázis több területre fiókok támogatása.

###<a name="1.7.0"/>1.7.0</a>

- Adja hozzá a dokumentumok idő való Live(TTL) szolgáltatás támogatása.

###<a name="1.6.0"/>1.6.0</a>

- Megvalósított [particionált webhelycsoportok](documentdb-partition-data.md) és a [felhasználó által definiált teljesítményszint](documentdb-performance-levels.md).

###<a name="1.5.6"/>1.5.6</a>

- Javított RangePartitionResolver.resolveForRead hibát, ahol azt volt nem visszaadó hivatkozások miatt egy hibás összefűzés az eredmények.

###<a name="1.5.5"/>1.5.5</a>

- Rögzített hashParitionResolver resolveForRead(): Ha nincs megadva partíciót kulcs lett értesítő a kivétel helyett adatszolgáltató összes regisztrált hivatkozásainak listája.

###<a name="1.5.4"/>1.5.4</a>

- Javítások hiba [#100](https://github.com/Azure/azure-documentdb-node/issues/100) - dedikált HTTPS-Agent: elkerülése a globális ügynök DocumentDB célokra módosítása. Egy dedikált ügynök használja az összes a tár kérelmeket.

###<a name="1.5.3"/>1.5.3</a>

- Javítások [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - megfelelően fogópont kötőjeleket media azonosítók a hiba.

###<a name="1.5.2"/>1.5.2</a>

- Javítások hiba [#95](https://github.com/Azure/azure-documentdb-node/issues/95) - figyelő előfordulhat, hogy figyelmeztetés EventEmitter.

###<a name="1.5.1"/>1.5.1</a>

- Javítások kivonat hiba a [#92](https://github.com/Azure/azure-documentdb-node/issues/90) - mappa átnevezése kivonat kis-és nagybetűk rendszerekhez.

### <a name="1.5.0"/>1.5.0</a>

- Megvalósítása sharding támogatási kivonat és a tartomány partíciót névfeloldók hozzáadásával.

### <a name="1.4.0"/>1.4.0</a>

- Upsert végrehajtása. Új upsertXXX módszerek documentClient meg.

### <a name="1.3.0"/>1.3.0</a>

- Ahhoz, hogy verziószámai az Igazítás az egyéb SDK hagyva.

### <a name="1.2.2"/>1.2.2</a>

- A felosztott kérdések új tárházba csomagolópapír ígéretet tesz.
- Frissítési npm rendszerleíró csomagfájlt.

### <a name="1.2.1"/>1.2.1</a>

- Eszközök azonosító alapján útválasztás.
- Megoldja a problémát [#49](https://github.com/Azure/azure-documentdb-node/issues/49) - módszer current() aktuális tulajdonság ütközik.

### <a name="1.2.0"/>1.2.0</a>

- Földrajzi index hozzáadott támogatása.
- Ellenőrzi az összes erőforrás-azonosító tulajdonság. Az erőforrások azonosítók nem tartalmazhat?, /, #, és #47; és #47; karaktereket vagy záró szóközzel.
- Új élőfej "index átalakítása előrehaladását" ResourceResponse hozzáadja.

### <a name="1.1.0"/>1.1.0</a>

- Az indexelési házirendjét V2 hozza.

### <a name="1.0.3"/>1.0.3</a>

- A probléma [40 #] (https://github.com/Azure/azure-documentdb-node/issues/40) – végrehajtott eslint és grunt konfigurációk core és cikk megvásárlását fel SDK csomagjában talál.

### <a name="1.0.2"/>1.0.2-es verziójú</a>

- A probléma [#45](https://github.com/Azure/azure-documentdb-node/issues/45) - tett csomagolópapír nem része a hibával fejléc.

### <a name="1.0.1"/>1.0.1-es verziójú</a>

- Megvalósított azt jelenti, hogy a lekérdezés readConflicts, readConflictAsync és queryConflicts hozzáadásával ütközéseket.
- Frissített API dokumentációt.
- A hiba [#41](https://github.com/Azure/azure-documentdb-node/issues/41) - client.createDocumentAsync hiba.

### <a name="1.0.0"/>1.0.0</a>

- KIADÁS SDK CSOMAGJÁBAN TALÁL.

## <a name="release--retirement-dates"></a>Megjelenés és elavulása dátumok
A Microsoft nyújt értesítés legalább **12 hónapos** használatból történő egy SDK annak érdekében, hogy egy újabb/támogatott verzióját az áttűnés sima előtt.

Az aktuális SDK új szolgáltatásokat és funkciókat és optimalizálásokat csak hozzáadni, ilyenként javasoljuk, hogy mindig frissítse SDK verziót legkorábban lehető.

A szolgáltatás kérésének DocumentDB egy visszavont SDK használatával történő elutasításra kerül.

> [AZURE.WARNING]
Az Azure DocumentDB SDK Node.js az összes verziójának verzió **1.0.0** előtt fog **2016 február 29**a inaktívvá tenni.

<br/>

| Verzió | Dátum visszaadása | Visszavonás dátuma
| ---     | ---          | ---
| [1.10.0](#1.10.0) | 2016 október 03 |---
| [1.9.0](#1.9.0) | 2016 július 07 |---
| [1.8.0](#1.8.0) | Június 14, 2016-ban |---
| [1.7.0](#1.7.0) | 2016 áprilisi 26. |---
| [1.6.0](#1.6.0) | Március 29, 2016-ban |---
| [1.5.6](#1.5.6) | Március 08, 2016-ban |---
| [1.5.5](#1.5.5) | 2016 február 02 |---
| [1.5.4](#1.5.4) | 2016 február 01 |---
| [1.5.2](#1.5.2) | 2016 január 26. |---
| [1.5.2](#1.5.2) | Január 22, 2016-ban |---
| [1.5.1](#1.5.1) | 2016 január 4 |---
| [1.5.0](#1.5.0) | 2015 december 31-én |---
| [1.4.0](#1.4.0) | 2015 október 06 |---
| [1.3.0](#1.3.0) | 2015 október 06 |---
| [1.2.2](#1.2.2) | 2015 szeptember 10 |---
| [1.2.1](#1.2.1) | 2015 augusztus 15 |---
| [1.2.0](#1.2.0) | 2015 augusztus 05 |---
| [1.1.0](#1.1.0) | 2015 július 09 |---
| [1.0.3](#1.0.3) | 2015 június 04 |---
| [1.0.2-es verziójú](#1.0.2) | 2015 május 23 |---
| [1.0.1-es verziójú](#1.0.1) | 2015 május 15 |---
| [1.0.0](#1.0.0) | 2015 április 08 |---
| 0.9.4-prerelease | 2015 április 06 | 2016 február 29
| 0.9.3-prerelease | 2015 január 14 | 2016 február 29
| 0.9.2-prerelease | December 18, 2014-es | 2016 február 29
| 0.9.1-prerelease | Augusztus 22, 2014-es | 2016 február 29
| 0.9.0-prerelease | Augusztus 21, 2014-es | 2016 február 29


## <a name="faq"></a>GYAKORI KÉRDÉSEK
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Lásd még:

DocumentDB kapcsolatos további tudnivalókért lásd: [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) szolgáltatás lapon.
