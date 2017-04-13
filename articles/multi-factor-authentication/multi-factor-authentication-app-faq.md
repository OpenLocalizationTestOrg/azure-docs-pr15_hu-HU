<properties
    pageTitle="A Microsoft hitelesítő alkalmazás – gyakori kérdések"
    description="Itt a gyakori kérdéseket és válaszokat, a Microsoft Authentication alkalmazás és a Azure többtényezős hitelesítés kapcsolatos listáját."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="pblachar, librown"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="kgremban"/>

# <a name="microsoft-authenticator-application-faq"></a>A Microsoft Authenticator alkalmazás – gyakori kérdések

A Microsoft Authenticator alkalmazás lecseréli az Azure hitelesítő alkalmazást, és Azure többtényezős hitelesítés használata az Excel alkalmazás esetén. Az alkalmazás Windows Phone, Android és iOS érhető el.

## <a name="frequently-asked-questions"></a>Gyakori kérdések

- **Mi történt az Azure hitelesítő többtényezős hitelesítés és Microsoft-fiók alkalmazások?**

    A Microsoft Authenticator app az alkalmazások felváltja egymást. Azure hitelesítő frissített a Microsoft Authenticator. Ha a többtényezős hitelesítés vagy a Microsoft-fiók alkalmazások, telepítse a Microsoft Authenticator, és újból vegye fel a fiókok. Győződjön meg arról, hogy a fiók hozzáadása az új alkalmazás a régi lehetőségekből törlés előtt befejezéséhez.

- **A Microsoft Authenticator alkalmazás már használok ellenőrzési kódok. Hogyan válthatok át egyetlen kattintással leküldéses értesítéseket?**  

    A bejelentkezés keresztül leküldéses értesítést jóváhagyásáért csak akkor Microsoft-fiók, nem pedig a külső partnerek, például a Google- vagy Facebook. Munkahelyi vagy iskolai Microsoft-fiókok esetén a szervezet választhat tiltsa le bár ezt a beállítást.

    Ha fiókjának személyes Microsoft-fiókot használ, és szeretné átkapcsolhatná a leküldéses értesítéseket, meg kell a fiókot. Regisztrálja újra az eszközt a fiókjához, és a leküldéses értesítések beállítása.  

    Ha a fiókja nem rendelkezik kétlépcsős hitelesítési engedélyezve, című témakörben [használatához kétlépcsős hitelesítési kapcsolatos](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) döntse el, ha szüksége.  

- **Amikor lesz egyetlen kattintással leküldéses értesítéseket használhatja az iPhone-on vagy Ipaden?**  

    Ez a funkció a béta augusztus, ha a Microsoft-fiók szélesebb körben elérhetővé válik végéig. A béta programhoz csatlakozni szeretne, e-mailt küldi msauthenticator@microsoft.com. Az utónév és vezetéknevet, valamint az Apple ID bele az üzenetet.  

- **A Microsoft fiókok egyetlen kattintással leküldéses értesítéseket is működik?**  

    Leküldéses értesítések nem, csak működik a Microsoft-fiókok és Azure Active Directory-fiókok. Ha a munkahelyi vagy iskolai használja az Azure Active Directory-fiókok, azok letilthatják ezt a szolgáltatást.  

- **E másolatból eszközömön, és a hiányzó vagy nem működik a fiókja kód. mi történt?**  

    Biztonsági okokból azt nincs visszaállíthatja az fiókok alkalmazás biztonsági mentés. Az iOS-alkalmazás meg visszaállítása biztonsági másolatból, ha a fiókok megjelennek, de nem működnek a beérkező bejelentkezési ellenőrzések és biztonsági-kódok létrehozása. A fiókok törlése után vissza az alkalmazást, és újból vegye fel.

- **Új eszközhöz kaptam Hogyan a Microsoft Authenticator alkalmazás eltávolítása a régi eszközről és az újba helyezze át?**

    A Microsoft Authenticator alkalmazás hozzáadása egy új eszköz automatikusan eltávolítás nem el más eszközökről. Kezelése, hogy milyen eszközöket a fiók van konfigurálva, látogasson el a azonos webhely használatához kétlépcsős hitelesítési kezelése, és válassza ki régi alkalmazások eltávolítása.

    Személyes Microsoft-fiókok esetén a webhelynek a [fiók biztonság](https://account.microsoft.com/security) lapra. Munkahelyi vagy iskolai Microsoft-fiók a webhelynek vagy az [alkalmazások parancsot](https://myapps.microsoft.com) , vagy az egyéni portál, hogy szervezete úgy állította be lehet.

## <a name="contact-us"></a>Kapcsolatfelvétel

Ha itt nem lett megválaszolja a kérdést, hagyja egy megjegyzés az oldal alján. Vagy, [lépjen kapcsolatba a támogatási](https://support.microsoft.com/contactus) , és azt fogja válaszoljon a problémát, amint azt is.

Ügyfélszolgálat, terjed ki annyi az alábbi adatokat, amennyit csak lehet:

- **Felhasználói azonosító** – Mi az, hogy az e-mail címet, próbált bejelentkezéshez?
- **A hiba általános leírása** – milyen pontos hibaüzenet jelent meg látni?  Ha nincs hibaüzenet volt, ismertetik a szokatlan viselkedést tapasztal láthatta, részletesen.
- **Lap** – milyen lap volt a, ha a hiba látta (például az URL-cím)?
- **Hibakód** - Igen hibakód.
- **Munkamenet** - az adott munkamenetben azonosító igen.
- **Korrelációs azonosító** – mi volt a korrelációs azonosító kódot jön létre, amikor a felhasználó a hiba bemutattuk.
- **Időbélyeg** – mi volt a pontos dátum- és a hiba látta (például az időzóna)?

Az információk nagy a bejelentkezési lapon találhatók. Amikor a bejelentkezéskor idő nem ellenőrzi, jelölje be a **Részletek**.

![Jelentkezzen be a részletek](./media/multi-factor-authentication-end-user-troubleshoot/view_details.png)

Következő információk szerepeltetését segít a probléma mielőbbi megoldásában.

## <a name="related-topics"></a>Kapcsolódó témakörök

- [Azure többtényezős hitelesítés – gyakori kérdések](multi-factor-authentication-faq.md)  
- [Tudnivalók a kétlépcsős hitelesítési](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) Microsoft-fiók
- [Gondjai vannak a kétlépcsős hitelesítési?](multi-factor-authentication-end-user-troubleshoot.md)
