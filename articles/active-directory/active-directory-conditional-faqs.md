<properties
    pageTitle="Azure Active Directory feltételes Access – gyakori kérdések |} Microsoft Azure"
    description="Feltételes access kapcsolatos gyakori kérdésekre "
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-conditional-access-faq"></a>Azure Active Directory feltételes Access – gyakori kérdések

## <a name="which-applications-work-with-conditional-access-policies"></a>Mely alkalmazások használata a feltételes hozzáférési?

**A:** Kérjük, olvassa el a [feltételes Mi az access alkalmazások használata támogatott](active-directory-conditional-access-supported-apps.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>Feltételes hozzáférési érvényesek B2B együttműködési és a vendégként való bekapcsolódáshoz felhasználók?

**A:** Házirendek akkor lépnek érvénybe, B2B együttműködési felhasználók számára. Azonban bizonyos esetekben a felhasználó előfordulhat, hogy nem tud szabálynak megfelelő házirend kötelező, ha például egy szervezet többtényezős hitelesítés nem támogatja. 

A házirend jelenleg nem érvényes SharePoint vendég felhasználók számára. A vendégként való bekapcsolódáshoz kapcsolat karbantartása SharePoint belül. A vendégként való bekapcsolódáshoz felhasználói fiókok nem vonatkoznak az access a hitelesítési kiszolgálón házirendeket. A SharePoint kezelheti a vendégként való hozzáférés.

## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>A SharePoint Online házirend is vonatkozik a OneDrive vállalati verzió?

**A:** igen.
 
## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a>Miért nem tudom beállítani a házirend ügyfél alkalmazások, például a Wordöt vagy az Outlook?

**A:** A feltételes hozzáférési házirend állítja be a szolgáltatások elérésére vonatkozó követelmények, és akkor lépnek érvénybe, ha a hitelesítési szolgáltatás történik. A házirend értéke nem közvetlenül az ügyfélalkalmazás; érvényesül helyette, amikor egy üzembe helyezés meghívja. A SharePoint-webhelyen beállított házirend vonatkozik ügyfelek meghívása a SharePoint és a csere beállított házirend vonatkozik Outlook.


## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>A feltételes hozzáférési házirend vonatkozik fiókok szolgáltatás?

**A:** Feltételes hozzáférési alkalmazása az összes felhasználói fiókok. Ide tartoznak a szolgáltatás partnerként használt felhasználói fiókokat. Sok esetben futtató felügyelet nélküli szolgáltatásfiók nem tudja felel meg a házirend. Ez akkor, például az esetben, ha MFA szükség. Ezekben az esetekben szolgáltatások fiókok is kell zárni a házirendet, feltételes hozzáférési házirend kezelési beállításainak használatával. További tudnivalók a házirend alkalmazása itt a felhasználók számára.
