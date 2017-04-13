<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Lucidchart |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Lucidchart az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-lucidchart"></a>Oktatóprogram: Azure Active Directory-integráció a Lucidchart
  
Ebben az oktatóanyagban célja integrálása az Azure és Lucidchart megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Lucidchart egyszeri bejelentkezéses engedélyezett előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Lucidchart rendelt fogja tudni az alkalmazás a Lucidchart vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Lucidchart engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-lucidchart-tutorial/IC791183.png "Eset")
##<a name="enabling-the-application-integration-for-lucidchart"></a>Az alkalmazás integrációját Lucidchart engedélyezése
  
Ez a szakasz célja, hogyan Lucidchart az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-lucidchart-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Lucidchart, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-lucidchart-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-lucidchart-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-lucidchart-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-lucidchart-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Lucidchart**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-lucidchart-tutorial/IC791184.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Lucidchart**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Lucidchart] (./media/active-directory-saas-lucidchart-tutorial/IC791185.png "Lucidchart")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Lucidchart hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Lucidchart** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-lucidchart-tutorial/IC791186.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Lucidchart felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-lucidchart-tutorial/IC791187.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás URL-cím beállítása** lapon az **URL a bejelentkezési Lucidchart** mezőbe írja be a bejelentkezni a Lucidchart alkalmazás a felhasználók által használt URL-CÍMÉT (például: "*https://chart2.office.lucidchart.com/saml/sso/azure*"), és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-lucidchart-tutorial/IC791188.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés Lucidchart a** lapon a metaadat-alapú letöltéséhez kattintson a **metaadat-alapú letöltése**, és mentse az adatfájlt a számítógépen helyben parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-lucidchart-tutorial/IC791189.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a Lucidchart vállalati webhely rendszergazdaként.

6.  A felső sávon kattintson a **csoport**.

    ![Csoportwebhely] (./media/active-directory-saas-lucidchart-tutorial/IC791190.png "Csoportwebhely")

7.  Kattintson a **alkalmazás \> kezelése a SAML**.

    ![SAML kezelése] (./media/active-directory-saas-lucidchart-tutorial/IC791191.png "SAML kezelése")

8.  A **SAML hitelesítési beállítások** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    1.  Jelölje ki a **SAML-hitelesítés engedélyezése**, és válassza a **nem kötelező**.
        ![SAML hitelesítési beállítások] (./media/active-directory-saas-lucidchart-tutorial/IC791192.png "SAML hitelesítési beállítások")
    2.  A **tartomány** mezőbe írja be a tartományt, és válassza a **Módosítás tanúsítvány**.
        ![Tanúsítvány módosítása] (./media/active-directory-saas-lucidchart-tutorial/IC791193.png "Tanúsítvány módosítása")
    3.  Nyissa meg a letöltött metaadat-fájlt, a tartalom másolása és beillesztése a **Metaadatok feltöltése** mezőben lévő értéket.
        ![Metaadat-alapú feltöltése] (./media/active-directory-saas-lucidchart-tutorial/IC791194.png "Metaadat-alapú feltöltése")
    4.  Jelölje be az **Új felhasználó automatikusan hozzáadja a csapat**, és kattintson a **módosítások mentéséhez**.
        ![Módosítások mentése] (./media/active-directory-saas-lucidchart-tutorial/IC791195.png "Módosítások mentése")

9.  Jelölje ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-lucidchart-tutorial/IC791196.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Nincs teendő konfigurálható kiépítési Lucidchart a felhasználó nem.  
Egy tevékenységhez rendelt felhasználó megkísérel jelentkezzen be a hozzáférés panelen Lucidchart, a Lucidchart ellenőrzi, hogy létezik-e a felhasználó.  
Ha nincs még nincs felhasználói fiók érhető el, a Lucidchart automatikusan létrehozott.
##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-lucidchart-perform-the-following-steps"></a>Felhasználók hozzárendelése Lucidchart, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Lucidchart **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-lucidchart-tutorial/IC791197.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-lucidchart-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).