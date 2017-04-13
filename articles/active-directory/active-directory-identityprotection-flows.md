<properties
    pageTitle="Azure Active Directory-identitás védelemmel találkozik bejelentkezési |} Microsoft Azure"
    description="Áttekintést nyújt a felhasználói felület identitás védelem van szünteti meg, vagy a felhasználó remediated, vagy ha egy házirend többtényezős hitelesítést igényel."
    services="active-directory"
    keywords="Azure active directory identitás védelmét, cloud app feltárás, alkalmazásokat, biztonsági, kockázat, kockázat szint, rés, biztonsági házirendek kezelése"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="markvi"/>

# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a>Bejelentkezési Azure Active Directory-identitás védelem szolgáltatások

Az Azure Active Directory identitást védelmét a következőkre van lehetősége:

- többtényezős hitelesítés regisztrálni a felhasználóknak

- kockázatos bejelentkezési bővítmények és a biztonságos felhasználók kezelése

Ezeket a problémákat, a rendszer a választ egy felhasználónak bejelentkezés módja hatással van, mert csak közvetlenül bejelentkezni beépülő modul, mert a felhasználónév és jelszó nem lehet eltűnt. További lépésekre van szükség biztonságosan beszerzése a felhasználók az üzleti.

Ez a témakör áttekintést, a felhasználónak bejelentkezés módja, amely akkor fordulhat elő, minden esetben.

**Többtényezős hitelesítés**

- Többtényezős hitelesítés regisztráció



**Bejelentkezés a kockázat**

- Kockázatos bejelentkezési helyreállítás

- Kockázatos bejelentkezés blokkolt

- Többtényezős hitelesítés regisztrációs kockázatos bejelentkezés során
 

**A kockázat felhasználói**

- Biztonságos fiók-helyreállítás

- Letiltott biztonságos fiókok




## <a name="multi-factor-authentication-registration"></a>Többtényezős hitelesítés regisztráció

Mindkét, a biztonságos fiókok helyreállítási folyamat és a kockázatos bejelentkezési folyamat, a legjobb felhasználói felület esetén, a felhasználó saját helyreállíthatók. Többtényezős hitelesítés regisztrált felhasználók, ha már rendelkeznek biztonsági kihívásokkal átadni használható a fiókkal társított telefonszám. Nincs súgó asztali vagy a rendszergazda bevonása fiók biztonságát helyreállítása van szükség. Így erősen ajánlott beszerzése a felhasználók többtényezős hitelesítés regisztrált. 

A rendszergazdák az alábbi lehetőségeket:

- állított be egy házirendet, amely nagyobb biztonságot az ellenőrzéshez a fiókok beállítása a felhasználóknak. 
- Engedélyezze a többtényezős hitelesítés regisztrációs kihagyása legfeljebb 30 napig abban az esetben, ha szeretnék, amelyből a felhasználók a türelmi időszak Regisztrálás előtt.

**Többtényezős hitelesítés regisztráció három lépést foglal magában:**

1. Az első lépésben az a felhasználó megkapja a fiók tagjának többtényezős hitelesítés beállítása kötelező értesítést. 

    ![Remediation] (./media/active-directory-identityprotection-flows/140.png "Remediation")


2. Többtényezős hitelesítés beállítása, hogy meg kell, hogy a rendszer, hogy hogyan lehet Önnel kapcsolatba lépni szeretne.

    ![Remediation] (./media/active-directory-identityprotection-flows/141.png "Remediation")
 
3. A rendszer elküldi a kérést, és meg kell válaszolni.

    ![Remediation] (./media/active-directory-identityprotection-flows/142.png "Remediation")

 



## <a name="risky-sign-in-recovery"></a>Kockázatos bejelentkezési helyreállítás

Egy rendszergazda úgy állította be a bejelentkezési kockázatok egy házirendet, az érintett felhasználónál értesítést kap, amikor megpróbálnak, hogy jelentkezzen be. 

**A kockázatos bejelentkezési folyamat két lépést foglal magában:** 

1. A felhasználót, hogy szokatlan tények kapcsolatban a bejelentkezés, például egy új helyre, eszközről vagy alkalmazás bejelentkezés észlelt értesítést kap. 

    ![Remediation] (./media/active-directory-identityprotection-flows/120.png "Remediation")

2. A felhasználó által biztonsági bonyolulttá megoldási a személyazonosság van szükség. Ha a felhasználó regisztrált többtényezős hitelesítéshez azok kell round-trip telefonszámukat biztonsági kódot. Mivel ez érdekesebbet, kockázatos bejelentkezés, és nem biztonságos fiókok, a felhasználónak nem kell módosítani a jelszót a folyamat. 

    ![Remediation] (./media/active-directory-identityprotection-flows/121.png "Remediation")



 
## <a name="risky-sign-in-blocked"></a>Kockázatos bejelentkezés blokkolt
A rendszergazdák bejelentkezési kockázat házirend beállítása a felhasználók letiltása után bejelentkezés kockázat szinttől függően is választhat. Úgy juthat az blokkolatlan, a végfelhasználók kapcsolatba kell lépnie egy rendszergazdának vagy a Súgó asztali, vagy azok is próbáljon meg bejelentkezni egy már jól ismert helyre vagy eszközről. Többtényezős hitelesítés megoldási által önálló helyreállítása lehetőség nem ebben az esetben.

![Remediation] (./media/active-directory-identityprotection-flows/200.png "Remediation")




## <a name="compromised-account-recovery"></a>Biztonságos fiók-helyreállítás

Amikor egy felhasználó kockázat biztonsági házirend konfigurálva, a felhasználók, akik felel meg a felhasználó kockázat szintű házirend megadott (és ezért feltételezik sérül) haladjon végig a felhasználó biztonságos helyreállítási folyamat, mielőtt azok jelentkezhetnek be. 

**A felhasználó biztonságos helyreállítási folyamat három lépést foglal magában:**

1. A felhasználó értesítést kap, hogy a fiók biztonsági kockázatára gyanús tevékenység miatt vagy kiszivárogtatott hitelesítő adatait.

    ![Remediation] (./media/active-directory-identityprotection-flows/101.png "Remediation")

2.  A felhasználó által biztonsági bonyolulttá megoldási a személyazonosság van szükség. Ha a felhasználó regisztrált többtényezős hitelesítéshez azokat is önálló helyreállítása illetéktelen kezekbe. Azok kell round-trip telefonszámukat biztonsági kódot. 

    ![Remediation] (./media/active-directory-identityprotection-flows/110.png "Remediation")


3.  Végül a felhasználónak meg kell szeretné módosítani a jelszavát, mivel az access a fiókjára volna valaki másnak. E, amikor képernyőképeket alatt találhatók.
 
    ![Remediation] (./media/active-directory-identityprotection-flows/111.png "Remediation")



## <a name="compromised-account-blocked"></a>Letiltott biztonságos fiókok 

Ha egy felhasználó, amely nincs zárolva felhasználói kockázat biztonsági házirendek blokkolta, a felhasználó kell kérje a rendszergazda segítségét vagy belső ügyfélszolgálat:. Többtényezős hitelesítés megoldási által önálló helyreállítása lehetőség nem ebben az esetben.


![Remediation] (./media/active-directory-identityprotection-flows/104.png "Remediation")



 
## <a name="reset-password"></a>Jelszó alaphelyzetbe állítása

Felhasználók biztonságos bejelentkezés programfájltípusok, ha a rendszergazda miatt ideiglenes jelszót az őket. A felhasználók kell módosítani a jelszavát, a következő bejelentkezés során.

![Remediation] (./media/active-directory-identityprotection-flows/160.png "Remediation")


 




 

## <a name="see-also"></a>Lásd még:

- [Azure Active Directory identitás-védelem](active-directory-identityprotection.md) 