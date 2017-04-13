<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Mixpanel |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és Mixpanel között."
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


# <a name="tutorial-azure-active-directory-integration-with-mixpanel"></a>Oktatóprogram: Azure Active Directory-integráció a Mixpanel

Ebben az oktatóanyagban célja mutatja, hogy miként Mixpanel integrálása az Azure Active Directory (Azure Active Directory).

Mixpanel integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel Mixpanel megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való Mixpanel (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra


Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a Mixpanel van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy Mixpanel egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése. 

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Mixpanel hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-mixpanel-from-the-gallery"></a>Mixpanel hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a Mixpanel, meg kell Mixpanel hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből Mixpanel felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Mixpanel**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_01.png)

7. Az eredmény ablaktáblában jelölje ki a **Mixpanel**, és válassza a **kész** , az alkalmazás hozzáadása gombra.


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy állítsa be, és a "Britta Simon" nevű próba felhasználón alapuló Mixpanel Azure AD az egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a partner felhasználó Mixpanel az Azure Active Directory-felhasználónak. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó Mixpanel hivatkozás viszonya kell létrehozni.

A hivatkozás kapcsolat jön létre által az Azure Active Directory Mixpanel **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való Mixpanel tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó létrehozása egy Mixpanel tesztelése](#creating-a-mixpanel-test-user)** - van-e egy megfelelője a Britta Simon Mixpanel, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a Mixpanel alkalmazásban az egyszeri bejelentkezés beállítása.



**Állítson be Azure AD az egyszeri bejelentkezés Mixpanel, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon **Mixpanel** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az Mixpanel felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_04.png) 


    egy. **Bejelentkezési a URL-cím** mezőbe írja be a bejelentkezés az Mixpanel alkalmazás a következő mintát használatával a felhasználók által használt URL-címe: **"https://mixpanel.com/login/"**.

    > [AZURE.NOTE] Regisztrálja a [https://mixpanel.com/register/](https://mixpanel.com/register/) állíthatja be a bejelentkezési adatok és a kapcsolattartó ahhoz, hogy egyszeri bejelentkezési beállításait a [Mixpanel támogatási csoport](mailto:support@Mixpanel.com) bérlői. Is megnyithatja a bejelentkezési a URL-címe lesz szükség esetén a Mixpanel támogatási szolgálatához.

    b. Kattintson a **Tovább gombra**,



4. A **beállítás az egyszeri bejelentkezés Mixpanel a** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_05.png) 

    egy. A **Tanúsítvány letöltése**gombra, és mentse a fájlt a számítógépen. 

    b. Kattintson a **Tovább**gombra.


5. Egy másik böngészőablakban bejelentkezéses az Mixpanel alkalmazás rendszergazdaként.
   
6. Kattintson a lap alján a kisméretű **, fogaskereket** ikonra a bal alsó sarokban. 

    ![Mixpanel Single Sign-On](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_06.png) 

7. Kattintson az **Access biztonsági** fülre, és válassza a **Beállítások módosítása hivatkozásra**.

    ![Mixpanel beállításai](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_08.png) 
    
8. **Módosítása a tanúsítvány** párbeszédpanel lapon kattintson a **fájl kiválasztása** a letöltött tanúsítvány tölthet fel, és kattintson a **Tovább gombra**.

    ![Mixpanel beállításai](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_09.png) 

9. Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Mixpanel** párbeszédpanel lapon másolja az **Egyszeri bejelentkezéses szolgáltatás URL-címe** értéket, illessze be a hitelesítés URL-címe mezőben lévő értéket **a hitelesítés URL-cím módosítása** párbeszédpanel lapon, és kattintson a **Tovább gombra**.
 
    ![Mixpanel beállításai](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_10.png) 
    
1. Kattintson a **kész**gombra.


6. Az Azure klasszikus portálon válassza ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
 
    ![Azure Active Directory Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja próba felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.
A felhasználók listában jelölje ki a **Britta Simon**.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót első** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-mixpanel-test-user"></a>Mixpanel vizsgálat felhasználó létrehozása

Ez a szakasz célja Britta Simon nevű Mixpanel felhasználó létrehozása. 

1.  Bejelentkezés a Mixpanel vállalati webhelyre rendszergazdaként.

2.  Kattintson a lap aljára, nyissa meg a **Beállítások** ablak bal oldali sarokban található kis fogaskerék gombjára.

3.  Kattintson a **csoportwebhely** fülre.

4. Az **csapatban dolgozó munkatárssal** mezőbe írja be a Britta meg e-mail címét az Azure-ban.
   
    ![Mixpanel beállításai](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_11.png) 

4.  Kattintson a **Meghívás**gombra. 

A felhasználó saját profil beállítása e-mailben kap.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít Mixpanel használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Mixpanel, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Mixpanel**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_50.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.

Amikor az Access panel a Mixpanel mozaik gombra kattint, meg kell első automatikusan aláírt-on az Mixpanel alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_205.png
