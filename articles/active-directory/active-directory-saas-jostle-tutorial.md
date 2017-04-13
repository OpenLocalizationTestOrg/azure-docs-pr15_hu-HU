<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Jostle |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és Jostle között."
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
    ms.date="10/14/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-jostle"></a>Oktatóprogram: Azure Active Directory-integráció a Jostle

Ebből az oktatóanyagból megismerheti, hogyan Jostle integrálása az Azure Active Directory (Azure Active Directory).

Jostle integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel Jostle megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való Jostle (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a Jostle van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy **Jostle** egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni Azure AD az egyszeri bejelentkezés tesztkörnyezetben. Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Jostle hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-jostle-from-the-gallery"></a>Jostle hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a Jostle, meg kell Jostle hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből Jostle felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Jostle**.

    ![Az Azure Active Directory vizsgálat felhasználó létrehozása](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_01.png)

7. Az eredmény ablaktáblában jelölje ki a **Jostle**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Az Azure Active Directory vizsgálat felhasználó létrehozása](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ebben a részben konfigurálása és a "Britta Simon" nevű próba felhasználón alapuló Jostle az Azure Active Directory egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell tudható, hogy mi a Jostle megfelelőjük a felhasználó egy felhasználóhoz, az Azure Active Directory. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó Jostle hivatkozás viszonya kell létrehozni.
Ez a hivatkozás kapcsolat jön létre által az Azure Active Directory Jostle **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való Jostle tesztelése, az alábbi építőelemeket szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó létrehozása egy Jostle tesztelése](#creating-a-jostle-test-user)** - van-e egy megfelelője a Britta Simon Jostle, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a Jostle alkalmazásban az egyszeri bejelentkezés beállítása.


**Állítson be Azure AD az egyszeri bejelentkezés Jostle, hajtsa végre az alábbi lépéseket:**

1. A felső sávon kattintson a **Quick Start**.

    ![Egyszeri bejelentkezés beállítása][6]

2. Az alkalmazás integrációs **Jostle** lapján klasszikus-portálon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][7] 

3. **Hogyan megtekinti Jostle jelentkezzen be felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_06.png)

4. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: 

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_07.png)


    egy. Az bejelentkezési a URL-cím mezőbe írja be az URL-cím használata a következő mintát:`https://<subdomain>.jostle.us/jostle-prod/`

    b. Kattintson a **Tovább**gombra.

5. **Konfigurálása az egyszeri bejelentkezés a Jostle** lapján kattintson a **metaadat-alapú letöltése**, majd mentse a fájlt a számítógépen.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_08.png)

6. Ha be van állítva az alkalmazás SSO, forduljon a fiókkezelő Jostle vagy Jostle támogatás. Azok segítséget a megfelelő csatorna egyszeri bejelentkezés beállítása. Kérjük, vegye figyelembe, hogy meg kell küldhet e-mailt, és csatolja letölti-e metaadat-fájlt szeretne <support@jostle.me>.

7. A klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.
    
    ![Azure Active Directory Single Sign-On][10]

8. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
    
    ![Azure Active Directory Single Sign-On][11]

### <a name="creating-an-azure-ad-test-user"></a>Az Azure Active Directory vizsgálat felhasználó létrehozása
Ebben a szakaszban a klasszikus portálon Britta Simon nevű próba felhasználó létrehozása.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.
    
    ![Az Azure Active Directory vizsgálat felhasználó létrehozása](./media/active-directory-saas-jostle-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.
    
    ![Az Azure Active Directory vizsgálat felhasználó létrehozása](./media/active-directory-saas-jostle-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Az Azure Active Directory vizsgálat felhasználó létrehozása](./media/active-directory-saas-jostle-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:
 
    ![Az Azure Active Directory vizsgálat felhasználó létrehozása](./media/active-directory-saas-jostle-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Az Azure Active Directory vizsgálat felhasználó létrehozása](./media/active-directory-saas-jostle-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót első** lapon kattintson a **Hozzon létre**.

    ![Az Azure Active Directory vizsgálat felhasználó létrehozása](./media/active-directory-saas-jostle-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Az Azure Active Directory vizsgálat felhasználó létrehozása](./media/active-directory-saas-jostle-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-jostle-test-user"></a>Jostle vizsgálat felhasználó létrehozása

Ebben a részben hozzon létre egy felhasználót Britta Simon nevű Jostle. Ha nem tudja, hogy miként vehet fel Britta Simon Jostle, kérje a próba-felhasználó hozzáadása és az egyszeri bejelentkezés engedélyezése Jostle részlegtől. Kapcsolatba léphet az illetővel, <support@jostle.me>.

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ebben a részben Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít Jostle használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Jostle, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Jostle**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_50.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. Az összes felhasználó listában válassza a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]


### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.

Amikor az Access panel a Jostle mozaik gombra kattint, meg kell első automatikusan aláírt-on az Jostle alkalmazás.

## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-jostle-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_205.png
