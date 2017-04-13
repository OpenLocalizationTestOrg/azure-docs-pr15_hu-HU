<properties
   pageTitle="Az Azure Active Directory B2B együttműködéshez aktuális előzetes korlátozások |} Microsoft Azure"
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
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-current-preview-limitations"></a>Azure Active Directory B2B együttműködési előzetes: aktuális korlátozások megtekintése

- Többtényezős hitelesítés (MFA) nem támogatott a külső felhasználókat. Például ha Contoso MFA rendelkezik, de nem jelent a Partner szervezeti, majd Partner szervezeti felhasználók nem adható MFA B2B együttműködési keresztül.
- Felkérés lehetségesek csak keresztül CSV; egyes felkérés és API az access nem támogatottak.
- Azure Active Directory a globális rendszergazdák csak .csv fájlokat is feltölthet.
- Felkérés fogyasztói e-mail címét (például hotmail.com, Gmail.com vagy comcast.net) jelenleg nem támogatott.
- Külső felhasználók hozzáférését a helyszíni nem vizsgálni alkalmazásokat.
- Külső felhasználók nem automatikusan törlődnek a tényleges felhasználó törlésekor a címtárból.
- Terjesztési listák szóló meghívók nem támogatottak.
- Legfeljebb 2000 rekordok tölthetők CSV keresztül.

## <a name="related-articles"></a>Kapcsolódó cikkek
Tallózással keresse meg a többi cikkek az Azure Active Directory B2B együttműködési:

- [Mi az Azure Active Directory B2B együttműködési?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Működése](active-directory-b2b-how-it-works.md)
- [Részletes útmutató](active-directory-b2b-detailed-walkthrough.md)
- [CSV-fájl formázása hivatkozás](active-directory-b2b-references-csv-file-format.md)
- [Külső felhasználók jogkivonat formázása](active-directory-b2b-references-external-user-token-format.md)
- [Külső felhasználók objektum attribútum megváltozik](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
