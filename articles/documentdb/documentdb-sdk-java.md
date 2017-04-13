
<properties
    pageTitle="DocumentDB Java API-val és a SDK |} Microsoft Azure"
    description="Megismerheti az összes a Java API-val és a fontos dátumok, elavulása dátumok és a DocumentDB Java SDK minden verzió közötti végrehajtott módosítások SDK csomagjában talál."
    services="documentdb"
    documentationCenter="java"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB API-k és SDK

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [NODE.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [TÖBBI](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-java-api-and-sdk"></a>DocumentDB Java API-val és a SDK

<table>
<tr><td>**SDK letöltése**</td><td>[Maven tesztelése](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>
<tr><td>**API dokumentációja**</td><td>[Java API hivatkozási dokumentáció](http://azure.github.io/azure-documentdb-java/)</td></tr>
<tr><td>**Közreműködés SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-java/)</td></tr>
<tr><td>**Első lépések**</td><td>[A Java SDK – első lépések](documentdb-java-application.md)</td></tr>
<tr><td>**Aktuális támogatott futtatókörnyezet**</td><td>[JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a>Kibocsátási megjegyzések

### <a name="a-name191191httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb191"></a><a name="1.9.1"/>[1.9.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.1)

  - Verziónak támogatnia kell az BoundedStaleness konzisztencia szintjét.
  - Közvetlen kapcsolatok CRUD műveletekhez particionált gyűjtemények hozzáadott támogatása.
  - Lekérdezés SQL adatbázis hibáját rögzíteni.
  - A hiba rögzíteni a munkamenet gyorsítótárban, ahol munkamenet jogkivonat is be lehet állítani helytelenül.

### <a name="a-name190190httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb190"></a><a name="1.9.0"/>[1.9.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.0)

  - Verziónak támogatnia kell az tartományok partíciót a párhuzamos lekérdezések.
  - Verziónak támogatnia kell az particionált gyűjtemények felső/ORDER BY lekérdezések.
  - Erős konzisztencia hozzáadott támogatása.
  - Verziónak támogatnia kell az név sokaság kérelmek, közvetlen kapcsolat használata esetén.
  - Rögzített, hogy az összes kérelem újrapróbálkozások keresztül egységes kapcsolatának ActivityId.
  - Rögzített újbóli létrehozása egy ilyen nevű gyűjtemény a munkamenet-gyorsítótár kapcsolatos hiba.
  - A felvett sokszög és LineString adattípusok webhelycsoport indexelés geo-kerítés térbeli lekérdezések házirend megadása során.
  - Rögzített problémáinak Java 1.8 Java-dokumentumot.

### <a name="a-name181181httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb181"></a><a name="1.8.1"/>[1.8.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.1)
  - A hiba rögzített partíciót gyűjtemények gyorsítótár, és nem fő kérések partition további fájllehívási PartitionKeyDefinitionMap.
  - A hiba nem újra, ha nem a megfelelő partíciót elsődlegeskulcs-értékének rögzíteni.

### <a name="a-name180180httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb180"></a><a name="1.8.0"/>[1.8.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.0)
  - Adja hozzá az adatbázis több területre fiókok támogatása.
  - Az automatikus újrapróbálkozási csökkentett kérések beállításokkal szabhatja testre a max újrapróbálkozási kísérletek és a max újrapróbálkozási várakozási idő a hozzáadott támogatása.  Lásd: RetryOptions és ConnectionPolicy.getRetryOptions().
  - Elavult IPartitionResolver alapuló egyéni particionáló kódot. Használjon particionált gyűjtemények magasabb tárolására és kapacitása.

### <a name="a-name171171httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb171"></a><a name="1.7.1"/>[1.7.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.1)
- A felvett újrapróbálkozási házirend támogatása szabályozásának.  

### <a name="a-name170170httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb170"></a><a name="1.7.0"/>[1.7.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.0)
- Dokumentumok live (TTL) támogatás hozzáadott ideje.

### <a name="a-name160160httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb160"></a><a name="1.6.0"/>[1.6.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.6.0)
- Megvalósított [particionált webhelycsoportok](documentdb-partition-data.md) és a [felhasználó által definiált teljesítményszint](documentdb-performance-levels.md).

### <a name="a-name151151httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb151"></a><a name="1.5.1"/>[1.5.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.1)
- A hiba HashPartitionResolver – Négybájt biztosítani szeretné konzisztens és más SDK az kivonat értékeket szeretne rögzíteni.

### <a name="a-name150150httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb150"></a><a name="1.5.0"/>[1.5.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.0)
- Kivonat és tartomány hozzáadása több partíciót keresztül sharding alkalmazások megkönnyítése névfeloldók partition.

### <a name="a-name140140httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb140"></a><a name="1.4.0"/>[1.4.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.4.0)
- Upsert végrehajtása. Új upsertXXX módszerek Upsert szolgáltatást támogató hozzáadott.
- Azonosító alapján útválasztás megvalósítása. Nyilvános API-módosítások nem, minden módosítás belső.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
- Ahhoz, hogy a verziószám és más SDK igazítás a kihagyott megjelenés

### <a name="a-name120120httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb120"></a><a name="1.2.0"/>[1.2.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.2.0)
- Támogatja a földrajzi Index
- Ellenőrzi az összes erőforrás-azonosító tulajdonság. Az erőforrások azonosítók nem tartalmazhat ?, /, #, \, karaktereket vagy szóközzel vége.
- Új élőfej "index átalakítása előrehaladását" ad ResourceResponse.

### <a name="a-name110110httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb110"></a><a name="1.1.0"/>[1.1.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.1.0)
- Az indexelési házirendjét V2 hozza

### <a name="a-name100100httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb100"></a><a name="1.0.0"/>[1.0.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.0.0)
- KIADÁS SDK

## <a name="release--retirement-dates"></a>Megjelenés és elavulása dátumok
A Microsoft nyújt értesítés legalább **12 hónapos** használatból történő egy SDK annak érdekében, hogy az áttűnés egy újabb/támogatott verzióját sima előtt.

Új szolgáltatások és funkciók és optimalizálásokat csak hozzáadni a jelenlegi SDK, ilyenként javasoljuk, hogy mindig frissítse SDK verziót legkorábban lehetőség.

A szolgáltatás kérésének DocumentDB egy visszavont SDK használatával történő elutasításra kerül.

> [AZURE.WARNING]
Az Azure DocumentDB SDK Java megelőző verziója **1.0.0** valamennyi nyelvi verziójában a **2016 február 29**visszavonásának.

<br/>

| Verzió | Dátum visszaadása | Visszavonás dátuma
| ---     | ---          | ---
| [1.9.1](#1.9.1) | 2016 október 28 |---
| [1.9.0](#1.9.0) | 2016 október 03 |---
| [1.8.1](#1.8.1) | Június 30, 2016-ban |---
| [1.8.0](#1.8.0) | Június 14, 2016-ban |---
| [1.7.1](#1.7.1) | 2016 áprilisi 30 |---
| [1.7.0](#1.7.0) | 2016 áprilisi 27 |---
| [1.6.0](#1.6.0) | Március 29, 2016-ban |---
| [1.5.1](#1.5.1) | 2015 december 31-én |---
| [1.5.0](#1.5.0) | December 04 2015 |---
| [1.4.0](#1.4.0) | 2015 október 05 |---
| [1.3.0](#1.3.0) | 2015 október 05 |---
| [1.2.0](#1.2.0) | 2015 augusztus 05 |---
| [1.1.0](#1.1.0) | 2015 július 09 |---
| [1.0.1-es verziójú](#1.0.1) | 2015 május 12 |---
| [1.0.0](#1.0.0) | 2015 április 07 |---
| 0.9.5-prelease | 2015 március 09 | 2016 február 29
| 0.9.4-prelease | 2015 február 17 | 2016 február 29
| 0.9.3-prelease | 2015 január 13 | 2016 február 29
| 0.9.2-prelease | December 19, 2014-es | 2016 február 29
| 0.9.1-prelease | December 19, 2014-es | 2016 február 29
| 0.9.0-prelease | December 10 2014 | 2016 február 29

## <a name="faq"></a>GYAKORI KÉRDÉSEK
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Lásd még:

DocumentDB kapcsolatos további tudnivalókért lásd: [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) szolgáltatás lapon.
