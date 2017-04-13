<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Innotas |} Microsoft Azure"
    description="Megtudhatja, hogyan használhatja a Innotas az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-innotas"></a>Oktatóprogram: Azure Active Directory-integráció a Innotas
  
Ebben az oktatóanyagban célja integrálása az Azure és Innotas megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Innotas bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók Innotas rendelt tudnak egyetlen bejelentkezés az alkalmazásba, a Innotas vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md).
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Innotas engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-innotas-tutorial/IC777331.png "Eset")
##<a name="enabling-the-application-integration-for-innotas"></a>Az alkalmazás integrációját Innotas engedélyezése
  
Ez a szakasz célja, hogyan Innotas az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-innotas-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Innotas, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-innotas-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-innotas-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-innotas-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-innotas-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Innotas**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-innotas-tutorial/IC777332.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Innotas**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Innotas] (./media/active-directory-saas-innotas-tutorial/IC777333.png "Innotas")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Innotas hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Innotas** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-innotas-tutorial/IC777334.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az Innotas felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-innotas-tutorial/IC777335.png "Beállítás az egyszeri bejelentkezés")

3.  Az **App URL-címe konfigurálása** lapon **Innotas bejelentkezési az URL** mezőbe írja be az URL-t, a következő mintát "*https://\<bérlői neve\>. Innotas.com*", majd kattintson a **Tovább**gombra.

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-innotas-tutorial/IC777336.png "Állítsa be az app URL-címe")

4.  A **beállítás az egyszeri bejelentkezés Innotas a** lapra, töltse le a metaadatok, kattintson a **metaadat-alapú letöltése**, és kattintson az adatfájl helyi meghajtóra **c:\\InnotasMetaData.xml**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-innotas-tutorial/IC777337.png "Beállítás az egyszeri bejelentkezés")

5.  Előre a metaadat-alapú fájl Innotas támogatási csoporthoz. A támogatási csapat igényeinek beállítja az egyszeri bejelentkezés meg.

6.  Jelölje ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-innotas-tutorial/IC777338.png "Beállítás az egyszeri bejelentkezés")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Nincs teendő konfigurálható kiépítési Innotas a felhasználó nem.  
Egy tevékenységhez rendelt felhasználó megkísérel jelentkezzen be a hozzáférés panelen Innotas, a Innotas ellenőrzi, hogy létezik-e a felhasználó.  
Ha nincs még nincs felhasználói fiók érhető el, a Innotas automatikusan létrehozott.
##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-innotas-perform-the-following-steps"></a>Felhasználók hozzárendelése Innotas, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Innotas **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-innotas-tutorial/IC777339.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-innotas-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).