<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Heroku |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és Heroku között."
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
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-heroku"></a>Oktatóprogram: Azure Active Directory-integráció a Heroku

Ebből az oktatóanyagból megismerheti, hogyan Heroku integrálása az Azure Active Directory (Azure Active Directory).

Heroku integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel Heroku megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való Heroku (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a Heroku van szükség az alábbiakat:

- Az Azure előfizetéssel
- Egy Heroku egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni Azure AD az egyszeri bejelentkezés tesztkörnyezetben.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Heroku hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-heroku-from-the-gallery"></a>Heroku hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a Heroku, meg kell Heroku hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből Heroku felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Heroku**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_01.png)

7. Az eredmény ablaktáblában jelölje ki a **Heroku**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ebben a részben konfigurálása és a "Britta Simon" nevű próba felhasználón alapuló Heroku az Azure Active Directory egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell tudható, hogy mi a Heroku megfelelőjük a felhasználó egy felhasználóhoz, az Azure Active Directory. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó Heroku hivatkozás viszonya kell létrehozni.  
A hivatkozás kapcsolat jön létre által az Azure Active Directory Heroku **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való Heroku tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó létrehozása egy Heroku tesztelése](#creating-an-heroku-test-user)** - van-e egy megfelelője a Britta Simon Heroku, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ebben a részben Azure AD az egyszeri bejelentkezés a klasszikus portálon engedélyezése, és a Heroku alkalmazásban az egyszeri bejelentkezés beállítása.


**Állítson be Azure AD az egyszeri bejelentkezés Heroku, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portálon **Heroku** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az Heroku felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.
 
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:
 
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_04.png) 

    > [AZURE.NOTE] Ha nem tudja, hogy Mik azok a bejelentkezési URL és azonosító URL-cím helyes értékek, olvassa el a "[Heroku, az egyszeri bejelentkezés engedélyezéséhez tegye a következőket](#x123)" útmutatást el őket.   


    egy. **Bejelentkezési a URL-cím** mezőbe írja be a bejelentkezés az Heroku alkalmazás a következő mintát használatával a felhasználók által használt URL-címe: **"https://sso.heroku.com/saml/\<cégnév\>/init"**. 

    b. Az **azonosító** mezőbe írja be a következő mintát URL-címe: "**https://sso.heroku.com/saml/\<vállalatnév\>**".  

    c billentyűkombinációt. Kattintson a **Tovább**gombra.


4. A **beállítás az egyszeri bejelentkezés Heroku a** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_05.png) 

    egy. Kattintson a **metaadat-alapú letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.


5. Ahhoz, hogy a Heroku SSO, hajtsa végre az alábbi lépéseket:
 
    egy. Jelentkezzen be rendszergazdaként a Heroku fiókkal.

    b. Kattintson a **Beállítások** fülre.

    c billentyűkombinációt. **Egyszeri bejelentkezési oldalon**kattintson a **Metaadat feltöltése**gombra.
 
    d. Töltse fel a Azure klasszikus portáljáról letöltött metaadat-fájlt.

    e. Amikor a telepítő sikerrel jár, rendszergazdák megjelenik egy megerősítést kérő párbeszédpanelen, és megjelenik a végfelhasználók számára az Egyszeri bejelentkezés URL-CÍMÉT.

    f. <a name="x123"></a>Másolja a **Heroku bejelentkezési URL-cím** és a **Heroku entitás azonosítója**, és ezt követően az Azure Active Directory klasszikus portálon visszatérhet az **Alkalmazás-beállítások megadása** lap, és illessze be az értékeket a kapcsolódó szövegdobozok.

  
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_52.png) 

    g. Kattintson a **Tovább**gombra.
  
6. Jelölje ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  

    ![Azure Active Directory Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ebben a szakaszban a klasszikus portálon Britta Simon nevű próba felhasználó létrehozása.  

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-heroku-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-heroku-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-heroku-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-heroku-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-heroku-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-heroku-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-heroku-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-an-heroku-test-user"></a>Egy Heroku teszt felhasználó létrehozása

Ebben a részben hozzon létre egy felhasználót Britta Simon nevű Heroku. Heroku támogatja a kiépítési just time, amely alapértelmezés szerint engedélyezve van.

Nincs teendő ebben a szakaszban a meg nem. Egy új felhasználót hoznak létre, Heroku elérésekor, ha a felhasználó még nem létezik. Után a fiók már kiépítve a végfelhasználói egy megerősítő e-mailt kap, és kattintson a visszaigazoló hivatkozásra kell.

> [AZURE.NOTE] Ha egy felhasználó létrehozása manuálisan kell szüksége a Heroku támogatási csoport munkatársaitól kaphat.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ebben a részben Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít Heroku használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Heroku, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Heroku**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_50.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a részben tesztelje az Azure Active Directory egyetlen bejelentkezés beállítási a Access panelen.  
Ha az Access panel a Heroku mozaik gombra kattint, meg kell első automatikusan aláírt-on az Heroku alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_205.png
