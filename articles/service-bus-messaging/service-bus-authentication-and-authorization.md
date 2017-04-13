<properties 
    pageTitle="Szolgáltatási Bus hitelesítési és engedélyezési |} Microsoft Azure"
    description="A megosztott Access-aláírás (Társítások) hitelesítési áttekintése."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="service-bus-authentication-and-authorization"></a>Szolgáltatás Bus hitelesítése és engedélyezése

Alkalmazások hitelesíthet Azure Service Bus bármelyik megosztott Access-aláírás (Társítások) hitelesítéssel vagy Azure Active Directory hozzáférés-vezérlés (más néven vezérlő szolgáltatást vagy ACS) keresztül. A megosztott Access aláírás hitelesítés lehetővé teszi, hogy alkalmazások használata a névtér, vagy a szervezet, amellyel adott tartalomvédelmi beállítva hívóbetű szolgáltatás Bus hitelesíteni. A kulcs megosztott Access-aláírás jogkivonat, amely az ügyfelek használatával szolgáltatás Bus hitelesíteni létrehozásához használhatja.

>AZURE. FONTOS Társítások ajánlott fölé ACS, mint egy egyszerű, rugalmas és könnyen használható hitelesítési módot nyújt szolgáltatás Bus. Alkalmazások használhatják Társítások esetekben, ahol azok nem kell kezelni az állomástól hivatalos "felhasználók."

## <a name="shared-access-signature-authentication"></a>Megosztott Access aláírás hitelesítés

[Társítások hitelesítési](service-bus-sas-overview.md) lehetővé teszi, hogy a felhasználó hozzáférést szolgáltatás Bus erőforrások egyedi engedélyekkel. Szolgáltatás Bus Társítások hitelesítés magában foglalja a titkosítási kulcs szolgáltatás Bus erőforrás társított jogosultságokkal rendelkező beállításait. Ügyfelek majd hozzáférhetnek adott erőforrás által áll, az erőforrás-használatban URI Társítások token bemutatót tart, és a beállított kulccsal aláírt megszüntetésének.

A Társítások billentyűk adhatja meg a szolgáltatás Bus névteret. A kulcs adott névtér az összes üzenetben entitás vonatkozik. Billentyűparancsok a szolgáltatás Bus sorban várakozó és témaköröket is beállítható. Társítások is támogatott szolgáltatás Bus továbbítja.

Társítások használni, állítsa be a [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) objektum névtér, várólista vagy az alábbiak szerint:

- *Kulcsnév* , amely azonosítja a szabályt.

- *PrimaryKey* érvényesítéséhez használt bejelentkezési/Társítások tokenek a titkosítási kulcs.

- *Másodlagos kulcs* a titkosítási kulcs érvényesítéséhez használt bejelentkezési/Társítások tokenek.

- *Tartalomvédelmi* meghallgatását, a Küldés vagy a kezelés jogosultságait gyűjteménye, amely.

A névtér szintjén beállított engedélyezési szabályokat is hozzáférést névtér az összes entitás ügyfelek számára a megfelelő kulccsal aláírt alapelemeket. Felfelé 12 engedély szabályok szolgáltatás Bus névtér, várólista, vagy a témakör beállíthatók. Alapértelmezés szerint egy [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) összes jogosultságokkal rendelkező be van állítva az minden névtér amikor először már kiépítve.

Az ügyfél entitás eléréséhez szükséges a egy adott [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx)használatával létrehozott biztonsági jogkivonat. A biztonsági jogkivonat jön létre, és egy erőforrás-karakterlánc, amely az access igénylik, amelyhez az erőforrás URI-lejárati HMAC-SHA256 használata a titkosítási kulcs az engedélyezési szabály társított.

Társítások hitelesítés támogatása szolgáltatás Bus abban az Azure .NET SDK 2.0-s verziója és a későbbi. Társítások egy [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx)támogatását tartalmazza. Fogadja el a kapcsolati karakterlánc paraméterként API-k Társítások csatlakozási_karakterlánc tartalmazzák.

## <a name="acs-authentication"></a>ACS hitelesítés

Szolgáltatás Bus hitelesítési keresztül ACS kezeli a kereső "-sb" ACS névtér. Ha azt szeretné, hogy a kereső ACS névtér szolgáltatás Bus névteret jön létre, nem hozható létre a szolgáltatás Bus névtér az Azure klasszikus portálon; a [New-AzureSBNamespace](https://msdn.microsoft.com/library/azure/dn495165.aspx) PowerShell parancsmaggal névtér létre kell hoznia. Példa:

```
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $true
```

Egy ACS névtér létrehozása elkerüléséhez hiba az alábbi parancsot:

```
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $false
```

Például ha **contoso.servicebus.windows.net**nevű szolgáltatás Bus névteret hoz létre, **contoso-sb.accesscontrol.windows.net** nevű companion ACS névteret már kiépítve automatikusan. Minden névtér augusztus 2014-es előtt létrehozott egy kapcsolódó ACS névtér is készült.

Alapértelmezett szolgáltatás identitás "tulajdonosa," az összes alapértelmezés szerint a kereső ACS névtér a már kiépítve. A megfelelő bizalmi kapcsolatok konfigurálásával keresztül ACS szolgáltatás Bus szervezet egyedi vezérlőelem szerezhet be. További szolgáltatás identitások szolgáltatás Bus szervezetek való hozzáférés kezelése a is beállíthatja.

Entitás eléréséhez az ügyfél kér egy SWT jogkivonat ACS a megfelelő követelések saját hitelesítő adatok alapján. A SWT jogkivonat kell majd kell küldeni a kérelem részeként szolgáltatás Bus ahhoz, hogy az ügyfél, a szervezet elérésére vonatkozó engedély.

Szolgáltatás Bus ACS hitelesítés támogatása abban az Azure .NET SDK 2.0-s verziója és a későbbi. Ez a hitelesítés támogatása a [SharedSecretTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.sharedsecrettokenprovider.aspx)tartalmazza. Fogadja el a kapcsolati karakterlánc paraméterként API-k ACS csatlakozási_karakterlánc tartalmazzák.

## <a name="next-steps"></a>Következő lépések

Továbbra is a [megosztott Access-aláírás hitelesítéssel szolgáltatás Bus](service-bus-shared-access-signature-authentication.md) olvasási Társítások olvashat bővebben.

Társítások szolgáltatás Bus a magas szintű áttekintéséhez olvassa el a [Megosztott Access aláírások](service-bus-sas-overview.md)című témakört.

Talál további információt a ACS tokenek [hogyan: jogkivonat kérése az OAuth TÖRDELÉSE protokollon keresztül ACS](https://msdn.microsoft.com/library/hh674475.aspx).



