<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Wikispaces |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Wikispaces az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!." 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-wikispaces"></a>Oktatóprogram: Azure Active Directory-integráció a Wikispaces
  
Ebben az oktatóanyagban célja integrálása az Azure és Wikispaces megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Wikispaces egyszeri bejelentkezéses engedélyezett előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Wikispaces rendelt fogja tudni az alkalmazás a Wikispaces vállalati webhely (a szolgáltatás által kezdeményezett szolgáltató bejelentkezési) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Wikispaces engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Sceanrio] (./media/active-directory-saas-wikispaces-tutorial/IC787182.png "Sceanrio")

##<a name="enabling-the-application-integration-for-wikispaces"></a>Az alkalmazás integrációját Wikispaces engedélyezése
  
Ez a szakasz célja, hogyan Wikispaces az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-wikispaces-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Wikispaces, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-wikispaces-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-wikispaces-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-wikispaces-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-wikispaces-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Wikispaces**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-wikispaces-tutorial/IC787186.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Wikispaces**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Wikispaces] (./media/active-directory-saas-wikispaces-tutorial/IC787187.png "Wikispaces")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Wikispaces hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Wikispaces** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-wikispaces-tutorial/IC787188.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Wikispaces felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-wikispaces-tutorial/IC787189.png "Egyszeri bejelentkezés beállítása")

3.  A **App URL-címe beállítása** lapon az **URL a bejelentkezési Wikispaces** mezőbe írja be az URL-t, a következő mintát "*http://company.wikispaces.net*", és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-wikispaces-tutorial/IC787190.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés a Wikispaces** lapon kattintson a **metaadat-alapú letöltése**elemre, és mentse a fájlt a számítógépen a metaadat.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-wikispaces-tutorial/IC787191.png "Egyszeri bejelentkezés beállítása")

5.  Küldje el a metadatafile a Wikispaces részlegtől.

    >[AZURE.NOTE] Egyszeri bejelentkezés konfiguráció a Wikispaces támogatási csoportja által végrehajtandó tartalmaz. Értesítést kaphat arról, amint a konfiguráció befejeződött.

6.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-wikispaces-tutorial/IC787192.png "Egyszeri bejelentkezés beállítása")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Wikispaces, hogy ki kell építenie Wikispaces be.  
Wikispaces, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **Wikispaces** vállalati webhely.

2.  Nyissa meg **a tagok**.

    ![A tagok] (./media/active-directory-saas-wikispaces-tutorial/IC787193.png "A tagok")

3.  Kattintson a **Személyek meghívása lehetőséget**.

    ![Személyek meghívása] (./media/active-directory-saas-wikispaces-tutorial/IC787194.png "Személyek meghívása")

4.  A **Személyek meghívása** csoportban hajtsa végre az alábbi lépéseket:

    ![Személyek meghívása] (./media/active-directory-saas-wikispaces-tutorial/IC787208.png "Személyek meghívása")

    1.  Írja be a **felhasználónevét vagy E-mail címét** be a kapcsolódó szövegdobozok kiépítése kívánt érvényes AAD fiók.
    2.  Kattintson a **Küldés**gombra.  

        >[AZURE.NOTE] Az Azure Active Directory fióktulajdonos kap e-mailben, beleértve a előtt aktívvá válik, győződjön meg arról, hogy a fiók mutató hivatkozást.

>[AZURE.NOTE] Bármely más Wikispaces felhasználói fiók létrehozása eszközöket is használhatja, illetve Wikispaces rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-wikispaces-perform-the-following-steps"></a>Felhasználók hozzárendelése Wikispaces, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Wikispaces **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-wikispaces-tutorial/IC787195.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-wikispaces-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).