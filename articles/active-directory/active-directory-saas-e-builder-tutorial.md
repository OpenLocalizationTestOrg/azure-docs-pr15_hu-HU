<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a az e-szerkesztő |} Microsoft Azure" 
    description="Megtudhatja, hogy miként az e-szerkesztő használata az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-e-builder"></a>Oktatóprogram: Azure Active Directory-integráció a az e-szerkesztő
  
Ebben az oktatóanyagban célja integrálása az Azure és az e-szerkesztő megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Egy e-szerkesztő bérlői
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók e-szerkesztő rendelt tudnak egyetlen bejelentkezés az alkalmazásba, az e-szerkesztő vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md).
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az e-szerkesztő alkalmazás integrációját engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-e-builder-tutorial/IC777378.png "Eset")
##<a name="enabling-the-application-integration-for-e-builder"></a>Az e-szerkesztő alkalmazás integrációját engedélyezése
  
Ez a szakasz célja, hogyan engedélyezhető az e-szerkesztő alkalmazás integrációját tagolása.

###<a name="to-enable-the-application-integration-for-e-builder-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integráció az e-szerkesztő, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-e-builder-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-e-builder-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-e-builder-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-e-builder-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be **Az e-szerkesztő**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-e-builder-tutorial/IC777379.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki **Az e-szerkesztő**pontra, és kattintson a **kész** az alkalmazás hozzáadása parancsra.

    ![Az e-szerkesztő] (./media/active-directory-saas-e-builder-tutorial/IC777380.png "Az e-szerkesztő")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja engedélyezése a felhasználóknak, hogy az e-szerkesztő fiókjukhoz az összevonási alapján a SAML protokoll használatával Azure Active Directory hitelesíteni a szerkezet.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **E-szerkesztő** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-e-builder-tutorial/IC777381.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az e-szerkesztő felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-e-builder-tutorial/IC777382.png "Beállítás az egyszeri bejelentkezés")

3.  **Alkalmazás URL-címet konfigurálni** lap, az **e-szerkesztő jelentkezzen be az URL-cím** mezőbe írja be az URL-t, a következő mintát "*https://\<bérlői neve\>.e-Builder.com*", és kattintson a **Tovább gombra**.

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-e-builder-tutorial/IC777383.png "Állítsa be az app URL-címe")

4.  A **beállítás az egyszeri bejelentkezés az e-szerkesztő a** lapra, töltse le a metaadatok, kattintson a **metaadat-alapú letöltése**, és kattintson az adatfájl helyi meghajtóra **c:\\az e-BuilderMetaData.xml**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-e-builder-tutorial/IC777384.png "Beállítás az egyszeri bejelentkezés")

5.  Előre, hogy az e-szerkesztő metaadatok fájl támogatási csoporthoz. A támogatási csapat igényeinek beállítja az egyszeri bejelentkezés meg.

6.  Jelölje ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-e-builder-tutorial/IC777385.png "Beállítás az egyszeri bejelentkezés")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Nincs teendő állíthatja be az e-szerkesztő kiépítési felhasználói számára nem.  
Egy tevékenységhez rendelt felhasználó megkísérel jelentkezzen be az e-szerkesztő használata az access panelen, amikor e-szerkesztő ellenőrzi, hogy létezik-e a felhasználó.  
Ha nincs még nincs felhasználói fiók érhető el, a az e-szerkesztő automatikusan létrehozott.
##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-e-builder-perform-the-following-steps"></a>Az e-szerkesztő felhasználók rendelni, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Az **E-szerkesztő **alkalmazás integrációs lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-e-builder-tutorial/IC777386.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-e-builder-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
