<properties
    pageTitle="Azure AD-alkalmazás proxyval alkalmazások közzététele |} Microsoft Azure"
    description="Azure Active Directory szolgáltatásalkalmazás-Proxy a felhőbe a helyszíni alkalmazások közzététele."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="kgremban"/>


# <a name="publish-applications-using-azure-ad-application-proxy"></a>Azure Active Directory szolgáltatásalkalmazás-Proxy alkalmazások közzététele

Azure Active Directory szolgáltatásalkalmazás-Proxy segít, hogy a távoli dolgozók támogatja az interneten keresztül érhető el az alkalmazások helyszíni közzétéve. Ez a pont már rendelkeznie kell [az Azure klasszikus portál szolgáltatásalkalmazás-Proxy engedélyezve van](active-directory-application-proxy-enable.md). Ez a cikk végigvezeti az alkalmazásokat, amelyek a helyi hálózaton futtatja, és adja meg a biztonságos távelérési a hálózaton kívüli közzététele. Ez a cikk elvégzése után is készen áll a konfigurálása a személyre szabott információk, illetve biztonsági követelményeknek.

> [AZURE.NOTE] Szolgáltatásalkalmazás-Proxy lehetővé teszi az elérhető, csak akkor, ha a Premium vagy az Azure Active Directory Basic edition frissített. További tudnivalókért lásd: az [Azure Active Directory kiadásai](active-directory-editions.md).

## <a name="publish-an-app-using-the-wizard"></a>A varázsló segítségével alkalmazás közzététele

1. Jelentkezzen be rendszergazdaként az [Azure klasszikus portálon](https://manage.windowsazure.com/).
2. Nyissa meg az Active Directory, és jelölje ki a könyvtárat, ha szolgáltatásalkalmazás-Proxy engedélyezte.

    ![Az Active Directory - ikon](./media/active-directory-application-proxy-publish/ad_icon.png)

3. Kattintson az **alkalmazások** fülre, és kattintson a **Hozzáadás** gombra, a képernyő alján

    ![Alkalmazás hozzáadása](./media/active-directory-application-proxy-publish/aad_appproxy_selectdirectory.png)

4. Válassza a **Közzététel az alkalmazás, amely a hálózatán kívülről elérhető lesz**.

    ![Alkalmazás, amely lesz elérhető a hálózaton kívüli közzététele](./media/active-directory-application-proxy-publish/aad_appproxy_addapp.png)

5. Adja meg az alkalmazás a következő információkat:

    - **Név**: A könnyen megjegyezhető nevet az alkalmazás. A címtáron belül egyedinek kell lennie.
    - **Belső URL-cím**: A cím, az alkalmazás Proxy összekötő használja az alkalmazás a magánhálózaton belül eléréséhez. Akkor megadhatja, hogy egy adott helyre szeretné közzétenni, míg a többi kiszolgáló közzé nem tett kódmentes kiszolgálón. Ezzel a módszerrel közzététele különböző helyeken ugyanarra a kiszolgálóra, és adjon egyenként a saját neve és a hozzáférési szabályokat.

        > [AZURE.TIP] Ha egy elérési utat tesz közzé, ügyeljen arra, hogy azt a szükséges képek, parancsfájlok és az alkalmazás stíluslapok. Például ha az alkalmazás https://yourapp/app van, és használja az https://yourapp/media található képek, majd kell közzétenni, https://yourapp/ a mappa elérési útjaként.

    - **Előhitelesítés módszer**: hogyan szolgáltatásalkalmazás-Proxy ellenőrzi a felhasználót, mielőtt a meghatalmazást őket az alkalmazás. A legördülő menüből válassza a lehetőségek közül.

        - Azure Active Directory: Szolgáltatásalkalmazás-Proxy átirányítja a felhasználóknak, hogy jelentkezzen be az Azure Active Directory, amely ellenőrzi a számukra megadott engedélyektől a könyvtár és az alkalmazás.
        - Átadó: Felhasználók nem kell az alkalmazás eléréséhez hitelesítést végezni.

    ![Szolgáltatásalkalmazás tulajdonságainak](./media/active-directory-application-proxy-publish/aad_appproxy_appproperties.png)  

6. A varázsló végzett, kattintson a pipa ikonra a képernyő alján. Az alkalmazás letöltése Azure Active Directory van megadva.


## <a name="assign-users-and-groups-to-the-application"></a>Felhasználók és csoportok hozzárendelése az alkalmazás

Ahhoz, hogy a felhasználók a közzétett alkalmazás eléréséhez meg kell osztani egyenként vagy csoportosan. (Ne felejtse el rendeljen saját magához is elérheti,.) Ehhez az, hogy minden felhasználó rendelkezik-e az Azure egyszerű vagy magasabb licenccel. Hozzárendelheti a licenceket, egyenként vagy csoportokba. További információt talál [az alkalmazás hozzárendelése felhasználók](active-directory-applications-guiding-developers-assigning-users.md) . 

A későbbi intézkedést igénylő előhitelesítésre alkalmazásai ez engedélyt alkalmazással. Nem igénylő előhitelesítésre alkalmazásokat, a felhasználók továbbra is rendelhetők a alkalmazásba, hogy megjelenik az alkalmazás a listában, például az alkalmazások parancsot.

1. Az alkalmazás hozzáadása varázsló a Befejezés után megjelenik a rövid útmutató lap az alkalmazás. Kinek van hozzáférése az alkalmazás kezeléséhez kattintson a **felhasználók és csoportok ablakot**.

    ![Rövid útmutató az alkalmazás Proxy rendelnie - képernyőképe](./media/active-directory-application-proxy-publish/aad_appproxy_usersgroups.png)

2. Adott csoportokat a címtárában kereshet, illetve megjelenítheti az összes felhasználó. A keresési eredmények megjelenítéséhez kattintson a pipára.

    ![A csoportok és felhasználók - kép keresése](./media/active-directory-application-proxy-publish/aad_appproxy_search.png)

2. Jelölje be a minden felhasználó vagy csoport el ehhez az alkalmazáshoz rendelni, majd kattintson a **hozzárendelni**kívánt. Ez a művelet megerősítését kéri.

> [AZURE.NOTE] Windows-hitelesítés integrált alkalmazások hozzárendelheti az csak a felhasználók és csoportok, hogy a rendszer szinkronizálja a helyszíni Active Directoryból. A felhasználók, akik jelentkezzen be Microsoft-fiók és vendégek az Azure Active Directory szolgáltatásalkalmazás-Proxy közzétett alkalmazások nem lehet kiosztani. Győződjön meg arról, hogy a felhasználók hitelesítő adatok, az alkalmazás webkiszolgálón teszi közzé a tartományra részét képező jelentkezzen be.

## <a name="test-your-published-application"></a>A közzétett alkalmazás tesztelése

Amikor az alkalmazás közzétett, tesztelheti azt az URL-címre közzétett navigálással. Győződjön meg arról, hogy akkor elérhetik azt, hogy helyesen alapú-e, és, hogy minden a várt módon működik. Ha problémát tapasztal, illetve hibaüzenet, próbálja ki [hibaelhárítási útmutatójának](active-directory-application-proxy-troubleshoot.md).

## <a name="configure-your-application"></a>Az alkalmazás beállítása

Módosítsa a közzétett alkalmazások, vagy állítsa be a Speciális beállítások a lap. Ezen a lapon testre szabhatja az alkalmazás nevét és a embléma feltöltése. Hozzáférési szabályokat, például a előhitelesítésre módszer vagy többtényezős hitelesítés is kezelheti.

![Speciális beállítások](./media/active-directory-application-proxy-publish/aad_appproxy_configure.png)


Alkalmazások közzététele után használatával az Azure Active Directory szolgáltatásalkalmazás-Proxy, azok listájában jelennek meg az alkalmazások az Azure Active Directory, és ott kezelheti őket.

Szolgáltatásalkalmazás-Proxy szolgáltatások letiltása alkalmazások közzétételét követően, ha már nincs elérhető a személyes hálózatán kívülről. Ez a program nem törli az alkalmazásokat.

Megtekintheti egy alkalmazást, és győződjön meg arról, hogy az elérhető, kattintson duplán az alkalmazás nevére. A szolgáltatásalkalmazás-Proxy szolgáltatás ki van kapcsolva, és az alkalmazás nem érhető el, ha egy figyelmeztető üzenet jelenik meg a képernyő tetején.

Ha törölni szeretne egy alkalmazást, jelöljön ki egy alkalmazást a listában, és kattintson a **törlése**gombra.

## <a name="next-steps"></a>Következő lépések

- [A saját tartománynév alkalmazások közzététele](active-directory-application-proxy-custom-domains.md)
- [Egyszeri bejelentkezés engedélyezése](active-directory-application-proxy-sso-using-kcd.md)
- [Feltételes elérésének engedélyezése](active-directory-application-proxy-conditional-access.md)
- [Követelések tartsa szem előtt alkalmazások használata](active-directory-application-proxy-claims-aware-apps.md)

A legújabb hírek és frissítések tanulmányozza a [blog szolgáltatásalkalmazás-Proxy](http://blogs.technet.com/b/applicationproxyblog/)
