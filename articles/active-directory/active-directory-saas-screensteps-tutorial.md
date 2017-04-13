<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a ScreenSteps |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a ScreenSteps az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Oktatóprogram: Azure Active Directory-integráció a ScreenSteps
  
Ebben az oktatóanyagban célja integrálása az Azure és ScreenSteps megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   ScreenSteps bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók ScreenSteps rendelt fogja tudni az alkalmazás a ScreenSteps vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját ScreenSteps engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-screensteps-tutorial/IC778516.png "Eset")
##<a name="enabling-the-application-integration-for-screensteps"></a>Az alkalmazás integrációját ScreenSteps engedélyezése
  
Ez a szakasz célja, hogyan ScreenSteps az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-screensteps-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját ScreenSteps, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-screensteps-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-screensteps-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-screensteps-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-screensteps-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **ScreenSteps**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-screensteps-tutorial/IC778517.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **ScreenSteps**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![ScreenSteps] (./media/active-directory-saas-screensteps-tutorial/IC778518.png "ScreenSteps")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy ScreenSteps hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **ScreenSteps** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-screensteps-tutorial/IC778519.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az ScreenSteps felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-screensteps-tutorial/IC778520.png "Beállítás az egyszeri bejelentkezés")

3.  Az **App URL-címe konfigurálása** lapon **ScreenSteps bejelentkezési az URL** mezőbe írja be az URL-t, a következő mintát "*https://\<bérlői neve\>. ScreenSteps.com*", majd kattintson a **Tovább**gombra.

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-screensteps-tutorial/IC778521.png "Állítsa be az app URL-címe")

4.  A **Configure egyszeri bejelentkezés ScreenSteps a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen parancsra.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-screensteps-tutorial/IC778522.png "Beállítás az egyszeri bejelentkezés")

5.  A különböző webes böngészőablakban jelentkezzen be a ScreenSteps vállalati webhely rendszergazdaként.

6.  Kattintson a **fiók kezelése**gombra.

    ![Fiókok kezelése] (./media/active-directory-saas-screensteps-tutorial/IC778523.png "Fiókok kezelése")

7.  **Távoli hitelesítés**gombra.

    ![Távoli hitelesítés] (./media/active-directory-saas-screensteps-tutorial/IC778524.png "Távoli hitelesítés")

8.  Kattintson a **Létrehozás hitelesítési végpontot**.

    ![Távoli hitelesítés] (./media/active-directory-saas-screensteps-tutorial/IC778525.png "Távoli hitelesítés")

9.  **Hitelesítés végpont létrehozása** csoportjában hajtsa végre az alábbi lépéseket:

    ![Hitelesítés végpont létrehozása] (./media/active-directory-saas-screensteps-tutorial/IC778526.png "Hitelesítés végpont létrehozása")

    1.  Az a **cím** mezőbe írja be a címet.
    2.  A **mód** listából válassza ki a **SAML**.
    3.  Kattintson a **létrehozása**gombra.

10. A **Távoli hitelesítési végpont** csoportban hajtsa végre az alábbi lépéseket:

    ![Távoli hitelesítési végpont] (./media/active-directory-saas-screensteps-tutorial/IC778527.png "Távoli hitelesítési végpont")

    1.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés ScreenSteps a** lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Távoli bejelentkezési URL-cím** mezőben lévő értéket.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés ScreenSteps a** lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **Kijelentkezés az URL-címe** mezőben lévő értéket.
    3.  Kattintson a **Válassza ki a fájlt**, és töltse ki a letöltött tanúsítvány.
    4.  Kattintson a **frissítés**gombra.

11. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-screensteps-tutorial/IC778542.png "Beállítás az egyszeri bejelentkezés")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a **ScreenSteps**, hogy ki kell építenie **ScreenSteps**be.  
**ScreenSteps**, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-account-to-screensteps-perform-the-following-steps"></a>Építse ScreenSteps egy felhasználói fiókot, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be az **ScreenSteps** bérlői webhelyen.

2.  Kattintson a **fiók kezelése**gombra.

    ![Fiókok kezelése] (./media/active-directory-saas-screensteps-tutorial/IC778523.png "Fiókok kezelése")

3.  Kattintson a **felhasználók**elemre.

    ![Felhasználók] (./media/active-directory-saas-screensteps-tutorial/IC778544.png "Felhasználók")

4.  Kattintson **a felhasználó létrehozása**elemre.

    ![Minden felhasználónak] (./media/active-directory-saas-screensteps-tutorial/IC778545.png "Minden felhasználónak")

5.  **Felhasználói szerepkör** listában jelölje ki a szerepkör a felhasználó számára.

6.  A felhasználói szerepkör csoportban írja be az**"Utónév**, **Vezetéknév**, **e-mailek**, **Bejelentkezés**, **jelszó** és **Jelszó megerősítése"**kiépítése be a kapcsolódó szövegdobozok kívánt érvényes AAD fiók.

    ![Új felhasználó] (./media/active-directory-saas-screensteps-tutorial/IC778546.png "Új felhasználó")

7.  A csoportok szakaszban jelölje be a **Hitelesítés csoport SAML ()**, és válassza a **Create User**.

    ![Csoportok] (./media/active-directory-saas-screensteps-tutorial/IC778547.png "Csoportok")

>[AZURE.NOTE]Bármely más ScreenSteps felhasználói fiók létrehozása eszközöket is használhatja, illetve ScreenSteps rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-screensteps-perform-the-following-steps"></a>Felhasználók hozzárendelése ScreenSteps, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **ScreenSteps **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-screensteps-tutorial/IC773094.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-screensteps-tutorial/IC778548.png "Adhatnak a felhasználóknak")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).