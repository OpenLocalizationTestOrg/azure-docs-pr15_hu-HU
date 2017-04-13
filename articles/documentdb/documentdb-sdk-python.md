<properties 
    pageTitle="DocumentDB Python API-val és a SDK |} Microsoft Azure" 
    description="Megismerheti az összes a Python API-val és a fontos dátumok, elavulása dátumok és a DocumentDB Python SDK minden verzió közötti végrehajtott módosítások SDK csomagjában talál." 
    services="documentdb" 
    documentationCenter="python" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB API-k és SDK

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [NODE.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [TÖBBI](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-python-api-and-sdk"></a>DocumentDB Python API-val és a SDK

<table>
<tr><td>**SDK letöltése**</td><td>[PyPI](https://pypi.python.org/pypi/pydocumentdb)</td></tr>
<tr><td>**API dokumentációja**</td><td>[Python API hivatkozás dokumentációja](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>
<tr><td>**SDK csomagjában talál utasításokat.**</td><td>[Python SDK csomagjában talál utasításokat.](http://azure.github.io/azure-documentdb-python/)</td></tr>
<tr><td>**Közreműködés SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-python)</td></tr>
<tr><td>**Első lépések**</td><td>[Első lépések a Python SDK](documentdb-python-application.md)</td></tr>
<tr><td>**Aktuális támogatott platform**</td><td>[Python 2.7](https://www.python.org/downloads/) és [Python 3.5-ös](https://www.python.org/downloads/)</td></tr>
</table></br>

## <a name="release-notes"></a>Kibocsátási megjegyzések

### <a name="a-name200200httpspypipythonorgpypipydocumentdb200"></a><a name="2.0.0"/>[2.0.0](https://pypi.python.org/pypi/pydocumentdb/2.0.0)
- Verziónak támogatnia kell az Python 3.5-ös.
- Verziónak támogatnia kell az kapcsolatkészletezés egy kéréseket modulról.
- Munkamenet konzisztencia hozzáadott támogatása.
- A legfelső/ORDERBY lekérdezések particionált gyűjtemények verziónak támogatnia.


### <a name="a-name190190httpspypipythonorgpypipydocumentdb190"></a><a name="1.9.0"/>[1.9.0](https://pypi.python.org/pypi/pydocumentdb/1.9.0)
- Csökkentett kérések hozzáadott újrapróbálkozási házirend támogatása. (Csökkentett kérések kap egy kérés ráta túl nagy a kivétel, 429 kódszámú hiba jelenik meg.) Alapértelmezés szerint DocumentDB próbálkozások kilenc időpontok minden kérelme 429 kódszámú hiba jelenik meg akkor fordul elő, amikor a retryAfter időpontot, a válasz fejlécében vállalta. A rögzített újrapróbálkozási időintervallum most beállítható a RetryOptions tulajdonság részeként a ConnectionPolicy objektum, ha a próbálkozások közötti kiszolgálótól kapott retryAfter idő mellőzni szeretne. Legfeljebb 30 másodperc minden egyes vár kéri, hogy most DocumentDB folyamatban van folyamatban (függetlenül a kísérletek száma), és 429 kódszámú hiba jelenik meg a választ adja eredményül. Ez esetben is felüldefiniálni ConnectionPolicy objektum RetryOptions tulajdonságban.

- DocumentDB most eredményez, x-ms-szabályozási-újrapróbálkozási-darab és a x-ms-throttle-retry-wait-time-ms, a válasz fejlécek, a szabályozási jelölésére minden összehívásban próbálkozzon újra a darab és a kérelem megvárta a próbálkozások közötti cummulative idő.

- Eltávolítja a RetryPolicy osztály és a megfelelő tulajdonságot (retry_policy), a document_client osztály elérhetővé tett, és helyette jelent meg néhány az alapértelmezett beállítások felülbírálása használható ConnectionPolicy osztály RetryOptions tulajdonsága közzéteszi RetryOptions osztály.

### <a name="a-name180180httpspypipythonorgpypipydocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://pypi.python.org/pypi/pydocumentdb/1.8.0)
  - Adja hozzá az adatbázis több területre fiókok támogatása.

### <a name="a-name170170httpspypipythonorgpypipydocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://pypi.python.org/pypi/pydocumentdb/1.7.0)
- Adja hozzá a dokumentumok idő való Live(TTL) szolgáltatás támogatása.

### <a name="a-name161161httpspypipythonorgpypipydocumentdb161"></a><a name="1.6.1"/>[1.6.1](https://pypi.python.org/pypi/pydocumentdb/1.6.1)
- A hibajavítás kapcsolódó kiszolgáló egymás szétválasztás speciális karakterek engedélyezni partitionkey elérési útját.

### <a name="a-name160160httpspypipythonorgpypipydocumentdb160"></a><a name="1.6.0"/>[1.6.0](https://pypi.python.org/pypi/pydocumentdb/1.6.0)
- Megvalósított [particionált webhelycsoportok](documentdb-partition-data.md) és a [felhasználó által definiált teljesítményszint](documentdb-performance-levels.md). 

### <a name="a-name150150httpspypipythonorgpypipydocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://pypi.python.org/pypi/pydocumentdb/1.5.0)
- Kivonat és tartomány hozzáadása partition névfeloldók segítésére sharding alkalmazásokkal több partíciót keresztül.

### <a name="a-name142142httpspypipythonorgpypipydocumentdb142"></a><a name="1.4.2"/>[1.4.2-es](https://pypi.python.org/pypi/pydocumentdb/1.4.2)
- Upsert végrehajtása. Új UpsertXXX módszerek Upsert szolgáltatást támogató hozzáadott.
- Azonosító alapján útválasztás megvalósítása. Nyilvános API-módosítások nem, minden módosítás belső.

### <a name="a-name120120httpspypipythonorgpypipydocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://pypi.python.org/pypi/pydocumentdb/1.2.0)
- Támogatja a földrajzi indexe.
- Ellenőrzi az összes erőforrás-azonosító tulajdonság. Az erőforrások azonosítók nem tartalmazhat ?, /, #, \, karaktereket vagy szóközzel vége.
- Új élőfej "index átalakítása előrehaladását" ResourceResponse hozzáadja.

### <a name="a-name110110httpspypipythonorgpypipydocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://pypi.python.org/pypi/pydocumentdb/1.1.0)
- Az indexelési házirendjét V2 hozza.

### <a name="a-name101101httpspypipythonorgpypipydocumentdb101"></a><a name="1.0.1"/>[1.0.1-es verziójú](https://pypi.python.org/pypi/pydocumentdb/1.0.1)
- Kapcsolat a proxy támogat.

### <a name="a-name100100httpspypipythonorgpypipydocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://pypi.python.org/pypi/pydocumentdb/1.0.0)
- KIADÁS SDK CSOMAGJÁBAN TALÁL.

## <a name="release--retirement-dates"></a>Megjelenés és elavulása dátumok
A Microsoft nyújt értesítés legalább **12 hónapos** használatból történő egy SDK annak érdekében, hogy egy újabb/támogatott verzióját az áttűnés sima előtt.

Az aktuális SDK új szolgáltatásokat és funkciókat és optimalizálásokat csak hozzáadni, ilyenként javasoljuk, hogy mindig frissítse SDK verziót legkorábban lehető. 

A szolgáltatás kérésének DocumentDB egy visszavont SDK használatával történő elutasításra kerül.

> [AZURE.WARNING]
Az Azure DocumentDB SDK Python megelőző verziója **1.0.0** valamennyi nyelvi verziójában a **2016 február 29**visszavonásának. 

<br/>

| Verzió | Dátum visszaadása | Visszavonás dátuma 
| ---     | ---          | ---
| [2.0.0](#2.0.0) | 2016 szeptember 29 |---
| [1.9.0](#1.9.0) | 2016 július 07 |---
| [1.8.0](#1.8.0) | Június 14, 2016-ban |---
| [1.7.0](#1.7.0) | 2016 áprilisi 26. |---
| [1.6.1](#1.6.1) | 2016 áprilisi 08 |---
| [1.6.0](#1.6.0) | Március 29, 2016-ban |---
| [1.5.0](#1.5.0) | Január 03, 2016-ban |---
| [1.4.2-es](#1.4.2) | 2015 október 06 |---
| [1.4.1](#1.4.1) | 2015 október 06 |---
| [1.2.0](#1.2.0) | 2015 augusztus 06 |---
| [1.1.0](#1.1.0) | 2015 július 09 |---
| [1.0.1-es verziójú](#1.0.1) | 2015 május 25 |---
| [1.0.0](#1.0.0) | 2015 április 07 |---
| 0.9.4-prelease | 2015 január 14 | 2016 február 29
| 0.9.3-prelease | December 09, 2014-es | 2016 február 29
| 0.9.2-prelease | November 25, 2014-es | 2016 február 29
| 0.9.1-prelease | 2014 szeptember 23 | 2016 február 29
| 0.9.0-prelease | Augusztus 21, 2014-es | 2016 február 29

## <a name="faq"></a>GYAKORI KÉRDÉSEK
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Lásd még:

DocumentDB kapcsolatos további tudnivalókért lásd: [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) szolgáltatás lapon. 
