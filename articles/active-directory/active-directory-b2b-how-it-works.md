<properties
   pageTitle="Azure Active Directory B2B együttműködési előzetes: hogyan működik |} Microsoft Azure"
   description="Ismerteti, hogyan Azure Active Directory B2B együttműködési támogatja, mivel az üzleti partnereket a vállalati alkalmazások szelektív eléréséhez a vállalatot érintő kapcsolatok"
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

# <a name="azure-ad-b2b-collaboration-preview-how-it-works"></a>Azure Active Directory B2B együttműködési előzetes: hogyan működik
Azure Active Directory B2B együttműködési-meghívás alapul, és be kell váltania modell. Megadhatja a használni kívánt alkalmazások együtt dolgozni szeretne felek e-mail címét. Azure Active Directory küldjön nekik hivatkozást egy e-mailek meghívás. A partner felhasználó követi a hivatkozást, és döntheti el, hogy jelentkezzen be az Azure Active Directory-fiókkal vagy bejelentkezési fiók egy új Azure Active Directory tagjának.

1. A rendszergazda által [a strukturált csv-fájlból](active-directory-b2b-references-csv-file-format.md) az Azure portálon feltöltése felkéri a felhasználók a partnert.
2. A portál küld e-mailek, a partner felhasználók meghívása.
3. A partner felhasználókat az e-mailben lévő hivatkozásra, és a program kéri, jelentkezzen be a munkahelyi hitelesítő adatait (Ha már Azure Active Directory legyenek), illetve az Azure Active Directory B2B együttműködési felhasználóként regisztráció.
4. Az alkalmazás felkérték, ahol most már hozzáféréssel rendelkeznek a rendszer átirányítja partner felhasználókat.

## <a name="directory-operations"></a>A címtár-műveletek
Partner felhasználók szerepel az Azure Active Directory külső felhasználóként. Ez azt jelenti, a rendszergazda kiépítése licencek, csoporttagság hozzárendelése és az Azure-portálra vagy Azure PowerShell csak, például a felhasználók a vállalat keresztül vállalati alkalmazások további hozzáférést.

Miközben egy fizetett Azure AD előfizetést (Basic vagy prémium verzióban) már nem szükséges Azure Active Directory B2B, akik fizetett bérlők használata Azure AD-előfizetés (Basic vagy prémium verzióban) további előnyökhöz juthat a következő:

 - Rendszergazdák oszthatnak csoportok alkalmazásokat, hogy a meghívott felhasználói hozzáférés kezelése egyszerűbb kezeléséről.
 - Rendszergazdai bérlői védjegyzés márkajegyeket helyezhet el a felkérés e-mailek szolgál, és a visszaváltás változat, további környezet biztosítása meghívott felhasználók a partnert.

## <a name="related-articles"></a>Kapcsolódó cikkek
 Keresse meg a többi cikkek az Azure Active Directory B2B együttműködési

 - [Mi az Azure Active Directory B2B együttműködési?](active-directory-b2b-what-is-azure-ad-b2b.md)
 - [Részletes útmutató](active-directory-b2b-detailed-walkthrough.md)
 - [CSV-fájl formázása hivatkozás](active-directory-b2b-references-csv-file-format.md)
 - [Külső felhasználók jogkivonat formázása](active-directory-b2b-references-external-user-token-format.md)
 - [Külső felhasználók objektum attribútum megváltozik](active-directory-b2b-references-external-user-object-attribute-changes.md)
 - [Aktuális előzetes korlátai](active-directory-b2b-current-preview-limitations.md)
 - [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
