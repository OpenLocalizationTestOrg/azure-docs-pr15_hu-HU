<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a SmarterU |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a SmarterU az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-smarteru"></a>Oktatóprogram: Azure Active Directory-integráció a SmarterU
  
Ebben az oktatóanyagban célja integrálása az Azure és SmarterU megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   SmarterU bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók SmarterU rendelt fogja tudni az alkalmazás a SmarterU vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját SmarterU engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-smarteru-tutorial/IC777320.png "Eset")

##<a name="enabling-the-application-integration-for-smarteru"></a>Az alkalmazás integrációját SmarterU engedélyezése
  
Ez a szakasz célja, hogyan SmarterU az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-smarteru-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját SmarterU, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-smarteru-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-smarteru-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-smarteru-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-smarteru-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **SmarterU**.

    ![Alkalmazás fallery] (./media/active-directory-saas-smarteru-tutorial/IC777321.png "Alkalmazás fallery")

7.  Az eredmény ablaktáblában jelölje ki a **SmarterU**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![SmarterU] (./media/active-directory-saas-smarteru-tutorial/IC777322.png "SmarterU")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy SmarterU hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **SmarterU** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-smarteru-tutorial/IC777323.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az SmarterU felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-smarteru-tutorial/IC777324.png "Egyszeri bejelentkezés beállítása")

3.  A **beállítás az egyszeri bejelentkezés SmarterU a** lapra, töltse le a metaadatok, kattintson a **metaadat-alapú letöltése**, és kattintson az adatfájl helyi meghajtóra **c:\\SmarterUMetaData.cer**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-smarteru-tutorial/IC777325.png "Egyszeri bejelentkezés beállítása")

4.  A különböző webes böngészőablakban jelentkezzen be a SmarterU vállalati webhely rendszergazdaként.

5.  Kattintson az eszköztáron a képernyő tetején kattintson a **Fiókbeállítások menügombra**.

    ![Fiókbeállítások] (./media/active-directory-saas-smarteru-tutorial/IC777326.png "Fiókbeállítások")

6.  A fiók konfigurálása lapon hajtsa végre az alábbi lépéseket:

    ![Külső engedély] (./media/active-directory-saas-smarteru-tutorial/IC777327.png "Külső engedély")

    1.  Jelölje be a **külső engedélyezése**.
    2.  A **Fő bejelentkezési vezérlő** csoportban jelölje ki az **SmarterU** lapot.
    3.  **Felhasználói alapértelmezett bejelentkezés** csoportban jelölje be a **SmarterU** fülre.
    4.  Jelölje ki a **Okta engedélyezése**.
    5.  A letöltött metaadat-fájl tartalmának másolja, és illessze be a **Metaadat-alapú Okta** mezőben lévő értéket.
    6.  Kattintson a **Mentés**gombra.

7.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-smarteru-tutorial/IC777328.png "Egyszeri bejelentkezés beállítása")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a SmarterU, hogy ki kell építenie SmarterU be.  
SmarterU, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be az **SmarterU** bérlői webhelyen.

2.  Kattintson a **felhasználók**.

3.  A felhasználó csoportban hajtsa végre az alábbi lépéseket:

    ![Új felhasználó] (./media/active-directory-saas-smarteru-tutorial/IC777329.png "Új felhasználó")

    1.  Válassza a **+ felhasználói**.
    2.  Írja be a következő szövegdobozok a kapcsolódó attribútum értékeket, a Azure AD-felhasználói fiók: **elsődleges E-mail**, **Alkalmazottkód**, **jelszót**, **Ellenőrizze a jelszó**, **a nevet**, **Vezetéknév**.
    3.  Kattintson **az aktív**.
    4.  Kattintson a **Mentés**gombra.

>[AZURE.NOTE] Bármely más SmarterU felhasználói fiók létrehozása eszközöket is használhatja, illetve SmarterU rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-smarteru-perform-the-following-steps"></a>Felhasználók hozzárendelése SmarterU, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **SmarterU **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-smarteru-tutorial/IC777330.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-smarteru-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).