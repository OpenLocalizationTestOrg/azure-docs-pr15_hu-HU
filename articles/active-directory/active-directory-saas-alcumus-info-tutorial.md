<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Alcumus információ Exchange |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és Alcumus információ Exchange között."
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-alcumus-info-exchange"></a>Oktatóprogram: Azure Active Directory-integráció a Alcumus információ Exchange

Ebben az oktatóanyagban célja mutatja, hogy miként Alcumus információ Exchange integrálása az Azure Active Directory (Azure Active Directory).  
Információ az Exchange Alcumus integrálása az Azure Active Directory biztosít a következő előnyökkel jár: 

- Az Azure Active Directory Alcumus információ Exchange hozzáféréssel rendelkező személyek megadhatja, hogy 
- Az Azure Active Directory-fiókok engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on Alcumus információ Exchange (Single Sign-On)
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek 

Azure Active Directory integráció a Alcumus információ Exchange van szükség az alábbiakat:

- Az [Azure Active Directory](https://azure.microsoft.com/) -előfizetéssel
- Egy [Alcumus információ Exchange](http://www.alcumusgroup.com/) egyszeri bejelentkezés engedélyezett előfizetésekből


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt három fő építőelemek áll:

1. Információ az Exchange Alcumus hozzáadása a gyűjteményből 
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-alcumus-info-exchange-from-the-gallery"></a>Információ az Exchange Alcumus hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a Alcumus információ Exchange, szüksége Alcumus információ Exchange hozzáadása felügyelt szoftver alkalmazásai a gyűjteményből.

**A gyűjteményből Alcumus információ Exchange felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Alcumus információ Exchange**.

    ![Alkalmazások][5]

7. Az eredmény ablaktáblában jelölje ki a **Alcumus információ Exchange**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Alkalmazások][400]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy állítsa be, és a "Britta Simon" nevű vizsgálat felhasználón alapuló Alcumus információ Exchange Azure AD az egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell tudható, hogy mi az Azure Active Directory-felhasználónak Alcumus információ Exchange megfelelőjük felhasználói. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó Alcumus információ Exchange hivatkozás viszonya kell létrehozni.  
Ez a hivatkozás kapcsolat által a **felhasználónév** értéket rendeli az Azure Active Directory, a program a **felhasználónév** Alcumus információ az Exchange jön létre.
 
Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezéssel való Alcumus információ Exchange, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó létrehozása egy Alcumus információ Exchange tesztelése](#creating-a-alcumus-info-exchange-test-user)** - van-e egy megfelelője a Britta Simon Alcumus információ Exchange, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a Alcumus információ Exchange alkalmazásban az egyszeri bejelentkezés beállítása.

**Állítson be Azure AD az egyszeri bejelentkezés Alcumus információ Exchange, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon **Alcumus információ Exchange** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6]

2. **Hogyan szeretné, hogy jelentkezzen be az Alcumus információ Exchange felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][7]

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: 

    ![Azure Active Directory Single Sign-On][8]
 
    egy. **Válasz URL-cím** mezőbe írja be a fogyasztói URL-címet, amely a Alcumus információ Exchange-támogatási csoport meg beállítása.

    > [AZURE.NOTE] Ha nem tudja, hogy mi az a megfelelő értéket, a Alcumus információ Exchange támogatási csoport munkatársaitól kaphat keresztül [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com).

    b. Kattintson a **Tovább**gombra.
 
4. **Konfigurálása az egyszeri bejelentkezés Alcumus információ Exchange a** lapon kattintson a **metaadat-alapú letöltése**elemre, és mentse a fájlt a számítógépen helyben metaadat.

    ![Mi az Azure AD Connect][9]

5. A Alcumus információ Exchange támogatási csoport munkatársaitól kaphat keresztül [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com), küldje el nekik a metaadat-fájlt, és őket arról, hogy vele, hogy azokat meg engedélyezze egyszeri Bejelentkezést.


6. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**. 

    ![Mi az Azure AD Connect][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  

    ![Mi az Azure AD Connect][11]




### <a name="creating-an-azure-ad-test-user"></a>Az Azure Active Directory vizsgálat felhasználó létrehozása
Ez a szakasz célja vizsgálat felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.  

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Azure Active Directory vizsgálat felhasználó létrehozása](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_02.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_03.png) 
 
4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához. 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket: 

    ![Az Azure Active Directory vizsgálat felhasználó létrehozása](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.
  
    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.
  
    c billentyűkombinációt. Kattintson a Tovább gombra.



6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: 

    ![Az Azure Active Directory vizsgálat felhasználó létrehozása](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_06.png) 
  

    egy. Az **első neve** mezőbe írja be a **Britta**.  
  
    b. Kattintson a **Vezetéknév** txtbox, típusa, **Simon**.
  
    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.
  
    d. A **szerepkör** listában jelölje ki a **felhasználót**.
  
    e. Kattintson a **Tovább**gombra.


7. A párbeszédpanel **ideiglenes jelszót első** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_07.png) 
 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Az Azure Active Directory vizsgálat felhasználó létrehozása](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.
  
    b. Kattintson a **kész**gombra.   

  
 
### <a name="creating-a-alcumus-info-exchange-test-user"></a>Alcumus információ Exchange vizsgálat felhasználó létrehozása

Ez a szakasz célja Britta Simon nevű Alcumus információ Exchange felhasználó létrehozása.

**A felhasználó Britta Simon nevű Alcumus információ Exchange létrehozásához tegye a következőket:**

1. A Alcumus információ Exchange támogatási csoport munkatársaitól kaphat keresztül [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com),


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít Alcumus információ Exchange használandó engedélyezése.

![Felhasználó hozzárendelése][200]

**Britta Simon hozzárendelése Alcumus információ Exchange, hajtsa végre az alábbi lépéseket:**

1. Az Azure portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201]

2. Az alkalmazások listájában jelölje ki a **Alcumus információ Exchange**.

    ![Felhasználó hozzárendelése][202]

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203]

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.  
Amikor az Access panel a Alcumus információ Exchange csempe gombra kattint, meg kell első automatikusan aláírt-on az Alcumus információ Exchange alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_01.png
[6]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_02.png
[7]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_03.png
[8]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_04.png
[9]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_05.png
[10]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_06.png
[11]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_07.png
[20]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_08.png
[203]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_205.png
[400]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_402.png