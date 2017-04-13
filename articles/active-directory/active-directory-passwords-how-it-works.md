<properties
    pageTitle="Hogyan működik: Azure Active Directory-jelszó szolgáltatások |} Microsoft Azure"
    description="További tudnivalók: Azure Active Directory-jelszó-kezelés, ahol a felhasználók regisztrálni, alaphelyzetbe állítása és jelszavukat, beleértve a különböző összetevőket és rendszergazdák adja meg, ahol jelentést, és engedélyezze a helyszíni Active Directory jelszavak kezelése."
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

# <a name="how-password-management-works"></a>Hogyan működik a jelszó kezelése

> [AZURE.IMPORTANT] **Itt azért, mert netán problémákat tapasztal bejelentkezéskor?** Ha igen, [az alábbiakban hogyan módosíthatja, és a saját jelszó alaphelyzetbe állítása](active-directory-passwords-update-your-own-password.md).

Jelszó-kezelés az Azure Active Directory tevődik több logikai összetevők, az alábbiakban ismertetjük.  Kattintson a minden hivatkozásra, ha többet szeretne tudni, hogy összetevőt.

- [**Jelszó Kezelőportálja konfiguráció**](#password-management-configuration-portal) – rendszergazdák szabályozhatja, hogy más metszettel a jelszavak kezelésének a bérlők a navigálással az [Azure Kezelőportálja](https://manage.windowsazure.com)konfigurálása a címtár fülre.
- [**Felhasználói regisztrációs portál**](#user-registration-portal) – önálló regisztrálhatnak felhasználói jelszó alaphelyzetbe állításához, a webes portál keresztül.
- [**Felhasználói jelszó alaphelyzetbe állítása portál**](#user-password-reset-portal) – felhasználók visszaállíthatják saját használatával számos különböző kihívásokkal megfelelően a rendszergazda által felügyelt jelszóházirend alaphelyzetbe állítása
- [**Felhasználói jelszó módosítása portál**](#user-password-change-portal) – felhasználója képes legyen módosítani a saját jelszó bármikor írja be a régi jelszót, és válassza az új jelszót a webes portál használatával
- [**Jelszó felügyeleti jelentések**](#password-management-reports) – rendszergazdák megtekintése és elemzése a jelszó alaphelyzetbe állítása és nyilvántartási tevékenység a bérlői webhelyemen navigálással az [Azure adatkezelési portál](https://manage.windowsazure.com) lapja a címtár "Jelentés" a "Tevékenység jelentések" szakaszhoz
- [**Azure AD Connect jelszó visszaírást összetevője**](#password-writeback-component-of-azure-ad-connect) – rendszergazdák tetszés szerint engedélyezheti a jelszó visszaírást szolgáltatás telepítése Azure AD Connect kezelésének engedélyezése összevont vagy jelszó szinkronizálja a felhőből felhasználói jelszavak.

## <a name="password-management-configuration-portal"></a>Jelszó beállítása Kezelőportálja segítségével
Beállíthatja, hogy egy adott könyvtár az [Azure Kezelőportálja](https://manage.windowsazure.com) segítségével a könyvtár **konfigurálása** lapon a **Felhasználói jelszó alaphelyzetbe állítása házirend** című navigálással jelszó adatkezelési házirend.  A konfigurációs lapján megadhatja, hogy hogyan kezeli az jelszavakat a szervezet számos tulajdonságát együtt:

- Engedélyezése és letiltása a jelszó alaphelyzetbe állítása az összes felhasználó a címtárban
- Kihívásokkal számának beállítása egy felhasználó (egy vagy két) kell hajtsa végre a jelszó alaphelyzetbe állítása
- Beállítás az adott típusú kihívásokkal engedélyezni szeretné a felhasználóknak a szervezet az alábbi lehetőségek közül:
 - Mobiltelefon (ellenőrzőkódot szöveg vagy a hanghívás)
 - Irodai telefon (hanghívás)
 - Másodlagos E-mail (ellenőrzőkódot mailben)
 - Biztonsági kérdések (Tudásbázis-alapú hitelesítés)
- Kérdések számának beállítása egy felhasználó regisztrálnia kell biztonsági kérdések hitelesítési mód (csak akkor látható, ha engedélyezve van a biztonsági kérdések) használatához
- Kérdések számának beállítása egy felhasználó kell megadnia visszaállítása biztonsági kérdések hitelesítési mód (csak akkor látható, ha engedélyezve van a biztonsági kérdések) használata során
- Előre konzerv, honosított, biztonsági kérdések, hogy a felhasználó választhatja a regisztrálásakor használt jelszót használja (csak akkor látható, ha engedélyezve van a biztonsági kérdések) alaphelyzetbe állítása
- Az egyéni biztonsági kérdések, hogy a felhasználó választhatja a regisztrálásakor használt jelszó megadása (csak akkor látható, ha engedélyezve van a biztonsági kérdések) alaphelyzetbe állítása
- Jelszó alaphelyzetbe állításához, amikor az alkalmazás hozzáférést Panel [http://myapps.microsoft.com](http://myapps.microsoft.com)regisztrálhatja felhasználók megkövetelése.
- Konfigurálható számú nap után előzőleg regisztrált adataikat újra megerősítéséhez igénylő felhasználók letelik (csak akkor látható, ha engedélyezve van a kényszerített regisztráció)
- Egy egyéni segélyszolgálat e-mail vagy az URL-címet, amely abban az esetben, ha azokat a jelszavak alaphelyzetbe állítása problémám van a felhasználó számára megjelenik
- Engedélyezése vagy letiltása a a jelszó visszaírást lehetőséget (Ha a jelszó visszaírást AAD összekapcsolás funkcióval lett telepítve)
- A jelszó visszaírást agent állapotának megtekintése (Ha a jelszó visszaírást AAD összekapcsolás funkcióval lett telepítve)
- Engedélyezése a felhasználóknak értesítő e-mailek, a saját jelszó alaphelyzetbe a következő (az [Azure Kezelőportálja](https://manage.windowsazure.com) **értesítések** szakaszában található)
- Egyéb a rendszergazdák saját jelszavukat (található az **értesítések** szakaszban az [Azure Kezelőportálja](https://manage.windowsazure.com) alaphelyzetbe, amely lehetővé teszi, hogy az értesítő e-mailek rendszergazdák számára
- Portál védjegyzés a felhasználó jelszavának alaphelyzetbe állítása és a jelszó alaphelyzetbe állítása a szervezet emblémáját és nevét tartalmazó e-mailek a bérlő védjegyzés testreszabási funkció (a **Címtár-tulajdonságok** szakaszában az [Azure adatkezelési portál](https://manage.windowsazure.com) használatával

Jelszó kezelése a szervezetben való beállításáról további információért lásd: [első lépések: Azure Active Directory jelszavak kezelését](active-directory-passwords-getting-started.md).

##<a name="user-registration-portal"></a>Felhasználói regisztrációs portál
Felhasználók is tudják használni a jelszó alaphelyzetbe állítása, mielőtt a felhőben felhasználói fiókjuk frissíteni kell a megfelelő hitelesítési adatok is haladnia a megfelelő számú jelszó alaphelyzetbe állítása kihívásokkal kapcsolatban a rendszergazda által meghatározott biztosítása érdekében.  A rendszergazdák is megadhatja a hitelesítési adatait a felhasználó nevében az Azure vagy az Office web portálokról, DirSync használatával / Azure AD Connect vagy a Windows PowerShell.

Azonban a felhasználók saját adatokat regisztrálhat inkább lenne, ha azt is biztosít egy weblap, hogy a felhasználók is nyissa meg ezt az információt a biztosítása érdekében.  Ezen az oldalon lehetővé teszi a felhasználóknak, hogy adja meg a hitelesítési adatait megfelelően a jelszó alaphelyzetbe állítása házirendeket, amelyek a szervezet engedélyezte.  Az adatok ellenőrzését követően a felhő felhasználói fiókot, várjon egy kis időt fiók-helyreállítás használandó vannak tárolva. Az alábbiakban Mi a regisztrációs portál hasonlít:

  ![][001]

További tudnivalókért lásd: [első lépések: Azure Active Directory jelszavak kezelését](active-directory-passwords-getting-started.md) és [ajánlott eljárások: Azure Active Directory jelszavak kezelését](active-directory-passwords-best-practices.md).

##<a name="user-password-reset-portal"></a>Felhasználói jelszó alaphelyzetbe állítása portál
Miután engedélyezte a önkiszolgáló jelszó alaphelyzetbe állítása, a szervezet önkiszolgáló jelszó alaphelyzetbe állítása házirend beállítása, és biztosítani, hogy a felhasználók rendelkezik a megfelelő kapcsolattartási adatokat a címtárban, a szervezet felhasználói tudják saját automatikusan bármelyik weblapról egy munkahelyi vagy iskolai fiókkal való bejelentkezéshez (például [portal.microsoftonline.com](https://portal.microsoftonline.com)) használó jelszavak alaphelyzetbe állítása. Például a következő oldalon megjelenik a felhasználók egy **nem tud hozzáférni a fiókjához?** hivatkozásra.

  ![][002]

Erre a hivatkozásra kattintva elindítja az önkiszolgáló jelszó alaphelyzetbe állítása portálon.

  ![][003]

Ha többet szeretne tudni, hogyan a felhasználók saját jelszókezelők állíthatják alaphelyzetbe, lásd: [első lépések: Azure Active Directory jelszavak kezelését](active-directory-passwords-getting-started.md).

##<a name="user-password-change-portal"></a>Felhasználói jelszó módosítása portál
Felhasználók a saját jelszavukat módosítani szeretné, ha azokat a a jelszó módosítása portál használatával bármikor megteheti.  Felhasználók hozzá tudnak férni az Access Panel profillapon vagy a "a jelszó módosítása" hivatkozás az Office 365-alkalmazásokból, kattintson a jelszó módosítása portálra.  Abban az esetben, ha az azok a jelszó érvényességi idejének, felhasználók is megkéri módosításukhoz automatikusan bejelentkezéskor.

  ![][004]

Mindkét esetben a jelszó visszaírást engedélyezve van, és a felhasználó vagy identitásszolgáltatóval összevont vagy jelszó-szinkronizálás módon, ha ilyen módosított jelszavak kerülnek vissza a helyszíni Active Directory. Az alábbiakban Mi a jelszó módosítása portál hasonlít:

  ![][005]

Hogyan felhasználója képes legyen módosítani saját helyszíni Active Directory jelszavukat kapcsolatos további információért lásd: [első lépések: Azure Active Directory jelszavak kezelését](active-directory-passwords-getting-started.md).

##<a name="password-management-reports"></a>Jelszó felügyeleti jelentések
A **Tevékenység naplók** szakaszban keresi, lépjen a **jelentések** fülre, látni fogja a két jelszót Management jelentés: **az új jelszó kérésének tevékenység** és a **jelszó alaphelyzetbe állítása az nyilvántartási tevékenység**.  Két jelentések használata esetén a felhasználók rögzítése és a jelszó alaphelyzetbe állításához, a szervezet használatával áttekintést kaphat. Az alábbiakban ezeket a jelentéseket néz az [Azure Kezelőportálja segítségével](https://manage.windowsazure.com):

  ![][006]

További tudnivalókért lásd: [kérjen mélyebb: Azure Active Directory-jelszó felügyeleti jelentések](active-directory-passwords-get-insights.md).

##<a name="password-writeback-component-of-azure-ad-connect"></a>Azure AD Connect jelszó visszaírást összetevője
A felhasználók a szervezet a jelszavakat a helyszíni környezet (vagy összevonási vagy a jelszó-szinkronizálás) származik, telepítheti az Azure AD Connect ahhoz, hogy ezek a jelszavak közvetlenül a felhőből frissítése legújabb verzióját.  Ez azt jelenti, hogy amikor a felhasználók elfelejti, vagy a AD jelszavát módosítani szeretné, tehetik így közvetlenül az internetről.  Az alábbiakban a hol találhatók a jelszó visszaírást az Azure AD Connect telepítővarázslóban:

  ![][007]

Azure AD Connect kapcsolatos további tudnivalókért lásd: [első lépések: Azure AD Connect](active-directory-aadconnect.md). Jelszó visszaírást kapcsolatos további tudnivalókért lásd: [első lépések: Azure Active Directory jelszavak kezelését](active-directory-passwords-getting-started.md).


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Jelszó mutató hivatkozások dokumentáció alaphelyzetbe állítása
Az alábbiakban minden Azure Active Directory-jelszó-visszaállítás dokumentáció mutató hivatkozások:

* **Itt azért, mert netán problémákat tapasztal bejelentkezéskor?** Ha igen, [az alábbiakban hogyan módosíthatja, és a saját jelszó alaphelyzetbe állítása](active-directory-passwords-update-your-own-password.md).
* [**Első lépések**](active-directory-passwords-getting-started.md) – megtudhatja, hogy miként lehetővé teszi a felhasználóknak, hogy alaphelyzetbe állítása és a felhő vagy a helyszíni jelszavukat
* [**Testreszabása**](active-directory-passwords-customize.md) – a Megjelenés és működés és a szolgáltatást, hogy a szervezet igényei működésének testreszabása
* [**Ajánlott eljárások**](active-directory-passwords-best-practices.md) – megtudhatja, hogy miként telepítését gyorsan és hatékonyan a jelszavak a szervezet kezelése
* [**Mélyebb get**](active-directory-passwords-get-insights.md) - az integrált jelentéskészítési képességeinek ismertetése
* [**Gyakori kérdések**](active-directory-passwords-faq.md) – gyakori kérdések a válaszok
* [**Hibaelhárítás**](active-directory-passwords-troubleshoot.md) – ismerje meg, a szolgáltatással gyorsan problémáinak megoldása
* [**Tudjon meg többet**](active-directory-passwords-learn-more.md) - lépjen a mély be a technikai részleteket az, hogy a szolgáltatás működése



[001]: ./media/active-directory-passwords-how-it-works/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-how-it-works/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-how-it-works/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-how-it-works/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-how-it-works/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-how-it-works/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-how-it-works/007.jpg "Image_007.jpg"
