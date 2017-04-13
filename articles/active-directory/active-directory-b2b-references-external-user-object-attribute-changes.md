<properties
   pageTitle="Külső felhasználók objektum attribútum megváltozik Azure Active Directory B2B együttműködési előzetes verzió |} Microsoft Azure"
   description="Azure Active Directory B2B támogatja a vállalatot érintő kapcsolatok, mivel az üzleti partnereket, hogy a szelektív hozzáférés a vállalati alkalmazások"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-external-user-object-attribute-changes"></a>Azure Active Directory B2B együttműködési előzetes verzió: külső felhasználó objektum attribútum megváltozik

A user objektumban minden felhasználó egy Azure Active Directory jelöli. A user objektumban, az Azure Active Directory többször a különféle attribútumok változásait a B2B együttműködési szakaszainak meghívás-beváltásához továbbításához. A felhasználó objektumra, amely a partner felhasználó a címtárban van a változó attribútumok beváltása alkalommal, amikor a partner felhasználó a felkérés e-mailben hivatkozásra kattint. Kifejezetten:

- A **SignInName** és **AltSecId** attribútumok vannak töltve
- A felhasználó egyszerű felhasználóneve módosítja a **DisplayName** attribútum (user_fabrikam.com#EXT#@contoso.com) a bejelentkezési név(user@fabrikam.com)

Nyomon követése a következő attribútumok az Azure Active Directory segíthet a elhárításában, attól függetlenül, hogy egy partner felhasználó rendelkezik beváltott azok B2B együttműködési meghívót.

## <a name="related-articles"></a>Kapcsolódó cikkek
Tallózással keresse meg a többi cikkek az Azure Active Directory B2B együttműködési:

- [Mi az Azure Active Directory B2B együttműködési?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Működése](active-directory-b2b-how-it-works.md)
- [Részletes útmutató](active-directory-b2b-detailed-walkthrough.md)
- [CSV-fájl formázása hivatkozás](active-directory-b2b-references-csv-file-format.md)
- [Külső felhasználók jogkivonat formázása](active-directory-b2b-references-external-user-token-format.md)
- [Aktuális előzetes korlátai](active-directory-b2b-current-preview-limitations.md)
- [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
