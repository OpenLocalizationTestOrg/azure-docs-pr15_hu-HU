<properties
    pageTitle="Oktatóprogram: Azure Active Directory integráció a Predictix szortiment tervezési |} Microsoft Azure"
    description="Egyszeri bejelentkezés Azure Active Directory és Predictix szortiment tervezése közötti konfigurálásának ismertetése."
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


# <a name="tutorial-azure-active-directory-integration-with-predictix-assortment-planning"></a>Oktatóprogram: Azure Active Directory integráció a Predictix szortiment tervezése

Ebből az oktatóanyagból megismerheti, hogyan Predictix szortiment tervezési integrálása az Azure Active Directory (Azure Active Directory).

Azure Active Directory Predictix szortiment tervezési integrálása biztosít a következő előnyökkel jár:

- Beállíthatja, hogy, aki rendelkezik hozzáféréssel a Predictix szortiment tervezési Azure AD
- Az Azure Active Directory-fiókok engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on Predictix szortiment tervezési (Single Sign-On)
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a Predictix szortiment tervezést van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy Predictix szortiment tervezési egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban, tesztelje a Microsoft Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben.

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Predictix szortiment tervezési hozzáadása a gyűjteményből
2. Microsoft Azure Active Directory egyszeri bejelentkezés a tesztelés és konfigurálása


## <a name="adding-predictix-assortment-planning-from-the-gallery"></a>Predictix szortiment tervezési hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a Predictix szortiment tervezési, szüksége hozzáadása Predictix szortiment tervezése a gyűjteményből a felügyelt szoftver alkalmazások listájában.

**Gyűjtemény használatával adhat hozzá Predictix szortiment tervezése a, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory][1]
2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Predictix szortiment megtervezése**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_01.png)

7. Az eredmény ablaktáblában jelölje ki a **Predictix szortiment tervezés**pontra, és kattintson a **kész** az alkalmazás hozzáadása parancsra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_02.png)


##  <a name="configuring-and-testing-microsoft-azure-ad-single-sign-on"></a>Microsoft Azure Active Directory egyszeri bejelentkezés a tesztelés és konfigurálása
Ebben a részben állítsa be, és tesztelje a Microsoft Azure Active Directory egyszeri bejelentkezés Predictix szortiment tervezéssel "Britta Simon" nevű vizsgálat felhasználón alapuló.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell tudható, hogy mi a megfelelőjük a felhasználó Predictix szortiment tervezés egy felhasználóhoz, az Azure Active Directory. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó Predictix szortiment tervezés hivatkozás viszonya kell létrehozni.

A hivatkozás kapcsolat jön létre a értéket a **felhasználó nevét** a **felhasználónév** Predictix szortiment tervezés értékként Azure AD hozzárendelésével.

Beállítása és tesztelése a Microsoft Azure Active Directory egyszeri bejelentkezés Predictix szortiment tervezéssel, az alábbi építőelemeket szükséges:

1. **[Microsoft Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - kipróbálni a Microsoft Azure Active Directory egyszeri bejelentkezéssel való Britta Simon.
3. **[A Predictix szortiment tervezési vizsgálat felhasználó létrehozásának](#creating-a-predictix-price-reporting-test-user)** - szeretné, hogy egy partner a Britta Simon Predictix szortiment megtervezésével kapcsolatban az Azure Active Directory ábrázolása amelyen csatolt.
4. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy a Microsoft Azure Active Directory egyszeri bejelentkezés használata Britta Simon.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-microsoft-azure-ad-single-sign-on"></a>Microsoft Azure Active Directory Single Sign-On konfigurálása

Ebben a részben Azure AD a Microsoft Single Sign-On a klasszikus portálon engedélyezése, és a Predictix szortiment tervezési alkalmazásban az egyszeri bejelentkezés beállítása.


**Predictix szortiment tervezéssel konfigurálása a Microsoft Azure Active Directory Single Sign-On, hajtsa végre az alábbi lépéseket:**

1. Az alkalmazás integrációs **Predictix szortiment tervezés** lapján a klasszikus portálon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.
     
    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az Predictix szortiment tervezési felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_04.png) 

    egy. **Bejelentkezési a URL-cím** mezőbe írja be a bejelentkezés az alkalmazásba Predictix szortiment megtervezése az alábbi minta használatával a felhasználók által használt URL-címe: **https://\<vállalat nevét árak\>.ap.predictix.com/sso/request**.
    
    b. Kattintson a **Tovább** gombra
 
4. A **konfigurálása az egyszeri bejelentkezés a Predictix szortiment tervezés** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_05.png)

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.


5. Ha be van állítva az alkalmazás SSO, Predictix szortiment tervezési támogatási csoport munkatársaitól kaphat, és küldje el nekik a következő:

    • A letöltött tanúsítvány

    • A **szervezet azonosítója**

    • A **SAML egyszeri bejelentkezési URL-címe**

    • **Egyetlen kijelentkezés szolgáltatás URL-címe**

6. A klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.
    
    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
 
    ![Azure Active Directory Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ebben a szakaszban a klasszikus portálon Britta Simon nevű próba felhasználó létrehozása.


![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_04.png) 

5. A **mondja el a felhasználóra vonatkozó** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:  ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-an-predictix-assortment-planning-test-user"></a>Egy Predictix szortiment tervezési próba-felhasználó létrehozása

Ebben a részben hozzon létre egy felhasználót Britta Simon nevű Predictix szortiment megtervezésével. Kérje meg a felhasználók felvétele a Predictix szortiment tervezési platform Predictix szortiment tervezési támogatási csoporthoz.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ebben a részben Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít Predictix szortiment tervezési használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Predictix szortiment tervezési, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Alkalmazások listájában jelölje ki a **Predictix szortiment megtervezése**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_50.png) 

3. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203]

4. A felhasználók listában jelölje ki a **Britta Simon**.

5. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]


### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a részben tesztelje a Microsoft Azure Active Directory egyszeri bejelentkezés a konfigurációban a Access panelen.

Amikor az Access panel Predictix szortiment tervezési mozaik gombra kattint, meg kell első automatikusan aláírt-on a Predictix szortiment tervezési alkalmazásba.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_205.png
