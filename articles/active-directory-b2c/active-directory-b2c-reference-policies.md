<properties
    pageTitle="Azure Active Directory B2C: Bővíthető házirend keretrendszer |} Microsoft Azure"
    description="A témakör az Azure Active Directory B2C bővíthető házirend keretén és a különféle házirend létrehozása"
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

# <a name="azure-active-directory-b2c-extensible-policy-framework"></a>Azure Active Directory B2C: Extensible keretet

## <a name="the-basics"></a>Az alapvető műveletei

Azure Active Directory (Azure Active Directory) B2C bővíthető házirend keretében a szolgáltatás core erőssége. Házirendek teljesen ismertetik a fogyasztói identitás szolgáltatások, például előfizetési, bejelentkezési vagy a profil szerkesztése. Például egy előfizetési házirenddel viselkedése szabályozhatja az alábbi beállítások konfigurálásával:

- Fióktípusok (közösségi fiókból, például a Facebook vagy e-mail címét például helyi fiókok) fogyasztói használható az alkalmazás regisztrálhat.
- Attribútumokat (például keresztnév, irányítószám és cipőméreten) előfizetési során az ügyféltől gyűjtendő.
- Többtényezős hitelesítés használata.
- A Megjelenés és a hangulat előfizetési oldalát.
- Információ (amely akkor jelentkezik, mint egy jogkivonat jogcímeken), hogy az alkalmazás megkapja mikor a házirendet, futtassa a befejeződik.

Különböző típusú több házirendek létrehozása a bérlői webhelyén, és szükség szerint használhatja őket az alkalmazásokban. Házirendek alkalmazásairól felhasználhassa őket. Ez lehetővé teszi, hogy a fejlesztők meghatározása és a fogyasztói identitás-elemeket a minimális vagy nincs változtatás kódjukat módosításához.

Szabályok használata egy egyszerű Fejlesztőeszközök felületén keresztül érhetők el. Az alkalmazás elindítja a házirendet a szabványos HTTP-hitelesítés kérelmek (házirend paraméter átadása a kérelem), és a testre szabott jogkivonat válaszként kap. Például csak különbségét kéri, hogy egy előfizetési házirend meghívását, és azokat a bejelentkezési házirend meghívása a szabály nevét a "p" karakterlánc lekérdezésparaméter:

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

Ha többet szeretne tudni a házirend keretrendszer olvassa el a [blogbejegyzésből](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx)című témakört.

## <a name="create-a-sign-up-policy"></a>Regisztráció házirend létrehozása

Ahhoz, hogy az alkalmazás a regisztrációs, szüksége lesz egy előfizetési házirend létrehozása. Ezzel a házirenddel ismerteti, hogy fogyasztói fog előfizetési során a változat és tartalmát, az alkalmazás fog kapni a sikeres címlistasablon tokenek.

1. [Kövesse ezeket a lépéseket követve nyissa meg azt a B2C szolgáltatások lap az Azure portálon](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kattintson az **előfizetés házirendek**.
3. Válassza a **+ Hozzáadás** a lap tetején.
4. A **név** egy előfizetési házirend nevére, az alkalmazás által használt határozza meg. Írja be például a "SiUp".
5. Kattintson a **Identitásszolgáltatók** , és válassza a "Regisztrációs E-mail". Tetszés szerint kijelölhet is közösségi Identitásszolgáltatók, ha már be van állítva. Kattintson az **OK gombra**.
6. Kattintson a **regisztráció attribútumok**elemre. Itt választhatja ki, amely során az ügyféltől gyűjtendő attribútumok. Jelölje be például a "Országot/régiót", "Megjelenítendő név" és "Irányítószám". Kattintson az **OK gombra**.
7. Kattintson az **alkalmazás követelések**. Itt választhatja ki, amelyet a tokenek küldeni az alkalmazás a sikeres-előfizetési használhatóság után a visszaadott követelések. Jelölje be például a "Megjelenítendő neve", "Identitásszolgáltató", "Irányítószám", "Felhasználó" és "Felhasználó Objektumazonosító".
8. Kattintson a **létrehozása**gombra. Látható, hogy az imént létrehozott csoportházirend jelenik meg, mint "**B2C_1_SiUp**" (a **B2C\_1\_ ** fragment automatikusan felkerül) a **regisztrációs házirendek** lap a.
9. Nyissa meg a házirend "**B2C_1_SiUp**" gombra kattintva.
10. Select "Contoso B2C alkalmazás" az **alkalmazások** legördülő és `https://localhost:44321/` a a **Válasz URL-cím / átirányítása URI** legördülő listában.
11. Kattintson a **Futtatás**gombra. Egy új böngészőlapon megnyílik, és a fogyasztói élményét fizet elő, az alkalmazás segítségével futtathatja.

    > [AZURE.NOTE]
    Foglal házirend létrehozása és frissítések perccé alakít csak akkor lépnek érvénybe.

## <a name="create-a-sign-in-policy"></a>Bejelentkezési házirend létrehozása

Bejelentkezés az alkalmazás a engedélyezéséhez szüksége lesz bejelentkezési házirend létrehozása. Ezzel a házirenddel a változat, hogy fogyasztói fog bejelentkezési, illetve az, hogy az alkalmazás kap tokenek tartalmát a sikeres bejelentkezési bővítmények ismerteti.

1. [Kövesse ezeket a lépéseket követve nyissa meg azt a B2C szolgáltatások lap az Azure portálon](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kattintson a **Sign in házirendek**.
3. Válassza a **+ Hozzáadás** a lap tetején.
4. A **név** határozza meg, hogy az alkalmazás által használt bejelentkezési házirend neve. Írja be például a "SiIn".
5. Kattintson a **Identitásszolgáltatók** , és válassza ki a "Helyi fiók – a bejelentkezési". Tetszés szerint kijelölhet is közösségi Identitásszolgáltatók, ha már be van állítva. Kattintson az **OK gombra**.
6. Kattintson az **alkalmazás követelések**. Itt választhatja ki, amelyet a tokenek küldött után egy sikeres bejelentkezés módja az alkalmazás a visszaadott követelések. Jelölje be például a "Megjelenítendő neve", "Identitásszolgáltató", "Irányítószám" és "Felhasználó Objektumazonosító". Kattintson az **OK gombra**.
7. Kattintson a **létrehozása**gombra. Látható, hogy az imént létrehozott csoportházirend jelenik meg, mint "**B2C_1_SiIn**" (a **B2C\_1\_ ** fragment automatikusan felkerül) be a **bejelentkezési házirendek** lap.
8. Nyissa meg a házirendet "**B2C_1_SiIn**" gombra kattintva.
9. Select "Contoso B2C alkalmazás" **alkalmazások** legördülő és `https://localhost:44321/` a a **Válasz URL-cím / átirányítás URI** legördülő listában.
10. Kattintson a **Futtatás**gombra. Egy új böngészőlapon megnyílik, és a bejelentkezés az alkalmazásba a fogyasztói felület segítségével futtathatja.

    > [AZURE.NOTE]
    Foglal házirend létrehozása és frissítések perccé alakít csak akkor lépnek érvénybe.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Regisztráció vagy bejelentkezés házirend létrehozása

Ezzel a házirenddel mindkét fogyasztói előfizetési és a bejelentkezési élményt az egyetlen beállításokkal kezeli. Fogyasztói vannak vezetett le a megfelelő elérési utat (regisztráció vagy bejelentkezés) a környezettől függően. Bemutatja, hogy az alkalmazás fog kapni a sikeres címlistasablon vagy bejelentkezések tokenek tartalmát is.  A regisztráció vagy bejelentkezés házirend kód minta [elérhető további lehetőségek](active-directory-b2c-devquickstarts-web-dotnet-susi.md).

1. [Kövesse ezeket a lépéseket követve nyissa meg azt a B2C szolgáltatások lap az Azure portálon](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kattintson a **regisztráció vagy bejelentkezés házirendek**.
3. Válassza a **+ Hozzáadás** a lap tetején.
4. A **név** egy előfizetési házirend nevére, az alkalmazás által használt határozza meg. Írja be például a "SiUpIn".
5. Kattintson a **Identitásszolgáltatók** , és válassza a "Regisztrációs E-mail". Tetszés szerint kijelölhet is közösségi Identitásszolgáltatók, ha már be van állítva. Kattintson az **OK gombra**.
6. Kattintson a **regisztráció attribútumok**elemre. Itt választhatja ki, amely során az ügyféltől gyűjtendő attribútumok. Jelölje be például a "Országot/régiót", "Megjelenítendő név" és "Irányítószám". Kattintson az **OK gombra**.
7. Kattintson az **alkalmazás követelések**. Itt választhatja ki, amelyet a tokenek küldeni az alkalmazás sikeres regisztráció vagy bejelentkezés élmény után a visszaadott követelések. Jelölje be például a "Megjelenítendő neve", "Identitásszolgáltató", "Irányítószám", "Felhasználó" és "Felhasználó Objektumazonosító".
8. Kattintson a **létrehozása**gombra. Látható, hogy az imént létrehozott csoportházirend jelenik meg, mint "**B2C_1_SiUpIn**" (a **B2C\_1\_ ** fragment automatikusan felkerül) kattintson a **regisztráció vagy bejelentkezés házirendek** lap.
9. Nyissa meg a házirendet "**B2C_1_SiUpIn**" gombra kattintva.
10. Select "Contoso B2C alkalmazás" az **alkalmazások** legördülő és `https://localhost:44321/` a a **Válasz URL-cím / átirányítása URI** legördülő listában.
11. Kattintson a **Futtatás**gombra. Egy új böngészőlapon megnyílik, és segítségével futtathatja a regisztráció vagy bejelentkezés fogyasztói felület beállítva.

    > [AZURE.NOTE]
    Foglal házirend létrehozása és frissítések perccé alakít érvénybelépéséhez.

## <a name="create-a-profile-editing-policy"></a>Hozzon létre egy profilt házirend szerkesztése

Ahhoz, hogy a profil szerkesztése az alkalmazásban, meg kell házirend szerkesztése profil létrehozása. Ezzel a házirenddel a változat, hogy fogyasztói fog profil szerkesztése, illetve az, hogy az alkalmazás fog kapni a sikeres befejezésétől tokenek tartalmát mutatja be.

1. [Kövesse ezeket a lépéseket követve nyissa meg azt a B2C szolgáltatások lap az Azure portálon](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kattintson a **Profil szerkesztése házirendeket**.
3. Válassza a **+ Hozzáadás** a lap tetején.
4. A **név** határozza meg, hogy a profil szerkesztése az alkalmazás által használt házirend neve. Írja be például a "SiPe".
5. Kattintson a **Identitásszolgáltatók** , és válassza a "E-mail címe". Tetszés szerint kijelölhet is közösségi Identitásszolgáltatók, ha már be van állítva. Kattintson az **OK gombra**.
6. Kattintson a **profil tulajdonságait**. Itt választhatja ki, hogy a felhasználó megtekintheti és szerkesztheti attribútumok. Jelölje be például a "Országot/régiót", "Megjelenítendő név" és "Irányítószám". Kattintson az **OK gombra**.
7. Kattintson az **alkalmazás követelések**. Itt választhatja ki, amelyet a tokenek küldeni az alkalmazás a sikeres profil élmény szerkesztése után a visszaadott követelések. Jelölje be például a "Megjelenítendő név" és "Irányítószám".
8. Kattintson a **létrehozása**gombra. Észreveheti, hogy az imént létrehozott csoportházirend jelenik meg, mint "**B2C_1_SiPe**" (a **B2C\_1\_ ** fragment automatikusan felkerül) kattintson a **Profil szerkesztése házirendek** lap.
9. Nyissa meg a házirend "**B2C_1_SiPe**" gombra kattintva.
10. Select "Contoso B2C alkalmazás" az **alkalmazások** legördülő és `https://localhost:44321/` a a **Válasz URL-cím / átirányítása URI** legördülő listában.
11. Kattintson a **Futtatás**gombra. Egy új böngészőlapon megnyílik, és a profil szerkesztése fogyasztói élmény az alkalmazás segítségével futtathatja.

    > [AZURE.NOTE]
    Foglal házirend létrehozása és frissítések perccé alakít csak akkor lépnek érvénybe.
    
## <a name="create-a-password-reset-policy"></a>Jelszó alaphelyzetbe állítása házirend létrehozása

Ahhoz, hogy egyedi jelszó alaphelyzetbe állítása az alkalmazásban, meg kell jelszó alaphelyzetbe állítása házirend létrehozása. Megjegyzés:, hogy a bérlői szintű jelszó alaphelyzetbe állítása megadott beállítás [Itt](active-directory-b2c-reference-sspr.md) bejelentkezési házirendek továbbra is használható. Ezzel a házirenddel a változat, hogy a fogyasztói fog jelszó alaphelyzetbe állítása, illetve az, hogy az alkalmazás fog kapni a sikeres befejezésétől tokenek tartalmát mutatja be.

1. [Kövesse ezeket a lépéseket követve nyissa meg azt a B2C szolgáltatások lap az Azure portálon](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kattintson **az új jelszó kérésének házirendeket**.
3. Válassza a **+ Hozzáadás** a lap tetején.
4. A **név** határozza meg, hogy a jelszó alaphelyzetbe állítása az alkalmazás által használt házirend neve. Írja be például a "SSPR".
5. Kattintson a **Identitásszolgáltatók** , és válassza az "E-mail címhez jelszó visszaállítása". Kattintson az **OK gombra**.
6. Kattintson az **alkalmazás követelések**. Itt választhatja ki, amelyet a tokenek küldött vissza az alkalmazás sikeres jelszó alaphelyzetbe élmény után a visszaadott jogcímeken. Jelölje be például a "Felhasználó Objektumazonosító".
7. Kattintson a **létrehozása**gombra. Megjegyzendő, hogy az imént létrehozott csoportházirend jelenik meg, mint "**B2C_1_SSPR**" (a **B2C\_1\_ ** fragment automatikusan felkerül) a **jelszó alaphelyzetbe állítása házirendek** lap a.
8. Nyissa meg a házirendet "**B2C_1_SSPR**" gombra kattintva.
9. Select "Contoso B2C alkalmazás" az **alkalmazások** legördülő és `https://localhost:44321/` a a **Válasz URL-cím / átirányítása URI** legördülő listában.
10. Kattintson a **Futtatás**gombra. Egy új böngészőlapon megnyílik, és a jelszó alaphelyzetbe állítása fogyasztói felület segítségével futtathatja az alkalmazásban.

    > [AZURE.NOTE]
    Foglal házirend létrehozása és frissítések perccé alakít csak akkor lépnek érvénybe.

## <a name="additional-resources"></a>További források

- [Jogkivonat, a munkamenet és az egyszeri bejelentkezéses konfigurációs](active-directory-b2c-token-session-sso.md).
