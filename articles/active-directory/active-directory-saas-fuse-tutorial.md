<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a biztosító |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és biztosító között."
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-fuse"></a>Oktatóprogram: Azure Active Directory-integráció a biztosító

Ebben az oktatóanyagban célja mutatja, hogy miként biztosító integrálása az Azure Active Directory (Azure Active Directory).

Biztosító integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel biztosító megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való biztosító (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a biztosító van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy biztosító egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése. 

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Biztosító hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-fuse-from-the-gallery"></a>Biztosító hozzáadása a gyűjteményből
Integráció a mindig az Azure Active Directory, meg kell biztosító hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből biztosító felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **biztosítéka**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_01.png)

7. Az eredmény ablaktáblában jelölje ki a **biztosítéka**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy állítsa be, és a "Britta Simon" nevű próba felhasználón alapuló biztosító Azure AD az egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a partner felhasználó biztosító az Azure Active Directory-felhasználónak. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó biztosító hivatkozás viszonya kell létrehozni.

A hivatkozás kapcsolat jön létre által az Azure Active Directory biztosító **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Állítsa be és Azure Active Directory egyszeri bejelentkezést biztosító a teszteléséhez az alábbi építőelemeket szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó létrehozása egy biztosító tesztelése](#creating-a-fuse-test-user)** - van-e egy megfelelője a Britta Simon biztosító, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és beállítása egyszeri bejelentkezést biztosító alkalmazás.



**Állítson be Azure Active Directory egyszeri bejelentkezést biztosító, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon alkalmazás integrációs **biztosítéka** lapján kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan megtekinti biztosító jelentkezzen be felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_04.png) 

    egy. Bejelentkezési a URL-cím mezőbe írja be a bejelentkezés az biztosító alkalmazás a következő mintát használatával a felhasználók által használt URL-címe: **"https://\<bérlői neve\>.fusion-universal.com/"**.

    > [AZURE.NOTE]Kérjük, forduljon a [biztosító támogatási csoport](mailto:support@fusion-universal.com) megszerezni a bejelentkezési a URL-CÍMÉT, ha nem tudja, hogy.
    
    b. Kattintson a **Tovább**gombra.


4. A **Configure egyszeri bejelentkezést biztosító a** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_05.png) 

    egy. A **Tanúsítvány letöltése**gombra, és mentse a számítógépre.

    b. Másolja a **kibocsátó URL-cím**, az **egyszeri bejelentkezéses szolgáltatás URL-cím** és az **egyetlen kijelentkezési szolgáltatás URL-címe**.

   
5. Ha be van állítva az alkalmazás SSO, a biztosító támogatási csoport munkatársaitól keresztül **support@fusion-universal.com**, a letöltött tanúsítvány-fájl csatolása és a **Kibocsátó URL-cím**, az **Egyszeri bejelentkezéses szolgáltatás URL-CÍMÉT** és az **Egyetlen Sign-Out szolgáltatás URL-CÍMÉT**.


6. Az Azure klasszikus portálon válassza ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
 
    ![Azure Active Directory Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja vizsgálat felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.

A felhasználók listában jelölje ki a **Britta Simon**.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Azure Active Directory vizsgálat felhasználó létrehozása](./media/active-directory-saas-fuse-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Az Azure Active Directory vizsgálat felhasználó létrehozása](./media/active-directory-saas-fuse-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-fuse-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-fuse-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-fuse-tutorial/create_aaduser_06.png)

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót első** lapon kattintson a **Hozzon létre**.

    ![Az Azure Active Directory vizsgálat felhasználó létrehozása](./media/active-directory-saas-fuse-tutorial/create_aaduser_07.png)

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Az Azure Active Directory vizsgálat felhasználó létrehozása](./media/active-directory-saas-fuse-tutorial/create_aaduser_08.png)

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-fuse-test-user"></a>Biztosító próba felhasználó létrehozása

Ez a szakasz célja Britta Simon nevű biztosító felhasználó létrehozása. Biztosító támogatja a kiépítési just time, amely alapértelmezés szerint ki van engedélyezve van.

Nincs teendő ebben a szakaszban a meg nem. Új felhasználóként biztosító eléréséhez, ha még nem létezik kísérlet során jön létre. Az [Azure Active Directory Single Sign-On konfigurálása](#configuring-azure-ad-single-single-sign-on).


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosító használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése biztosító, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **biztosítéka**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_50.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.

Ha a biztosító csempére az Access panel gombra kattint, meg kell első automatikusan aláírt-on az biztosító alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_205.png
