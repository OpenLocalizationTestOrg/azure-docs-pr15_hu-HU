<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a által Desire2Learn Brightspace |} Microsoft Azure" 
    description="Megtudhatja, hogy miként által Desire2Learn Brightspace használata az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-brightspace-by-desire2learn"></a>Oktatóprogram: Azure Active Directory-integráció a Brightspace Desire2Learn szerint

Ebben az oktatóanyagban célja Azure és Brightspace által Desire2Learn integrációja megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Egy Brightspace Desire2Learn egyszeri bejelentkezés által engedélyezett előfizetés

Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók által Desire2Learn Brightspace rendelt tudnak egyetlen bejelentkezés az alkalmazásba, a Brightspace Desire2Learn vállalati webhely (a szolgáltatás által kezdeményezett szolgáltató bejelentkezési), vagy a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)segítségével.

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás által Desire2Learn Brightspace integrációját engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798957.png "Eset")
##<a name="enabling-the-application-integration-for-brightspace-by-desire2learn"></a>Az alkalmazás által Desire2Learn Brightspace integrációját engedélyezése

Ez a szakasz célja, hogyan Brightspace Desire2Learn szerint az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-brightspace-by-desire2learn-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás által Desire2Learn Brightspace integrációját, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Brightspace Desire2Learn szerint**.

    ![Apllication gyűjtemény] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798958.png "Apllication gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Brightspace Desire2Learn által**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Brightspace Desire2Learn szerint] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC799321.png "Brightspace Desire2Learn szerint")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználók által Desire2Learn Brightspace hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **által Desire2Learn Brightspace** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798959.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Brightspace Desire2Learn által a felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798960.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás URL-cím beállítása** lapon hajtsa végre az alábbi lépéseket:

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798961.png "Állítsa be az App URL-címe")

    1.  A **Bejelentkezési a URL-cím** mezőbe írja be a jelentkezhet be a **Brightspace Desire2Learn által** a felhasználók által használt URL-CÍMÉT (például: *https://partnershowcase.desire2learn.com/Shibboleth.sso/Login?entityID=https://sts.windows-ppe.net/5caf9349-fd93-4a74-b064-0070f65bfb49/&target=https%3A%2F%2Fpartnershowcase.desire2learn.com%2Fd2l%2FshibbolethSSO%2Faspinfo.asp*).
    2.  Kattintson a **Tovább** gombra

4.  **Konfigurálása az egyszeri bejelentkezés Brightspace Desire2Learn által a** lapon a metaadat-alapú letöltéséhez kattintson a **metaadat-alapú letöltése**, és mentse a metaadat-alapú számítógépen parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798962.png "Egyszeri bejelentkezés beállítása")

5.  Küldje el a letöltött metaadat-fájlt a Brightspace Desire2Learn támogatási csoport által.

    >[AZURE.NOTE] A tényleges SSO konfigurációs lehetőségek a Brightspace Desire2Learn támogatási csoport által rendelkezik.
Értesítést fog kapni, amikor az egyszeri bejelentkezés engedélyezve van-előfizetéséhez.

6.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798963.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználók által Desire2Learn Brightspace való bejelentkezéshez, hogy ki kell építenie Brightspace be Desire2Learn szerint.  
Brightspace által Desire2Learn, amíg a felhasználói fiókok kell Desire2Learn támogatási csoport által a Brightspace hozható létre.

>[AZURE.NOTE] Bármely más Brightspace Desire2Learn felhasználói fiók létrehozása eszközök is használhatja, illetve API-k által biztosított Brightspace által Desire2Learn kiépítése Azure Active Directory felhasználói fiókok.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-brightspace-by-desire2learn-perform-the-following-steps"></a>Felhasználók által Desire2Learn Brightspace rendelni, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  A **Brightspace által Desire2Learn **alkalmazás integrációs lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798964.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
