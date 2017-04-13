<properties
    pageTitle="Azure MFA bejelentkezik a yammert az Azure többtényezős hitelesítés"
    description="Ez a lap fog útmutatásokat, helyre látogasson el az Azure MFA elérhető különböző bejelentkezik módszerek tekintheti meg."
    keywords="felhasználói hitelesítés, a bejelentkezés módja, jelentkezzen be a mobiltelefonon, jelentkezzen be az office-telefon"
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kgremban"/>

# <a name="the-sign-in-experience-with-azure-multi-factor-authentication"></a>A bejelentkezés a yammert az Azure többtényezős hitelesítés
> [AZURE.NOTE]  Ezen a lapon a következő talál egy tipikus bejelentkezés módja látható.  A bejelentkezéshez című cikkben talál segítséget [gondjai vannak a Azure többtényezős hitelesítés](multi-factor-authentication-end-user-manage-settings.md)



## <a name="what-will-your-sign-in-experience-be"></a>Milyen lesz a bejelentkezési élmény?
Attól függően, hogy jelentkezzen be, és többtényezős hitelesítést használ a felhasználói felület függvényében eltérőek.  Ebben a részben azt információt nyújt a Mire számíthat, amikor bejelentkezik.  Válassza ki azt, amely a legjobban, milyen módon:


mit csinálsz?|Leírás
:------------- | :------------- |
[Bejelentkezés a mobil vagy az office-telefon](#signing-in-with-mobile-or-office-phone) | Ez a amire számíthat a mobil vagy az office telefonon bejelentkezést.
[Bejelentkezés a Microsoft Authenticator alkalmazást értesítés](#signing-in-with-the-microsoft-authenticator-app-using-notification) | Amire számíthat: az értesítések, a Microsoft Authenticator alkalmazással.
[Bejelentkezés a Microsoft Authenticator alkalmazás ellenőrzőkódot használatával](#signing-in-with-the-microsoft-authenticator-app-using-verification-code)|Amire számíthat: az a Microsoft Authenticator thapp használata egy Ellenőrzőkód.
[Egy másik módszert bejelentkezés](#signing-in-with-an-alternate-method)|Ez megtudhatja, mire számíthat, ha szeretné-e egy másik módszert.

## <a name="signing-in-with-mobile-or-office-phone"></a>Bejelentkezés a mobil vagy az office-telefon

Többtényezős hitelesítés használatával a mobil vagy az office telefonjával tapasztalatait leírja a következő információkat.

### <a name="to-sign-in-with-a-call-to-your-office-or-mobile-phone"></a>Hívás kezdeményezése az office és a mobiltelefon jelentkeznie

- Jelentkezzen be az adott alkalmazás vagy szolgáltatás, például az Office 365-ben a felhasználónevet és jelszót használ.
- A Microsoft visszahívja Önt.

![A Microsoft-hívások](./media/multi-factor-authentication-end-user-signin-phone/call.png)

- A telefont, majd kattintson a # billentyűt.

![Válasz](./media/multi-factor-authentication-end-user-signin-phone/phone.png)

- Akkor célszerű most bejelentkezni.</li>

## <a name="signing-in-with-the-microsoft-authenticator-app-using-notification"></a>Bejelentkezés a Microsoft Authenticator alkalmazást értesítés

Többtényezős hitelesítés használata a Microsoft Authenticator alkalmazással, értesítést kapnak, amikor a felület leírja, az alábbi információkat.

### <a name="to-sign-in-with-a-notification-sent-the-microsoft-authenticator-app"></a>Jelentkezzen be értesítést küld a a Microsoft Authenticator alkalmazás

- Jelentkezzen be az adott alkalmazás vagy szolgáltatás, például az Office 365-ben a felhasználónevet és jelszót használ.
- Microsoft értesítést küld.

![A Microsoft értesítést küld.](./media/multi-factor-authentication-end-user-signin-app-notify/notify.png)


- A telefont, majd kattintson az ellenőrzés billentyűt.  Ha a vállalat szükséges PIN-kódot, megkéri, itt.

![Ellenőrzése](./media/multi-factor-authentication-end-user-signin-app-notify/phone.png)

![A telepítő](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

- Akkor célszerű most bejelentkezni.


## <a name="signing-in-with-the-microsoft-authenticator-app-using-verification-code"></a>Bejelentkezés a Microsoft Authenticator alkalmazás ellenőrzőkódot használatával

Az alábbi információk leírja a felület többtényezős hitelesítés használata a Microsoft Authenticator alkalmazással, amikor egy Ellenőrzőkód rendszer használata.

### <a name="to-sign-in-using-a-verification-code-with-the-microsoft-authenticator-app"></a>Bejelentkezés az egy Ellenőrzőkód használata a Microsoft Authenticator alkalmazással

- Jelentkezzen be az adott alkalmazás vagy szolgáltatás, például az Office 365-ben a felhasználónevet és jelszót használ.
- A Microsoft kérni fogja a ellenőrzőkódot.

![Ellenőrzőkód megadására](./media/multi-factor-authentication-end-user-signin-app-verify/verify.png)

- Nyissa meg a telefonon a Microsoft Authenticator alkalmazást, és a bejelentkezés hol mezőbe írja be a kódot.

![Ismerkedés a kódot.](./media/multi-factor-authentication-end-user-signin-app-verify/phone.png)



- Akkor célszerű most bejelentkezni.


## <a name="signing-in-with-an-alternate-method"></a>Egy másik módszert bejelentkezés


A következő szakasz megtudhatja, hogyan jelentkezzen be egy másik módszert, ha az elsődleges módszer nem feltétlenül vehető igénybe.

### <a name="to-sign-in-with-an-alternate-method"></a>Jelentkezzen be egy másik módszert a

- Jelentkezzen be az adott alkalmazás vagy szolgáltatás, például az Office 365-ben a felhasználónevet és jelszót használ.
- Jelölje be a használata egy másik ellenőrző lehetőséget.  Ön lesz bemutató különböző lehetőségek közül választhat. A szám látni fog számától beállítási alapján.

![Alternatív módszer használata](./media/multi-factor-authentication-end-user-signin-alt/alt.png)

- Válasszon egy másik módszert, és jelentkezzen be.
