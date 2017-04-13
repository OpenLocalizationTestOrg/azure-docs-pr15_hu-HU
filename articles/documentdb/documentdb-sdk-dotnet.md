<properties 
    pageTitle="DocumentDB .NET API-val és a SDK |} Microsoft Azure" 
    description="Megismerheti az összes a .NET API-val és a fontos dátumok, elavulása dátumok és a DocumentDB .NET SDK minden verzió közötti végrehajtott módosítások SDK csomagjában talál." 
    services="documentdb" 
    documentationCenter=".net" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB API-k és SDK 

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [NODE.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [TÖBBI](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-net-api-and-sdk"></a>DocumentDB .NET API-val és a SDK

<table>
<tr><td>**SDK letöltése**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)</td></tr>
<tr><td>**API dokumentációja**</td><td>[.NET API hivatkozási dokumentáció](https://msdn.microsoft.com/library/azure/dn948556.aspx)</td></tr>
<tr><td>**A minták**</td><td>[.NET mintakódok](documentdb-dotnet-samples.md)</td></tr>
<tr><td>**Első lépések**</td><td>[A DocumentDB .NET SDK – első lépések](documentdb-get-started.md)</td></tr>
<tr><td>**Web app oktatóprogram**</td><td>[A webes alkalmazások fejlesztése DocumentDB együtt](documentdb-dotnet-application.md)</td></tr>
<tr><td>**Aktuális támogatott keretrendszer**</td><td>[Microsoft .NET-keretrendszer 4.5-ös verzió](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a>Kibocsátási megjegyzések

> [AZURE.IMPORTANT] 1.9.2 verzió megjelenési kezdve jelenhet System.NotSupportedException particionált gyűjtemények lekérdezésére. Ez a hiba elkerülése érdekében győződjön meg arról, hogy a host folyamat 64 bites. Végrehajtható projektekhez ez történik jelölésének törlése a "Prefer 32 bites" beállítást, a project tulajdonságok ablak a Szerkesztés lapon.

### <a name="a-name11001100httpswwwnugetorgpackagesmicrosoftazuredocumentdb1100"></a><a name="1.10.0"/>[1.10.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.10.0)

  - Particionált webhelycsoportok közvetlen kapcsolatok hozzáadott támogatása.
  - Nagyobb teljesítmény az határolt Staleness konzisztencia szint.
  - A felvett sokszög és LineString adattípusok webhelycsoport indexelés geo-kerítés térbeli lekérdezések házirend megadása során.
  - A felvett LINQ StringEnumConverter, IsoDateTimeConverter és támogatása UnixDateTimeConverter predikátumok fordítása közben.
  - Különböző SDK hibajavítás.

### <a name="a-name195195httpswwwnugetorgpackagesmicrosoftazuredocumentdb195"></a><a name="1.9.5"/>[1.9.5](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.5)

  - Rögzített a következő NotFoundException okozó probléma: az olvasási munkamenet nem érhető el a beviteli munkamenet jogkivonat. A során kivételhiba lépett fel bizonyos esetekben lekérdezése egy geo elosztott fiókot az olvasás területhez tartozik.
  - Elérhető a responseStream kimeneti tulajdonság a ResourceResponse osztály, amely lehetővé teszi a közvetlen hozzáférés az alapul szolgáló adatfolyam a választ.

### <a name="a-name194194httpswwwnugetorgpackagesmicrosoftazuredocumentdb194"></a><a name="1.9.4"/>[1.9.4](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.4)

  - A megfelelő nyilvános illesztő végrehajtásához, hogy azok a próba-alapú telepítés (TDD) kell mocked ResourceResponse, FeedResponse, StoredProcedureResponse és MediaResponse osztályok módosított.
  - Rögzített szerializálási adatokat egy egyéni JsonSerializerSettings objektum használatakor a hibás partíciót kulcs élőfej okozó hibát.

### <a name="a-name193193httpswwwnugetorgpackagesmicrosoftazuredocumentdb193"></a><a name="1.9.3"/>[1.9.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.3)

  - Rögzített meghiúsító hosszú futó lekérdezések hibát okozó probléma: engedélyezési token nem érvényes az aktuális időpontban.
  - Rögzített a hibát, amely az eredeti SqlParameterCollection tartományok partíciót felső /-rendezés lekérdezések eltávolítja.

### <a name="a-name192192httpswwwnugetorgpackagesmicrosoftazuredocumentdb192"></a><a name="1.9.2"/>[1.9.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.2)

  - A párhuzamos lekérdezések particionált gyűjtemények hozzáadott támogatása.
  - Verziónak támogatnia kell az partíciót ORDER BY és a felső lekérdezések particionált gyűjtemények tartományok.
  - Rögzített a hiányzó hivatkozások DocumentDB.Spatial.Sql.dll és Microsoft.Azure.Documents.ServiceInterop.dll DocumentDB projekt hivatkozik DocumentDB Nuget csomag hivatkozásban a szükségesek.
  - Különböző típusú paraméterek használata, amikor a felhasználó által definiált függvények használata az LINQ rögzíteni. 
  - Rögzített globálisan replikált fiókok hiba, ahol Upsert hívások helyett írási helyek helyek olvasható éppen jutott.
  - Néhány további módszerek a IDocumentClient felületére, amelyeket a hiányzó: 
      - UpsertAttachmentAsync módszer, hogy mi mediaStream és paraméterként beállításai
      - CreateAttachmentAsync módszer, hogy mi a beállítások, mint egy paraméter
      - CreateOfferQuery módszer, hogy mi paraméterként querySpec.
  - Lezáratlan nyilvános osztályok IDocumentClient felületén elérhető.

### <a name="a-name180180httpswwwnugetorgpackagesmicrosoftazuredocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.8.0)
  - Adja hozzá az adatbázis több területre fiókok támogatása.
  - Verziónak támogatnia kell az csökkentett kérések kísérletek száma.  Felhasználói is testre szabhatja a újrapróbálkozások száma és a max várakozási idő a ConnectionPolicy.RetryOptions tulajdonság beállításával.
  - Adja hozzá az új IDocumentClient felhasználói felülete, amely definiálja az aláírások DocumenClient tulajdonságok és a módszereket.  Ez a változás részeként szintén módosul IQueryable és IOrderedQueryable létrehozása módszerek az DocumentClient osztály magát a bővítmény módszereket.
  - Adja hozzá a beállítás az egy adott DocumentDB végpont Uri ServicePoint.ConnectionLimit beállításához.  ConnectionPolicy.MaxConnectionLimit segítségével módosíthatja az alapértelmezett érték, amely 50 van beállítva.
  - Elavult IPartitionResolver és annak végrehajtását.  Elavult IPartitionResolver támogatása. Ajánlott használható particionálva gyűjtemények magasabb tárolására és kapacitása.


### <a name="a-name171171httpswwwnugetorgpackagesmicrosoftazuredocumentdb171"></a><a name="1.7.1"/>[1.7.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.1)
  - Adja hozzá a túlterhelés URI-ra, hogy mi paraméterként RequestOptions ExecuteStoredProcedureAsync módszer alapján.
  
### <a name="a-name170170httpswwwnugetorgpackagesmicrosoftazuredocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.0)
  - Dokumentumok live (TTL) támogatás hozzáadott ideje.

### <a name="a-name163163httpswwwnugetorgpackagesmicrosoftazuredocumentdb163"></a><a name="1.6.3"/>[1.6.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.3)
  - A hiba Nuget csomagban .NET SDK rögzíteni csomagolása, akkor egy Felhőszolgáltatásba Azure megoldást részeként.
  
### <a name="a-name162162httpswwwnugetorgpackagesmicrosoftazuredocumentdb162"></a><a name="1.6.2"/>[1.6.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.2)
  - Megvalósított [particionált webhelycsoportok](documentdb-partition-data.md) és a [felhasználó által definiált teljesítményszint](documentdb-performance-levels.md). 

### <a name="a-name153153httpswwwnugetorgpackagesmicrosoftazuredocumentdb153"></a><a name="1.5.3"/>[1.5.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.3)
  - **[Rögzített]** Lekérdezés DocumentDB végpont eredményez: "System.Net.Http.HttpRequestException: adatfolyam tartalom másolása során hiba.

### <a name="a-name152152httpswwwnugetorgpackagesmicrosoftazuredocumentdb152"></a><a name="1.5.2"/>[1.5.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.2)
  - Kibontott LINQ támogatja a lapozási, feltételes kifejezés új operátorokat, és összehasonlító tartományt.
    - Ahhoz, hogy VÁLASSZA a felső viselkedését LINQ operátor készítése
    - Ahhoz, hogy string tartományát összehasonlításokat CompareTo operátor
    - Feltételes (?) és egyesítés operátorok (?)
  - **[Rögzített]** A modell kivetítés egyesítésekor ArgumentOutOfRangeException Where modul linq lekérdezésben.  [#81](https://github.com/Azure/azure-documentdb-dotnet/issues/81)

### <a name="a-name151151httpswwwnugetorgpackagesmicrosoftazuredocumentdb151"></a><a name="1.5.1"/>[1.5.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.1)
 - **[Rögzített]** Ha kijelölése nem az utolsó kifejezés a LINQ szolgáltató nincs kivetítés tekinti, és jelölje ki a megtermelt * helytelen csoportosítását.  [#58](https://github.com/Azure/azure-documentdb-dotnet/issues/58)

### <a name="a-name150150httpswwwnugetorgpackagesmicrosoftazuredocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.0)
 - Megvalósított Upsert, hozzáadott UpsertXXXAsync módszerek
 - Az összes kérelmet teljesítménybeli fejlesztések
 - Feltételes, LINQ szolgáltató támogatása egyesítés és karakterláncok CompareTo módszerei
 - **[Rögzített]** LINQ szolgáltató--> módszer megvalósítása tartalmaz a listában, és az azonos SQL mint IEnumerable és tömb létrehozása
 - **[Rögzített]** BackoffRetryUtility használja az azonos HttpRequestMessage újra újat hozna létre ismét a helyett
 - **[Elavult]** UriFactory.CreateCollection--> használja a UriFactory.CreateDocumentCollection
 
### <a name="a-name141141httpswwwnugetorgpackagesmicrosoftazuredocumentdb141"></a><a name="1.4.1"/>[1.4.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.1)
 - **[Rögzített]** Honosítás problémák használatával nem en kulturális információ például Hollandia-Holland stb. 
 
### <a name="a-name140140httpswwwnugetorgpackagesmicrosoftazuredocumentdb140"></a><a name="1.4.0"/>[1.4.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.0)
  - A továbbítás azonosító alapján
    - Új UriFactory segítő tehát azonosító megkönnyítése alapú erőforrás-hivatkozások
    - Új túlterhelések DocumentClient URI figyelembe a
  - Hozzáadott IsValid() és a földrajzi LINQ a IsValidDetailed()
  - Továbbfejlesztett LINQ szolgáltató támogatás
    - **Matematikai** - Abs, ARCCOS, ARCSIN, ARCTAN, Cos kitevő, padló, bejelentkezés, Log10, Pow, ciklikus, bejelentkezési, Sin, gyök, Tan, felső határa csonkolása
    - **Karakterlánc** - összefűzés, tartalmaz, EndsWith, IndexOf, darabszám, ToLower, TrimStart, a csere, TrimEnd, StartsWith, karakterlánc, ToUpper megfordításához
    - **Tömb** - összefűzés, tartalmaz, a darabszám
    - **Az** operátorok

### <a name="a-name130130httpswwwnugetorgpackagesmicrosoftazuredocumentdb130"></a><a name="1.3.0"/>[1.3.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.3.0)
  - Az indexelési házirendek módosításával hozzáadott támogatása
    - Új ReplaceDocumentCollectionAsync módszer a DocumentClient
    - Új IndexTransformationProgress tulajdonság a ResourceResponse<T> az index házirend-módosítások százalékos végrehajtásának nyomon követése
    - Mostantól változtatható DocumentCollection.IndexingPolicy
  - Térbeli az indexelés és a lekérdezés hozzáadott támogatása
    - Új Microsoft.Azure.Documents.Spatial névtere térbeli típusok szerializálása/deszerializálása, mint a pont- és sokszög
    - Új SpatialIndex osztálya indexelés DocumentDB tárolt GeoJSON adatok
  - **[Rögzített]** : linq kifejezés [#38](https://github.com/Azure/azure-documentdb-net/issues/38) által létrehozott helytelen SQL-lekérdezés

### <a name="a-name120120httpswwwnugetorgpackagesmicrosoftazuredocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.2.0)
- Newtonsoft.Json v5.0.7 függés 
- Módosítások Order By támogatása
  - LINQ szolgáltató támogatása OrderBy() vagy OrderByDescending()
  - A támogatási Order By IndexingPolicy 
  
        **NB: Possible breaking change** 
  
        If you have existing code that provisions collections with a custom indexing policy, then your existing code will need to be updated to support the new IndexingPolicy class. If you have no custom indexing policy, then this change does not affect you.

### <a name="a-name110110httpswwwnugetorgpackagesmicrosoftazuredocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.1.0)
- Adatok szétválasztás az új HashPartitionResolver és RangePartitionResolver osztályok és a IPartitionResolver támogatása
- DataContract szerializálási
- Globálisan egyedi azonosítója támogatási LINQ szolgáltató
- A LINQ UDF-támogatás

### <a name="a-name100100httpswwwnugetorgpackagesmicrosoftazuredocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.0.0)
- KIADÁS SDK

> [AZURE.NOTE]
Hiba történt a NuGet csomag neve között a körlevél megtekintése és GA megváltoztatása Azt áthelyezése a **Microsoft.Azure.Documents.Client** **Microsoft.Azure.DocumentDB**
<br/>


### <a name="a-name09x-preview09x-previewhttpswwwnugetorgpackagesmicrosoftazuredocumentsclient"></a><a name="0.9.x-preview"/>[0.9.x-Preview](https://www.nuget.org/packages/Microsoft.Azure.Documents.Client)
- Előzetes SDK [elavult]

## <a name="release--retirement-dates"></a>Megjelenés és elavulása dátumok
A Microsoft nyújt értesítés legalább **12 hónapos** használatból történő egy SDK annak érdekében, hogy egy újabb/támogatott verzióját az áttűnés sima előtt.

Az aktuális SDK új szolgáltatásokat és funkciókat és optimalizálásokat csak hozzáadni, ilyenként javasoljuk, hogy mindig frissítse SDK verziót legkorábban lehető. 

A szolgáltatás kérésének DocumentDB egy visszavont SDK használatával történő elutasításra kerül.

> [AZURE.WARNING]
A .NET rendszerhez az Azure DocumentDB SDK verzió **1.0.0** előtt valamennyi nyelvi verziójában a **2016 február 29**visszavonásának. 
 
<br/>
 
| Verzió | Dátum visszaadása | Visszavonás dátuma 
| ---     | ---          | ---
| [1.10.0](#1.10.0) | 2016 szeptember 27 |---
| [1.9.5](#1.9.5) | 2016 szeptember 01 |---
| [1.9.4](#1.9.4) | 2016 augusztus 24 |---
| [1.9.3](#1.9.3) | 2016 augusztus 15 |---
| [1.9.2](#1.9.2) | 2016 július 23 |---
| 1.9.1 | Elavult |---
| 1.9.0 | Elavult |---
| [1.8.0](#1.8.0) | Június 14, 2016-ban |---
| [1.7.1](#1.7.1) | 2016 május 06 |---
| [1.7.0](#1.7.0) | 2016 áprilisi 26. |---
| [1.6.3](#1.6.3) | 2016 áprilisi 08 |---
| [1.6.2](#1.6.2) | Március 29, 2016-ban |---
| [1.5.3](#1.5.3) | Február 19, 2016-ban |---
| [1.5.2](#1.5.2) | December 14 2015 |---
| [1.5.1](#1.5.1) | November 23 2015 |---
| [1.5.0](#1.5.0) | 2015 október 05 |---
| [1.4.1](#1.4.1) | 2015 augusztus 25 |---
| [1.4.0](#1.4.0) | 2015 augusztus 13 |---
| [1.3.0](#1.3.0) | 2015 augusztus 05 |---
| [1.2.0](#1.2.0) | 2015 július 06 |---
| [1.1.0](#1.1.0) | 2015 április 30 |---
| [1.0.0](#1.0.0) | 2015 április 08 |---
| [0.9.3-prelease](#0.9.x-preview) | 2015 március 12 | 2016 február 29
| [0.9.2-prelease](#0.9.x-preview) | Január, 2015 | 2016 február 29
| [.9.1-prelease](#0.9.x-preview) | Október 13, 2014-es | 2016 február 29
| [0.9.0-prelease](#0.9.x-preview) | Augusztus 21, 2014-es | 2016 február 29

## <a name="faq"></a>GYAKORI KÉRDÉSEK
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Lásd még:

DocumentDB kapcsolatos további tudnivalókért lásd: [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) szolgáltatás lapon. 
