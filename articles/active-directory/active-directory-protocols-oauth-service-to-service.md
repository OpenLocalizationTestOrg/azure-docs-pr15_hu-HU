<properties
    pageTitle="Azure Active Directory szolgáltatás Auth OAuth2.0 használatával |} Microsoft Azure"
    description="Ez a cikk ismerteti a szolgáltatás hitelesítéssel OAuth2.0 ügyfél hitelesítő adatok támogatás munkafolyamat végrehajtásához használja a HTTP-üzenetek."
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

# <a name="service-to-service-calls-using-client-credentials"></a>Ügyfél hitelesítő adataival hívások szolgáltatás

Az OAuth 2.0 ügyfél hitelesítő adatok támogatás Flow lehetővé teszi, a saját hitelesítő adatok használata hitelesítést végezni, amikor egy felhasználó helyett egy másik webszolgáltatás hívása webszolgáltatás ( *bizalmas ügyfél*). Ebben az esetben az ügyfél nem általában középszintű webszolgáltatás, daemon szolgáltatás vagy webhelyet.

## <a name="client-credentials-grant-flow-diagram"></a>Ügyfél hitelesítő adatait adja meg az adatfolyam-diagram

Az alábbi ábra bemutatja, hogyan adja meg az ügyfél hitelesítő adatait az áramlás működése az Azure Active Directory (Azure Active Directory).

![OAuth2.0 ügyfél hitelesítő adatok támogatás továbbításához](media/active-directory-protocols-oauth-service-to-service/active-directory-protocols-oauth-client-credentials-grant-flow.jpg)

1. Az ügyfélalkalmazás hitelesíti az Azure Active Directory jogkivonat kiállítási végpontot, és egy hozzáférési jogkivonat kéri.
2. Az Azure Active Directory jogkivonat kiállítási végpontot az jogkivonat hibák.
3. A hozzáférési jogkivonat használják a védett erőforráshoz hitelesítést végezni.
4. A védett erőforrás adatainak a rendszer a webalkalmazás adja vissza.

## <a name="register-the-services-in-azure-ad"></a>A szolgáltatások regisztrálása az Azure Active Directory

A hívás és a befogadó szolgáltatást is regisztráljon az Azure Active Directory (Azure Active Directory). Részletes útmutatásért lásd: [hozzáadása, frissítése, és az alkalmazás eltávolítása](active-directory-integrating-applications.md#BKMK_Native)

## <a name="request-an-access-token"></a>Egy hozzáférési jogkivonat kérése

Kérjen egy hozzáférési jogkivonat, hozhatja létre a bérlői különleges HTTP POST Azure AD-végpontot.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

## <a name="service-to-service-access-token-request"></a>Szolgáltatás hozzáférési jogkivonat engedély kérése

A szolgáltatás hozzáférési jogkivonat engedély kérése a következő paraméterek tartalmazza.

| Paraméter | | Leírás |
|-----------|------|------------|
| response_type | szükséges | Itt adhatja meg a kért választípust. Az érték egy ügyfél hitelesítő adatok támogatási folyamat **client_credentials**kell lennie.|
| client_id | szükséges | Itt adhatja meg az Azure Active Directory ügyfél-azonosító a hívó webszolgáltatás. Az adatkezelési portálon Azure a hívó alkalmazás ügyfél-azonosító megkereséséhez kattintson **Az Active Directory**, kattintson a címtár, jelölje be az alkalmazás, és kattintson a **beállítás**.|
| client_secret | szükséges |  Adja meg az Azure Active Directory a hívó webszolgáltatás regisztrált egy billentyűt. Hozzon létre egy kulcsot, az adatkezelési portálon Azure, kattintson **Az Active Directory**, kattintson a címtárban, jelölje be az alkalmazás, és kattintson a **beállítás**. |
| erőforrás | szükséges | Írja be az alkalmazás azonosítója URI fogadó webszolgáltatás. Az Azure adatkezelési portálon az alkalmazás azonosítója URI megkereséséhez kattintson **Az Active Directory**, kattintson a címtár, kattintson az alkalmazásra, és kattintson a **beállítás**. |

## <a name="example"></a>Példa

A következő HTTP-bejegyzés-hozzáférési jogkivonat, a https://service.contoso.com/ webszolgáltatás kéri. A `client_id` azonosítja a webszolgáltatás, amely a hozzáférési jogkivonat kéri.

```
POST contoso.com/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=625bc9f6-3bf6-4b6d-94ba-e97cf07a22de&client_secret=qkDwDJlDfig2IpeuUZYKH1Wb8q1V0ju6sILxQQqhJ+s=&resource=https%3A%2F%2Fservice.contoso.com%2F
```

## <a name="service-to-service-access-token-response"></a>Szolgáltatás hozzáférési jogkivonat válasz

A siker választ az alábbi paraméterekkel JSON OAuth 2.0-s választ tartalmazza.

| Paraméter   | Leírás |
|-------------|-------------|
|access_token |A kért hozzáférési jogkivonat. A hívó webszolgáltatás a token segítségével a fogadó webszolgáltatás hitelesítést végezni. |
|access_type  | Jogkivonat típusa értékét jelöli. Az egyetlen típus, amely támogatja az Azure Active Directory **Bearer**. Bearer tokenek további információkért lásd a [OAuth 2.0-s engedélyezési keretrendszer: Bearer jogkivonat használatát (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt).
|expires_in   | Mennyi ideig a hozzáférési jogkivonat érvénytelen (másodpercben).|
|expires_on   |A hozzáférési jogkivonat lejártakor idő. A dátum megfelelője másodpercek számát a 1970-01-01T0:0:0Z UTC addig, amíg a lejárati idő. Ez az érték használatos gyorsítótárazott tokenek időtartama határozza meg. |
|erőforrás     | Az alkalmazás azonosítója URI fogadó webszolgáltatás. |

## <a name="example"></a>Példa

A következő példa bemutatja egy hozzáférési jogkivonat elküldése webszolgáltatásnak egy kérelemre sikeres választ.

```
{
"access_token":"eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0ODI2NywibmJmIjoxMzg4NDQ4MjY3LCJleHAiOjEzODg0NTIxNjcsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsInN1YiI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZS8iLCJhcHBpZCI6ImQxN2QxNWJjLWM1NzYtNDFlNS05MjdmLWRiNWYzMGRkNThmMSIsImFwcGlkYWNyIjoiMSJ9.aqtfJ7G37CpKV901Vm9sGiQhde0WMg6luYJR4wuNR2ffaQsVPPpKirM5rbc6o5CmW1OtmaAIdwDcL6i9ZT9ooIIicSRrjCYMYWHX08ip-tj-uWUihGztI02xKdWiycItpWiHxapQm0a8Ti1CWRjJghORC1B1-fah_yWx6Cjuf4QE8xJcu-ZHX0pVZNPX22PHYV5Km-vPTq2HtIqdboKyZy3Y4y3geOrRIFElZYoqjqSv5q9Jgtj5ERsNQIjefpyxW3EwPtFqMcDm4ebiAEpoEWRN4QYOMxnC9OUBeG9oLA0lTfmhgHLAtvJogJcYFzwngTsVo6HznsvPWy7UP3MINA",
"token_type":"Bearer",
"expires_in":"3599",
"expires_on":"1388452167",
"resource":"https://service.contoso.com/"
}
```

## <a name="see-also"></a>Lásd még:

* [OAuth 2.0-s az Azure Active Directory](active-directory-protocols-oauth-code.md)
