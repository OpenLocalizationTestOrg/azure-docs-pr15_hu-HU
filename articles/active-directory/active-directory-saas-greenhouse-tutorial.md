<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a üvegházhatású |} Microsoft Azure" 
    description="Megtudhatja, hogy miként üvegházhatású használata az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-greenhouse"></a>Oktatóprogram: Azure Active Directory-integráció a üvegházhatású
  
Ebben az oktatóanyagban célja Azure és üvegházhatású integrációja megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Üvegházhatású egyszeri bejelentkezéses előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók üvegházhatású rendelt tudnak való egyszeri bejelentkezési a üvegházhatású vállalati webhely (a szolgáltatás által kezdeményezett szolgáltató bejelentkezési) vagy használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az alkalmazásba.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját üvegházhatású engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-greenhouse-tutorial/IC790783.png "Eset")
##<a name="enabling-the-application-integration-for-greenhouse"></a>Az alkalmazás integrációját üvegházhatású engedélyezése
  
Ez a szakasz célja, hogyan üvegházhatású az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-greenhouse-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját üvegházhatású, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-greenhouse-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-greenhouse-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-greenhouse-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-greenhouse-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **üvegházhatású**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-greenhouse-tutorial/IC790784.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **üvegházhatású**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Üvegházhatású] (./media/active-directory-saas-greenhouse-tutorial/IC790785.png "Üvegházhatású")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy üvegházhatású hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **üvegházhatású** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-greenhouse-tutorial/IC790786.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az üvegházhatású felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-greenhouse-tutorial/IC790787.png "Beállítás az egyszeri bejelentkezés")

3.  Az **Alkalmazás URL-cím beállítása** lapon, a **Bejelentkezési a URL-cím** mezőbe írja be az URL-t, a következő mintát "*https://company.greenhouse.io*", és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-greenhouse-tutorial/IC790788.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés a üvegházhatású** lapon kattintson a **metaadat-alapú letöltése**elemre, és mentse a metaadat-fájlt a számítógépen helyben.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-greenhouse-tutorial/IC790789.png "Beállítás az egyszeri bejelentkezés")

5.  A metaadat-alapú fájl üvegházhatású támogatási csoportjának továbbítja.

    >[AZURE.NOTE] Egyszeri bejelentkezés a üvegházhatású támogatási csoport által engedélyezésük tartalmaz.

6.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-greenhouse-tutorial/IC790790.png "Beállítás az egyszeri bejelentkezés")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználók üvegházhatású való bejelentkezéshez, hogy ki kell építenie üvegházhatású be.  
Üvegházi, amíg egy manuális feladat kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **üvegházhatású** vállalati webhely.

2.  A felső sávon kattintson a **Konfigurálás**gombra, és kattintson a **felhasználók**.

    ![Felhasználók] (./media/active-directory-saas-greenhouse-tutorial/IC790791.png "Felhasználók")

3.  Kattintson az **Új felhasználók számára**.

    ![Új felhasználó] (./media/active-directory-saas-greenhouse-tutorial/IC790792.png "Új felhasználó")

4.  Az **Új felhasználó hozzáadása** csoportban hajtsa végre az alábbi lépéseket:

    ![Új felhasználó hozzáadása] (./media/active-directory-saas-greenhouse-tutorial/IC790793.png "Új felhasználó hozzáadása")

    1.  Az **Enter felhasználói e-mail fiókok** mezőbe írjon be egy érvényes Azure Active Directory-fiókját, rendelkezést szeretné annak az e-mail címét.
    2.  Kattintson a **Mentés**gombra.
        
        >[AZURE.NOTE] Az Azure Active Directory-fiók tulajdonosai kap e-mailben, beleértve a előtt aktívvá válik, győződjön meg arról, hogy a fiók mutató hivatkozást.

>[AZURE.NOTE] Bármely más üvegházhatású felhasználói fiók létrehozása eszközöket is használhatja, illetve üvegházhatású rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-greenhouse-perform-the-following-steps"></a>Felhasználók hozzárendelése üvegházhatású, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **üvegházhatású **alkalmazás integráció lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-greenhouse-tutorial/IC790794.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-greenhouse-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).