<properties 
    pageTitle="Oktatóprogram: Azure Active Directory integráció a Thoughtworks Mingle |} Microsoft Azure" 
    description="Megtudhatja, hogy miként használata Thoughtworks Mingle Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Oktatóprogram: Azure Active Directory integráció a Thoughtworks Mingle
  
Ebben az oktatóanyagban célja integrálása az Azure és Thoughtworks Mingle megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Thoughtworks Mingle bérlői webhelyre
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Thoughtworks Mingle engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785150.png "Eset")

##<a name="enabling-the-application-integration-for-thoughtworks-mingle"></a>Az alkalmazás integrációját Thoughtworks Mingle engedélyezése
  
Ez a szakasz célja, hogyan engedélyezhető az alkalmazás integrációját Thoughtworks Mingle tagolása.

###<a name="to-enable-the-application-integration-for-thoughtworks-mingle-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Thoughtworks Mingle, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **thoughtworks mingle**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785151.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Thoughtworks Mingle**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Thoughtworks Mingle] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785152.png "Thoughtworks Mingle")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja engedélyezése a felhasználóknak, hogy hitelesíteni Thoughtworks Mingle fiókjukhoz az összevonási alapján a SAML protokoll használatával Azure Active Directory tagolása.  
Ez az eljárás részeként szükségesek Thoughtworks Mingle feltöltése egy tanúsítványt.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon alkalmazás integrációs **Thoughtworks Mingle **lapján kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785153.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az Thoughtworks Mingle felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785154.png "Beállítás az egyszeri bejelentkezés")

3.  A **App URL-címe beállítása** lapon az **Thoughtworks Mingle bérlői webhely URL-cím** mezőbe írja be az URL-t, a következő mintát "*http://company.mingle.thoughtworks.com*", és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785155.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés Thoughtworks Mingle a** lapon kattintson a letöltés metaadat gombra, és mentse a számítógépre.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785156.png "Beállítás az egyszeri bejelentkezés")

5.  Jelentkezzen be rendszergazdaként a vállalat **Thoughtworks Mingle** webhelyen.

6.  Kattintson a **rendszergazda** fülre, és kattintson a **Config egyszeri Bejelentkezést**.

    ![Egyszeri bejelentkezés Config] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785157.png "Egyszeri bejelentkezés Config")

7.  **Egyszeri bejelentkezés Config** csoportban hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés Config] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785158.png "Egyszeri bejelentkezés Config")

    1.  A metaadat-fájl feltöltéséhez kattintson a **fájl kiválasztása**.
    2.  A **módosítások mentése**gombra.

8.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785159.png "Beállítás az egyszeri bejelentkezés")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
AAD felhasználóknak engedélyezni szeretné, hogy jelentkezzen be hogy ki kell építenie az Azure Active Directory-felhasználó nevük segítségével Thoughtworks Mingle alkalmazás.  
Thoughtworks Mingle, amíg a kézi tevékenység kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a vállalat Thoughtworks Mingle webhelyen.

2.  Kattintson a **profil**gombra.

    ![Az első projekt] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785160.png "Az első projekt")

3.  Kattintson a **rendszergazda** fülre, és kattintson a **felhasználók**.

    ![Felhasználók] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785161.png "Felhasználók")

4.  Kattintson az **Új felhasználó**gombra.

    ![Új felhasználó] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785162.png "Új felhasználó")

5.  Az **Új felhasználó** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Új felhasználó] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785163.png "Új felhasználó")

    1.  Írja be a **bejelentkezési nevét**, a **megjelenítendő név**, a **kiválasztása jelszót**, a **jelszó megerősítése** be a kapcsolódó szövegdobozok kiépítése kívánt érvényes AAD fiók.
    2.  **Felhasználó típusa**csoportban jelölje ki a **teljes felhasználói**.
    3.  Kattintson a **profil létrehozása**.

>[AZURE.NOTE] Bármely más Thoughtworks Mingle felhasználói fiók létrehozása eszközöket is használhatja, illetve API-khoz Thoughtworks Mingle által biztosított rendelkezést AAD felhasználói fiókok.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-thoughtworks-mingle-perform-the-following-steps"></a>Felhasználók hozzárendelése Thoughtworks Mingle, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Alkalmazás integrációs **Thoughtworks Mingle** lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785164.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
