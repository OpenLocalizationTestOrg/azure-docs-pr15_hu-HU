<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a EmpCenter |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a EmpCenter az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-empcenter"></a>Oktatóprogram: Azure Active Directory-integráció a EmpCenter
  
Ebben az oktatóanyagban célja integrálása az Azure és EmpCenter megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Egy EmpCenter egyszeri bejelentkezés engedélyezve van az előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók EmpCenter rendelt fogja tudni az alkalmazás a EmpCenter vállalati webhely (a szolgáltatás által kezdeményezett szolgáltató bejelentkezési) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját EmpCenter engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-empcenter-tutorial/IC802916.png "Eset")
##<a name="enabling-the-application-integration-for-empcenter"></a>Az alkalmazás integrációját EmpCenter engedélyezése
  
Ez a szakasz célja, hogyan EmpCenter az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-empcenter-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját EmpCenter, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-empcenter-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-empcenter-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-empcenter-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-empcenter-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **EmpCenter**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-empcenter-tutorial/IC802917.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **EmpCenter**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![EmpCentral] (./media/active-directory-saas-empcenter-tutorial/IC802918.png "EmpCentral")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy EmpCenter hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **EmpCenter** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-empcenter-tutorial/IC802919.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az EmpCenter felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-empcenter-tutorial/IC802920.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás beállításainak megadása** lapon hajtsa végre az alábbi lépéseket:

    ![Alkalmazás beállításainak konfigurálása] (./media/active-directory-saas-empcenter-tutorial/IC802921.png "Alkalmazás beállításainak konfigurálása")

    1.  **Jelentkezzen be URL-cím** mezőbe írja be a felhasználóknak, hogy a bejelentkezés az EmpCenter alkalmazás által használt URL-CÍMÉT (például: *https://partner-authenticati.empcenter.com/workforce/SSO.do*).
    2.  Kattintson a **Tovább** gombra

4.  **Konfigurálása az egyszeri bejelentkezés EmpCenter a** lapon a metaadat-alapú letöltéséhez kattintson a **metaadat-alapú letöltése**, és mentse a fájlt a számítógépen a metaadat parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-empcenter-tutorial/IC802922.png "Egyszeri bejelentkezés beállítása")

5.  A metaadatok letöltött fájl küldése a EmpCenter támogatási csoportnak.

    >[AZURE.NOTE] Végezze el a tényleges SSO konfigurációs rendelkezik a EmpCenter támogatási csoporthoz.
Értesítést fog kapni, amikor az egyszeri bejelentkezés engedélyezve van-előfizetéséhez.

6.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-empcenter-tutorial/IC802923.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a EmpCenter, hogy ki kell építenie EmpCenter be.  
EmpCenter, amíg a felhasználói fiókok kell a EmpCenter támogatási csoport által hozható létre.

>[AZURE.NOTE] Bármely más EmpCenter felhasználói fiók létrehozása eszközöket is használhatja, illetve API-k által biztosított EmpCenter kiépítése Azure Active Directory felhasználói fiókok.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-empcenter-perform-the-following-steps"></a>Felhasználók hozzárendelése EmpCenter, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **EmpCenter **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-empcenter-tutorial/IC802924.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-empcenter-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).