<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Dél-Alföld nevű munkaidő-nyilvántartás |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és a Dél-Alföld nevű munkaidő-nyilvántartás között."
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
    ms.date="10/06/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-pacific-timesheet"></a>Oktatóprogram: Azure Active Directory-integráció a Dél-Alföld nevű munkaidő-nyilvántartásba

Ebből az oktatóanyagból megismerheti, hogyan csendes-óceáni munkaidő-nyilvántartás integrálása az Azure Active Directory (Azure Active Directory).

A munkaidő-nyilvántartás csendes-óceáni integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel a munkaidő-nyilvántartás csendes-óceáni megadhatja, hogy
- Az Azure Active Directory-fiókok engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on (Single Sign-On) csendes-óceáni munkaidő-nyilvántartásba
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a Dél-Alföld nevű munkaidő-nyilvántartás van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- A **Dél-Alföld nevű munkaidő-nyilvántartás** egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni Azure AD az egyszeri bejelentkezés tesztkörnyezetben. Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. A munkaidő-nyilvántartás csendes-óceáni hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-pacific-timesheet-from-the-gallery"></a>A munkaidő-nyilvántartás csendes-óceáni hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a Dél-Alföld nevű munkaidő-nyilvántartás, szüksége csendes-óceáni munkaidő-nyilvántartás a gyűjteményből hozzáadása a felügyelt szoftver alkalmazások listájában.

**A gyűjteményből csendes-óceáni munkaidő-nyilvántartás felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be **a munkaidő-nyilvántartás csendes-óceáni**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacific_timesheet_01.png)

7. Az eredmény ablaktáblában jelölje ki a **Dél-Alföld nevű munkaidő-nyilvántartás**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacific_timesheet_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ebben a részben konfigurálása, és a Dél-Alföld nevű a munkaidő-nyilvántartás "Britta Simon" nevű próba felhasználón alapuló Azure AD az egyszeri bejelentkezés tesztelje.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell tudható, hogy mi a megfelelőjük felhasználó csendes-óceáni munkaidő-nyilvántartás egy felhasználóhoz, az Azure Active Directory. Más szóval viszonya Azure AD-felhasználó, és a kapcsolódó felhasználó csendes-óceáni munkaidő-nyilvántartás hivatkozásra kell létrehozni.
A hivatkozás kapcsolat jön létre a értéket a **felhasználó nevét** a **felhasználónév** csendes-óceáni munkaidő-nyilvántartás értékként Azure AD hozzárendelésével. Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való csendes-óceáni munkaidő-nyilvántartás tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Csendes-óceáni munkaidő-nyilvántartás létrehozása tesztelje a felhasználó](#creating-a-pacific-timesheet-test-user)** -, hogy egy partner a Britta Simon csendes-óceáni munkaidő-nyilvántartás, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory egyszeri bejelentkezés beállítása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a Dél-Alföld nevű munkaidő-nyilvántartás alkalmazásban az egyszeri bejelentkezés beállítása.

**Állítson be Azure AD az egyszeri bejelentkezés csendes-óceáni munkaidő-nyilvántartás, hajtsa végre az alábbi lépéseket:**

1. A felső sávon kattintson a **Quick Start**.

    ![Egyszeri bejelentkezés beállítása][6]

4. A klasszikus portálon, **A munkaidő-nyilvántartás csendes-óceáni** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][7] 

5. **Hogyan szeretné, hogy jelentkezzen be az csendes-óceáni munkaidő-nyilvántartás felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacific_timesheet_06.png)

6. **Alkalmazás beállításainak megadása** párbeszédpanel lapon az alkalmazás beállítása **IDP kezdeményezett mód**, hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacific_timesheet_07.png)


    egy. Az azonosító mezőbe írja be az URL-cím használata a következő mintát: `https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`.

    b. Válasz URL-cím mezőbe írja be az URL-cím használata a következő mintát: `https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`.

    b. Kattintson a **Tovább**gombra.

7. A **beállítás az egyszeri bejelentkezés csendes-óceáni munkaidő-nyilvántartás a** lapon. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacific_timesheet_09.png)

6. Ha be van állítva az alkalmazás SSO, csendes-óceáni munkaidő-nyilvántartás támogatási csoport munkatársaitól kaphat. Felhívjuk a figyelmét arra, hogy kell küldhet e-mailt a kibocsátó URL-címmel, a **Configure egyszeri bejelentkezés a Dél-Alföld nevű munkaidő-nyilvántartás** lap SAML egyszeri bejelentkezési URL-cím értékeit, és csatolja a letöltött tanúsítvány.

7. A klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.
    
    ![Azure Active Directory Single Sign-On][10]

8. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
    
    ![Azure Active Directory Single Sign-On][11]

### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ebben a szakaszban a klasszikus portálon Britta Simon nevű próba felhasználó létrehozása.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:
 
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Az Azure Active Directory vizsgálat felhasználó létrehozása](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót első** lapon kattintson a **Hozzon létre**.

    ![Az Azure Active Directory vizsgálat felhasználó létrehozása](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Az Azure Active Directory vizsgálat felhasználó létrehozása](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-pacific-timesheet-test-user"></a>A munkaidő-nyilvántartás csendes-óceáni próba felhasználó létrehozása

Ebben a részben egy úgynevezett Britta Simon csendes-óceáni munkaidő-nyilvántartás felhasználót hoz létre. Kérje meg a munkaidő-nyilvántartás csendes-óceáni támogatási csoporthoz hozhat létre egy felhasználót az alkalmazás.

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ebben a részben Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít a munkaidő-nyilvántartás csendes-óceáni használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Csendes-óceáni munkaidő-nyilvántartás Britta Simon rendelni, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Alkalmazások listájában jelölje ki **a munkaidő-nyilvántartás csendes-óceáni**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacific_timesheet_10.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. Az összes felhasználó listában válassza a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]


### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.

A Dél-Alföld nevű munkaidő-nyilvántartás csempére az Access panel gombra kattintva meg kell első automatikusan aláírt-on a Dél-Alföld nevű munkaidő-nyilvántartás alkalmazásba.

## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_205.png
