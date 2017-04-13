<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a BlueJeans |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a BlueJeans az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-ad-integration-with-bluejeans"></a>Oktatóprogram: Azure AD-integráció a BlueJeans

Ebben az oktatóanyagban célja integrálása az Azure és BlueJeans megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   BlueJeans egyszeri bejelentkezéses engedélyezett előfizetés

Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók BlueJeans rendelt fogja tudni az alkalmazás a BlueJeans vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját BlueJeans engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-bluejeans-tutorial/IC785860.png "Eset")
##<a name="enabling-the-application-integration-for-bluejeans"></a>Az alkalmazás integrációját BlueJeans engedélyezése

Ez a szakasz célja, hogyan BlueJeans az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-bluejeans-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját BlueJeans, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-bluejeans-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-bluejeans-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-bluejeans-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-bluejeans-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **BlueJeans**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-bluejeans-tutorial/IC785861.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **BlueJeans**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![BlueJeans] (./media/active-directory-saas-bluejeans-tutorial/IC785862.png "BlueJeans")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy BlueJeans hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **BlueJeans** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-bluejeans-tutorial/IC785863.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az BlueJeans felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-bluejeans-tutorial/IC785864.png "Egyszeri bejelentkezés beállítása")

3.  A **App URL-címe beállítása** lapon az **URL a bejelentkezési BlueJeans** mezőbe írja be az URL-t, a következő mintát "*https://company.BlueJeans.com*", és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-bluejeans-tutorial/IC785865.png "Állítsa be az App URL-címe")

4.  A **Configure egyszeri bejelentkezés BlueJeans a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-bluejeans-tutorial/IC785866.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a **BlueJeans** vállalati webhely rendszergazdaként.

6.  Nyissa meg a **felügyeleti \> csoportbeállítások \> biztonsági**.

    ![Rendszergazda] (./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Rendszergazda")

7.  A **biztonsági beállításai** szakaszban tegye a következőket:

    ![SAML egyszeri bejelentkezés] (./media/active-directory-saas-bluejeans-tutorial/IC785869.png "SAML egyszeri bejelentkezés")

    1.  Jelölje ki a **SAML egyszeri bejelentkezéses**.
    2.  Jelölje be **az automatikus kiépítési engedélyezése**.

8.  Áthelyezése a következő lépéseket:

    ![Tanúsítvány elérési út] (./media/active-directory-saas-bluejeans-tutorial/IC785870.png "Tanúsítvány elérési út")

    1.  **Válassza a fájl**gombra, és töltse ki a letöltött tanúsítvány.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a BlueJeans** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Bejelentkezési URL-cím** mezőben lévő értéket.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a BlueJeans** párbeszédpanel lapon másolja a vágólapra az **URL-cím módosítása jelszó** értéket, és illessze be a **Jelszó módosítása URL-címe** mezőben lévő értéket.
    4.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a BlueJeans** párbeszédpanel lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **Kijelentkezés URL-címe** mezőben lévő értéket.

9.  Áthelyezése a következő lépéseket:

    ![Módosítások mentése] (./media/active-directory-saas-bluejeans-tutorial/IC785874.png "Módosítások mentése")

    1.  A **felhasználói azonosító** mezőbe írja be a **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**.
    2.  Az **E-mail** mezőbe írja be a **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**.
    3.  A **módosítások mentése**gombra.

10. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-bluejeans-tutorial/IC785876.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a BlueJeans, hogy ki kell építenie BlueJeans be.  
BlueJeans, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **BlueJeans** vállalati webhely.

2.  Nyissa meg a **ADMIN \> felhasználókezelés \> felhasználó hozzáadása**.

    ![Rendszergazda] (./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Rendszergazda")

    >[AZURE.IMPORTANT] A **Felhasználó hozzáadása** lap csak akkor érhető el, ha a **Biztonság fülre**, a **engedélyezése automatikus kiépítési** nincs bejelölve.

3.  A **Felhasználó hozzáadása** csoportban hajtsa végre az alábbi lépéseket:

    ![Felhasználó hozzáadása] (./media/active-directory-saas-bluejeans-tutorial/IC785886.png "Felhasználó hozzáadása")

    1.  Írja be a **BlueJeans felhasználónév**, az **E-mail címét**, egy **BlueJeans Értekezletazonosító**, **Moderátornak PIN-kód**, **Teljes nevét**, és a **vállalat** érvényes AAD fiókkal szeretné építse be a kapcsolódó szövegdobozok.
    2.  Kattintson a **felhasználó hozzáadása**elemre.

>[AZURE.NOTE] Bármely más BlueJeans felhasználói fiók létrehozása eszközöket is használhatja, illetve BlueJeans rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-bluejeans-perform-the-following-steps"></a>Felhasználók hozzárendelése BlueJeans, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **BlueJeans **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-bluejeans-tutorial/IC785887.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-bluejeans-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).
