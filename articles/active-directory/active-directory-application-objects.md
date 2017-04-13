<properties
pageTitle="Azure Active Directory-alkalmazás és a szolgáltatás egyszerű objektumok |} Microsoft Azure"
description="Az alkalmazás és a szolgáltatás fő objektumok az Azure Active Directory közötti kapcsolatra vitafórumban"
documentationCenter="dev-center-name"
authors="bryanla"
manager="mbaldwin"
services="active-directory"
editor=""/>

<tags
ms.service="active-directory"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="identity"
ms.date="08/10/2016"
ms.author="bryanla;mbaldwin"/>

# <a name="application-and-service-principal-objects-in-azure-active-directory"></a>Alkalmazás és a szolgáltatás fő objektumok az Azure Active Directory
Tudnivalók az Azure Active Directory (AD) "alkalmazás" olvasásakor még nem mindig törlése pontosan mi van hivatkozott a szerző. Ez a cikk az a célja, hogy világosabb is, hogy az Azure Active Directory alkalmazás integráció, a regisztráció és a [több elem bérlői alkalmazás](active-directory-dev-glossary.md#multi-tenant-application)hozzájárulása példa elvi és konkrét szempontjait megadásával.

## <a name="overview"></a>– Áttekintés
Az Azure Active Directory-alkalmazások szélesebb, mint a csak a bábu szoftver. Elvi kifejezés, nem, csak az alkalmazás, hanem a regisztráció hivatkozik majd kerül (más néven: identitás konfigurációja) az Azure Active Directory, amely lehetővé teszi, hogy részt vehetnek az azon hitelesítési és engedélyezési "témák" futásidőben. Definíció szerint az alkalmazás a egy [ügyfél](active-directory-dev-glossary.md#client-application) szerepkör (használata más erőforrás), az [erőforrás a kiszolgálói](active-directory-dev-glossary.md#resource-server) szerepkörök (ügyfelek API közzéteszi) vagy akár mindkettőt is működik. A beszélgetés protokoll egy [OAuth 2.0-s engedély megadása a folyamat](active-directory-dev-glossary.md#authorization-grant), az access/erőforrás adatok védelme a kurzor az ügyfél erőforrás lehetővé teszi a cél határozza meg. Most már most nyissa mélyebb szintet, és hogyan a az Azure Active Directory alkalmazásmodell jelöli az alkalmazások belső. 

## <a name="application-registration"></a>Alkalmazás regisztráció
Amikor regisztrál az alkalmazás az [Azure klasszikus portál][AZURE-Classic-Portal], két objektumok a Azure AD-bérlő jönnek létre: alkalmazás objektum, és egy egyszerű-objektum.

#### <a name="application-object"></a>Objektum
Az Azure Active Directory-alkalmazások *definiált* annak az egyik, és csak alkalmazás objektum, amely a hol az alkalmazás regisztrálva lett Azure AD-bérlő található, a továbbiakban: az alkalmazás "otthoni" bérlői. Az alkalmazás objektum identitás kapcsolatos információt az alkalmazás, és a sablont, ahonnan a megfelelő szolgáltatás fő objektum(ok) *származtatott* futásidőben használatra. 

Érdemes elképzelnie, az alkalmazás (az összes bérlők között kell használni) *globális* ábrázolása az alkalmazás és a szolgáltatás egyszerű, mint a *helyi* megjelenítését (használható egy adott bérlői webhelyen). Az Azure Active Directory Graph [Alkalmazás egyed] [ AAD-Graph-App-Entity] alkalmazás objektum sémájának határozza meg. Alkalmazás objektum, ezért a szoftveralkalmazás és egy 1-1:1 kapcsolatban van: a megfelelő *n* szolgáltatás fő objektum(ok)*n* kapcsolatot.

#### <a name="service-principal-object"></a>Szolgáltatás fő objektum
A szolgáltatás fő objektum házirend és az alkalmazások alapjául szolgáló egy rendszerbiztonsági tag generáló futásidőben erőforrások elérésekor nyújtó engedélyeinek határozza meg. Az Azure Active Directory Graph [ServicePrincipal entitás] [ AAD-Graph-Sp-Entity] határozza meg a szolgáltatás fő objektum sémában. 

Egy egyszerű-objektum minden a bérlőhöz, amelynek az alkalmazás használatát egy példányának meg kell jelennie, azt a felhasználói fiókok tulajdonában erőforrásokhoz biztonságos hozzáférés szükséges. Egy egyetlen-bérlői alkalmazás lesz csak egy egyszerű (az otthoni bérlő esetében). Több elem bérlő [webalkalmazás](active-directory-dev-glossary.md#web-client) az egyes bérlői hol felhasználó(k) az adott bérlői rendszergazdának vagy hogy kifejezett engedélyét adta, amely lehetővé teszi, hogy azok forrásokat is egy egyszerű. Következő hozzájárulása az szolgáltatás fő objektum fog jövőbeli hitelesítési kérések véleményét. 

> [AZURE.NOTE] Az alkalmazás objektum végzett módosítások is megjelennek az alkalmazás kezdőlap-ös bérlői csak (a bérlő hol történt a regisztrálva) szolgáltatás alapvető célja. Több elem bérlői alkalmazások az alkalmazás objektum módosításai nem jelennek bármely fogyasztói bérlők szolgáltatás fő objektumok, mindaddig, amíg a fogyasztói bérlő access eltávolítja, és újra hozzáférést biztosít.

## <a name="example"></a>Példa
A következő ábra bemutatja az alkalmazások alkalmazás objektum és a megfelelő közötti kapcsolat szolgáltatás fő objektumokat, kattintson a minta több bérlői alkalmazások **HR alkalmazás**nevű környezetében. Ebben az esetben három Azure AD-bérlők van: 

- **Kontraktor** – a bérlő, a vállalatot, amely a **HR alkalmazás** fejlesztett által használt
- **A Contoso** – a bérlő, a Contoso szervezet, vagyis egy fogyasztói **HR alkalmazás** által használt
- **A Fabrikam** – a bérlő, a Fabrikam is fogyasztása a **HR alkalmazás** szervezet által használt

![Alkalmazás objektum és egy egyszerű-objektum viszonya](./media/active-directory-application-objects/application-objects-relationship.png)

Az előző diagram lépés 1 az alkalmazás és a szolgáltatás fő objektumok létrehozásának az alkalmazás kezdőlap bérlői webhelyen.

A 2 Contoso és Fabrikam rendszergazdái végezhetik hozzájárulása, amikor egy egyszerű-objektum létrejön a cég Azure AD-ös bérlői, és a rendszergazda jogosultsággal a engedélyeket. Tartsa szem előtt, hogy a HR alkalmazás konfigurált/célja lehet hozzájárulása engedélyezése a felhasználók egyéni használatra.

A 3 a fogyasztói bérlők HR-alkalmazás (Contoso és Fabrikam) minden saját szolgáltatás fő objektum van. Minden egyes jelöl, amely az alkalmazás az engedélyek szabályozzák futásidőben egy példányának is használatuk átadni kívánt hozzájárult e a megfelelő rendszergazdája.

## <a name="next-steps"></a>Következő lépések
Az alkalmazások alkalmazás objektum az Azure Active Directory Graph API keresztül is elérhető, az OData- [Alkalmazás egyed] képvisel[AAD-Graph-App-Entity]

Az alkalmazások szolgáltatás fő objektum az Azure Active Directory Graph API keresztül is elérhető, az OData- [ServicePrincipal entitás] képvisel[AAD-Graph-Sp-Entity]



<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Classic-Portal]: https://manage.windowsazure.com