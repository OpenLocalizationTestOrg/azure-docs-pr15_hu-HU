<properties
    pageTitle="Ajánlott eljárások: Azure AD-jelszó Management |} Microsoft Azure"
    description="Ajánlott eljárások a telepítési és használati, minta végfelhasználói dokumentációja és az Azure Active Directory jelszókezelési oktatás útmutatók."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="deploying-password-management-and-training-users-to-use-it"></a>Jelszavak kezelését és képzési felhasználók használatához üzembe helyezése

> [AZURE.IMPORTANT] **Itt azért, mert netán problémákat tapasztal bejelentkezéskor?** Ha igen, [az alábbiakban hogyan módosíthatja, és a saját jelszó alaphelyzetbe állítása](active-directory-passwords-update-your-own-password.md).

Miután engedélyezése jelszó alaphelyzetbe állításához szeretné vinni lépés, hogy a felhasználók a szervezet szolgáltatással. Művelet, ellenőrizze, hogy a felhasználó beállítva a szolgáltatás használatához a megfelelő és is kell, hogy a felhasználó rendelkezik-e a szükségük van a saját jelszavak kezelése a sikeres használatáról. Ez a cikk Önnek fog ismertetik az alábbi fogalmak:

* [**A felhasználók jelszókezelési konfigurált beszerzése**](#how-to-get-users-configured-for-password-reset)
  * [Miből be van állítva a jelszó-visszaállítás fiók](#what-makes-an-account-configured)
  * [Annak érdekében, hogy saját maga hitelesítési adatok feltöltése módjai](#ways-to-populate-authentication-data)
* [**A legjobb módszerek az jelszó alaphelyzetbe állításához, hogy szervezete bevezetésére**](#what-is-the-best-way-to-roll-out-password-reset-for-users)
  * [Bevezetés az e-mailes + minta e-mail kommunikáció](#email-based-rollout)
  * [Hozzon létre saját egyéni jelszó kezelőportálja segítségével a felhasználók számára](#creating-your-own-password-portal)
  * [Hogyan bejelentkezési regisztrálhatja a felhasználókat a kényszerített regisztrációs segítségével](#using-enforced-registration)
  * [Felhasználói fiókok hitelesítési adatok feltöltése](#uploading-data-yourself)
* [**Minta felhasználó és a támogatási képzési anyagokat (hamarosan!)**](#sample-training-materials)

## <a name="how-to-get-users-configured-for-password-reset"></a>A jelszó-visszaállítás felhasználók beszerzése be van állítva
Ez a szakasz, ismerteti a különböző módszereket, amelyekkel biztosíthatja, hogy a szervezet minden felhasználójának használhatja az önkiszolgáló jelszó alaphelyzetbe állítása hatékony abban az esetben, ha azokat az elfelejtette a jelszavát.

### <a name="what-makes-an-account-configured"></a>Miből beállított fiók
Mielőtt a felhasználó a jelszó-visszaállítás használni, **az összes** alábbi feltétel teljesülniük kell:

1.  A jelszó-visszaállítás a címtárban engedélyezve kell lennie.  Megtudhatja, hogy miként jelszó olvasásával [engedélyezése a felhasználóknak az Azure Active Directory jelszavak alaphelyzetbe állítása](active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords) vagy [engedélyezése a felhasználóknak, hogy alaphelyzetbe állítása vagy módosítása az Active Directory jelszavukat](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) alaphelyzetbe engedélyezése
2.  A felhasználónak kell lennie licence.
 - A felhasználók a felhőben a felhasználónak kell rendelkeznie, **bármilyen fizetős Office 365-licenc**, vagy egy **Egyszerű AAD** vagy **AAD Premium licenc** kiosztani.
 - A helyszíni felhasználóknak (szövetséges vagy kivonat szinkronizált), a felhasználónak **rendelkeznie kell egy AAD prémium licenccel**.
3.  A felhasználónak kell rendelkeznie a jelszó-visszaállítás házirend megfelelően **minimálisan definiált hitelesítési adatok** .
 - Hitelesítési adatok tekintendő definiálva, ha a megfelelő mezőben, a címtárban szabályos adatokat tartalmaz.
 - Hitelesítési adatok minimális halmazának **legalább egy** , vagy ha be van állítva egy kapu házirend engedélyezett hitelesítési beállítások **legalább két** engedélyezett hitelesítési beállítások: van megadva, ha egy két kapu házirend konfigurálva.
4.  Ha a felhasználó használja a helyszíni fiók, majd [Jelszó visszaírást](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) engedélyeznie kell és be van kapcsolva

### <a name="ways-to-populate-authentication-data"></a>Hitelesítési adatok feltöltése módjai
A jelszó-visszaállítás használandó a felhasználói adatok megadása a szervezet több lehetőség közül választhat.

- Felhasználók szerkesztése az az [Azure felügyeleti portál](https://manage.windowsazure.com) vagy az [Office 365 felügyeleti központból](https://portal.microsoftonline.com)
- Azure AD-szinkronizálás használata a felhasználó tulajdonságainak szinkronizálása az Azure Active Directory-a helyszíni Active Directory tartományi a
- [Az alábbi lépéseket](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users)követve a felhasználó tulajdonságainak szerkesztése a Windows PowerShell használatával.
- Engedélyezése a felhasználók saját adatok regisztrálása irányadó őket a regisztrációs-portálra a [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)
- Jelszó alaphelyzetbe állításához, ha az azok jelentkezzen be az Azure Active Directory-fiókját beállítva regisztrálhatja a felhasználóknak a [**regisztrálhatja a bejelentkezéskor felhasználóknak?**](active-directory-passwords-customize.md#require-users-to-register-when-signing-in) beállítás **Igen**értékre.

Felhasználók nem szükséges regisztrálni a jelszó alaphelyzetbe állításához, a rendszer a munkát.  Ha meglévő mobil vagy az office telefonszámok a helyi könyvtárában, például szinkronizálhatja őket az Azure Active Directory, és fogjuk használni őket a jelszó átállításához automatikusan.

További információ [hogyan adatokat használja, jelszó alaphelyzetbe állítása](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) és [hogyan kitöltheti a PowerShell egyéni hitelesítési mezők](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users)érdemes elolvasnia.

## <a name="what-is-the-best-way-to-roll-out-password-reset-for-users"></a>Mi az, hogy a legjobb módszer a jelszó alaphelyzetbe állítása az felhasználók bevezetésére?
A jelszó-visszaállítás bevezetési általános lépései a következők:

1.  Engedélyezze a jelszó alaphelyzetbe állításához címtárában, ha megnyitja a **Configure** lapra az [Azure adatkezelési portál](https://manage.windowsazure.com) és **Igen** **Jelszó alaphelyzetbe állítása az engedélyezett felhasználók** beállítással.
2.  A megfelelő licenceket rendelhet minden olyan felhasználóhoz, akinek szeretné, hogy állítsa alaphelyzetbe a jelszó kínálják a nyissa meg a **licencek** lapra az [Azure Kezelőportálja segítségével](https://manage.windowsazure.com).
3.  Tetszés szerint korlátozhatja a jelszó alaphelyzetbe állításához, hogy bevezetésére a szolgáltatás lassú adott idő alatt a **Hozzáférés korlátozása a jelszó alaphelyzetbe állítása** váltása állítsa **Igen értékre** , és válassza a biztonsági csoport jelszó-visszaállítás (ezek a felhasználók hozzájuk rendelt licencek kell rendelkeznie Megjegyzés) engedélyezése a felhasználók egy csoportja.
4.  Kérje meg a felhasználóknak, hogy a jelszó alaphelyzetbe állításához, vagy elküldheti őket őket a regisztrált, amely e-mailben használata az access a Vezérlőpulton vagy feltöltésével ezen felhasználók megfelelő hitelesítési adatait saját maga DirSync, PowerShell és az [Azure Kezelőportálja](https://manage.windowsazure.com)regisztrációs engedélyezése vannak érvényben.  További információra kíváncsi, ez a alatt találhatók.
5.  Az idő olvassa el a felhasználók, lépjen a jelentések fülre a [**Jelszó alaphelyzetbe állítása nyilvántartási tevékenység**](active-directory-passwords-get-insights.md#view-password-reset-registration-activity) jelentés megtekintésekor regisztrálása.
6.  Miután regisztrált egy jó felhasználók számát, nézze meg őket, használja a jelszó alaphelyzetbe állításához, lépjen a jelentések fülre a [**Jelszó alaphelyzetbe állítása tevékenység**](active-directory-passwords-get-insights.md#view-password-reset-activity) jelentés megtekintésekor.

Tájékoztassa a felhasználókat, hogy azok regisztrálhat, és használja a jelszó alaphelyzetbe állításához, a szervezet többféle módon lehet.  A részletezi alatt.

### <a name="email-based-rollout"></a>Bevezetés az e-mailes
Talán a legegyszerűbb megoldás, hogy a felhasználók tájékoztatása regisztrálhat, vagy használja a jelszó alaphelyzetbe állítása el őket ehhez kap e-mailt küld nekik.  Az alábbi képen egy sablont, ezt is használhatja.  A színek kicserélendő nyugodtan / emblémák össze saját igényeinek megfelelően testre ellenében.

  ![][001]

Azt is megteheti, hogy [az e-mailek sablon itt letöltése](http://1drv.ms/1xWFtQM).

### <a name="creating-your-own-password-portal"></a>A saját jelszó portál létrehozása
Több stratégia, amely nagyobb ügyfelek üzembe helyezése a jelszó-kezelési lehetőségek jól használható, ha a "jelszó portál", amely a felhasználók kezelése az összes dolog, amit segítségével egyetlen kapcsolódó egyetlen helyen jelszavukat.  

Legnagyobb ügyfeleink számos válassza ki a legfelső szintű DNS-bejegyzés, például https://passwords.contoso.com hivatkozásokkal az Azure Active Directory-jelszó alaphelyzetbe állítása portál, az új jelszó kérésének regisztrációs portál és jelszó módosítása lapok létrehozásához.  Ezzel a módszerrel bármelyik kommunikáció e-mailben vagy szórólapok küld ki, is elhelyezhet egy könnyen megjegyezhetővé teheti, URL-címet, amely egy második első lépések a szolgáltatással rendelkező felhasználók látogathat el.

Az alábbi áttekintés, azt lapot hozott létre egyszerű a legújabb válaszol felhasználói felület Tervező szakaiban használó és böngészők és a mobileszközök is működik.

  ![][007]

Azt is megteheti, hogy [a webhely sablon itt letöltése](https://github.com/kenhoff/password-reset-page).  Azt javasoljuk, hogy az embléma és a szervezet szükséges a színek testreszabása.

### <a name="using-enforced-registration"></a>Kényszerített regisztrációs használatával
Ha azt szeretné, hogy a felhasználóknak, hogy a jelszó alaphelyzetbe állításához maguk regisztrálása, is lehetséges őket regisztrálása, ha az access panelre a [http://myapps.microsoft.com](http://myapps.microsoft.com)bejelentkezni.  Ezt a lehetőséget a címtárában **konfigurálása** lapon engedélyezheti a **Felhasználók csak az Access panelre bejelentkezéskor nyilvántartásba** beállítás engedélyezésével.  

Tetszés szerint is meghatározhatja, függetlenül attól, azok pedig arra kéri, újraregisztrálása konfigurálható idő után a **felhasználók kapcsolattartási adataikat igazolnia kell, mielőtt a napok száma** beállítást kell nullától különböző érték módosításával. [Testreszabása felhasználói jelszót kezelése a viselkedést](active-directory-passwords-customize.md#password-management-behavior) tapasztalta további információt.

  ![][002]

Miután engedélyezte ezt a lehetőséget, amikor a felhasználók jelentkezzen be az access panelen, megjelenik számukra a helyi menü, amely tájékoztatja, hogy a rendszergazda őket a kapcsolattartási adataikat ellenőrzéséhez szükséges. Azok a jelszó alaphelyzetbe állítása, ha az azok minden eddiginél elveszíti hozzáférését fiókjukat felhasználhatja.

  ![][003]

**Ellenőrizze most** gombra kattintva életre őket a **jelszó alaphelyzetbe állítása regisztrációs portál** [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) , és szüksége van rájuk regisztrálni.  Regisztrációs e ez a módszer is nyitva, kattintson a **Mégse** gombra, vagy zárja be az ablakot, de a felhasználók figyelmezteti, minden alkalommal, amikor azok jelentkezzen be, ha nem regisztrálni.

  ![][004]

### <a name="uploading-data-yourself"></a>Adatfeltöltés, saját maga
Ha feltöltése hitelesítési adatok saját maga szeretné, majd nem felhasználók szükséges regisztrálni jelszó alaphelyzetbe állításához, mielőtt a jelszavak alaphelyzetbe állítása.  Mindaddig, amíg a felhasználók rendelkeznek a hitelesítési adatok definiált a fiókot, amely megfelel a jelszó alaphelyzetbe állítása beállított házirend, majd ezek a felhasználók tudják a jelszavak alaphelyzetbe állítása.

Milyen beállításával is AAD csatlakozás vagy a Windows PowerShell használatával című témakörben talál [milyen adatokat használja, a jelszó-visszaállítás](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset).

Az alábbi lépéseket követve is feltölthet a hitelesítési adatok az [Azure Kezelőportálja](https://manage.windowsazure.com) keresztül:

1.  Nyissa meg a címtárában, az **Active Directory-bővítmény** az [Azure Kezelőportálja segítségével](https://manage.windowsazure.com).
2.  Kattintson a **felhasználók** lapon.
3.  Jelölje ki a felhasználót, érdeklik a listából.
4.  Az első lapon a **Másodlagos E-mail**, a jelszó-visszaállítás engedélyezése tulajdonság felhasználható találja.

    ![][005]

5.  Kattintson a **Munkaadatok** lapon.
6.  Ezen a lapon **Irodai telefon**, a **mobiltelefon**, a **Hitelesítési telefon**és a **Hitelesítési E-mail**kifejezésre keresve.  A tulajdonságok is beállítható engedélyezése a felhasználók számára, hogy állítsa alaphelyzetbe jelszavát.

    ![][006]

Látható, hogy [milyen adatokat használja, a jelszó-visszaállítás](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) , hogyan tulajdonságokból mindegyikére is használható.

Lásd: a [elsajátíthatja a jelszó alaphelyzetbe állítása az adatokat a felhasználók a PowerShell](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users) látható, hogy el tudja olvasni és ezeket az adatokat a PowerShell beállítása.

## <a name="sample-training-materials"></a>Minta – oktatóanyagok
Dolgozunk a minta képzési anyagok, amelyek segítségével gyorsan letölthető és szervezete informatikai és a felhasználók gyorsabb telepítése és használata a jelszó alaphelyzetbe állítása.  Figyelje az ezzel!


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Jelszó mutató hivatkozások dokumentáció alaphelyzetbe állítása
Az alábbiakban minden Azure Active Directory-jelszó-visszaállítás dokumentáció mutató hivatkozások:

* **Itt azért, mert netán problémákat tapasztal bejelentkezéskor?** Ha igen, [az alábbiakban hogyan módosíthatja, és a saját jelszó alaphelyzetbe állítása](active-directory-passwords-update-your-own-password.md).
* , [**Hogy hogyan működik**](active-directory-passwords-how-it-works.md) - megismerheti a szolgáltatást, és mit hat különböző összetevői minden tartalmaz
* [**Első lépések**](active-directory-passwords-getting-started.md) – megtudhatja, hogy miként lehetővé teszi a felhasználóknak, hogy alaphelyzetbe állítása és a felhő vagy a helyszíni jelszavukat
* [**Testreszabása**](active-directory-passwords-customize.md) – a Megjelenés és működés és a szolgáltatást, hogy a szervezet igényei működésének testreszabása
* [**Mélyebb get**](active-directory-passwords-get-insights.md) - az integrált jelentéskészítési képességeinek ismertetése
* [**Gyakori kérdések**](active-directory-passwords-faq.md) – gyakori kérdések a válaszok
* [**Hibaelhárítás**](active-directory-passwords-troubleshoot.md) – ismerje meg, a szolgáltatással gyorsan problémáinak megoldása
* [**Tudjon meg többet**](active-directory-passwords-learn-more.md) - lépjen a mély be a technikai részleteket az, hogy a szolgáltatás működése



[001]: ./media/active-directory-passwords-best-practices/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-best-practices/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-best-practices/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-best-practices/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-best-practices/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-best-practices/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-best-practices/007.jpg "Image_007.jpg"
