<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Microsoft utasításokhoz |} Microsoft Azure" 
    description="Megtudhatja, hogy miként Útbaigazítás használja a Microsoft Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a>Oktatóprogram: Azure Active Directory-integráció a Microsoft kattintson Útbaigazítás

Ebben az oktatóanyagban célja, ha meg szeretné jeleníteni a Microsoft Azure Active Directory és az Útbaigazításról integrációja.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   A megjelenő utasításokat a Microsoft-előfizetés

Ha a szövetséges útmutatást a Microsoft-előfizetés még nincs, e-mailben kérelmének "*service@DirectionsOnMicrosoft.com*".

Ebben az oktatóanyagban befejeztével rendelt útmutatást a Microsoft Azure Active Directory-felhasználók fogja tudni az alkalmazás használata az egyszeri bejelentkezés egyszeri bejelentkezési.

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrálását kapcsolatos utasításokhoz Microsoft engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-directions-microsoft-tutorial/IC786877.png "Eset")
##<a name="enabling-the-application-integration-for-directions-on-microsoft"></a>Az alkalmazás integrálását kapcsolatos utasításokhoz Microsoft engedélyezése

Ez a szakasz célja, hogyan kapcsolatos utasításokhoz Microsoft alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-directions-on-microsoft-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Microsoft kattintson Útbaigazítás, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-directions-microsoft-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-directions-microsoft-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-directions-microsoft-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-directions-microsoft-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Microsoft utasításokhoz**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-directions-microsoft-tutorial/IC786878.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Microsoft kattintson Útbaigazítás**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Eset] (./media/active-directory-saas-directions-microsoft-tutorial/IC793922.png "Eset")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Microsoft kattintson Útbaigazítás hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon alkalmazás integrációs **Microsoft kattintson Útbaigazítás** lapon kattintson **konfigurálása az egyszeri bejelentkezés** **Beállítása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés engedélyezése] (./media/active-directory-saas-directions-microsoft-tutorial/IC786879.png "Egyszeri bejelentkezés engedélyezése")

2.  **Hogyan szeretné, hogy jelentkezzen be Microsoft kattintson Útbaigazítás felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Microsoft Azure Active Directory Singel Sign-On] (./media/active-directory-saas-directions-microsoft-tutorial/IC786880.png "Microsoft Azure Active Directory Singel Sign-On")

3.  Az **Alkalmazás URL-cím beállítása** lapon, a bejelentkezési a URL-cím mezőbe írja be a **https://www.directionsonmicrosoft.com/user/login**, és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-directions-microsoft-tutorial/IC786881.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés a Microsoft kattintson Útbaigazítás** lapon kattintson a **metaadat-alapú letöltése**elemre, és mentse a fájlt a számítógépen helyben metaadat.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-directions-microsoft-tutorial/IC786882.png "Egyszeri bejelentkezés beállítása")

5.  A metaadat-fájl küldése a Microsoft támogatási csoport utasításokhoz (*service@DirectionsOnMicrosoft.com*). Ahhoz, hogy a Microsoft támogatási csoport keresse meg a szövetséges webhely tagság utasításokat, tartalmazza a vállalat adatainak az e-mailben.

    >[AZURE.NOTE] Egyszeri bejelentkezés kapcsolatos utasításokhoz Microsoft kell a Microsoft támogatási csoport irányú engedélyezhető.
Értesítést kap, ha az egyszeri bejelentkezés engedélyezve van.

6.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-directions-microsoft-tutorial/IC786883.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Nincs teendő állíthatja be a Microsoft kattintson Útbaigazítás kiépítési felhasználói számára nem.  
Egy tevékenységhez rendelt felhasználó megkísérel bejelentkezni a Microsoft access panelen a utasításokhoz, a Microsoft kattintson Útbaigazítás ellenőrzi, hogy létezik-e a felhasználó. Ha nincs még nincs felhasználói fiók érhető el, a Microsoft kattintson Útbaigazítás automatikusan létrehozott.
##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-directions-on-microsoft-perform-the-following-steps"></a>Felhasználók hozzárendelése Microsoft kattintson Útbaigazítás, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  A **Microsoft kattintson Útbaigazítás **alkalmazás integrációs lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-directions-microsoft-tutorial/IC786884.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-directions-microsoft-tutorial/IC767830.png "Igen")
