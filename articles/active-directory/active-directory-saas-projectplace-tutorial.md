<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Projectplace |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Projectplace az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-projectplace"></a>Oktatóprogram: Azure Active Directory-integráció a Projectplace
  
Ebben az oktatóanyagban célja integrálása az Azure és Projectplace megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Projectplace egyszeri bejelentkezéses engedélyezett előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Projectplace rendelt fogja tudni az alkalmazás a Projectplace vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Projectplace engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-projectplace-tutorial/IC790217.png "Eset")
##<a name="enabling-the-application-integration-for-projectplace"></a>Az alkalmazás integrációját Projectplace engedélyezése
  
Ez a szakasz célja, hogyan Projectplace az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-projectplace-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Projectplace, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-projectplace-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-projectplace-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-projectplace-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-projectplace-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Projectplace**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-projectplace-tutorial/IC790218.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Projectplace**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![ProjectPlace] (./media/active-directory-saas-projectplace-tutorial/IC790219.png "ProjectPlace")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Projectplace hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Projectplace** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-projectplace-tutorial/IC790220.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Projectplace felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-projectplace-tutorial/IC790221.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás URL-cím beállítása** lapon az **URL a bejelentkezési Projectplace** mezőbe írja be a ProjectPlace bérlői webhely URL-címet (például: "*http://company.projectplace.com*"), és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-projectplace-tutorial/IC790222.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés a Projectplace** lapon kattintson a **metaadat-alapú letöltése**elemre, és mentse a fájlt a számítógépen a metaadat.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-projectplace-tutorial/IC790223.png "Egyszeri bejelentkezés beállítása")

5.  Küldje el a metadatafile a Projectplace részlegtől.

    >[AZURE.NOTE] Egyszeri bejelentkezés konfiguráció a Projectplace támogatási csoportja által végrehajtandó tartalmaz. Értesítést kaphat arról, amint a konfiguráció befejeződött.

6.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-projectplace-tutorial/IC790227.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Projectplace, hogy ki kell építenie Projectplace be.  
Projectplace, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **Projectplace** vállalati webhely.

2.  Lépjen a **személyek**lapra, és kattintson a **tagok**gombra.

    ![Személyek] (./media/active-directory-saas-projectplace-tutorial/IC790228.png "Személyek")

3.  Kattintson a **tag hozzáadása**elemre.

    ![A tagok hozzáadása] (./media/active-directory-saas-projectplace-tutorial/IC790232.png "A tagok hozzáadása")

4.  **Tag hozzáadása** csoportban hajtsa végre az alábbi lépéseket:

    ![Új tagok] (./media/active-directory-saas-projectplace-tutorial/IC790233.png "Új tagok")

    1.  Az **Új tagok** mezőbe írja be a kapcsolódó szövegdobozok kiépítése kívánt érvényes AAD fiókkal annak az e-mail címét.
    2.  Kattintson a **Küldés** gombra

        >[AZURE.NOTE] Az Azure Active Directory fióktulajdonos többek között a előtt aktívvá válik, győződjön meg arról, hogy a fiók mutató hivatkozást egy e-mailt küldi.
    
>[AZURE.NOTE]Bármely más Projectplace felhasználói fiók létrehozása eszközöket is használhatja, illetve Projectplace rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-projectplace-perform-the-following-steps"></a>Felhasználók hozzárendelése Projectplace, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Projectplace **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-projectplace-tutorial/IC790234.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-projectplace-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).