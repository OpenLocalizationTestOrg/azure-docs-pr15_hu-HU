<properties
    pageTitle="Az Azure Active Directory-kompatibilis alkalmazásokat eszköz-alapú feltételes hozzáférési házirend beállítása |} Microsoft Azure"
    description="Az Azure Active Directory-kompatibilis alkalmazásokat feltételes access eszköz-alapú házirendek beállítása."
    services="active-directory"
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
    ms.date="09/14/2016"
    ms.author="markvi"/>


# <a name="set-device-based-conditional-access-policy-for-azure-active-directory-connected-applications"></a>Az Azure Active Directory-kompatibilis alkalmazásokat eszköz-alapú feltételes hozzáférési házirend beállítása


Azure Active Directory (Azure Active Directory), eszköz alapú feltételes access segít a szervezeti erőforrásokat védelme:

- Az Access megpróbálja ismeretlen vagy nem felügyelt eszközökről.
- Eszközök, amelyek nem felelnek meg a szervezet a biztonsági házirendek:.

Feltételes access áttekintése lásd: az [Azure Active Directory-feltételes hozzáférést](active-directory-conditional-access.md).

Beállíthatja, hogy ezeket az alkalmazásokat védelme feltételes access eszköz-alapú házirendek:

- Az Office 365 SharePoint online-ban, a szervezet webhelyek és dokumentumok védelme
- Az Office 365 Exchange online-ban, a szervezet e-mailek védelme
- Azure AD-hitelesítés csatlakozó (szoftver) szolgáltatás alkalmazásként szoftver
- A helyszíni alkalmazások közzétett Azure Active Directory szolgáltatásalkalmazás-Proxy szolgáltatások használatával

A feltételes eszköz-alapú hozzáférés beállítása házirendet, az Azure-portálon lépjen az adott alkalmazás a címtárban.


  ![Az Azure portál címtár-alkalmazások listájából] (./media/active-directory-conditional-access-policy-connected-applications/01.png "Alkalmazások")


Jelölje ki azt az alkalmazást, és kattintson a feltételes hozzáférési házirend beállítása a **beállítás** fülre.  


  ![Az alkalmazás beállítása] (./media/active-directory-conditional-access-policy-connected-applications/02.png "Eszköz hozzáférési szabályok alapján")




Egy eszköz-alapú feltételes hozzáférési házirend beállítása **eszköz alapú hozzáférési szabályok** csoportjában a **Hozzáférési szabályok engedélyezése**, jelölje be a **beállítást**.

Egy eszköz-alapú feltételes hozzáférési házirend három összetevője:

- **Vonatkozik**-e. A házirend vonatkozik felhasználók körét.

- **Eszköz szabályokat**. A feltételek egy eszközt kell megfelelnie előtt hozzá tud férni az alkalmazást.

- **Alkalmazás kényszerítési**. Az ügyfél-alkalmazások (és a böngésző natív) a házirend vonatkozik.

  ![Egy eszköz-alapú hozzáférési házirend három összetevője] (./media/active-directory-conditional-access-policy-connected-applications/03.png "Eszköz hozzáférési szabályok alapján")


## <a name="select-the-users-the-policy-applies-to"></a>Jelölje ki a felhasználókat a házirend vonatkozik.

A **Vonatkozó** csoportban jelölje be a házirend vonatkozik felhasználók körét.

Egy access-házirend hatókör felhasználók létrehozásakor két lehetőség közül választhat:

- **Minden felhasználónak**. A szabály alkalmazása az összes olyan felhasználókkal, akik az alkalmazást.

- **Csoportok**. Korlátozza a házirendet, amely a felhasználók, akik az adott csoport tagja.

![Az alkalmazás házirendet, az összes felhasználó vagy csoport] (./media/active-directory-conditional-access-policy-connected-applications/11.png "Alkalmazása")


 A felhasználó kizárása egy házirendet, jelölje be a **kivételével** jelölőnégyzetet. Ez akkor hasznos, ha a szükséges engedélyek adjon meg egy adott felhasználóhoz, az alkalmazás ideiglenes eléréséhez. Ezzel a beállítással, például a felhasználók egy része, amelyek nem készen áll a feltételes access eszközök esetén. Az eszközök előfordulhat, hogy kell még nem regisztrált, vagy azok körülbelül megfelelőségi ki kell.


## <a name="select-the-conditions-that-devices-must-meet"></a>Jelölje ki a megfelelő feltételekhez tartozó eszközök meg kell felelniük

Az alkalmazás eléréséhez, meg kell felelnie a **Eszközt szabályok** beállítása a feltételek egy eszközt.

Beállíthatja, hogy eszköz alapuló feltételes hozzáférés az ilyen eszköz:

- A Windows 10 évforduló frissítés, a Windows 8.1 és Windows 7 rendszerben
- A Windows Server 2016-ban, a Windows Server 2012 R2, a Windows Server 2012 és a Windows Server 2008 R2 rendszerben
- az iOS-eszközök (iPad, iPhone-alapú)
- Android-eszközön

Mac támogatása hamarosan.

  ![Az alkalmazás házirendet, amely eszközök] (./media/active-directory-conditional-access-policy-connected-applications/04.png "Alkalmazások")

 >[AZURE.NOTE] A tartományhoz tartozó és Azure Active Directory-tartományhoz eszközök közötti különbségek tudni [használja a Windows 10-es eszköz, a munkahelyi](active-directory-azureadjoin-windows10-devices.md)megjelenítéséhez.

Az eszköz szabályok két lehetőség közül választhat:

- Az **összes eszközön meg kell felelnie**. Az összes eszközt platformokon elérhetik az alkalmazást, meg kell felelnie. Nem támogatott feltételes access eszköz-alapú platformon futó eszközök van megtagadja a hozzáférést.

- A **kijelölt eszközök csak meg kell felelnie**. Csak az adott eszköz platformokon meg kell felelnie. Más platformokhoz készült Worddel és a más platformokon futó verziói, amelyek férhet hozzá az alkalmazás, hozzáférhetnek.

  ![A hatókör az eszköz szabályok beállítása] (./media/active-directory-conditional-access-policy-connected-applications/05.png "Alkalmazások")

Azure Active Directory-tartományhoz-e kompatibilis, ha vannak megjelölve **kompatibilis** a címtárban Intune, illetve egy harmadik fél mobileszközén a projektirányítási rendszerek, amely integrálódik az Azure Active Directory.

A tartományhoz eszközt kompatibilis ha:

- Azure Active Directory regisztrált azt. Sok szervezetek megbízható eszközök tartományhoz csatlakoztatott eszközök tekinti át.
- Ez hibaként **kompatibilis** az Azure Active Directory System Center konfigurációkezelőjének 2016 által.

 ![A tartományhoz eszközök, amelyek kompatibilis] (./media/active-directory-conditional-access-policy-connected-applications/06.png "Eszköz szabályok")

Személyes eszközök Windows ha vannak megjelölve **kompatibilis** a címtárban Intune, illetve egy harmadik fél mobileszközén a projektirányítási rendszerek, amely integrálódik az Azure Active Directory kompatibilisek.

Nem Windows kompatibilis, ha vannak megjelölve **kompatibilis** a címtárban Intune szerint.

 >[AZURE.NOTE] Arról, hogy miként állíthatja be az eszközön a megfelelőségi Azure AD a különböző kezelési rendszerekben további tudnivalókért lásd: [Azure Active Directory-feltételes hozzáférést](active-directory-conditional-access.md).


Kijelölhet egy vagy több eszköz platformokon eszköz-alapú hozzáférési házirend. Ide tartoznak a Android, az iOS, a Windows Mobile (Windows 8.1 telefonokon és táblagépeken) és a Windows (minden más Windows eszközről, többek között az összes Windows 10-es eszköz).
Házirend-értékelést csak a kijelölt platformokon fordul elő. Próbálja meg az access eszközt nem fut a kijelölt platformon közül, az eszköz érhető el az alkalmazást, ha a felhasználónak van hozzáférési jogosultsága. Nincs eszköz házirend kiértékeli.

![Jelölje ki az eszköz szabályokat platformok] (./media/active-directory-conditional-access-policy-connected-applications/07.png "Eszköz szabályokat")


## <a name="set-policy-evaluation-for-a-type-of-application"></a>Házirend-értékelést alkalmazás típusú beállítása

**Alkalmazás végrehajtás** szakaszában állítsa be a házirendet kiértékeli a felhasználó és eszköz-hozzáférési kérelmek a típusát.

Az alkalmazás, amelyet fel szeretne venni típusú két lehetőség közül választhat:

- Böngészőben és a saját alkalmazások
- Csak a saját alkalmazások

![Választható böngészőben vagy a saját alkalmazások] (./media/active-directory-conditional-access-policy-connected-applications/08.png "Alkalmazások")

A hivatkozási hozzáférési házirendet az alkalmazásokat, jelölje be **a böngészőben és a natív alkalmazásokhoz**. Ezután felvehet:

- Böngésző (például: Microsoft Edge a Windows 10) vagy a Safari az iOS.
- Alkalmazások, használja az Active Directory hitelesítési tár (ADAL) bármely platformon vagy, amely a WebAccountManager (WAM) API-t a Windows 10-ben.

>[AZURE.NOTE] További információt a támogatott böngészők és egyéb szempontok egy felhasználóhoz, akinek van elérése eszköz alapú, tanúsítvány szervezet védett alkalmazást, olvassa el a [Azure Active Directory feltételes access](active-directory-conditional-access.md)című témakört.

## <a name="help-protect-email-access-from-exchange-activesync-based-applications"></a>Az Exchange ActiveSync-alapú alkalmazások e-mail cím védelme

Az Office 365-ben az Exchange Online-alkalmazásokban az Exchange ActiveSync is használhatja az Exchange ActiveSync-alapú levelezési alkalmazások e-mail elérésének letiltása.

![Az Exchange ActiveSync megfelelőségi beállításai] (./media/active-directory-conditional-access-policy-connected-applications/09.png "Alkalmazások")

![A kompatibilis eszközt elérheti e-mailjeit megkövetelése] (./media/active-directory-conditional-access-policy-connected-applications/10.png "Alkalmazások")


## <a name="next-steps"></a>Következő lépések

- [Azure Active Directory feltételes hozzáférés](active-directory-conditional-access.md)
