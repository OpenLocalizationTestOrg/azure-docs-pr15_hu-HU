<properties
   pageTitle="Külső felhasználók jogkivonat formátum Azure Active Directory B2B együttműködési előzetes verzió |} Microsoft Azure"
   description="Azure Active Directory B2B támogatja a vállalatot érintő kapcsolatok, mivel az üzleti partnereket a vállalati alkalmazások szelektív eléréséhez"
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

# <a name="azure-ad-b2b-collaboration-preview-external-user-token-format"></a>Azure Active Directory B2B együttműködési előzetes verzió: külső felhasználók jogkivonat formázása

A jogcímalapú egy szabványos Azure AD-jogkivonat megkötésekről [jogkivonat támogatott és a felelős típusok](active-directory-token-and-claims.md) című témakörben a azure.microsoft.com.

A követelések, amelyek különböző hitelesített B2B együttműködési külső felhasználó a következők:<br/>
- **Objektumazonosító:** az erőforrás-bérlőből Objektumazonosító<br/>
- **TID**: bérlői azonosító az erőforrás-bérlőből<br/>
- **Kibocsátó**: Ez az, hogy az erőforrás bérlői webhely<br/>
- **IDP**: Ez az otthoni a bérlői webhely, a felhasználó<br/>
- **AltSecId**: átlátszatlanná válik, amely alternatív biztonsági azonosító: az<br/>

## <a name="related-articles"></a>Kapcsolódó cikkek
Tallózással keresse meg a többi cikkek az Azure Active Directory B2B együttműködési:

- [Mi az Azure Active Directory B2B együttműködési?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Működése](active-directory-b2b-how-it-works.md)
- [Részletes útmutató](active-directory-b2b-detailed-walkthrough.md)
- [CSV-fájl formázása hivatkozás](active-directory-b2b-references-csv-file-format.md)
- [Külső felhasználók objektum attribútum megváltozik](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Aktuális előzetes korlátai](active-directory-b2b-current-preview-limitations.md)
- [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
