<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a kiindulópont OnDemand |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a kiindulópont OnDemand az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a>Oktatóprogram: Azure Active Directory-integráció a kiindulópont OnDemand

Ebben az oktatóanyagban célja Azure és kiindulópont OnDemand integrációja megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   A kiindulópont OnDemand bérlői

Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók kiindulópont OnDemand rendelt tudnak egyetlen bejelentkezés az alkalmazásba, a kiindulópont OnDemand vállalati webhely (a szolgáltatás által kezdeményezett szolgáltató bejelentkezési) és használatáról a [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  A kiindulópont OnDemand alkalmazás integrációját engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781593.png "Eset")
##<a name="enabling-the-application-integration-for-cornerstone-ondemand"></a>A kiindulópont OnDemand alkalmazás integrációját engedélyezése

Ez a szakasz célja, hogyan engedélyezhető az alkalmazás-integráció a kiindulópont OnDemand tagolása.

###<a name="to-enable-the-application-integration-for-cornerstone-ondemand-perform-the-following-steps"></a>Ha engedélyezni szeretné a kiindulópont OnDemand alkalmazás integrációját, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Kiindulópont ondemand**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781594.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Kiindulópont OnDemand**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Kiindulópont OnDemand] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781595.png "Kiindulópont OnDemand")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy a kiindulópont OnDemand hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon a **Kiindulópont OnDemand** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés engedélyezése] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781596.png "Egyszeri bejelentkezés engedélyezése")

2.  **Hogyan szeretné, hogy jelentkezzen be kiindulópont OnDemand felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Microsoft Azure Active Directory Single Sign-On] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781597.png "Microsoft Azure Active Directory Single Sign-On")

3.  Az **Alkalmazás URL-cím beállítása** lapon, a **Kiindulópont OnDemand bejelentkezési az URL-cím** mezőbe írja be az URL-t, a következő mintát "*http://company.csod.com*", és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781598.png "Állítsa be az App URL-címe")

4.  A **Configure egyszeri bejelentkezés a kiindulópont OnDemand a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen helyben parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781599.png "Egyszeri bejelentkezés beállítása")

5.  Küldje el az alábbi elemek a kiindulópont OnDemand támogatási csoportjának:

    1.  A letöltött tanúsítvány
    2.  A **Távoli bejelentkezési URL-cím** értéket.
    3.  A **Távoli kijelentkezés URL-címe** értéket.

    >[AZURE.NOTE] Egyszeri bejelentkezés kell beállítania a kiindulópont OnDemand támogatási csoport által.
A konfiguráció befejezése után, értesítést kap a támogatási csoport.

6.  Jelölje ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781600.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a kiindulópont OnDemand, hogy ki kell építenie kiindulópont OnDemand be.  
Kiindulópont OnDemand, ha a feladat manuális kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Az információk küldése (pl.: nevét, Emial) a kiindulópont OnDemand biztosítása az Azure Active Directory felhasználót kapcsolatos támogatási csoportjának.

>[AZURE.NOTE] Bármely más kiindulópont OnDemand felhasználói fiók létrehozása eszközök vagy a kiindulópont OnDemand rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-cornerstone-ondemand-perform-the-following-steps"></a>Felhasználók hozzárendelése kiindulópont OnDemand, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  A **Kiindulópont OnDemand **alkalmazás integrációs lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC775564.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781601.png "Adhatnak a felhasználóknak")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
