<properties
    pageTitle="SAML protokoll Azure egyszeri bejelentkezéses |} Microsoft Azure"
    description="Ez a cikk ismerteti az Azure Active Directory egyszeri bejelentkezési a SAML Protocol (protokoll)"
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

# <a name="single-sign-on-saml-protocol"></a>Egyszeri bejelentkezés SAML Protocol (protokoll)

Ez a cikk bemutatja a SAML 2.0-s hitelesítési kérések és válaszok, amely támogatja az egyszeri bejelentkezés az Azure Active Directory (Azure Active Directory).

A Protocol (protokoll) az alábbi ábra az egyszeri bejelentkezéses sorozat ismerteti. A felhőalapú szolgáltatást (a szolgáltató) használ egy HTTP átirányítás kötés átadni egy `AuthnRequest` Azure Active Directory (hitelesítés kérelem) elemet (az identitásszolgáltató). Azure Active Directory majd használja a közzétételi kötelező HTTP post egy `Response` a felhőbeli szolgáltatástól elemet.

![Munkafolyamat egyszeri bejelentkezés](media/active-directory-single-sign-on-protocol-reference/active-directory-saml-single-sign-on-workflow.png)

## <a name="authnrequest"></a>AuthnRequest

Egy felhasználó hitelesítést igényel, cloud services küldés egy `AuthnRequest` Azure Active Directory elemet. A SAML 2.0 minta `AuthnRequest` ábrája ilyen:

```
<samlp:AuthnRequest
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="id6c1c178c166d486687be4aaf5e482730"
Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
</samlp:AuthnRequest>
```


| Paraméter | | Leírás |
| ----------------------- | ------------------------------- | --------------- |
| AZONOSÍTÓ | szükséges | Azure Active Directory használja az attribútum kitöltéséhez a `InResponseTo` a visszaadott válasz attribútum. Azonosító nem lehet egy szám, így a közös stratégia prepend egy karakterlánc, például "azonosító" karakterlánc ábrázolása egy globálisan egyedi azonosítója. Ha például `id6c1c178c166d486687be4aaf5e482730` van érvényes azonosítót. |
| Verzió | szükséges | **2.0-s verzióját**kell.|
| IssueInstant | szükséges | Ez a DateTime karakterlánc Egyezményes érték és [oda-vissza formátum ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD egy DateTime típusú érték, az ilyen típusú vár, de nem értékeli, és írja be az értéket. |
| AssertionConsumerServiceUrl | nem kötelező. | Ha megadták, ezt meg kell egyeznie a `RedirectUri` a felhőalapú szolgáltatás az Azure Active Directory. |
| ForceAuthn | nem kötelező. | Ha megadták, ezt kell hamis. Minden más érték, az hibát okoz.|
| IsPassive | nem kötelező. | Ha megadták, ezt kell hamis. Minden más érték, az hibát okoz. |  

Minden más `AuthnRequest` attribútumok, például a felhasználó hozzájárul ahhoz, cél, AssertionConsumerServiceIndex, AttributeConsumerServiceIndex és ProviderName **figyelmen kívül hagyja**.

Azure Active Directory is figyelmen kívül hagyja a `Conditions` elemének `AuthnRequest`.

### <a name="issuer"></a>Kibocsátó

A `Issuer` eleme egy `AuthnRequest` pontosan egyeznie kell a **ServicePrincipalNames** a felhőszolgáltatásában közül az Azure Active Directory. A szokásos ez értékre van állítva az **Alkalmazás azonosítója URI** alkalmazás regisztráció során van megadva.

Egy minta SAML kivonat tartalmazó a `Issuer` elem így néz ki:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
```

### <a name="nameidpolicy"></a>NameIDPolicy

Ez az elem adott azonosító névformátum a válaszban kéri, és nem kötelező, a `AuthnRequest` Azure Active Directory küldött elemek.

Minta `NameIdPolicy` elem így néz ki:

```
<NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
```

Ha `NameIDPolicy` áll rendelkezésre a választható is `Format` attribútum. A `Format` attribútum is rendelkezik, csak az alábbi értékek közül; minden más érték hibát eredményez.

-  `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`: Azure Active Directory a páros azonosítóként NameID igényt hibák.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`: Azure Active Directory hibák a NameID kárigény e-mail cím formátum.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`: Ez az érték lehetővé teszi az Azure Active Directory állítást formátumának kiválasztásához. Azure Active Directory a páros azonosítóként a NameID hibák.

Ne kerüljön bele a `SPNameQualifer` attribútum. Azure Active Directory figyelmen kívül hagyja a `AllowCreate` attribútum.

### <a name="requestauthncontext"></a>RequestAuthnContext

A `RequestedAuthnContext` elemét adja meg a kívánt hitelesítési módszereket. Ez a lépés nem kötelező, a `AuthnRequest` Azure Active Directory küldött elemek. Azure Active Directory támogatja a csak az egyik `AuthnContextClassRef` érték: `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`.

### <a name="scoping"></a>Hatókör meghatározása

A `Scoping` elemet, amely tartalmazza a Identitásszolgáltatók listáját, a lépés nem kötelező, a `AuthnRequest` Azure Active Directory küldött elemek.

Ha megadták, ne kerüljön bele a `ProxyCount` tulajdonság, `IDPListOption` vagy `RequesterID` elemet, mint nem támogatottak.

### <a name="signature"></a>Aláírás

Ne kerüljön bele a `Signature` elemének `AuthnRequest` elemeket, mint nem támogatja az Azure Active Directory aláírt hitelesítési kérések.

### <a name="subject"></a>Tárgy

Azure Active Directory figyelmen kívül hagyja a `Subject` eleme `AuthnRequest` elemeket.

## <a name="response"></a>Válasz

Kért bejelentkezéses sikeresen végrehajtotta, amikor az Azure Active Directory bejegyzések a felhőbeli szolgáltatástól választ. A sikeres bejelentkezési kísérletet minta adott válasz néz ki:

```
<samlp:Response ID="_a4958bfd-e107-4e67-b06d-0d85ade2e76a" Version="2.0" IssueInstant="2013-03-18T07:38:15.144Z" Destination="https://contoso.com/identity/inboundsso.aspx" InResponseTo="id758d0ef385634593a77bdf7e632984b6" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
    ...
  </ds:Signature>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
  <Assertion ID="_bf9c623d-cc20-407a-9a59-c2d0aee84d12" IssueInstant="2013-03-18T07:38:15.144Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      ...
    </ds:Signature>
    <Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
  </Assertion>
</samlp:Response>
```

### <a name="response"></a>Válasz

A `Response` elem tartalmaz-e az engedélyezési kérelmet eredményét. Azure Active Directory-készletek a `ID`, `Version` és `IssueInstant` szereplő értékek a `Response` elemet. A állítja be a következő attribútumok is:

- `Destination`: Ha bejelentkezés sikeresen befejeződött, a rendszer értékre van állítva a `RedirectUri` a szolgáltató (felhőalapú szolgáltatás).
- `InResponseTo`: Ez a `ID` attribútuma a `AuthnRequest` elemet, amelyet a kérdésre adott választ kezdeményezni.

### <a name="issuer"></a>Kibocsátó

Azure Active Directory-készletek a `Issuer` elem `https://login.microsoftonline.com/<TenantIDGUID>/` hol <TenantIDGUID> az Azure AD-bérlő bérlői azonosítója.

Ha például egy minta válasz kibocsátó elemmel ábrája ilyen:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

### <a name="status"></a>Állapot

A `Status` elem tartalmát a megoldás sikeres vagy sikertelen bejelentkezés. Tartalmazza a `StatusCode` elemet, amely tartalmaz egy kódot, vagy egy sor olyan beágyazott kódok a kérelem állapotát megjelenítő. Is tartalmazza a `StatusMessage` elemet, amely tartalmazza az egyéni hibaüzenetek jelennek meg, hogy a program hozza létre a bejelentkezés során.

<!-- TODO: Add a authentication protocol error reference -->

Sikertelen bejelentkezés kísérlet SAML adott válasz a következő:

```
<samlp:Response ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Requester">
      <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:RequestUnsupported" />
    </samlp:StatusCode>
    <samlp:StatusMessage>AADSTS75006: An error occurred while processing a SAML2 Authentication request. AADSTS90011: The SAML authentication request property 'NameIdentifierPolicy/SPNameQualifier' is not supported.
Trace ID: 66febed4-e737-49ff-ac23-464ba090d57c
Timestamp: 2013-03-18 08:49:24Z</samlp:StatusMessage>
  </samlp:Status>
```

### <a name="assertion"></a>Állítás

Mellett a `ID`, `IssueInstant` és `Version`, Azure Active Directory állítja be az alábbi elemeket a `Assertion` a válasz elem.

#### <a name="issuer"></a>Kibocsátó

A beállított `https://sts.windows.net/<TenantIDGUID>/`hol <TenantIDGUID> az Azure AD-bérlő bérlői azonosítója.

```
<Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

#### <a name="signature"></a>Aláírás

Azure AD a állítás válaszként sikeres bejelentkezéses ellátja. A `Signature` elem a digitális aláírás, amely a felhőalapú szolgáltatást használhatja hitelesítést végezni a forrás ellenőrizni a állítás integritását tartalmaz.

A digitális aláírás létrehozásához Azure Active Directory használja az aláírási kulcsot a `IDPSSODescriptor` a metaadat-dokumentum elemet.

```
<ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      digital_signature_here
    </ds:Signature>
```

#### <a name="subject"></a>Tárgy

Ez a adja meg a tőketörlesztés mértékét, amelyek tárgya: a állítás az utasításokat. Tartalmaz egy `NameID` elemet, amely a hitelesített felhasználó jelöli. A `NameID` értéke egy célzott azonosítót, amely csak a szolgáltató, amely a felhasználók, akik a token irányítja. Állandó – is vonni, de van soha nem rendeli. Érdemes emellett átlátszatlanná válik, abban, hogy nem jelenítse meg a felhasználó semmit, és nem használhatók fel azonosítóként attribútum lekérdezések.

A `Method` attribútuma a `SubjectConfirmation` elem értéke mindig `urn:oasis:names:tc:SAML:2.0:cm:bearer`.

```
<Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
</Subject>
```

#### <a name="conditions"></a>Feltételek

Ez az elem megadja a SAML Előfeltételek elfogadható felhasználása definiálása feltételeket.

```
<Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
</Conditions>
```

A `NotBefore` és `NotOnOrAfter` attribútumok adja meg az intervallumra, amely során az állítás érvényes.

- Értékét a `NotBefore` attribútuma megegyezik a, illetve a kissé (második kisebb, mint a) értékének később `IssueInstant` attribútuma a `Assertion` elemet. Azure Active Directory nem veszi figyelembe bármely időkülönbség magát, és a felhőalapú szolgáltatást (service provider) között, és nem adja hozzá minden olyan pufferelési ezúttal.
- Értékét a `NotOnOrAfter` attribútum 70 perccel később értékénél a `NotBefore` attribútum.

#### <a name="audience"></a>A célközönség

Ez a URI, amely azonosítja a célközönség tartalmaz. Azure Active Directory: Ez az elem értékének beállítása értékének `Issuer` eleme a `AuthnRequest` , hogy a bejelentkezés kezdeményezni. Ki szeretné számítani a `Audience` érték, használja az értéket a `App ID URI` alkalmazás regisztrációs közben megadott.

```
<AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
</AudienceRestriction>
```

Például a `Issuer` érték, a `Audience` érték pontosan egyeznie kell a szolgáltatásnevek, amely a felhőalapú szolgáltatást az Azure Active Directory közül. Jó helyen jár Ha értékét a `Issuer` elem nem URI érték, a `Audience` a kérdésre adott választ az érték a `Issuer` érték előtaggal együtt `spn:`.

#### <a name="attributestatement"></a>AttributeStatement

A tárgy vagy a felhasználó kapcsolatos igények tartalmaz. A következő kivonat tartalmazza mintaként `AttributeStatement` elemet. A három pontot, az azt jelzi, hogy az elemet is elhelyezhet, több attribútumok és attribútum értékek.

```
<AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
</AttributeStatement>
```     

- **Név állítást** : értékét a `Name` attribútum (`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`) a hitelesített felhasználó egyszerű felhasználónév például van `testuser@managedtenant.com`.
- **ObjectIdentifier állítást** : értékét a `ObjectIdentifier` attribútum (`http://schemas.microsoft.com/identity/claims/objectidentifier`) van a `ObjectId` a, amely a hitelesített felhasználó az Azure Active Directory-objektum. `ObjectId`egy megváltoztatható globálisan egyedi, és újra felhasználhat a hitelesített felhasználó megbízható azonosítója.

#### <a name="authnstatement"></a>AuthnStatement

Ez az elem asserts, hogy egy adott módon az adott időpontban hitelesített állítás tárgyát.

- A `AuthnInstant` attribútum az Azure AD, amikor a felhasználó hitelesített idő határozza meg.
- A `AuthnContext` elemét adja meg az hitelesíti a felhasználói hitelesítés környezetben.

```
<AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
</AuthnStatement>
```
