<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Benefitsolver |} Microsoft Azure"
    description="Megtudhatja, hogyan használhatja a Benefitsolver az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a>Oktatóprogram: Azure Active Directory-integráció a Benefitsolver

Ebben az oktatóanyagban célja integrálása az Azure és Benefitsolver megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Benefitsolver egyszeri bejelentkezéses engedélyezett előfizetés

Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Benefitsolver rendelt fogja tudni az alkalmazás használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)egyszeri bejelentkezési.

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Benefitsolver engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Eset")
##<a name="enabling-the-application-integration-for-benefitsolver"></a>Az alkalmazás integrációját Benefitsolver engedélyezése

Ez a szakasz célja, hogyan Benefitsolver az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-benefitsolver-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Benefitsolver, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Benefitsolver**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Benefitsolver**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Benefitssolver] (./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Benefitsolver hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
A Benefitsolver alkalmazása egy bizonyos formátumban, amelyhez egyéni attribútum megfeleltetések hozzáadása a **saml jogkivonat attribútumok** konfigurációs várja meg a SAML előfeltételek.  
Az alábbi képernyőképen látható, a.

![Attribútumok] (./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Attribútumok")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Benefitsolver** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Benefitsolver felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás beállításainak megadása** lapon hajtsa végre az alábbi lépéseket:

    ![Alkalmazás beállításainak konfigurálása] (./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "Alkalmazás beállításainak konfigurálása")

    1.  Az **Bejelentkezési a URL-cím** mezőbe írja be a **http://azure.benefitsolver.com**.
    2.  **Válasz URL-cím** mezőbe írja be a **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.  


    3.  Kattintson a **Tovább**gombra.

4.  **Konfigurálása az egyszeri bejelentkezés Benefitsolver a** lapon a metaadat-alapú letöltéséhez kattintson a **metaadat-alapú letöltése**, és mentse a fájlt a számítógépen helyben metaadat parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Egyszeri bejelentkezés beállítása")

5.  A metaadatok letöltött fájl küldése a Benefitsolver támogatási csoportnak.

    >[AZURE.NOTE] Végezze el a tényleges SSO konfigurációs rendelkezik a Benefitsolver támogatási csoporthoz.
Értesítést fog kapni, amikor az egyszeri bejelentkezés engedélyezve van-előfizetéséhez.

6.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Egyszeri bejelentkezés beállítása")

7.  Kattintson a menü felső **attribútumok** a **SAML jogkivonat attribútumok** párbeszédpanel megnyitásához.

    ![Attribútumok] (./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "Attribútumok")

8.  Kötelező attribútum megfeleltetések felvenni, hajtsa végre az alábbi lépéseket:

    ![Attribútumok] (./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Attribútumok")

  	|Attribútumnév|Attribútumérték|
  	|---|---|
  	|ClientID|Ez az érték beolvasása a Benefitsolver támogatási csoport szüksége.|
  	|ClientKey|Ez az érték beolvasása a Benefitsolver támogatási csoport szüksége.|
  	|LogoutURL|Ez az érték beolvasása a Benefitsolver támogatási csoport szüksége.|
  	|Alkalmazottkód|Ez az érték beolvasása a Benefitsolver támogatási csoport szüksége.|

    1.  Az adatok tábla minden sorához a fenti kattintson a **felhasználó attribútum hozzáadása**gombra.
    2.  Az **Attribútum neve** mezőbe írja be a attribútumnév meg abban a sorban látható.
    3.  Az **Attribútumérték** mezőbe az attribútumérték jelennek meg, hogy a sor kijelölése
    4.  Kattintson a **kész**gombra.

9.  Kattintson a **módosítások alkalmazásához**.

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Benefitsolver, hogy ki kell építenie Benefitsolver be.  
Az alkalmazás keresztül (általában nightly) a HRIS rendszerből népszámlálás fájl töltve Benefitsolver, amíg alkalmazottak adataiból van.  

>[AZURE.NOTE] Bármely más Benefitsolver felhasználói fiók létrehozása eszközöket is használhatja, illetve Benefitsolver rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-benefitsolver-perform-the-following-steps"></a>Felhasználók hozzárendelése Benefitsolver, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Benefitsolver **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
