<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Huddle |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Huddle az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-huddle"></a>Oktatóprogram: Azure Active Directory-integráció a Huddle
  
Ebben az oktatóanyagban célja integrálása az Azure és Huddle megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Huddle egyszeri bejelentkezéses engedélyezett előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Huddle rendelt fogja tudni az alkalmazás a Huddle vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Huddle engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-huddle-tutorial/IC787830.png "Egyszeri bejelentkezés beállítása")
##<a name="enabling-the-application-integration-for-huddle"></a>Az alkalmazás integrációját Huddle engedélyezése
  
Ez a szakasz célja, hogyan Huddle az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-huddle-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Huddle, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-huddle-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-huddle-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-huddle-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-huddle-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Huddle**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-huddle-tutorial/IC787831.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Huddle**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Huddle] (./media/active-directory-saas-huddle-tutorial/IC787832.png "Huddle")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Huddle hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon alkalmazás integrációs **Huddle** lapján kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-huddle-tutorial/IC787833.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan megtekinti Huddle jelentkezzen be felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-huddle-tutorial/IC787834.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás URL-cím beállítása** lapon, a **Bejelentkezési Huddle az URL-cím** mezőbe írja be a Huddle bérlő használatáról a következő mintát "*http://company.huddle.com*" URL-CÍMÉT, és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-huddle-tutorial/IC787835.png "Állítsa be az App URL-címe")

4.  A **beállítás az egyszeri bejelentkezés Huddle a** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-huddle-tutorial/IC787836.png "Egyszeri bejelentkezés beállítása")

    1.  Kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen.
    2.  Másolja a **Kibocsátó URL-címe** értéket, a **SAML egyszeri bejelentkezési URL-címe** értéket, és a letöltött tanúsítvány, és küldje el őket a Huddle támogatási csoportnak.

    >[AZURE.NOTE] Egyszeri bejelentkezés kell engedélyezni kell a Huddle támogatási csoport által.
A konfiguráció befejeződésekor értesítést fog kapni.

5.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-huddle-tutorial/IC787837.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Huddle, hogy ki kell építenie Huddle be.  
Huddle, ha a feladat manuális kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a vállalat **Huddle** webhelyen.

2.  **Munkaterület**elemre.

3.  Kattintson a **személyek \> személyek meghívása**.

    ![Személyek] (./media/active-directory-saas-huddle-tutorial/IC787838.png "Személyek")

4.  **Az új szóló meghívó létrehozása** csoportjában hajtsa végre az alábbi lépéseket:

    ![Új meghívó] (./media/active-directory-saas-huddle-tutorial/IC787839.png "Új meghívó")

    1.  **Meghívhat másokat, hogy csatlakozzanak csoport kiválasztása** listájában válassza a **csoportja**.
    2.  Írja be a rendelkezés be a kapcsolódó mezőben lévő értéket szeretné érvényes AAD fiók **E-mail címét** .
    3.  Kattintson a **Meghívás**gombra.

    >[AZURE.NOTE] Az Azure Active Directory fióktulajdonos kap e-mailben, beleértve a előtt aktívvá válik, győződjön meg arról, hogy a fiók mutató hivatkozást.

>[AZURE.NOTE] Bármely más Huddle felhasználói fiók létrehozása eszközöket is használhatja, illetve kiépítése AAD Huddle által biztosított API-khoz felhasználói fiókok.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-huddle-perform-the-following-steps"></a>Felhasználók hozzárendelése Huddle, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Alkalmazás integrációs **Huddle **lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-huddle-tutorial/IC787840.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-huddle-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).