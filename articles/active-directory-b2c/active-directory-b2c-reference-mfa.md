<properties
    pageTitle="Azure Active Directory B2C: Többtényezős hitelesítés |} Microsoft Azure"
    description="Többtényezős hitelesítés engedélyezése az Azure Active Directory B2C által biztosított fogyasztói elérésű-alkalmazásokban"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-enable-multi-factor-authentication-in-your-consumer-facing-applications"></a>Azure Active Directory B2C: Többtényezős hitelesítés engedélyezése a fogyasztói elérésű alkalmazásokban

Azure Active Directory (Azure Active Directory) B2C integrálódik közvetlenül [Azure többtényezős hitelesítés](../multi-factor-authentication/multi-factor-authentication.md) , hogy vehet egy második biztonsági szint előfizetési és a bejelentkezési élményt a fogyasztói elérésű alkalmazásokban. És az egysoros kód írása nélkül teheti meg. Jelenleg támogatjuk telefonnal intézett hívás és a szöveges üzenet ellenőrzése. Ha már létrehozott előfizetési és a bejelentkezési házirendeket, akkor is engedélyezheti a többtényezős hitelesítés.

> [AZURE.NOTE]
Többtényezős hitelesítés is engedélyezhető előfizetési és a bejelentkezési házirendeket, nem csak a meglévő házirendek módosításával létrehozásakor.

Ez a szolgáltatás segítségével az alkalmazásokat, például a következő esetekben kezelheti:

- Többtényezős hitelesítés egy alkalmazás eléréséhez nem szükséges, de azt egy másikra eléréséhez szükséges. Ha például a fogyasztói alkalmazásba automatikus biztosítási közösségi vagy helyi fiókot, de a telefonszámát ellenőriznie kell, mielőtt szeretne hozzáférni az otthoni biztosítási alkalmazás regisztrált ugyanabban a mappában.
- Többtényezős hitelesítés alkalmazás általános eléréséhez nem szükséges, de van szüksége, benne a bizalmas részeket eléréséhez. Például a fogyasztói banki alkalmazás egy közösségi vagy helyi fiók és az ellenőrzés számla egyenlege, de a vezetékes átadás előtt ellenőriznie kell a telefonszámot.

## <a name="modify-your-sign-up-policy-to-enable-multi-factor-authentication"></a>Többtényezős hitelesítés engedélyezése az előfizetési házirend módosítása

1. [Kövesse ezeket a lépéseket követve nyissa meg azt a B2C szolgáltatások lap az Azure-portálon](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kattintson az **előfizetés házirendek**.
3. Kattintson a regisztráció házirendre (például "B2C_1_SiUp") a megnyitáshoz.
4. **Többtényezős hitelesítés** gombra, és kapcsolja **be**az **állapot** . Kattintson az **OK gombra**.
5. Kattintson a **Mentés** elemre a lap tetején.

A "Most futtatni" funkció ellenőrizze a fogyasztói felület felhasználhatja a házirend. Ellenőrizze a következőket:

A fogyasztói fiók jön létre a címtárában előtt a többtényezős hitelesítés lépés fordul elő. Lépés során az ügyfél van arra kéri, adja meg az illető weblapját telefonszámát, és ellenőrizze, hogy azt. Sikeres igazolás esetén a telefonszám csatolva van a fogyasztói fiók későbbi felhasználás céljából. Akkor is, ha a fogyasztói megszakítja, vagy esik, akkor hatékonyabbnak is kérni telefonszám ellenőrizze ismét során a következő bejelentkezés (ha az engedélyezve többtényezős hitelesítés).

## <a name="modify-your-sign-in-policy-to-enable-multi-factor-authentication"></a>Többtényezős hitelesítés engedélyezése a bejelentkezési házirend módosítása

1. [Kövesse ezeket a lépéseket követve nyissa meg azt a B2C szolgáltatások lap az Azure-portálon](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kattintson a **Sign in házirendek**.
3. Kattintson a bejelentkezés irányelvek (például "B2C_1_SiIn") a megnyitáshoz. Kattintson a lap tetején a **szerkesztése** elemre.
4. **Többtényezős hitelesítés** gombra, és kapcsolja **be**az **állapot** . Kattintson az **OK gombra**.
5. Kattintson a **Mentés** elemre a lap tetején.

A "Most futtatni" funkció ellenőrizze a fogyasztói felület felhasználhatja a házirend. Ellenőrizze a következőket:

Amikor a felhasználó bejelentkezik (közösségi vagy helyi fiókot használ), ha igazolt telefonszám csatolva van a fogyasztói fiókot, akkor hatékonyabbnak meg kell adnia megerősítéséhez. Ha nincs telefonszám be van kapcsolva, a fogyasztói egyet, és ellenőrizze, hogy kéri a program. A sikeres igazolás a telefonszám csatolva van a fogyasztói fiók későbbi felhasználás céljából.

## <a name="multi-factor-authentication-on-other-policies"></a>Többtényezős hitelesítés más házirendek

A fenti előfizetési és a bejelentkezési házirendek leírtak ahhoz, hogy a regisztrációs többtényezős hitelesítés lehetőség arra is, vagy bejelentkezési házirendek és a jelszó alaphelyzetbe állítása házirendek. Legyen elérhető, amint a felhasználóiprofil-házirendek szerkesztése.
