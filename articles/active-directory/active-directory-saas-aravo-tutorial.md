<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Aravo |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és Aravo között."
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


# <a name="tutorial-azure-active-directory-integration-with-aravo"></a>Oktatóprogram: Azure Active Directory-integráció a Aravo

Ebben az oktatóanyagban célja mutatja, hogy miként Aravo integrálása az Azure Active Directory (Azure Active Directory).

Aravo integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel Aravo megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan kérjen jelentkezett-on való Aravo (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a Aravo van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy Aravo egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi a Microsoft Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Aravo hozzáadása a gyűjteményből
2. Microsoft Azure Active Directory egyszeri bejelentkezés a tesztelés és konfigurálása


## <a name="adding-aravo-from-the-gallery"></a>Aravo hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a Aravo, meg kell Aravo hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből Aravo felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .
    
    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Aravo**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-aravo-tutorial/tutorial_aravo_01.png)
7. Az eredmény ablaktáblában jelölje ki a **Aravo**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Az alkalmazás kijelöli a gyűjteményben](./media/active-directory-saas-aravo-tutorial/tutorial_aravo_0001.png)


##  <a name="configuring-and-testing-microsoft-azure-ad-single-sign-on"></a>Beállítása és tesztelése a Microsoft Azure Active Directory Single Sign-On
Ez a szakasz célja mutatja, hogy beállítása és tesztelése a Microsoft Azure Active Directory egyszeri bejelentkezés Aravo az úgynevezett "Britta Simon" vizsgálat felhasználón alapuló.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a partner felhasználó Aravo az Azure Active Directory-felhasználónak. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó Aravo hivatkozás viszonya kell létrehozni.

A hivatkozás kapcsolat jön létre által az Azure Active Directory Aravo **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Beállítása és tesztelése a Microsoft Azure Active Directory egyszeri bejelentkezéssel való Aravo, az alábbi építőelemeket szükséges:

1. **[Microsoft Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - kipróbálni a Microsoft Azure Active Directory egyszeri bejelentkezéssel való Britta Simon.
3. **[Felhasználó létrehozása egy Aravo tesztelése](#creating-a-aravo-test-user)** - van-e egy megfelelője a Britta Simon Aravo, amelyen az Azure Active Directory ábrázolt csatolt.
4. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy a Microsoft Azure Active Directory egyszeri bejelentkezés használata Britta Simon.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-microsoft-azure-ad-single-sign-on"></a>Microsoft Azure Active Directory Single Sign-On konfigurálása

Ebben a részben a Microsoft Azure Active Directory Single Sign-On a klasszikus portálon engedélyezése, és a Aravo alkalmazásban az egyszeri bejelentkezés beállítása.

**Állítson be a Microsoft Azure Active Directory egyszeri bejelentkezés Aravo, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portálon **Aravo** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.
     
    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az Aravo felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-aravo-tutorial/tutorial_aravo_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább**gombra:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-aravo-tutorial/tutorial_aravo_04.png)

    egy. Az **azonosító** mezőbe írja be az URL-cím használata a következő mintát:`https://<company name>.aravo.com`

    b. Az **Válasz URL-cím** mezőbe írja be az URL-cím használata a következő mintát:`https://<company name>.aravo.com/aems/login.do`

    c billentyűkombinációt. Kattintson a **Tovább** gombra

    > [AZURE.NOTE] Felhívjuk a figyelmét arra, hogy ezek nem a tényleges érték. Az értékek frissítése a tényleges azonosító és a válasz URL-címe van. Ha ezeket az értékeket, forduljon a Aravo.

4. A **beállítás az egyszeri bejelentkezés Aravo a** lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább**gombra:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-aravo-tutorial/tutorial_aravo_05.png)

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.

5. Ha be van állítva az alkalmazás SSO, a Aravo támogatási csoport munkatársaitól, és küldje el nekik a következő: 

    -A **letöltött tanúsítvány** -fájl

    -A **kibocsátó URL-címe**

    -A **SAML egyszeri bejelentkezési URL-címe**

    – Az **egyetlen kijelentkezési szolgáltatás URL-címe**

6. A klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  

    ![Azure Active Directory Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja próba felhasználó létrehozása az úgynevezett Britta Simon klasszikus portálon.
    
![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-aravo-tutorial/create_aaduser_09.png)

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-aravo-tutorial/create_aaduser_03.png)

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-aravo-tutorial/create_aaduser_04.png)

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-aravo-tutorial/create_aaduser_05.png)

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-aravo-tutorial/create_aaduser_06.png)

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-aravo-tutorial/create_aaduser_07.png)

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-aravo-tutorial/create_aaduser_08.png)

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-aravo-test-user"></a>Aravo próba felhasználó létrehozása

Ebben a szakaszban az a célja, hogy a felhasználók felvétele az Aravo fiók Britta Simon nevű Aravo.Please munka Aravo támogatási csoporthoz felhasználó létrehozása.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít Aravo használandó engedélyezése.
    
![Felhasználó hozzárendelése][200]

**Britta Simon hozzárendelése Aravo, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201]

2. Az alkalmazások listájában jelölje ki a **Aravo**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-aravo-tutorial/tutorial_aravo_50.png)

3. A felső sávon kattintson a **felhasználók**elemre.
    
    ![Felhasználó hozzárendelése][203]

4. A felhasználók listában jelölje ki a **Britta Simon**.

5. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Microsoft Azure Active Directory egyszeri bejelentkezés beállítások tesztelése a Access panelen.

Amikor az Access panel a Aravo mozaik gombra kattint, meg kell első automatikusan aláírt-on az Aravo alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_205.png
