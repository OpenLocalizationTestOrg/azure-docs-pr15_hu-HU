<properties
   pageTitle=".NET 2.x API-tárolóból elemre kibocsátási megjegyzések főbb |} Microsoft Azure"
   description=".NET-fejlesztők fogja használni a kód API Azure kulcs tárolóból elemre"
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="CSharp"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/07/2016"
   ms.author="bruceper" />

# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Azure kulcs tárolóra .NET 2.0 – kibocsátási megjegyzések és az áttelepítési útmutató

Az alábbi jegyzeteket, és útmutatást vannak a fejlesztők az Azure kulcs tárolóra .NET használata és C# tárat. A változtatások az 1,0 verzióról a 2.0-s verziója, a frissítések számos esett áttelepítési munka mielőtt élvezhetik az funkcionális javítása és kiegészítések például kulcs tárolóból elemre a tanúsítványok támogatási szolgáltatás a kód megkövetelése teszi.

Kulcs tárolóból elemre a tanúsítványok támogatási biztosít a x509 irányításának tanúsítványok és az alábbiak egyike:  

-   Lehetővé teszi a tanúsítvány tulajdonosa hozhat létre egy tanúsítványt egy kulcsot tárolóra létrehozási folyamatot vagy egy meglévő tanúsítvány importálása keresztül. Ide tartoznak a mindkét önaláírt, és a hitelesítésszolgáltató generált tanúsítványok.

- Lehetővé teszi, hogy egy kulcsot tárolóból elemre tanúsítvány tulajdonosának biztonságos tárolása és kezelése a X509 végrehajtásához tanúsítványok titkos kulcs anyag közreműködése nélkül.  

-   Lehetővé teszi a tanúsítvány tulajdonosa, hozzon létre egy házirendet, amely arra utasítja kezelése az életciklus-tanúsítvány a kulcs tárolóból elemre.  

-   Lehetővé teszi, hogy az értesítési elérhetőségi adatok lejárati és a tanúsítvány megújítás életciklus-események megadása a tulajdonosok tanúsítvány.  

-   Támogatja az automatikus megújítás a kijelölt kibocsátó - kulcs tárolóra partner X509 tanúsítvány-szolgáltatók / hitelesítésszolgáltatók.
    - Ne FELEDJE - szolgáltatók/hivatkozásjegyzék-közösen is engedélyezettek, de nem támogatja az automatikus megújítás funkció.


## <a name="net-support"></a>.NET-támogatás
- Az Azure kulcs tárolóra .NET 2.0-s verziója nem támogatott a **.NET 4.0-s** és C# tárban
- Az Azure kulcs tárolóra .NET 2.0-s verziója által támogatott **.NET Core** és C# tárban

## <a name="namespaces"></a>Névtér
- A névtér **modellek** **Microsoft.Azure.KeyVault.Models**a **Microsoft.Azure.KeyVault** változik.
- A **Microsoft.Azure.KeyVault.Internal** névtér megszakad.
- Az Azure SDK függőségek névtér változnak **Hyak.Common** és **Hyak.Common.Internals** **Microsoft.Rest** és **Microsoft.Rest.Serialization**


## <a name="type-changes"></a>Típus módosítása
- *Titkos* *SecretBundle* módosítva
- *Szótár* *IDictionary* módosítva
- *Lista<T>, [] karakterlánc* átállítása *IList<T> *
- *NextList* *NextPageLink* módosítva


## <a name="return-types"></a>Visszatérési típus
- **KeyList** és **SecretList** ad vissza *IPage<T> * *ListKeysResponseMessage* helyett
- A létrehozott **BackupKeyAsync** *érték* (mentéséről blob) tartalmaz *BackupKeyResult* ad eredményül. A mód volt csomagolt előtt, majd visszatérés a csak az értéket.

## <a name="exceptions"></a>A kivételek
- *KeyVaultClientException* *KeyVaultErrorException* változik
- A szolgáltatás hiba *kivétel változik. Hiba* *kivétel szeretne. Body.Error.Message*.
- További információ eltávolítja a hibaüzenet **[JsonExtensionData]**.

## <a name="constructors"></a>Konstruktorok
- Helyett egy *HttpClient* konstruktor argumentumként fogad, a konstruktor csak fogad el *HttpClientHandler* vagy *DelegatingHandler []*.



## <a name="downloaded-packages"></a>A letöltött csomagok  
Amikor egy ügyfél által feldolgozott függő kulcs tárolóból elemre a következő letöltött
#### <a name="previous-package-list"></a>Előző csomagok listája
- verzió id="Hyak.Common csomag" = "1.0.2-es verziójú" targetFramework = "net45"
- verzió id="Microsoft.Azure.Common csomag" = "2.0.4" targetFramework = "net45"
- verzió id="Microsoft.Azure.Common.Dependencies csomag" = "1.0.0" targetFramework = "net45"
- verzió id="Microsoft.Azure.KeyVault csomag" = "1.0.0" targetFramework = "net45"
- verzió id="Microsoft.Bcl csomag" = "1.1.9" targetFramework = "net45"
- verzió id="Microsoft.Bcl.Async csomag" = "1.0.168" targetFramework = "net45"
- verzió id="Microsoft.Bcl.Build csomag" = "1.0.14" targetFramework = "net45"
- verzió id="Microsoft.Net.Http csomag" = "2.2.22" targetFramework = "net45"

#### <a name="current-package-list"></a>Aktuális csomagok listája
- verzió id="Microsoft.Azure.KeyVault csomag" = "2.0.0-preview" targetFramework = "net45"
- verzió id="Microsoft.Rest.ClientRuntime csomag" = "2.2.0" targetFramework = "net45"
- verzió id="Microsoft.Rest.ClientRuntime.Azure csomag" = "3.2.0" targetFramework = "net45"


## <a name="class-changes"></a>A módosítások osztály

- **UnixEpoch** osztály el lett távolítva.
- **Base64UrlConverter** osztály **Base64UrlJsonConverter** új neve

## <a name="other-changes"></a>Egyéb módosítások

- A konfiguráció KV művelet újrapróbálkozási házirend tranziens hibákkal kapcsolatos már támogatja az API verziójában.



## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet
- A műveletek által visszaküldött *tárolóból elemre*a visszatérési típus volt a tárolóból elemre tulajdonságait tartalmazó osztály. A visszatérési típus ettől *tárolóból elemre*.
- *PermissionsToKeys* és *PermissionsToSecrets* , most már *Permissions.Keys* és *Permissions.Secrets*
- A vezérlő-sík, valamint néhány, a visszatérési típus változtatást vonatkoznak.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet
- A csomag megszakad **Microsoft.Azure.KeyVault.Extensions** és **Microsoft.Azure.KeyVault.Cryptography** a titkosítási műveleteket.
