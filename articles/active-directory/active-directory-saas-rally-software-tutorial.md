<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a szervez szoftver |} Microsoft Azure" 
    description="Megtudhatja, hogy miként Rally szoftver használata az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-rally-software"></a>Oktatóprogram: Azure Active Directory-integráció a szervez szoftver
  
Ebben az oktatóanyagban célja Azure és a szoftver Rally integrációja megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   A szoftver Rally bérlői webhelyre
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrálását Rally szoftverek engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-rally-software-tutorial/IC769525.png "Eset")
##<a name="enabling-the-application-integration-for-rally-software"></a>Az alkalmazás integrálását Rally szoftverek engedélyezése
  
Ez a szakasz célja, hogyan Rally szoftverek alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-rally-software-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrálását Rally szoftverek, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-rally-software-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-rally-software-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-rally-software-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-rally-software-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **szervez**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-rally-software-tutorial/IC769526.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában válassza a **Szoftver Rally**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Szoftver Rally] (./media/active-directory-saas-rally-software-tutorial/IC769527.png "Szoftver Rally")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja engedélyezése a felhasználóknak, hogy a saját fiók az összevonási alapján a SAML protokoll használatával Azure Active Directory hitelesítő Rally szoftver tagolása.  
Ez az eljárás részeként szükség van egy tanúsítványt feltöltése Rally szoftver.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon a **Szoftver Rally **alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-rally-software-tutorial/IC749323.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan megtekinti szervez jelentkezzen be felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Microsoft Azure Active Directory Single Sign-On] (./media/active-directory-saas-rally-software-tutorial/IC769528.png "Microsoft Azure Active Directory Single Sign-On")

3.  **App URL-címe beállítása** lapon az **Rally szoftver bérlői webhely URL-cím** mezőbe írja be az URL-t, a következő mintát "*https://\<bérlői neve\>. rally.com*", majd kattintson a **Tovább**gombra.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-rally-software-tutorial/IC769529.png "Állítsa be az App URL-címe")

4.  A **Configure egyszeri bejelentkezés szervez a** lapon kattintson a letöltés metaadatok, és mentse a számítógépre.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-rally-software-tutorial/IC769530.png "Beállítás az egyszeri bejelentkezés")

5.  Jelentkezzen be az **Rally szoftver** bérlőhöz.

6.  Kattintson az eszköztáron a képernyő tetején kattintson a **beállítás**gombra, és válassza az **előfizetés**.

    ![Előfizetés] (./media/active-directory-saas-rally-software-tutorial/IC769531.png "Előfizetés")

7.  A **művelet** gombra az eszköztáron a jobb oldalon a képernyő tetején, és válassza az **Előfizetés szerkesztése**.

8.  Az **előfizetés** párbeszédpanel lapon hajtsa végre az alábbi lépéseket, és kattintson a **Mentés és Bezárás gombjára**:

    ![Hitelesítés] (./media/active-directory-saas-rally-software-tutorial/IC769542.png "Hitelesítés")

    1.  Hitelesítés legördülő listából válassza ki a **szervez, vagy egyszeri bejelentkezés hitelesítés**
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a szoftver Rally** párbeszédpanel lapon az **Identitás-szolgáltató azonosítója** értéket másolja és illessze be a **Identitás szolgáltató URL-címe** mezőben lévő értéket
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a szoftver Rally** párbeszédpanel lapon másolja a vágólapra a **Távoli kijelentkezés URL-címe** értéket.

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-rally-software-tutorial/IC769547.png "Beállítás az egyszeri bejelentkezés")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
AAD felhasználóknak engedélyezni szeretné, hogy jelentkezzen be hogy ki kell építenie Rally szoftveralkalmazás Azure Active Directory-felhasználó nevük segítségével.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be a szoftver Rally bérlői.

2.  Nyissa meg a **beállítási \> felhasználók**, és kattintson az **+ Add New**.

    ![Felhasználók] (./media/active-directory-saas-rally-software-tutorial/IC781039.png "Felhasználók")

3.  Az új felhasználó szövegmezőbe írja be a nevet, és kattintson a **Hozzáadás a részletek**gombra.

4.  **Create User** csoportban hajtsa végre az alábbi lépéseket:

    ![Felhasználó létrehozása] (./media/active-directory-saas-rally-software-tutorial/IC781040.png "Felhasználó létrehozása")

    1.  A **Felhasználónév** mezőbe írja be a kívánt kiépítése Azure AD-felhasználó nevét.
    2.  Az **E-mail cím** mezőbe írja be a kívánt kiépítése Azure AD-felhasználó e-mail címét.
    3.  Kattintson a **Mentés és Bezárás gombra**.

>[AZURE.NOTE]Bármely más Rally szoftver felhasználói fiók létrehozása eszközöket is használhatja, illetve API-hoz megadott Rally szoftverrel rendelkezést AAD felhasználói fiókok.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-rally-software-perform-the-following-steps"></a>Felhasználók hozzárendelése Rally szoftver, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  A **Szoftver Rally** alkalmazás integrációs lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-rally-software-tutorial/IC769548.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-rally-software-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).




