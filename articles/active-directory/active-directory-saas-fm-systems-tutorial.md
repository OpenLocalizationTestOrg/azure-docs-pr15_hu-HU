<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a FM: rendszerek |} Microsoft Azure" 
    description="Megtudhatja, hogy miként használhatja a FM: Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatizált kiépítési és az egyéb rendszerekhez!" 
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

#<a name="tutorial-azure-active-directory-integration-with-fm-systems"></a>Oktatóprogram: Azure Active Directory-integráció a FM: rendszerek
  
Ebben az oktatóanyagban célja integrálása az Azure és FM:Systems megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   FM:Systems egyszeri bejelentkezéses engedélyezett előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók FM:Systems rendelt fogja tudni az alkalmazás a FM:Systems vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját FM:Systems engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-fm-systems-tutorial/IC795899.png "Eset")
##<a name="enabling-the-application-integration-for-fmsystems"></a>Az alkalmazás integrációját FM:Systems engedélyezése
  
Ez a szakasz célja, hogyan FM:Systems az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-fmsystems-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját FM:Systems, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-fm-systems-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-fm-systems-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-fm-systems-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-fm-systems-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **FM:Systems**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-fm-systems-tutorial/IC795900.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **FM:Systems**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![FM: rendszerek] (./media/active-directory-saas-fm-systems-tutorial/IC800213.png "FM: rendszerek")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja, hogyan engedélyezheti a felhasználóknak, hogy az összevonási alapján a SAML protokoll használatával Azure AD a saját fiók FM:Systems hitelesíteni a szerkezet.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **FM:Systems** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-fm-systems-tutorial/IC790810.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az FM:Systems felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-fm-systems-tutorial/IC795901.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás URL-cím beállítása** lapon hajtsa végre az alábbi lépéseket:

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-fm-systems-tutorial/IC795902.png "Állítsa be az App URL-címe")

    1.  Az **URL a bejelentkezési FM:Systems** mezőbe írja be a FM:Systems **Válasz URL-CÍMÉT** (például: *https://dpr.fmshosted.com/fminteract/ConsumerService2.aspx*).  

        >[AZURE.WARNING] Ez az érték beolvasása a FM: rendszerek támogatási csoporthoz.

    2.  Kattintson a **Tovább** gombra

4.  **Konfigurálása az egyszeri bejelentkezés FM:Systems a** lapon a metaadat-alapú letöltéséhez kattintson a **metaadat-alapú letöltése**, és mentse a metaadat-alapú számítógépen parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-fm-systems-tutorial/IC795903.png "Egyszeri bejelentkezés beállítása")

5.  A metaadatok letöltött fájlt a FM elküldése: rendszerek támogatási csoporthoz.

    >[AZURE.NOTE] A FM: Rendszerek támogatási csoport végezze el a tényleges SSO konfigurációs rendelkezik.
Értesítést fog kapni, amikor az egyszeri bejelentkezés engedélyezve van-előfizetéséhez.

6.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-fm-systems-tutorial/IC795904.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a FM:Systems, hogy ki kell építenie FM:Systems be.  
FM:Systems, ha a feladat manuális kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Egy webböngészőablakban jelentkezzen be a FM:Systems vállalati webhely rendszergazdaként.

2.  Nyissa meg a **rendszergazdai \> kezelése biztonsági \> felhasználók \> a felhasználó**.

    ![Rendszergazdai] (./media/active-directory-saas-fm-systems-tutorial/IC795905.png "Rendszergazdai")

3.  Kattintson az **Új felhasználó létrehozása**gombra.

    ![Új felhasználó létrehozása] (./media/active-directory-saas-fm-systems-tutorial/IC795906.png "Új felhasználó létrehozása")

4.  **Create User** csoportban hajtsa végre az alábbi lépéseket:

    ![Felhasználó létrehozása] (./media/active-directory-saas-fm-systems-tutorial/IC795907.png "Felhasználó létrehozása")

    1.  Írja be a felhasználó nevét, a jelszót, és a megerősítést kérő, az e-mail címet és egy érvényes Azure Active Directory-fiókját, kiépítése be a kapcsolódó szövegdobozok kívánt alkalmazott azonosítója.
    2.  Kattintson a **Tovább**gombra.

>[AZURE.NOTE] Bármely más FM:Systems felhasználói fiók létrehozása eszközöket is használhatja, illetve rendelkezést AAD felhasználói fiókok FM:Systems által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-fmsystems-perform-the-following-steps"></a>Felhasználók hozzárendelése FM:Systems, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **FM:Systems **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-fm-systems-tutorial/IC795908.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-fm-systems-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).