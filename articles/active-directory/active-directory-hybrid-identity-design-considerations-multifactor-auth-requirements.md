<properties
    pageTitle="Azure Active Directory hibrid identitás tervezési szempontjait - többtényezős hitelesítés követelmények meghatározása"
    description="Feltételes hozzáférés-vezérlés az Azure Active Directory ellenőrzi az adott feltételeknek választhat, amikor a felhasználó hitelesítése és előtt, hogy az alkalmazás hozzáférést. Ha ezen feltételek teljesülése esetén a felhasználó a hitelesített és az alkalmazás férhet hozzá."
    documentationCenter=""
    services="active-directory"
    authors="femila"
    manager="billmath"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>Többtényezős hitelesítés követelmények a hibrid identitás megoldás meghatározása

A világ mobilitás felhasználói adatok és a felhőben, és bármilyen eszközről alkalmazások elérése biztonságossá tétele ezt az információt vált kiemelkedő.  Minden nap van egy olyan biztonsági kötelezettségek megsértésével kapcsolatos új címsort.  Bár a nincs semmilyen garanciát ilyen szabályok megsértésével szemben, a többtényezős hitelesítés, egy további biztonsági felvételével megelőzheti, hogy a következő szabályok megsértésével réteget biztosít.
Többtényezős hitelesítés szervezetek követelményeinek értékelése először. Ez azt jelenti, hogy mi a szervezet próbálja biztonságos.  A kiértékelés fontos, hogy a szervezetek felhasználók többtényezős hitelesítéshez engedélyezése és beállítása technikai követelményeinek meghatározása.

>[AZURE.NOTE]
Ha nem ismeri a MFA, és mit jelent, erősen ajánlott olvassa el a cikk [Mi az Azure többtényezős hitelesítést?](../multi-factor-authentication/multi-factor-authentication.md) előzetes olvasási ebben a szakaszban a folytatáshoz.

Ellenőrizze, hogy a válasz, a következőket:

- A vállalat próbál biztonságos Microsoft alkalmazások? 
- Hogyan közzétett az alkalmazások?
- A vállalat nyújt lehetővé teszi az alkalmazottaknak, hogy az access-alkalmazások a helyszíni távoli eléréséhez?

Ha igen, milyen típusú távelérési? Is van szüksége, ahol a felhasználók, akik férnek hozzá ezeket az alkalmazásokat lesz található ki szeretné számítani. A kiértékelés lépése egy másik fontos a megfelelő többtényezős hitelesítés stratégia meghatározásához. Győződjön meg arról, hogy az alábbi kérdésekre választ:

- Hol vannak a felhasználók fogom található?
- Azok található bárhol lehet?
- Nem a vállalat szeretne megfelelően a felhasználói hely korlátozások létrehozni?

Ha megértette ezeknek a követelményeknek, fontos, a felhasználó követelményei többtényezős hitelesítés is ki szeretné számítani. A kiértékelés fontos, mert azt határozza meg többtényezős hitelesítés közbeni követelményei. Győződjön meg arról, hogy az alábbi kérdésekre választ:

- A felhasználók jól ismert többtényezős hitelesítést?
- Néhány felhasználási kell nyújt további hitelesítést?  
 - Ha igen, minden alkalommal, amikor külső hálózatok vagy elérésekor egyes alkalmazások, vagy egyéb feltételekkel érkező?
- Esetén a felhasználók kell oktatás beállítása és megvalósítása többtényezős hitelesítést?
- Mik azok az esetek, amelyek a vállalat szeretne engedélyezése a felhasználóknak többtényezős hitelesítést?

A fenti kérdések megválaszolása, után fogja tudni megérthető, ha már végrehajtott többtényezős hitelesítés a helyszíni. A kiértékelés fontos, hogy a szervezetek felhasználók többtényezős hitelesítéshez engedélyezése és beállítása a technikai követelményei megadása. Győződjön meg arról, hogy az alábbi kérdésekre választ:

- A vállalat-nek MFA a kiemelt partnerek védelmét?
- A vállalat-nek ahhoz, hogy az egyes alkalmazások megfelelőségi okokból MFA?
- A vállalat-nek MFA ahhoz, hogy a fenti alkalmazások vagy csak a rendszergazdák minden jogosult felhasználók számára?
- Szükség van MFA mindig legyen engedélyezve, és csak ha a felhasználó jelentkezett a vállalati hálózaton kívüli?


## <a name="next-steps"></a>Következő lépések
[A hibrid identitás átálláshoz meghatározása](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)


## <a name="see-also"></a>Lásd még:
[Tervezési szempontjait – áttekintés](active-directory-hybrid-identity-design-considerations-overview.md)
