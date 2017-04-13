<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a 15Five |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a 15Five az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-15five"></a>Oktatóprogram: Azure Active Directory-integráció a 15Five

Ebben az oktatóanyagban célja integrálása az Azure és 15Five megjelenítéséhez. Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   15Five egyszeri bejelentkezéses engedélyezett előfizetés

Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók 15Five rendelt fogja tudni az alkalmazás a 15Five vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját 15Five engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-15five-tutorial/IC784667.png "Eset")
##<a name="enabling-the-application-integration-for-15five"></a>Az alkalmazás integrációját 15Five engedélyezése

Ez a szakasz célja, hogyan 15Five az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-15five-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját 15Five, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-15five-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-15five-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-15five-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-15five-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **15Five**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-15five-tutorial/IC784668.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **15Five**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![15Five] (./media/active-directory-saas-15five-tutorial/IC784669.png "15Five")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja, hogyan engedélyezheti a felhasználóknak, hogy az összevonási alapján a SAML protokoll használatával Azure AD a saját fiók 15Five hitelesíteni a szerkezet.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **15Five** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-15five-tutorial/IC784670.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az 15Five felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-15five-tutorial/IC784671.png "Beállítás az egyszeri bejelentkezés")

3.  A **App URL-címe beállítása** lapon az **15Five bejelentkezési az URL** mezőbe írja be az URL-t, a következő mintát "*https://company.15Five.com*", és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-15five-tutorial/IC784672.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés 15Five a** lapon kattintson a **metaadat-alapú letöltése**, és ezután továbbítása a metaadat-fájlt a 15Five támogatási csoportjának.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-15five-tutorial/IC784673.png "Beállítás az egyszeri bejelentkezés")

    >[AZURE.NOTE] Egyszeri bejelentkezés kell engedélyezni kell a 15Five támogatási csoport által.

5.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-15five-tutorial/IC784674.png "Beállítás az egyszeri bejelentkezés")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a 15Five, hogy ki kell építenie 15Five be.  
15Five, ha a feladat manuális kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **15Five** vállalati webhely.

2.  Nyissa meg a **vállalat kezelése**.

    ![Kezelheti a vállalat] (./media/active-directory-saas-15five-tutorial/IC784675.png "Kezelheti a vállalat")

3.  Nyissa meg a **személyek \> felvehet személyeket**.

    ![Személyek] (./media/active-directory-saas-15five-tutorial/IC784676.png "Személyek")

4.  Új személy hozzáadása csoportban hajtsa végre az alábbi lépéseket:

    ![Új személy felvétele] (./media/active-directory-saas-15five-tutorial/IC784677.png "Új személy felvétele")

    1.  Írja be az **utónevet**, **Vezetéknevét**, **cím**, egy érvényes Azure Active Directory-fiókját, kiépítése a kapcsolódó szövegdobozok be a kívánt **E-mail címet** .
    2.  Kattintson a **kész**gombra.

    >[AZURE.NOTE] Az Azure Active Directory fióktulajdonos kap e-mailben, beleértve a előtt aktívvá válik, győződjön meg arról, hogy a fiók mutató hivatkozást.

>[AZURE.NOTE] Bármely más 15Five felhasználói fiók létrehozása eszközöket is használhatja, illetve rendelkezést AAD felhasználói fiókok 15Five által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-15five-perform-the-following-steps"></a>Felhasználók hozzárendelése 15Five, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **15Five **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-15five-tutorial/IC784678.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-15five-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).
