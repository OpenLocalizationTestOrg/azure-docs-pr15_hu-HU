<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Asana |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és Asana között."
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
    ms.date="10/24/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-asana"></a>Oktatóprogram: Azure Active Directory-integráció a Asana

Ebből az oktatóanyagból megismerheti, hogyan Asana integrálása az Azure Active Directory (Azure Active Directory).

Asana integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel Asana megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való Asana (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a Asana van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy **Asana** egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni Azure AD az egyszeri bejelentkezés tesztkörnyezetben. Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Asana hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-asana-from-the-gallery"></a>Asana hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a Asana, meg kell Asana hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből Asana felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Asana**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-asana-tutorial/tutorial_asana_01.png)

7. Az eredmény ablaktáblában jelölje ki a **Asana**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-asana-tutorial/tutorial_asana_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ebben a részben konfigurálása és a "Britta Simon" nevű próba felhasználón alapuló Asana az Azure Active Directory egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell tudható, hogy mi a Asana megfelelőjük a felhasználó egy felhasználóhoz, az Azure Active Directory. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó Asana hivatkozás viszonya kell létrehozni.
A hivatkozás kapcsolat jön létre által az Azure Active Directory Asana **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való Asana tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó létrehozása egy Asana tesztelése](#creating-an-Asana-test-user)** - van-e egy megfelelője a Britta Simon Asana, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory konfigurálása egyszeri bejelentkezés

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a Asana alkalmazásban az egyszeri bejelentkezés beállítása.

**Állítson be Azure AD az egyszeri bejelentkezés Asana, hajtsa végre az alábbi lépéseket:**

1. A felső sávon kattintson a **Quick Start**.

    ![Egyszeri bejelentkezés beállítása][6]
2. A klasszikus portálon **Asana** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][7] 

3. **Hogyan szeretné, hogy jelentkezzen be az Asana felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-asana-tutorial/tutorial_asana_06.png)

4. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: 

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-asana-tutorial/tutorial_asana_07.png)


    egy. Az bejelentkezési a URL-cím mezőbe írja be az URL-cím használata a következő mintát:`https://app.asana.com`

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

5. **Konfigurálása az egyszeri bejelentkezés a Asana** lapján kattintson a **tanúsítvány letöltése**, majd mentse a fájlt a számítógépen. SAML egyszeri bejelentkezési URL-címet is, másolja az az érték.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-asana-tutorial/tutorial_asana_08.png)

8. Kattintson a tanúsítvány a jobb gombbal, majd nyissa meg a Jegyzettömb vagy a használni kívánt szöveg szerkesztő biztonságitanúsítvány-fájl. Másolja a tartalmat, a kezdő és záró tanúsítvány címe között. Ez a használandó egyszeri bejelentkezés beállítása a Asana X.509 tanúsítvány.

6. Egy másik böngészőablakban bejelentkezéses az Asana alkalmazás. Asana SSO konfigurálásához elérheti a munkaterület nevét a képernyő jobb felső sarkában kattintson a munkaterület beállításainak. Kattintson a ** \<a munkaterület neve\> beállítások**. 

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-asana-tutorial/tutorial_asana_09.png)

7. A **szervezeti beállításai** ablakban kattintson a **felügyeleti**. Kattintson a **tagok SAML keresztül kell jelentkeznie** az egyszeri bejelentkezés konfiguráció engedélyezése. A végrehajtása az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-asana-tutorial/tutorial_asana_10.png)

    egy. A **bejelentkezési URL-címe** texbox illessze be a SAML bejelentkezési URL-CÍMÉT az Azure AD meg.

    b. Az **X.509-es tanúsítvány** mezőbe illessze be az Azure Active Directory másolta x.509-es tanúsítvány.

9. Kattintson a **Mentés**gombra. Nyissa meg a [Asana SSO beállításának útmutató](https://asana.com/guide/help/premium/authentication#gl-saml) , ha Ön további segítségre van szüksége.

7. Nyissa meg a **konfigurálása az egyszeri bejelentkezés Asana,** oldal az Azure Active Directory, jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább**gombra.
    
    ![Azure Active Directory Single Sign-On][10]

8. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
    
    ![Azure Active Directory Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ebben a szakaszban a klasszikus portálon Britta Simon nevű próba felhasználó létrehozása.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-asana-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-asana-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-asana-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:
 
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-asana-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-asana-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-asana-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-asana-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-an-asana-test-user"></a>Egy Asana teszt felhasználó létrehozása

Ebben a részben hozzon létre egy felhasználót Britta Simon nevű Asana.

1. A **Asana**nyissa meg a **csoportok** szakaszban kattintson a bal oldali ablaktáblában. Kattintson a pluszjel gombra. 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-asana-tutorial/tutorial_asana_12.png) 

2. Írja be az e-mailt britta.simon@contoso.com a szövegdoboz-és **meghívása**jelölje be.
3. Kattintson a **Meghívás küldése**gombra. Az új felhasználó kap e-mailben a saját e-mail fiókba. Noémi kell létrehozni, és ellenőrizze a fiók.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ebben a részben Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít Asana használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Asana, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Asana**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-asana-tutorial/tutorial_asana_50.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. Az összes felhasználó listában válassza a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]


### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja, tesztelje az Azure Active Directory egyszeri bejelentkezést.

Nyissa meg a Asana bejelentkezési lapjára. E-mail cím szövegdoboz beszúrása az e-mail címet britta.simon@contoso.com. Hagyja üresen a jelszó mezőben lévő értéket, és kattintson a **Log In**gombra. Az Azure Active Directory bejelentkezési lapjára irányítja. Töltse ki az Azure Active Directory hitelesítő adatait. Most be van jelentkezve Asana.

## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-asana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asana-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-asana-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-asana-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-asana-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-asana-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-asana-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-asana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asana-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-asana-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-asana-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-asana-tutorial/tutorial_general_205.png
