<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a SimpleNexus |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a SimpleNexus az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-simplenexus"></a>Oktatóprogram: Azure Active Directory-integráció a SimpleNexus
  
Ebben az oktatóanyagban célja integrálása az Azure és SimpleNexus megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   SimpleNexus egyszeri bejelentkezéses engedélyezett előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók SimpleNexus rendelt fogja tudni az alkalmazás a SimpleNexus vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját SimpleNexus engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-simplenexus-tutorial/IC785893.png "Eset")
##<a name="enabling-the-application-integration-for-simplenexus"></a>Az alkalmazás integrációját SimpleNexus engedélyezése
  
Ez a szakasz célja, hogyan SimpleNexus az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-simplenexus-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját SimpleNexus, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-simplenexus-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-simplenexus-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-simplenexus-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-simplenexus-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be az **egyszerű Nexus készüléken**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-simplenexus-tutorial/IC785894.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **SimpleNexus**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Egyszerű Nexus készüléken] (./media/active-directory-saas-simplenexus-tutorial/IC809578.png "Egyszerű Nexus készüléken")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy SimpleNexus hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **SimpleNexus** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-simplenexus-tutorial/IC785896.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az SimpleNexus felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-simplenexus-tutorial/IC785897.png "Egyszeri bejelentkezés beállítása")

3.  Az **App URL-címe konfigurálása** lapon **SimpleNexus bejelentkezési az URL** mezőbe írja be az URL-t, a következő mintát "*https://simplenexus.com/CompanyName\_bejelentkezési*", majd kattintson a **Tovább**gombra.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-simplenexus-tutorial/IC786904.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés SimpleNexus a** lapon kattintson a **metaadat-alapú letöltése**, és ezután továbbítása a metaadat-fájlt a SimpleNexus támogatási csoportjának.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-simplenexus-tutorial/IC785899.png "Egyszeri bejelentkezés beállítása")

    >[AZURE.NOTE] Egyszeri bejelentkezés kell engedélyezni kell a SimpleNexus támogatási csoport által.

5.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-simplenexus-tutorial/IC785900.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a SimpleNexus, hogy ki kell építenie SimpleNexus be.  
SimpleNexus, amíg a bérlői rendszergazda által végzett kézi tevékenység kiépítési.

>[AZURE.NOTE] Bármely más SimpleNexus felhasználói fiók létrehozása eszközöket is használhatja, illetve SimpleNexus rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-simplenexus-perform-the-following-steps"></a>Felhasználók hozzárendelése SimpleNexus, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **SimpleNexus **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-simplenexus-tutorial/IC785901.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-simplenexus-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).