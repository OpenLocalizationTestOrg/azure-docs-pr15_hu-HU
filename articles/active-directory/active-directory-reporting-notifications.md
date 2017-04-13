<properties
    pageTitle="Azure Active Directory jelentéskészítés értesítések"
    description="Gyanús bejelentkezési értesítések jelentéskészítés az Azure Active Directory használata mellől."
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/07/2016"
    ms.author="dhanyahk"/>

# <a name="azure-active-directory-reporting-notifications"></a>Azure Active Directory jelentéskészítés értesítések

## <a name="what-reports-generate-email-notifications"></a>Milyen jelentések készítése az értesítő e-mailek

Jelenleg csak a tevékenység jelentés indítók szabálytalan bejelentkezési e-mail értesítések.

## <a name="what-is-an-irregular-sign-in"></a>Mi az, hogy egy "szabálytalan bejelentkezés"?

Szabálytalan bejelentkezési bővítmények azok, amely a gépi tanulási algoritmusok, egy "lehetetlenné utazási" feltétel, egy anomalous bejelentkezési helyet és egy eszközt kombinálni alapján azonosította. Ez jelezheti, hogy egy támadó próbálkozott jelentkezzen be a fiókkal.

## <a name="who-receives-the-email-notifications"></a>Az e-mailben értesítést kap ki?

Az összes globális rendszergazdái, aki az Active Directory prémium licenccel rendelkezik az e-mailt küldi. Annak érdekében, hogy érkeznek meg, hogy küldje el a rendszergazdák másodlagos E-mail címét, valamint. A rendszergazdák tartalmaznia kell aad-alerts-noreply@mail.windowsazure.com a a megbízható feladók listájához, hogy ne maradjak le az e-mailt.

## <a name="how-often-are-these-emails-sent"></a>Milyen gyakran vannak küldeni az e-maileket?

Az e-mailt küldi el, ha az elmúlt 30 nap 10 új szabálytalan bejelentkezési tevékenységek fordul elő, vagy az utolsó e-mailt küldött, mivel attól kisebb.

## <a name="how-do-i-access-the-report-mentioned-in-the-email"></a>Hogyan férhetek hozzá az e-mailek említett jelentés?

Amikor a hivatkozásra kattint, átirányítja a jelentéslapra belül az Azure klasszikus portálon. A jelentés eléréséhez kell lennie mindkettőt:

- Egy rendszergazda vagy a további-rendszergazda az Azure előfizetése

- A globális rendszergazdája a címtárban, és az Active Directory prémium licenc kiosztott. További tudnivalókért lásd: az [Azure Active Directory kiadásai](active-directory-editions.md).

## <a name="can-i-turn-off-these-emails"></a>Ki lehet kapcsolni az e-maileket?

Igen, anomalous bejelentkezési bővítmények belül az Azure klasszikus portál kapcsolatos értesítések kikapcsolásához kattintson a **Konfigurálás**gombra, és válassza a **Letiltva** lehetőséget az **értesítések** szakaszban.

## <a name="whats-next"></a>Következő lépések
- Kíváncsi, hogy milyen biztonsági, a naplózási és a tevékenység állnak rendelkezésre? Nézze meg az [Azure Active Directory biztonsági, a naplózási, és a tevékenység jelentések](active-directory-view-access-usage-reports.md)
- [Azure Active Directory prémium verzió első lépések](active-directory-get-started-premium.md)
- [A vállalat arculati elemek a Sign In és az Access Panel lapok hozzáadása](active-directory-add-company-branding.md)
