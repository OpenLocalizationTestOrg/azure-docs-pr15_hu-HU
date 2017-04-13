<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Kintone |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Kintone az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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
    ms.date="09/01/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-kintone"></a>Oktatóprogram: Azure Active Directory-integráció a Kintone
  
Ebben az oktatóanyagban célja integrálása az Azure és Kintone megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Kintone egyszeri bejelentkezéses engedélyezett előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Kintone rendelt fogja tudni az alkalmazás a Kintone vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Kintone engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-kintone-tutorial/IC785859.png "Eset")
##<a name="enabling-the-application-integration-for-kintone"></a>Az alkalmazás integrációját Kintone engedélyezése
  
Ez a szakasz célja, hogyan Kintone az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-kintone-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Kintone, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-kintone-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-kintone-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-kintone-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-kintone-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Kintone**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-kintone-tutorial/IC785867.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Kintone**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Kintone] (./media/active-directory-saas-kintone-tutorial/IC785871.png "Kintone")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Kintone hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Kintone** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-kintone-tutorial/IC785872.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Kintone felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-kintone-tutorial/IC785873.png "Egyszeri bejelentkezés beállítása")

3.  A **App URL-címe beállítása** lapon az **URL a bejelentkezési Kintone** mezőbe írja be az URL-t, a következő mintát "*https://company.kintone.com*", és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-kintone-tutorial/IC785875.png "Állítsa be az App URL-címe")

4.  A **Configure egyszeri bejelentkezés Kintone a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-kintone-tutorial/IC785878.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a **Kintone** vállalati webhely rendszergazdaként.

6.  Kattintson a **Beállítások**gombra.

    ![Beállítások] (./media/active-directory-saas-kintone-tutorial/IC785879.png "Beállítások")

7.  Kattintson a **felhasználók és a rendszer felügyeleti**.

    ![Felhasználók és a rendszer felügyelete] (./media/active-directory-saas-kintone-tutorial/IC785880.png "Felhasználók és a rendszer felügyelete")

8.  A **rendszergazdai \> biztonsági** kattintson a **Bejelentkezés**gombra.

    ![Bejelentkezés] (./media/active-directory-saas-kintone-tutorial/IC785881.png "Bejelentkezés")

9.  **Hitelesítés SAML engedélyezése**gombra.

    ![SAML-hitelesítés] (./media/active-directory-saas-kintone-tutorial/IC785882.png "SAML-hitelesítés")

10. A SAML hitelesítési csoportban hajtsa végre az alábbi lépéseket:

    ![SAML-hitelesítés] (./media/active-directory-saas-kintone-tutorial/IC785883.png "SAML-hitelesítés")

    1.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Kintone** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Bejelentkezési URL-cím** mezőben lévő értéket.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Kintone** párbeszédpanel lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **Kijelentkezés URL-címe** mezőben lévő értéket.
    3.  **Tallózással keresse meg** a letöltött tanúsítvány feltöltése gombra.
    4.  Kattintson a **Mentés**gombra.

11. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-kintone-tutorial/IC785884.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Kintone, hogy ki kell építenie Kintone be.  
Kintone, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **Kintone** vállalati webhely.

2.  Kattintson a **beállítás**gombra.

    ![Beállítások] (./media/active-directory-saas-kintone-tutorial/IC785879.png "Beállítások")

3.  Kattintson a **felhasználók és a rendszer felügyeleti**.

    ![Felhasználói és a rendszer felügyelete] (./media/active-directory-saas-kintone-tutorial/IC785880.png "Felhasználói és a rendszer felügyelete")

4.  A **Felhasználók felügyelete**csoportban kattintson a **Részlegek és a felhasználók**elemre.

    ![Részleg és a felhasználók] (./media/active-directory-saas-kintone-tutorial/IC785888.png "Részleg és a felhasználók")

5.  Kattintson az **Új felhasználó**gombra.

    ![Új felhasználók számára] (./media/active-directory-saas-kintone-tutorial/IC785889.png "Új felhasználók számára")

6.  **Új felhasználói** csoportban hajtsa végre az alábbi lépéseket:

    ![Új felhasználók számára] (./media/active-directory-saas-kintone-tutorial/IC785890.png "Új felhasználók számára")

    1.  Írja be a **Megjelenítendő név**, **Bejelentkezési nevét**, **Új jelszót**, **A jelszó megerősítése**, **E-mail címét** és egyéb részletek be a kapcsolódó texboxes kiépítése kívánt érvényes AAD fiók.
    2.  Kattintson a **Mentés**gombra.

>[AZURE.NOTE] Bármely más Kintone felhasználói fiók létrehozása eszközöket is használhatja, illetve Kintone rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-kintone-perform-the-following-steps"></a>Felhasználók hozzárendelése Kintone, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Kintone **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-kintone-tutorial/IC785891.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-kintone-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).