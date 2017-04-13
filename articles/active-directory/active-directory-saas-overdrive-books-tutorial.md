<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a gyorsmeneti könyvek |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a gyorsmeneti könyvek az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-overdrive-books"></a>Oktatóprogram: Azure Active Directory-integráció a gyorsmeneti könyvek
  
Ebben az oktatóanyagban célja integrálása az Azure és gyorsmeneti megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Egy gyorsmeneti egyszeri bejelentkezés engedélyezve van az előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók gyorsmeneti rendelt tudnak egyetlen bejelentkezés az alkalmazásba, a gyorsmeneti vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md).
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját gyorsmeneti engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-overdrive-books-tutorial/IC784462.png "Eset")
##<a name="enabling-the-application-integration-for-overdrive"></a>Az alkalmazás integrációját gyorsmeneti engedélyezése
  
Ez a szakasz célja, hogyan gyorsmeneti az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-overdrive-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját gyorsmeneti, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-overdrive-books-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-overdrive-books-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-overdrive-books-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-overdrive-books-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **gyorsmeneti**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-overdrive-books-tutorial/IC784463.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **gyorsmeneti**pontra, és kattintson a **kész** az alkalmazás hozzáadása parancsra.

    ![Gyorsmeneti] (./media/active-directory-saas-overdrive-books-tutorial/IC799950.png "Gyorsmeneti")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy gyorsmeneti hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **gyorsmeneti** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés engedélyezése] (./media/active-directory-saas-overdrive-books-tutorial/IC784465.png "Egyszeri bejelentkezés engedélyezése")

2.  **Hogyan megtekinti gyorsmeneti jelentkezzen be felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-overdrive-books-tutorial/IC784466.png "Beállítás az egyszeri bejelentkezés")

3.  A **App URL-címe beállítása** lapon az **Gyorsmeneti bejelentkezési az URL** mezőbe írja be az URL-t, a következő mintát "*http://mslibrarytest.libraryreserve.com*", és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-overdrive-books-tutorial/IC784467.png "Állítsa be az App URL-címe")

4.  A lapon **konfigurálása az egyszeri bejelentkezés gyorsmeneti,** töltse le a metaadatok fájlt, és küldje el a gyorsmeneti támogatási csoportnak.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-overdrive-books-tutorial/IC784468.png "Beállítás az egyszeri bejelentkezés")

    >[AZURE.NOTE]A gyorsmeneti támogatási csoport beállítja az egyszeri bejelentkezés meg, és értesítést küld a konfiguráció befejezése után.

5.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-overdrive-books-tutorial/IC784469.png "Beállítás az egyszeri bejelentkezés")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Nincs teendő konfigurálható a felhasználó létesítése való gyorsmeneti nem.  
Amikor egy kiosztott felhasználó megkísérel bejelentkezni a gyorsmeneti, gyorsmeneti fiók automatikusan létrejön, szükség esetén.

>[AZURE.NOTE]Bármely más gyorsmeneti felhasználói fiók létrehozása eszközöket is használhatja, illetve gyorsmeneti rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-overdrive-perform-the-following-steps"></a>Felhasználók hozzárendelése gyorsmeneti, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **gyorsmeneti **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-overdrive-books-tutorial/IC784470.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-overdrive-books-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).