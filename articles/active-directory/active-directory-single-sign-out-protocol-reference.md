<properties
    pageTitle="SAML Protocol Azure egyetlen kijelentkezés |} Microsoft Azure"
    description="Ez a cikk ismerteti az Azure Active Directory egyetlen Sign-Out SAML protokoll"
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


# <a name="single-sign-out-saml-protocol"></a>Egyetlen kijelentkezési SAML Protocol (protokoll)

Azure Active Directory (Azure Active Directory) támogatja a SAML 2.0-s webes böngésző egyetlen kijelentkezési profilhoz. Az egyetlen kijelentkezési működik helyesen, Azure Active Directory kell a metaadat-alapú URL-cím regisztrálásához alkalmazás regisztráció során. Azure AD a Kijelentkezés URL-cím és a felhőbeli szolgáltatástól aláírási kulcsát megszerzi a metaadatokat. Azure AD az aláírási kulcs használja az aláírás a bejövő LogoutRequest ellenőrzéséhez, és használja a LogoutURL után azok a Kijelentkezés irányítsa át a felhasználókat.

Ha a felhőbeli szolgáltatástól nem támogatja a metaadat-alapú végpont után az alkalmazások, a Fejlesztőeszközök kell ügyfélszolgálatát a Microsoft a Kijelentkezés URL-CÍMEK és az aláírási kulcs megadásához.

A ábrán a munkafolyamatot az Azure Active Directory egyetlen kijelentkezési folyamat.

![Munkafolyamat egyetlen kijelentkezés](media/active-directory-single-sign-out-protocol-reference/active-directory-saml-single-sign-out-workflow.png)

## <a name="logoutrequest"></a>LogoutRequest

A felhőalapú szolgáltatás elküldi a `LogoutRequest` Azure Active Directory üzenet jelzi, hogy befejeződött-e a munkamenetet. A következő kivonat megmutatja, `LogoutRequest` elemet.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### <a name="logoutrequest"></a>LogoutRequest

A `LogoutRequest` Azure Active Directory küldött elem van szükség a következő attribútumok:

- `ID`: Ez azonosítja a kijelentkezési kérelmet. Értékének `ID` nem kell kezdődnie egy számot. A szokásos gyakorlat **azonosító** hozzáfűzése GUID karakterlánc ábrázolása.

- `Version`: Ez az elem értékének beállítása a **2.0-s**. Ez az érték szükség.

- `IssueInstant`: Ez egy `DateTime` karakterlánc koordinálása világidő (UTC) szerint érték és [oda-vissza formátum ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD egy értéket az ilyen típusú vár, de nem kényszeríti.

- A `Consent`, `Destination`, `NotOnOrAfter` és `Reason` attribútumok figyelmen kívül hagyja, ha még nem szerepelnek a `LogoutRequest` elemet.

### <a name="issuer"></a>Kibocsátó

A `Issuer` eleme egy `LogoutRequest` pontosan egyeznie kell a **ServicePrincipalNames** a felhőszolgáltatásában közül az Azure Active Directory. A szokásos ez értékre van állítva az **Alkalmazás azonosítója URI** alkalmazás regisztráció során van megadva.

### <a name="nameid"></a>NameID

Értékét a `NameID` elem pontosan egyeznie kell a `NameID` annak a felhasználónak meg aláírással.
## <a name="logoutresponse"></a>LogoutResponse

Azure Active Directory küld egy `LogoutResponse` válaszként egy `LogoutRequest` elemet. A következő kivonat megmutatja, `LogoutResponse`.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### <a name="logoutresponse"></a>LogoutResponse

Azure Active Directory-készletek a `ID`, `Version` és `IssueInstant` szereplő értékek a `LogoutResponse` elemet. Akkor állítja be a `InResponseTo` értékének elem a `ID` attribútuma a `LogoutRequest` , amely megállapított a kérdésre adott választ.

### <a name="issuer"></a>Kibocsátó

Azure Active Directory állítja be ezt az értéket `https://login.microsoftonline.com/<TenantIdGUID>/` hol <TenantIdGUID> az Azure AD-bérlő bérlői azonosítója.

Értékének ki szeretné számítani a `Issuer` elem, használja az **Alkalmazás azonosítója URI** értékének megadott alkalmazás regisztráció során.

### <a name="status"></a>Állapot

Azure Active Directory használja a `StatusCode` elemet a `Status` , hogy a megoldás sikeres vagy sikertelen kijelentkezési elemet. Ha nem sikerül a kijelentkezési kísérletet, a `StatusCode` elem tartalmazhatja egyéni hibaüzenetek jelennek meg.
