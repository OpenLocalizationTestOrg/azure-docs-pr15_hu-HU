<properties
    pageTitle="Azure Active Directory összevonási metaadatok |} Microsoft Azure"
    description="Ez a cikk ismerteti az Azure Active Directory közzé, a szolgáltatásokhoz, fogadja el a tokenek Azure Active Directory összevonási metaadat-dokumentum."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="priyamo"/>


# <a name="federation-metadata"></a>Metaadat-alapú összevonás

Azure Active Directory (Azure Active Directory) közzé, a szolgáltatásokhoz, az Azure Active Directory alkotta biztonsági tokenek fogadására összevonási metaadatok dokumentum. Az [1.2-es verzió Web Services szövetség Language (Webszolgáltatás-összevonás)](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) [OASIS biztonsági állítás Markup Language (SAML) 2.0-s verzió metaadatait](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf)nyúló az összevonási metaadatok dokumentumformátum ismertetése.

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a>Bérlői kötött és a bérlői beállítástól független metaadatok végpontok

Azure Active Directory teszi közzé a bérlői webhely-specifikus és bérlői beállítástól független végpontok.

Egy adott bérlői bérlői-specifikus végpontok készültek. A bérlői webhely-specifikus összevonási metaadatok a bérlői, többek között a bérlői kibocsátó és végpont adatait információkat tartalmaz. Egy egyetlen bérlői webhelyhez való hozzáférés korlátozása alkalmazásokat a bérlői-specifikus végpontok használata.

Bérlői beállítástól független végpontok információkat az összes Azure AD-bérlők közös. Ez az információ üzemeltetett *login.microsoftonline.com* a bérlők vonatkozik, és a bérlők megoszthatja. Bérlői beállítástól független végpontok ajánlott több bérlői alkalmazások, mivel ezek nem társulnak bármely adott bérlői webhely.

## <a name="federation-metadata-endpoints"></a>Összevonás metaadat-végpontok

Azure Active Directory összevonási lévő metaadatok közzététele `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.

A **bérlői webhely-specifikus végpontok**a `TenantDomainName` lehetnek az alábbiak egyikét:

- Az Azure AD a bejegyzett tartománynév bérlői, például: `contoso.onmicrosoft.com`.
- A megváltoztatható azonosító tartomány, például: bérlői `72f988bf-86f1-41af-91ab-2d7cd011db45`.

A **bérlői beállítástól független végpontok**a `TenantDomainName` van `common`. A dokumentum csak a metaadat-alapú összevonás elemeket, amelyek megegyeznek a login.microsoftonline.com-on tárolt összes Azure AD-bérlők sorolja fel.

A bérlői webhely-specifikus végpont lehet például `https:// login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`. A bérlői beállítástól független végpont [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml). Megtekintheti az összevonási metaadat-dokumentum Ez az URL-cím beírásával a böngészőben.

## <a name="contents-of-federation-metadata"></a>Metaadat-alapú összevonás tartalma

A következő szakaszban az Azure Active Directory által kibocsátott tokenek felhasználása szolgálatai adatait találja.

### <a name="entity-id"></a>Szervezet azonosítója

A `EntityDescriptor` elem tartalmaz egy `EntityID` attribútum. Értékét a `EntityID` attribútum a kibocsátó, ez azt jelenti, hogy a biztonsági jogkivonat-szolgáltatás, amely a token kiadott jelöli. Fontos a kibocsátó jogkivonat fogadásakor érvényesítéséhez.

A következő metaadatok megmutatja, bérlői-specifikus `EntityDescriptor` elem egy `EntityID` elemet.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
A bérlői beállítástól független végpont bérlői Azonosítóra lecserélheti a bérlői azonosító létrehozása egy bérlői adott `EntityID` értéket. Az eredményül kapott értékkel ugyanaz, mint a token kibocsátó lesz. A stratégia lehetővé teszi, hogy a több elem bérlői alkalmazások a kibocsátó adott bérlő érvényesítéséhez.

A következő metaadatok megmutatja, bérlői beállítástól független `EntityID` elemet. Felhívjuk a figyelmét arra, hogy a `{tenant}` szövegkonstans, helyőrző nem.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a>Jogkivonat-aláíró tanúsítványok

Amikor szolgáltatás Azure AD-bérlő által kibocsátott token kap, az aláírást a jogkivonat ellenőrizni kell az összevonási metaadatok dokumentumban közzétett aláírási használatával. Az összevonási metaadatokat használni a bérlők jogkivonat-aláíró tanúsítvány nyilvános része tartalmaz. A tanúsítvány nyers bájtok jelennek meg a `KeyDescriptor` elemet. Az aláíró tanúsítvány token csak ha aláíráshoz érvényes értékét a `use` attribútum `signing`.

Közzétett Azure Active Directory összevonási metaadat-alapú dokumentum beállíthatja, hogy több aláírási kulcsok, például amikor előkészíti a Azure AD az aláíró tanúsítvány frissítését. Metaadat-alapú összevonás dokumentum egynél több tanúsítvány tartalmazza, amikor a szolgáltatás, amely ellenőrzi a tokenek kell a dokumentum összes tanúsítványok támogatja.

A következő metaadatok megmutatja, `KeyDescriptor` aláírási használatával elemet.

```
<KeyDescriptor use="signing">
<KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
<X509Data>
<X509Certificate>
MIIDPjCCAiqgAwIBAgIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTIwNjA3MDcwMDAwWhcNMTQwNjA3MDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArCz8Sn3GGXmikH2MdTeGY1D711EORX/lVXpr+ecGgqfUWF8MPB07XkYuJ54DAuYT318+2XrzMjOtqkT94VkXmxv6dFGhG8YZ8vNMPd4tdj9c0lpvWQdqXtL1TlFRpD/P6UMEigfN0c9oWDg9U7Ilymgei0UXtf1gtcQbc5sSQU0S4vr9YJp2gLFIGK11Iqg4XSGdcI0QWLLkkC6cBukhVnd6BCYbLjTYy3fNs4DzNdemJlxGl8sLexFytBF6YApvSdus3nFXaMCtBGx16HzkK9ne3lobAwL2o79bP4imEGqg+ibvyNmbrwFGnQrBc1jTF9LyQX9q+louxVfHs6ZiVwIDAQABo2IwYDBeBgNVHQEEVzBVgBCxDDsLd8xkfOLKm4Q/SzjtoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAA4IBAQAkJtxxm/ErgySlNk69+1odTMP8Oy6L0H17z7XGG3w4TqvTUSWaxD4hSFJ0e7mHLQLQD7oV/erACXwSZn2pMoZ89MBDjOMQA+e6QzGB7jmSzPTNmQgMLA8fWCfqPrz6zgH+1F1gNp8hJY57kfeVPBiyjuBmlTEBsBlzolY9dd/55qqfQk6cgSeCbHCy/RU/iep0+UsRMlSgPNNmqhj5gmN2AFVCN96zF694LwuPae5CeR2ZcVknexOWHYjFM0MgUSw0ubnGl0h9AJgGyhvNGcjQqu9vd1xkupFgaN+f7P3p3EVN5csBg5H94jEcQZT7EKeTiZ6bTrpDAnrr8tDCy8ng
</X509Certificate>
</X509Data>
</KeyInfo>
</KeyDescriptor>
  ```

A `KeyDescriptor` elem jelenik meg a metaadat-alapú összevonás dokumentum; két helyen az a WS kötött összevonási és a SAML-specifikus szakaszát. A tanúsítványok mindkét szakaszban közzétett azonos lesz.

WS kötött összevonási csoportban a Webszolgáltatás-összevonás metaadat-olvasó olvashatók, mint a tanúsítványok egy `RoleDescriptor` elem a `SecurityTokenServiceType` típusát.

A következő metaadatok megmutatja, `RoleDescriptor` elemet.

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

SAML-specifikus csoportban a Webszolgáltatás-összevonás metaadat-olvasó olvashatók, mint a tanúsítványok egy `IDPSSODescriptor` elemet.

A következő metaadatok megmutatja, `IDPSSODescriptor` elemet.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
Bérlői-specifikus és bérlői beállítástól független tanúsítványok formátuma nem különbségek vannak.

### <a name="ws-federation-endpoint-url"></a>Webszolgáltatás-összevonás végpont URL-címe

Az összevonási metaadatok egyszeri bejelentkezésről és a Webszolgáltatás-összevonás protokoll kijelentkezési egyetlen Azure Active Directory használja-e az URL-cím tartalmazza. Megjelenik a végpont a `PassiveRequestorEndpoint` elemet.

A következő metaadatok megmutatja, `PassiveRequestorEndpoint` egy bérlői-specifikus végpontot elemet.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
A bérlői beállítástól független végpont Webszolgáltatás-összevonás URL-címe megjelenik az Webszolgáltatás-összevonás végpontot, az alábbi példa látható módon.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a>SAML protokoll végpont URL-címe

Az összevonási metaadatok Azure Active Directory által az egyszeri bejelentkezésről és a SAML 2.0-s protokoll kijelentkezési egy URL-CÍMÉT tartalmazza. Ezeket a végpontokat jelennek meg a `IDPSSODescriptor` elemet.

A bejelentkezési és kijelentkezési URL-cím látható a `SingleSignOnService` és `SingleLogoutService` elemeket.

A következő metaadatok megmutatja, `PassiveResistorEndpoint` egy bérlői-specifikus végpontot.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

Hasonlóképpen a közös SAML 2.0-s protokoll végpontok végpontjait közzétett a bérlői beállítástól független összevonási metaadatok a következő példa látható módon.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
