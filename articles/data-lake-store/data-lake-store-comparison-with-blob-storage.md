<properties
   pageTitle="Azure Blob-tároló Azure tó adattár összehasonlítás |} Microsoft Azure"
   description="Azure Blob-tároló Azure tó adattár összehasonlítása"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/15/2016"
   ms.author="nitinme"/>

# <a name="comparing-azure-data-lake-store-and-azure-blob-storage"></a>Azure tó adattár és Azure Blob-tárolóhoz összehasonlítása

Ez a cikk a táblázatban néhány fontosabb szempontok nagy adatfeldolgozás mentén Azure tó adattár és Azure Blob-tárolóhoz közötti különbségeket. Azure Blob-tárolóhoz egy általános célú, számos különböző típusú tároló esetek készült méretezhető objektum áruházból. Azure tó adattár hyper skála tárháza nagy adatok analytics munkaterhelésekből van optimalizálva.

|    | Azure tó adattárhoz  | Azure Blob-tárolóhoz  |
|----|-----------------------|--------------------|
| Cél  | Nagy adatok analytics munkaterhelésekből optimalizált tárterület                                                                          | Általános célú objektum tárolása számos különböző típusú tároló felhasználási területei                                                                                |
| Használati eset                                   | Köteg, interaktív, például streaming analitikai és gépi tanulási adatok naplófájlokba IoT adatok, és válassza a adatfolyamok, nagyméretű adatkészletek | Szöveg vagy a bináris adatokat, például alkalmazás bármilyen típusú biztonsági vége, adatok biztonsági másolatának, adatfolyam és az általános célját adatok media tárterület |
| Alapfogalmak                                | Tó adattár fiók tartalmazza azokat a mappákat, amelyben a fájlokban tárolt adatok | Tárterület-fiók van tárolók, amelyet viszont adatok BLOB formájában |
| Struktúra | Hierarchikus fájlrendszerben                                                                                                    | Az egyszerű, strukturálatlan névtér objektum áruházból  |
| API   | HTTPS REST API-val | HTTP/HTTPS REST API-val                                                                                                                            |
| Kiszolgálóoldali API                             | [WebHDFS-kompatibilis REST API-val](https://msdn.microsoft.com/library/azure/mt693424.aspx)                                                                                                 | [Azure Blob-tárolóhoz REST API-val](https://msdn.microsoft.com/library/azure/dd135733.aspx)                                                                                                                         |
| Rendszer-ügyfél Hadoop-fájlt                   | igen                                                                                                                         | igen                                                                                                                                                 |
| Adatműveleteket - hitelesítés            | [Azure Active Directory identitások](../active-directory/active-directory-authentication-scenarios.md) alapján | A következők - [Fiók hívóbetűk](../storage/storage-create-storage-account.md#manage-your-storage-account) és a [Megosztott hívóbetűk aláírás](../storage/storage-dotnet-shared-access-signature-part-1.md)alapján.                                                                       |
| Adatműveleteket - hitelesítés Protocol (protokoll)     | OAuth 2.0-s verziója. Hívások egy érvényes JWT (JSON webes jogkivonat) Azure Active Directory által kibocsátott kell tartalmaznia.                     | Üzenet ujjlenyomat-alapú hitelesítés kód (HMAC). Hívások Base64 kódolású SHA-256 kivonat tartalmaznia kell a HTTP-kérés része fölé. |
| Adatműveleteket - hitelesítés               | POSIX Access vezérlő vezérlési listákat.  Fájlok és mappák szintű hozzáférés-vezérlési listák alapján az Azure Active Directory identitások állítható be. | A fiók szintű engedélyt – [Fiók hívóbetűk](../storage/storage-create-storage-account.md#manage-your-storage-account) használata<br>A fiók, a tároló vagy a blob engedélyezése - [Megosztott aláírás hívóbetűk](../storage/storage-dotnet-shared-access-signature-part-1.md) használata |
| A naplózás - adatgyűjtési-műveletek                    | Érhető el. Lásd: [Itt](data-lake-store-diagnostic-logs.md) további információkat.                                                                                                                   | Érhető el                                                                                                                                           |
| A többi titkosítási adatok                     | Áttetsző, kiszolgálóoldalon (hamarosan)<ul><li>Szolgáltatás-felügyelt billentyűkkel</li><li>Az Azure KeyVault ügyfél által kezelt billentyűkkel</li></ul>| <ul><li>Áttetsző, kiszolgálóoldali</li> <ul><li>Szolgáltatás-felügyelt billentyűkkel</li><li>Ügyfél által kezelt billentyűkkel az Azure KeyVault (hamarosan)</li></ul><li>Ügyféloldali titkosítás:</li></ul> |
| Adatkezelési műveletek (például fiók létrehozásához) | [Szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md) Fiókok kezelése Azure által biztosított (RBAC)                                                                       | [Szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md) Fiókok kezelése Azure által biztosított (RBAC)                                                                                               |
| Fejlesztőeszközök SDK                              | A .NET Java Node.js                                                                                                         | A .net Java Python, Node.js, C++, fonetikus                                                                                                              |
| Analytics terhelést teljesítmény              | A párhuzamos analytics-munkaterhelésekből optimalizált teljesítményt. Nagy teljesítmény és IOPS.                                           | Nem optimalizált analytics-munkaterhelésekből                                                                                                               |
| Fájlméretet érintő korlátozásokról                                 | Nincs korlát fiók méretű, a fájlok méretének vagy a fájlok száma                                                                   | Bizonyos korlátozások dokumentált [Itt](../azure-subscription-service-limits.md#storage-limits)                                                                                                                     |
| A redundancia GEO                              | Helyi meghajtóra-felesleges (Azure régióban adatok több példányban)                                                             | Helyben felesleges (LRS), a globálisan felesleges (GRS), az olvasásra globálisan felesleges (TS-GRS). Lásd: [Itt](../storage/storage-redundancy.md) olvashat |
| Szolgáltatás állapota                               | Nyilvános előzetes verzió                                                                                                              | Általában elérhető                                                                                                                                 |
| Területi elérhetősége  | Lásd: [Itt](https://azure.microsoft.com/regions/#services)| Lásd: [Itt](https://azure.microsoft.com/regions/#services) |
| Ár                                       |     Lásd: a [árak](https://azure.microsoft.com/pricing/details/data-lake-store/)| Lásd: a [árak](https://azure.microsoft.com/pricing/details/storage/) |

### <a name="next-steps"></a>Következő lépések

- [Azure tó adattár áttekintése](data-lake-store-overview.md)
- [Első lépések a tó adattárhoz](data-lake-store-get-started-portal.md)



