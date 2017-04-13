<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Clever |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Clever az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-clever"></a>Oktatóprogram: Azure Active Directory-integráció a Clever

Ebben az oktatóanyagban célja integrálása az Azure és Clever megjelenítéséhez. Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Intelligens bérlői webhelyre

Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók Clever rendelt tudnak egyetlen bejelentkezés az alkalmazásba intelligens vállalat webhelye (a szolgáltatás által kezdeményezett szolgáltató bejelentkezési) vagy használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md).

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Clever engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-clever-tutorial/IC798977.png "Eset")
##<a name="enabling-the-application-integration-for-clever"></a>Az alkalmazás integrációját Clever engedélyezése

Ez a szakasz célja, hogyan Clever az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-clever-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Clever, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-clever-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-clever-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-clever-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-clever-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Clever**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-clever-tutorial/IC798978.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Clever**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Intelligens] (./media/active-directory-saas-clever-tutorial/IC798979.png "Intelligens")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Clever hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Az intelligens alkalmazás a SAML Előfeltételek vár, egy bizonyos formátumban, amelyhez egyéni attribútum megfeleltetések hozzáadása a **saml jogkivonat attribútumok** konfigurációs.  
Az alábbi képernyőképen látható, a.

![Attribútumok] (./media/active-directory-saas-clever-tutorial/IC798980.png "Attribútumok")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Clever** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-clever-tutorial/IC784682.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Clever felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-clever-tutorial/IC798981.png "Egyszeri bejelentkezés beállítása")

3.  A **App URL-cím beállítása** lapon az **Intelligens bejelentkezési a URL-cím** mezőbe írja be a felhasználóknak, hogy a bejelentkezés az intelligens alkalmazás által használt URL-CÍMÉT (például: *https://clever.com/in/azsandbox*), majd kattintson a **Tovább**gombra.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-clever-tutorial/IC798982.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés Clever a** lapon a metaadat-alapú letöltéséhez kattintson a **metaadat-alapú letöltése**, és mentse a fájlt a számítógépen helyben a metaadat parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-clever-tutorial/IC798983.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be az intelligens vállalati webhely rendszergazdaként.

6.  Kattintson az eszköztáron kattintson a **Csevegés bejelentkezés elemre**.

    ![Azonnali Login] (./media/active-directory-saas-clever-tutorial/IC798984.png "Azonnali Login")

7.  Az **Azonnali Login** lapon hajtsa végre az alábbi lépéseket:

    ![Azonnali bejelentkezés] (./media/active-directory-saas-clever-tutorial/IC798985.png "Azonnali bejelentkezés")

    1.  Írja be a **bejelentkezési URL-cím**.  

        >[AZURE.NOTE] A **Bejelentkezési URL-cím** egyéni érték.
A tényleges érték elérheti az intelligens támogatási szolgálatához.

    2.  **Identitás rendszer**jelölje be az **ADFS**.
    3.  Kattintson a **Mentés**gombra.

8.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-clever-tutorial/IC798986.png "Egyszeri bejelentkezés beállítása")

9.  Kattintson a menü felső **attribútumok** a **SAML jogkivonat attribútumok** párbeszédpanel megnyitásához.

    ![Attribútumok] (./media/active-directory-saas-clever-tutorial/IC795920.png "Attribútumok")

10. Kötelező attribútum megfeleltetések felvenni, hajtsa végre az alábbi lépéseket:

    ![SAML jogkivonat attribútumok] (./media/active-directory-saas-clever-tutorial/IC795921.png "SAML jogkivonat attribútumok")

  	|Attribútumnév|Attribútumérték|
  	|---|---|
  	|clever.Student.credentials.District\_felhasználónév|User.userPrincipalName|

    1.  Az adatok tábla minden sorához a fenti kattintson a **felhasználó attribútum hozzáadása**gombra.
    2.  Az **Attribútum neve** mezőbe írja be a attribútumnév meg abban a sorban látható.
    3.  Az **Attribútumérték** mezőbe az attribútumérték jelennek meg, hogy a sor kijelölése
    4.  Kattintson a **kész**gombra.

11. Kattintson a **módosítások alkalmazásához**.

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Clever, hogy ki kell építenie Clever be.  
Clever, ha a kiépítési szüksége van az intelligens támogatási csoportja által végrehajtandó kézi feladatot.

>[AZURE.NOTE] Bármely más intelligens felhasználói fiók létrehozása eszközöket is használhatja, illetve Clever rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-clever-perform-the-following-steps"></a>Felhasználók hozzárendelése Clever, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Clever **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-clever-tutorial/IC798987.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-clever-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
