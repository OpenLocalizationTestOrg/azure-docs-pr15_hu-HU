<properties
    pageTitle="Jelszóházirendek és korlátozásokat az Azure Active Directory |} Microsoft Azure"
    description="A jelszavak az Azure Active Directory, a soros például engedélyezett karaktereket, a hossz és a lejárat házirendeket ismerteti."
  services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Jelszóházirendek és Azure Active Directory korlátozásairól

Ez a cikk ismerteti a jelszóházirendek és a felhasználói fiókokat az Azure Active directory tárolt társított összetettsége követelményeknek.

> [AZURE.IMPORTANT] **Itt azért, mert netán problémákat tapasztal bejelentkezéskor?** Ha igen, [az alábbiakban hogyan módosíthatja, és a saját jelszó alaphelyzetbe állítása](active-directory-passwords-update-your-own-password.md).

## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>Az összes felhasználói fiókok UserPrincipalName házirendeket

Minden felhasználói fiókot, jelentkezzen be az Azure Active Directory authentication rendszernek kell, hogy egy egyedi felhasználói egyszerű felhasználónév (UDN) attribútumérték, hogy a fiókkal társított kell rendelkeznie. A következő táblázat a körvonal a házirendeket, mind a helyszíni Active Directory-kifejezéskészletébe felhasználói fiókok alkalmazása (a felhővel szinkronizált) és a felhőben felhasználói fiókjaihoz.

|   A tulajdonság           |     UserPrincipalName vonatkozó követelmények  |
|   ----------------------- |   ----------------------- |
|  Tiltott karaktert    |  <ul> <li>A – Z</li> <li>a – z-ig </li><li>a 0 – 9</li> <li> . - \_ ! \# ^ \~</li></ul> |
|  Tiltott karaktert  | <ul> <li>Bármely '@' , hogy a felhasználó nevét a tartomány nem az elválasztó karaktert.</li> <li>A pont karakter nem tartalmazhat '. "közvetlenül megelőző a '@' szimbólum</li></ul> |
| Hossz korlátozásokkal  |       <ul> <li>Teljes hossza nem haladhatja meg a 113 karakterek</li><li>Mielőtt 64 karaktert a ‘@’ szimbólum</li><li>után 48 karaktert a ‘@’ szimbólum</li></ul>

## <a name="password-policies-that-apply-only-to-cloud-user-accounts"></a>Jelszóházirendek, amelyek csak az felhő felhasználói fiókok

Az alábbi táblázat ismerteti, amelyek a felhasználói fiókok létrehozása és kezelése az Azure Active Directory alkalmazhatók elérhető jelszó házirend-beállítások.

|  A tulajdonság       |    Követelmények          |
|   ----------------------- |   ----------------------- |
|  Tiltott karaktert   |   <ul><li>A – Z</li><li>a – z-ig </li><li>a 0 – 9</li> <li>@# $ % ^ & * - _ ! + [] {} = & #124; \ : ‘ , . ? / ` ~ “ ( ) ;</li></ul> |
|  Tiltott karaktert   |       <ul><li>Unicode-karakterek</li><li>Szóköz</li><li> **Csak az erős jelszavak**: pont karakter nem tartalmazhat "." közvetlenül megelőző a '@' szimbólum</li></ul> |
|   Jelszóra vonatkozó korlátozások | <ul><li>8 minimális és 16 karakterek maximális</li><li>**Csak az erős jelszavak**: 3 igényel kívül az alábbi 4:<ul><li>Kisbetűk</li><li>Nagybetűk</li><li>Számok (0 – 9)</li><li>Szimbólumok (lásd a fenti jelszóra vonatkozó korlátozások)</li></ul></li></ul> |
| Jelszó lejáratának időtartam      | <ul><li>Alapértelmezett érték: **90** napon </li><li>A Set-MsolPasswordPolicy parancsmaggal az Azure Active Directory modul Windows Powershellhez konfigurálható található érték.</li></ul> |
| Jelszó lejáratának értesítés |  <ul><li>Alapértelmezett érték: (előtt a jelszó lejár) **14** nappal</li><li>A Set-MsolPasswordPolicy parancsmaggal konfigurálható található érték.</li></ul> |
| Jelszó lejáratának |  <ul><li>Alapértelmezett érték: **false** nap (azt jelzi, hogy a jelszó lejáratának engedélyezve van) </li><li>Érték a Set-MsolUser parancsmaggal egyes felhasználói fiókok beállíthatók. </li></ul> |
|  Jelszó előzmények  | Utolsó jelszót újra nem használhatók. |
|  Jelszó előzmények időtartam | Örökkévalóság |
|  Fiók zárolása | Próbálkozás után sem 10 sikertelen bejelentkezés (bejelentkezni helytelen jelszóval) a felhasználó rendszer zárolja egy percig. További nem a megfelelő bejelentkezési kísérleteket fog zárolni ki a felhasználó számára időtartamának növelésével. |


## <a name="next-steps"></a>Következő lépések

* **Itt azért, mert netán problémákat tapasztal bejelentkezéskor?** Ha igen, [az alábbiakban hogyan módosíthatja, és a saját jelszó alaphelyzetbe állítása](active-directory-passwords-update-your-own-password.md).
* [A jelszavak kezelése bárhonnan](active-directory-passwords.md)
* [Hogyan működik a jelszó kezelése](active-directory-passwords-how-it-works.md)
* [Jelszó képtárakhoz – első lépések](active-directory-passwords-getting-started.md)
* [Testre jelszóval kezelése](active-directory-passwords-customize.md)
* [Adatkezelési Helyszavak helyes használatáról](active-directory-passwords-best-practices.md)
* [Jelszó jelentésekkel műveleti háttérismeretek beszerzése](active-directory-passwords-get-insights.md)
* [Jelszó-kezelés – gyakori kérdések](active-directory-passwords-faq.md)
* [Jelszó-kezelés – problémamegoldás](active-directory-passwords-troubleshoot.md)
* [tudj meg többet](active-directory-passwords-learn-more.md)
