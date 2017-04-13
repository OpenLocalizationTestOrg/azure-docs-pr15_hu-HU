<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Mindflash |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Mindflash az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mindflash"></a>Oktatóprogram: Azure Active Directory-integráció a Mindflash
  
Ebben az oktatóanyagban célja integrálása az Azure és Mindflash megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Mindflash egyszeri bejelentkezéses engedélyezett előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Mindflash rendelt fogja tudni az alkalmazás a Mindflash vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Mindflash engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-mindflash-tutorial/IC787132.png "Eset")
##<a name="enabling-the-application-integration-for-mindflash"></a>Az alkalmazás integrációját Mindflash engedélyezése
  
Ez a szakasz célja, hogyan Mindflash az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-mindflash-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Mindflash, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-mindflash-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-mindflash-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-mindflash-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-mindflash-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Mindflash**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-mindflash-tutorial/IC787133.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Mindflash**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Mindflash] (./media/active-directory-saas-mindflash-tutorial/IC787134.png "Mindflash")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Mindflash hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Mindflash** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-mindflash-tutorial/IC787135.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Mindflash felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-mindflash-tutorial/IC787136.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás URL-cím beállítása** lapon, a **Bejelentkezési a URL-cím** mezőbe írja be az URL-t, a következő mintát "*http://company.mindflash.com*", és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-mindflash-tutorial/IC787137.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés a Mindflash** lapon kattintson a **metaadat-alapú letöltése**elemre, és mentse a fájlt a számítógépen a metaadat.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-mindflash-tutorial/IC787138.png "Egyszeri bejelentkezés beállítása")

5.  Küldje el a metadatafile a Mindflash részlegtől.

    >[AZURE.NOTE] Egyszeri bejelentkezés konfiguráció a Mindflash támogatási csoportja által végrehajtandó tartalmaz. Értesítést kaphat arról, amint a konfiguráció befejeződött.

6.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-mindflash-tutorial/IC787139.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Mindflash, hogy ki kell építenie Mindflash be.  
Mindflash, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **Mindflash** vállalati webhely.

2.  Nyissa meg a **felhasználók kezelése**.

    ![Felhasználókezelés] (./media/active-directory-saas-mindflash-tutorial/IC787140.png "Felhasználókezelés")

3.  Kattintson a **Felhasználó hozzáadása**elemre, és kattintson az **Új**gombra.

4.  **Új felhasználók hozzáadása** csoportban hajtsa végre az alábbi lépéseket:

    ![Új felhasználók hozzáadása] (./media/active-directory-saas-mindflash-tutorial/IC787141.png "Új felhasználók hozzáadása")

    1.  Írja be az **Utónév**és **vezetéknevet** , valamint **e-mailek** be a kapcsolódó szövegdobozok kiépítése kívánt érvényes AAD fiók.
    2.  Kattintson a **hozzáadása**gombra.

>[AZURE.NOTE]Bármely más Mindflash felhasználói fiók létrehozása eszközöket is használhatja, illetve Mindflash rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-mindflash-perform-the-following-steps"></a>Felhasználók hozzárendelése Mindflash, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Mindflash **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-mindflash-tutorial/IC787142.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-mindflash-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).