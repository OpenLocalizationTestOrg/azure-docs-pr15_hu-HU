<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Keylight |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és Keylight között."
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


# <a name="tutorial-azure-active-directory-integration-with-keylight"></a>Oktatóprogram: Azure Active Directory-integráció a Keylight

Ebből az oktatóanyagból megismerheti, hogyan Keylight integrálása az Azure Active Directory (Azure Active Directory).

Keylight integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel Keylight megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való Keylight (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a Keylight van szükség az alábbiakat:

- Az Azure előfizetéssel
- Egy Keylight egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni Azure AD az egyszeri bejelentkezés tesztkörnyezetben. 

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Keylight hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-keylight-from-the-gallery"></a>Keylight hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a Keylight, meg kell Keylight hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből Keylight felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Keylight**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_01.png)

7. Az eredmény ablaktáblában jelölje ki a **Keylight**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ebben a részben konfigurálása és a "Britta Simon" nevű próba felhasználón alapuló Keylight az Azure Active Directory egyszeri bejelentkezés tesztelése.

Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való Keylight tesztelése, az alábbi építőelemeket szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó létrehozása egy Keylight tesztelése](#creating-a-keylight-test-user)** - van-e egy megfelelője a Britta Simon Keylight, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ebben a részben Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése, és a Keylight alkalmazásban az egyszeri bejelentkezés beállítása.


**Állítson be Azure AD az egyszeri bejelentkezés Keylight, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon **Keylight** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6] 


2. **Hogyan szeretné, hogy jelentkezzen be az Keylight felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:
 
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_04.png) 


    egy. Bejelentkezési a URL-cím mezőbe írja be a bejelentkezés az Keylight alkalmazás a következő mintát használatával a felhasználók által használt URL-címe: **"https://\<vállalatnév\>.keylightgrc.com/Login.aspx?saml=1"**.


4. A **beállítás az egyszeri bejelentkezés Keylight a** lapon hajtsa végre az alábbi lépéseket:
 
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_05.png) 

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.


5. Ahhoz, hogy a Keylight SSO, hajtsa végre az alábbi lépéseket:
 
    egy. Bejelentkezés fiókjába Keylight rendszergazdaként.

    b. A felső sávon kattintson a **személyt**, és **Keylight beállítás**kiválasztása.
       
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-keylight-tutorial/401.png) 

    c billentyűkombinációt. Kattintson a bal oldali treeview, a **SAML**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-keylight-tutorial/402.png) 

    d. A **SAML beállításai** párbeszédpanelen kattintson a **Szerkesztés**gombra.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-keylight-tutorial/404.png) 
  

5. A **SAML-beállítások szerkesztése** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-keylight-tutorial/405.png) 

    egy. Állítsa a **SAML hitelesítést** **aktív**.


    b. Az Azure Active Directory-klasszikus portálon a **SAML egyszeri bejelentkezési URL-címe** értéket másolja és illessze be a **Identitás szolgáltató bejelentkezési URL-cím** mezőben lévő értéket.

    c billentyűkombinációt. Az Azure Active Directory-klasszikus portálon másolja az **Egyetlen Sign-Out szolgáltatás URL-címe** értéket, és illessze be a **Identitás szolgáltató kijelentkezés URL-címe** mezőben lévő értéket.

    d. Jelölje ki a letöltött Keylight tanúsítványt **Válassza a fájl** gombra, és kattintson a **Megnyitás** töltse fel a tanúsítvány.


    e. **SAML felhasználói azonosító hely** beállítása **a tárgy kimutatásról NameIdentifier**elemre.
   
    f. Adja meg a **használatáról a következő mintát Keylight szolgáltató: **https://&lt;vállalatnév&gt;. keylightgrc.com**.

    g. **Az aktív** **felhasználók automatikus-létrehozására** beállítása.

    h. **Automatikus-rendelkezést fióktípus** meg a **Teljes felhasználói**.

    lehet. **A rendelkezés automatikus biztonsági szerepkört**jelölje ki a **SAML szabványos felhasználót**.
   
    j. **A rendelkezés automatikus biztonsági config**jelölje ki a **Szokásos felhasználó konfigurációja**.
   
    k. Az e-mailek attribútum mezőbe írja be a **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    l. Az **Utónév attribútum** mezőbe írja be a **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    m =. Az **utolsó név attribútum** mezőbe írja be a **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

    az n billentyűt. Kattintson a **Mentés**gombra.
   
  
   
  
6. Az Azure klasszikus portálon válassza ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  

    ![Azure Active Directory Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ebben a részben az Azure klasszikus portálon Britta Simon nevű próba felhasználó létrehozása.

A felhasználók listában jelölje ki a **Britta Simon**.

![Azure Active Directory-felhasználó létrehozása][20]



**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/create_aaduser_09.png) 


2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/create_aaduser_03.png) 


4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-keylight-test-user"></a>Keylight próba felhasználó létrehozása

Ebben a részben hozzon létre egy felhasználót Britta Simon nevű Keylight. Keylight támogatja a kiépítési közvetlenül az idő, amely alapértelmezés szerint engedélyezve van.

Nincs teendő ebben a szakaszban a meg nem. Egy új felhasználót hoznak létre, Keylight elérésekor, ha a felhasználó még nem létezik. 

> [AZURE.NOTE] Ha egy felhasználó létrehozása manuálisan kell szüksége a Keylight támogatási csoport munkatársaitól kaphat.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ebben a részben Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít Keylight használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Keylight, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Keylight**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_50.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a részben tesztelje az Azure Active Directory egyetlen bejelentkezés beállítási a Access panelen.

Amikor az Access panel a Keylight mozaik gombra kattint, meg kell első automatikusan aláírt-on az Keylight alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_205.png
