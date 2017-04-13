<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a RightAnswers |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a RightAnswers az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-rightanswers"></a>Oktatóprogram: Azure Active Directory-integráció a RightAnswers
  
Ebben az oktatóanyagban célja integrálása az Azure és RightAnswers megjelenítéséhez. Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   RightAnswers egyszeri bejelentkezéses engedélyezett előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók RightAnswers rendelt fogja tudni az alkalmazás használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját RightAnswers engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-rightanswers-tutorial/IC802925.png "Eset")
##<a name="enabling-the-application-integration-for-rightanswers"></a>Az alkalmazás integrációját RightAnswers engedélyezése
  
Ez a szakasz célja, hogyan RightAnswers az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-rightanswers-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját RightAnswers, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-rightanswers-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-rightanswers-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-rightanswers-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-rightanswers-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **RightAnswers**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-rightanswers-tutorial/IC802926.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **RightAnswers**, és válassza a **kész** , az alkalmazás hozzáadása gombra.
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy RightAnswers hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **RightAnswers** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-rightanswers-tutorial/IC802927.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az RightAnswers felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-rightanswers-tutorial/IC802928.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás-beállítások megadása** lap **Bejelentkezési a URL-cím** mezőbe írja be a felhasználóknak, hogy a bejelentkezés az RightAnswers alkalmazás által használt URL-CÍMÉT (például: *https://fortify.rightanswers.com/portal/ss/*), majd kattintson a **Tovább**gombra.

    ![Alkalmazás beállításainak konfigurálása] (./media/active-directory-saas-rightanswers-tutorial/IC802929.png "Alkalmazás beállításainak konfigurálása")

4.  A **konfigurálása az egyszeri bejelentkezés RightAnswers,** oldal metaadatainak letöltéséhez kattintson a **metaadat-alapú letöltése**és mentse a fájlt a számítógépen helyben metaadat.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-rightanswers-tutorial/IC802930.png "Egyszeri bejelentkezés beállítása")

5.  A metaadatok letöltött fájl küldése a RightAnswers támogatási csoportnak.

    >[AZURE.NOTE] Végezze el a tényleges SSO konfigurációs rendelkezik a RightAnswers támogatási csoporthoz.
Értesítést fog kapni, amikor az egyszeri bejelentkezés engedélyezve van-előfizetéséhez.

6.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-rightanswers-tutorial/IC802931.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a RightAnswers, hogy ki kell építenie RightAnswers be.  
RightAnswers, amíg az automatizált feladat kiépítési.  
Nincs teendő meg nem.
  
Felhasználók automatikusan létrejön egy szükség esetén az első egyszeri bejelentkezéses kísérlet során.

>[AZURE.NOTE]Bármely más RightAnswers felhasználói fiók létrehozása eszközöket is használhatja, illetve RightAnswers rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-rightanswers-perform-the-following-steps"></a>Felhasználók hozzárendelése RightAnswers, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **RightAnswers **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-rightanswers-tutorial/IC802932.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-rightanswers-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).