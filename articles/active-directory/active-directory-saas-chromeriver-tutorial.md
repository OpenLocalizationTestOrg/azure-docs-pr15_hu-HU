<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Chromeriver |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Chromeriver az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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


#<a name="tutorial-azure-active-directory-integration-with-chromeriver"></a>Oktatóprogram: Azure Active Directory-integráció a Chromeriver

Ebben az oktatóanyagban célja integrálása az Azure és Chromeriver megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Chromeriver egyszeri bejelentkezéses engedélyezett előfizetés

Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Chromeriver rendelt fogja tudni az alkalmazás a Chromeriver vállalati webhely (a szolgáltatás által kezdeményezett szolgáltató bejelentkezési) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Chromeriver engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-chromeriver-tutorial/IC802755.png "Eset")
##<a name="enabling-the-application-integration-for-chromeriver"></a>Az alkalmazás integrációját Chromeriver engedélyezése

Ez a szakasz célja, hogyan Chromeriver az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-chromeriver-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Chromeriver, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-chromeriver-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-chromeriver-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-chromeriver-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-chromeriver-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Chromeriver**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-chromeriver-tutorial/IC802756.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Chromeriver**, és válassza a **kész** , az alkalmazás hozzáadása gombra.
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Chromeriver hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Chromeriver** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-chromeriver-tutorial/IC802757.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Chromeriver felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-chromeriver-tutorial/IC802758.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás beállításainak megadása** lapon hajtsa végre az alábbi lépéseket:

    ![Alkalmazás beállításainak konfigurálása] (./media/active-directory-saas-chromeriver-tutorial/IC802759.png "Alkalmazás beállításainak konfigurálása")

    1.  **Válasz URL-cím** mezőbe írja be a Chromeriver **AssertionConsumerService URL-CÍMÉT** (például: *https://qa-app.chromeriver.com/login/sso/saml/consume?customerId=911*).  

        >[AZURE.NOTE] A Chromeriver támogatási szolgálatához érheti ezt az értéket.

    2.  Kattintson a **Tovább** gombra

4.  **Konfigurálása az egyszeri bejelentkezés Chromeriver a** lapon a metaadat-alapú letöltéséhez kattintson a **metaadat-alapú letöltése**, és mentse a fájlt a számítógépen a metaadat parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-chromeriver-tutorial/IC802760.png "Egyszeri bejelentkezés beállítása")

5.  A metaadatok letöltött fájl küldése a Chromeriver támogatási csoportnak.

    >[AZURE.NOTE]Hajtsa végre a tényleges SSO konfigurációs rendelkezik a Chromeriver támogatási csoporthoz.  
    Értesítést fog kapni, amikor az egyszeri bejelentkezés engedélyezve van-előfizetéséhez.

6.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-chromeriver-tutorial/IC802761.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Chromeriver, hogy ki kell építenie Chromeriver be.  
Chromeriver, amíg a felhasználói fiókok kell a Chromeriver támogatási csoport által hozható létre.

>[AZURE.NOTE] Bármely más Chromeriver felhasználói fiók létrehozása eszközöket is használhatja, illetve API-k által biztosított Chromeriver kiépítése Azure Active Directory felhasználói fiókok.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-chromeriver-perform-the-following-steps"></a>Felhasználók hozzárendelése Chromeriver, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Chromeriver **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-chromeriver-tutorial/IC802762.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-chromeriver-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
