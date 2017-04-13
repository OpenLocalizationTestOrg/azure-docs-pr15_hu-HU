<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Clarizen |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Clarizen az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Oktatóprogram: Azure Active Directory-integráció a Clarizen

Ebben az oktatóanyagban célja integrálása az Azure és Clarizen megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Clarizen egyszeri bejelentkezéses engedélyezett előfizetés

Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Clarizen rendelt fogja tudni az alkalmazás a Clarizen vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Clarizen engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-clarizen-tutorial/IC784679.png "Eset")
##<a name="enabling-the-application-integration-for-clarizen"></a>Az alkalmazás integrációját Clarizen engedélyezése

Ez a szakasz célja, hogyan Clarizen az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-clarizen-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Clarizen, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-clarizen-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-clarizen-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-clarizen-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-clarizen-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Clarizen**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-clarizen-tutorial/IC784680.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Clarizen**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Clarizen] (./media/active-directory-saas-clarizen-tutorial/IC784681.png "Clarizen")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Clarizen hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Clarizen** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-clarizen-tutorial/IC784682.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Clarizen felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-clarizen-tutorial/IC784683.png "Egyszeri bejelentkezés beállítása")

3.  A **Configure egyszeri bejelentkezés Clarizen a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-clarizen-tutorial/IC784684.png "Egyszeri bejelentkezés beállítása")

4.  A különböző webes böngészőablakban, jelentkezzen be rendszergazdaként vállalat **Clarizen** webhelye (pl.: *https://app2.clarizen.com/Clarizen/Pages/Service/Login.aspx*).

5.  Kattintson a felhasználó nevét, és kattintson a **Beállítások**gombra.

    ![Beállítások] (./media/active-directory-saas-clarizen-tutorial/IC784685.png "Beállítások")

6.  Kattintson az **Általános beállítások** fülre, és melletti **Összevont hitelesítés**, kattintson a **Szerkesztés**gombra.

    ![Globális beállításai] (./media/active-directory-saas-clarizen-tutorial/IC786906.png "Globális beállításai")

7.  A **Szövetséges hitelesítési** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Összevont hitelesítés] (./media/active-directory-saas-clarizen-tutorial/IC785892.png "Összevont hitelesítés")

    1.  Kattintson a **Töltse fel** a letöltött tanúsítvány feltöltése elemre.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Clarizen** párbeszédpanel lapon másolja az **Egyszeri bejelentkezéses szolgáltatás URL-címe** értéket, és illessze be a **bejelentkezési URL** mezőben lévő értéket.
    3.  Az Azure klasszikus portálon **beállítása egyetlen kijelentkezési a Clarizen** párbeszédpanel lapon az **Egyetlen Sign-Out szolgáltatás URL-címe** értéket másolja és illessze be a **Sign-out URL-címe** mezőben lévő értéket.
    4.  Jelölje ki a **BEJEGYZÉST használja**.
    5.  Kattintson a **Mentés**gombra.

8.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-clarizen-tutorial/IC784688.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Clarizen, hogy ki kell építenie Clarizen be.  
Clarizen, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **Clarizen** vállalati webhely.

2.  Kattintson a **személyek**elemre.

    ![Személyek] (./media/active-directory-saas-clarizen-tutorial/IC784689.png "Személyek")

3.  Kattintson a **felhasználó felkérése**gombra.

    ![Felhasználók meghívása] (./media/active-directory-saas-clarizen-tutorial/IC784690.png "Felhasználók meghívása")

4.  A személyek meghívása párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Személyek meghívása] (./media/active-directory-saas-clarizen-tutorial/IC784691.png "Személyek meghívása")

    1.  Az **E-mail** mezőbe írja be egy érvényes Azure Active Directory-fiókját, rendelkezést szeretné annak az e-mail címét.
    2.  Kattintson a **Meghívás**gombra.

    >[AZURE.NOTE] Az Azure Active Directory fióktulajdonos a fog egy e-mailt, és kövesse a előtt aktívvá válik, győződjön meg arról, hogy a fiók mutató hivatkozást.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-clarizen-perform-the-following-steps"></a>Felhasználók hozzárendelése Clarizen, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Clarizen **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-clarizen-tutorial/IC784692.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-clarizen-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
