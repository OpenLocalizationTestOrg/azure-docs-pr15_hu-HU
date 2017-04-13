<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a & frankly |} Microsoft Azure"
    description="Megtudhatja, hogy miként egyszeri bejelentkezés közötti Azure Active Directory konfigurálása és & frankly."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-frankly"></a>Oktatóprogram: Azure Active Directory-integráció a & frankly

Ebben az oktatóanyagban célja mutatja, hogy hogyan integrálja & frankly az Azure Active Directory (Azure Active Directory).

Integrálása és frankly az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel a & frankly megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett be, a & frankly (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure AD-integráció a & frankly, akkor szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- A & frankly egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Hozzáadás és a gyűjteményből frankly
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-frankly-from-the-gallery"></a>Hozzáadás és a gyűjteményből frankly
& Frankly az Azure Active Directory integrációja konfigurálásához, hogy fel kell vennie és frankly a felügyelt szoftver alkalmazásai a gyűjteményből.

**Adja hozzá, és frankly a gyűjteményből hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .
    
    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.
    
    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be az **& frankly**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_01.png)

7. Az eredmények panel **& frankly**kijelölése, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Az alkalmazás kijelöli a gyűjteményben](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja bemutatják, hogyan beállítása és tesztelése Azure AD egy bejelentkezés, és frankly alapján a "Britta Simon" nevű próba felhasználó.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a partner felhasználó és frankly el az Azure Active Directory-felhasználók. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználói hivatkozás viszonya & frankly kell létrehozni.

Ez a hivatkozás kapcsolat hozzárendelésével a értéket a **felhasználó nevét** a **felhasználónév** értékként Azure AD, & frankly jön létre.

Állítsa be és tesztelje az Azure Active Directory egyszeri bejelentkezés, és frankly, akkor kell végrehajtanom az alábbi építőelemek:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
3. **[Létrehozása egy & frankly teszt felhasználó](#creating-a-&frankly-test-user)** - szeretné, hogy egy partner a Britta Simon & frankly Ez azt jelenti hogy Azure Active Directory ábrázolása csatolt.
4. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory egyszeri bejelentkezés beállítása

Ebben a részben Azure AD az egyszeri bejelentkezés a klasszikus portálon engedélyezése és konfigurálása az egyszeri bejelentkezés az a és frankly alkalmazást.

**Konfigurálása az Azure Active Directory egyetlen bejelentkezéses a & frankly, hajtsa végre az alábbi lépéseket:**

1. Az a **és frankly** alkalmazás integrációs lapon, a klasszikus portálon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.
     
    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné használni a & frankly jelentkezzen be felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_03.png)

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon szeretne az alkalmazás beállítása **IDP kezdeményezett mód**, ha hajtsa végre az alábbi lépéseket, és kattintson a **Tovább**gombra:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_04.png)

    egy. Az **azonosító** mezőbe írja be az URL-cím használata a következő mintát:`https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/metadata.php/<tenant id>`

    b. Az **Válasz URL-cím** mezőbe írja be az URL-cím használata a következő mintát:`https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/saml2-acs.php/<tenant id>`

    c billentyűkombinációt. Kattintson a **Tovább** gombra

4. Ha ki szeretne beállítása az alkalmazás **SP kezdeményezett mód** az **Alkalmazás beállításainak megadása** párbeszédpanel oldalon, kattintson a **"Megjelenítése speciális beállítások (nem kötelező)"** , és írja be a **Bejelentkezési a URL-CÍMÉT** , és kattintson a **Tovább**gombra.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_05.png)

    egy. Az **Bejelentkezési a URL-cím** mezőbe írja be az URL-cím használata a következő mintát:`https://andfrankly.com/saml/okta/?saml_sso=<tenant id>`

    b. Kattintson a **Tovább** gombra

    > [AZURE.NOTE] Felhívjuk a figyelmét arra, hogy ezek nem a tényleges értékek. Ezek az értékek frissítése a tényleges bejelentkezési a URL-CÍMÉT, azonosítóját és válasz URL-címe van. Partner [help@andfrankly.com](emailTo:help@andfrankly.com) megszerezni ezeket az értékeket.

5. Kattintson a **egyszeri bejelentkezés a konfigurálása és frankly** lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább**gombra:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_06.png)

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.

6. Ha be van állítva az alkalmazás SSO, lépjen kapcsolatba a & frankly támogatja a csapat keresztül [help@andfrankly.com](emailTo:help@andfrankly.com). A letöltött metaadat-fájl csatolása és megosztani a fájlt, és frankly csoportnak, hogy egyszeri bejelentkezés beállítása a oldalán.

7. A klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.
    
    ![Azure Active Directory Single Sign-On][10]

8. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
    
    ![Azure Active Directory Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja próba felhasználó létrehozása az úgynevezett Britta Simon klasszikus portálon.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_09.png)

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_03.png)

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_04.png)

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_05.png)

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_06.png)

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót első** lapon kattintson a **Hozzon létre**.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_07.png)

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_08.png)

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-frankly-test-user"></a>Létrehozás egy & frankly teszt felhasználó

Ebben a szakaszban a nevű Britta Simon, frankly és a felhasználó létrehozása. Kérje & frankly támogatja a csapat keresztül [help@andfrankly.com](emailTo:help@andfrankly.com) a felhasználók hozzáadása, a & frankly platform.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon használandó Azure egyszeri bejelentkezés a saját hozzáférést & frankly engedélyezése.
    
![Felhasználó hozzárendelése][200]

**A & frankly Britta Simon hozzárendeléséhez tegye a következőket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.
    
    ![Felhasználó hozzárendelése][201]

2. Az alkalmazások listájában válassza a **és frankly**lehetőséget.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_50.png)

1. A felső sávon kattintson a **felhasználók**elemre.
    
    ![Felhasználó hozzárendelése][203]

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.
    
    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.
 
Kattintáskor & frankly csempe az Access panel, akkor érdemes automatikusan jelentkezett-on való a & frankly alkalmazást.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_205.png
