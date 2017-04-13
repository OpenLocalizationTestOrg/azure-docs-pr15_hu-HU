<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Veracode |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Veracode az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-veracode"></a>Oktatóprogram: Azure Active Directory-integráció a Veracode
  
Ebben az oktatóanyagban célja integrálása az Azure és Veracode megjelenítéséhez. Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Veracode egyszeri bejelentkezéses engedélyezett előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Veracode rendelt fogja tudni az alkalmazás használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Veracode engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-veracode-tutorial/IC802903.png "Eset")

##<a name="enabling-the-application-integration-for-veracode"></a>Az alkalmazás integrációját Veracode engedélyezése
  
Ez a szakasz célja, hogyan Veracode az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-veracode-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Veracode, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-veracode-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-veracode-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-veracode-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-veracode-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Veracode**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-veracode-tutorial/IC802904.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Veracode**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Veracode] (./media/active-directory-saas-veracode-tutorial/IC802905.png "Veracode")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Veracode hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
A Veracode alkalmazása egy bizonyos formátumban, amelyhez egyéni attribútum megfeleltetések hozzáadása a **saml jogkivonat attribútumok** konfigurációs várja meg a SAML előfeltételek.  
Az alábbi képernyőképen látható, a.

![Attribútumok] (./media/active-directory-saas-veracode-tutorial/IC802906.png "Attribútumok")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Veracode** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-veracode-tutorial/IC802907.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Veracode felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-veracode-tutorial/IC802908.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás beállításainak konfigurálása** lapon kattintson a **Tovább**gombra.

    ![Alkalmazás beállításainak konfigurálása] (./media/active-directory-saas-veracode-tutorial/IC802909.png "Alkalmazás beállításainak konfigurálása")

4.  A **Configure egyszeri bejelentkezés Veracode a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen helyben parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-veracode-tutorial/IC802910.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a Veracode vállalati webhely rendszergazdaként.

6.  A felső sávon kattintson a **Beállítások**gombra, és kattintson a **rendszergazda**.

    ![Felügyelete] (./media/active-directory-saas-veracode-tutorial/IC802911.png "Felügyelete")

7.  Kattintson a **SAML** fülre.

8.  A **Szervezet SAML beállításai** csoportban hajtsa végre az alábbi lépéseket:

    ![Felügyelete] (./media/active-directory-saas-veracode-tutorial/IC802912.png "Felügyelete")

    1.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Veracode** párbeszédpanel lapon a **Kibocsátó URL-címe** értéket másolja és illessze be a **kibocsátó** mezőben lévő értéket
    2.  A letöltött tanúsítvány feltölteni, kattintson a **Fájl kiválasztása**.
    3.  Jelölje ki az **Önkiszolgáló regisztráció engedélyezése**.

9.  A **Saját regisztrációs beállításai** csoportban hajtsa végre az alábbi lépéseket, és kattintson a **Mentés**:

    ![Felügyelete] (./media/active-directory-saas-veracode-tutorial/IC802913.png "Felügyelete")

    1.  **Új felhasználói aktiválást**jelölje be a **Nincs aktiválási szükséges**jelölőnégyzetet.
    2.  **Felhasználói adatok frissítések**területen jelölje ki a **Preferencia-sorrend Veracode felhasználói adatok**.
    3.  **SAML attribútum adatai**a válassza ki a következőket:
        -   **Felhasználói szerepkörök**
        -   **Házirend-rendszergazda**
        -   **Véleményező**
        -   **Biztonsági érdeklődő**
        -   **Ügyvezető**
        -   **Küldő**
        -   **Készítő**
        -   **Az ellenőrzési típusai**
        -   **Csoport tagsági**
        -   **Alapértelmezett csoportja**

10. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-veracode-tutorial/IC802914.png "Egyszeri bejelentkezés beállítása")

11. Kattintson a menü felső **attribútumok** a **SAML jogkivonat attribútumok** párbeszédpanel megnyitásához.

    ![Attribútumok] (./media/active-directory-saas-veracode-tutorial/IC795920.png "Attribútumok")

12. Kötelező attribútum megfeleltetések felvenni, hajtsa végre az alábbi lépéseket:

    ![Attribútumok] (./media/active-directory-saas-veracode-tutorial/IC802906.png "Attribútumok")

  	| Attribútumnév | Attribútumérték |
  	|:---------------|:----------------|
  	| Utónév      | User.givenName  |
  	| Utónév       | User.surname    |
  	| e-mailben          | User.mail       |

    1.  Az adatok tábla minden sorához a fenti kattintson a **felhasználó attribútum hozzáadása**gombra.
    
    2.  Az **Attribútum neve** mezőbe írja be a attribútumnév meg abban a sorban látható.

    3.  Az **Attribútumérték** mezőbe az attribútumérték jelennek meg, hogy a sor kijelölése

    4.  Kattintson a **kész**gombra.

13. Kattintson a **módosítások alkalmazásához**.

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Veracode, hogy ki kell építenie Veracode be.  
Veracode, amíg az automatizált feladat kiépítési.  
Nincs teendő meg nem.
  
Felhasználók automatikusan létrejön egy szükség esetén az első egyszeri bejelentkezéses kísérlet során.

>[AZURE.NOTE] Bármely más Veracode felhasználói fiók létrehozása eszközöket is használhatja, illetve Veracode rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-veracode-perform-the-following-steps"></a>Felhasználók hozzárendelése Veracode, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Veracode **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-veracode-tutorial/IC802915.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-veracode-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).