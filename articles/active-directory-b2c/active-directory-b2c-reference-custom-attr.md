<properties
    pageTitle="Azure Active Directory B2C: Az egyéni attribútumok |} Microsoft Azure"
    description="Hogyan Azure Active Directory B2C egyéni attribútumok segítségével a fogyasztói vonatkozó információk gyűjtését"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

#  <a name="azure-active-directory-b2c-use-custom-attributes-to-collect-information-about-your-consumers"></a>Azure Active Directory B2C: Az egyéni attribútumok segítségével a fogyasztói vonatkozó információk gyűjtését

Azure Active Directory (Azure Active Directory) B2C címtárában megtalálható egy beépített készletre információk (attribútumok): utónév, Vezetéknév, City, irányítószám és más attribútumait. Minden fogyasztói elérésű alkalmazásnak azonban fogyasztói gyűjtése meg, hogy milyen attribútumokkal egyedi követelményeinek. Az Azure Active Directory B2C bővítheti az egyes fogyasztói fiókjában tárolt attribútumokat. Egyéni attribútumok létrehozása az [Azure-portálra](https://portal.azure.com/) , és használhatja a regisztrációs házirendek alább látható módon. Is olvasása és írása a következő attribútumok az [Azure Active Directory Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md)segítségével.

> [AZURE.NOTE]
Egyéni attribútumok [Azure Active Directory Graph API címtár séma bővítmények](https://msdn.microsoft.com/library/azure/dn720459.aspx)használata.

## <a name="create-a-custom-attribute"></a>Hozzon létre egy egyéni attribútum

1. [Kövesse ezeket a lépéseket követve nyissa meg azt a B2C szolgáltatások lap az Azure portálon](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kattintson a **felhasználó attribútumok**elemre.
3. Válassza a **+ Hozzáadás** a lap tetején.
4. Adjon **nevet** az egyéni attribútum (például "ShoeSize"), és tetszés szerint egy **leírást**. Kattintson a **létrehozása**gombra.

    > [AZURE.NOTE]
    A problémának jelenleg csak az "Karakterlánc" **Adattípust** .

Az egyéni attribútum érhető el a lista **felhasználói**attribútumok és a regisztrációs házirendek használatban.

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a>A regisztráció irányelvek egy egyéni attribútum használata

1. [Kövesse ezeket a lépéseket követve nyissa meg azt a B2C szolgáltatások lap az Azure portálon](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kattintson a **regisztráció irányelvei**.
3. Kattintson a regisztráció házirendre (például "B2C_1_SiUp") a megnyitáshoz. Kattintson a lap tetején a **szerkesztése** elemre.
4. Kattintson az **előfizetés attribútumok** elemre, és válassza az egyéni attribútum (például "ShoeSize"). Kattintson az **OK gombra**.
5. Kattintson az **alkalmazás követelések** , és válassza ki az egyéni attribútumot. Kattintson az **OK gombra**.
6. Kattintson a **Mentés** elemre a lap tetején.

A "Most futtatni" funkció ellenőrizze a fogyasztói felület felhasználhatja a házirend. Meg kell látni fogom "ShoeSize" a fogyasztói előfizetési során gyűjtött attribútumok listájában, és vissza az alkalmazás küldött jogkivonat-ben.

## <a name="notes"></a>Jegyzetek

- Regisztráció házirendek együtt egyéni attribútumok is használható regisztráció vagy bejelentkezés házirendeket és a felhasználóiprofil-házirendek szerkesztése.
- Egyéni attribútumok ismert korlátozása van. Csak az első alkalommal használja bármely házirend, és nem vesz fel azt a **felhasználói attribútumok**listáját létrehozása.
