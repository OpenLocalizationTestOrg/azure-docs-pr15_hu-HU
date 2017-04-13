<properties
    pageTitle="Oktatóprogram: Azure Active Directory integráció a Antik megtudhatja |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory, és ismerje meg, antik között."
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


# <a name="tutorial-azure-active-directory-integration-with-blackboard-learn"></a>Oktatóprogram: Azure Active Directory integráció a Antik további tudnivalók

Ebből az oktatóanyagból megismerheti, hogyan Antik további integrálása az Azure Active Directory (Azure Active Directory).

Ismerje meg, antik integrálása Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel Antik megtudhatja, megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on Antik (Single Sign-On) megtudhatja, hogy az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a Antik ismerje meg, van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy Antik ismerje meg, felhőalapú Platform egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni Azure AD az egyszeri bejelentkezés tesztkörnyezetben.

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Ismerje meg, antik hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-blackboard-learn-from-the-gallery"></a>Ismerje meg, antik hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a Antik ismerje meg, szüksége hozzáadása Antik további tudnivalók a gyűjteményből a felügyelt szoftver alkalmazások listájában.

**Gyűjtemény használatával adhat hozzá Antik további tudnivalók a, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory][1]
2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Antik megtudhatja**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_01.png)

7. Az eredmény ablaktáblában jelölje ki a **Antik ismerje meg**, és válassza a **kész** , az alkalmazás hozzáadása.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_06.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ebben a részben konfigurálása és antik megtudhatja, hogy "Britta Simon" nevű próba felhasználón alapuló az Azure Active Directory egyszeri bejelentkezés tesztelése.

Egyszeri bejelentkezés a munkát Azure AD meg kell adni szeretné, hogy mi a megfelelőjük felhasználó Antik ismerje meg, a felhasználónak az Azure Active Directory. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó Antik további tudnivalók a hivatkozás viszonya kell létrehozni.

Ez a hivatkozás kapcsolat az Azure Active Directory **felhasználónév** Antik ismerje meg, a program a **felhasználónév** értékének hozzárendelésével jön létre.

Állítsa be, és Azure Active Directory egyszeri bejelentkezés Antik további tudnivalók a tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
3. **[Felhasználó létrehozása egy Antik megtudhatja tesztelése](#creating-a-blackboard-learn-test-user)** - van-e egy megfelelője a Britta Simon Antik megtudhatja, hogy az Azure Active Directory ábrázolt csatolt.
4. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ebben a részben Azure AD az egyszeri bejelentkezés a klasszikus portálon engedélyezése, és ismerje meg, antik alkalmazás egyszeri bejelentkezés beállítása.

Antik ismerje meg, hogy alkalmazás a SAML Előfeltételek vár, egy bizonyos formátumban. Állítsa be a következő jogcímalapú ehhez az alkalmazáshoz. A **"Atrribute"** lapon, az alkalmazás a következő attribútumok értékének kezelheti. Az alábbi képernyőképen látható, a. 

![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_51.png) 

**Állítson be Azure AD az egyszeri bejelentkezés Antik ismerje meg, végezze el az alábbi lépéseket:**


1. Az Azure klasszikus portálon **Antik megtudhatja** alkalmazás integrációs lapján a menüben, a képernyő tetején kattintson a **attribútumok**elemre.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_80.png) 


1. A **SAML jogkivonat attribútumok** párbeszédpanelen minden egyes sorára, az alábbi táblázatban látható a következő lépésekkel: azt van feleltesse meg a Userprincipalname, az egyedi felhasználói attribútum itt, de a megfelelő értékre, amely egyedileg, a felhasználó a szervezet különböző megfeleltetése és, hogy Antik térképek bemutatja, hogy username mező.

  	| Attribútumnév | Attribútumérték |
  	| --- | --- |    
  	| urn:oid:1.3.6.1.4.1.5923.1.1.1.6  | User.userPrincipalName |
   
 
    egy. Kattintson a **felhasználó attribútum hozzáadása** a **Felhasználó Attribure hozzáadása** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_81.png) 


    b. **Attrubute neve** mezőbe írja be a attribútumnév meg abban a sorban látható.

    c billentyűkombinációt. Az **Attribútumérték** listából selsect az attribútumérték meg abban a sorban látható.

    d. Kattintson a **kész**gombra.  

2. Az alkalmazás integrációs **Antik tudnivalók** lapon klasszikus-portálon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.
     
    ![Egyszeri bejelentkezés beállítása][6] 

3. **Hogyan szeretné, hogy jelentkezzen be az antik további felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_03.png) 

4. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_04.png) 

    egy. **Bejelentkezési a URL-cím** mezőbe írja be a bejelentkezés az alkalmazásba Antik ismerje meg, az alábbi minta használatával a felhasználók által használt URL-címe: **https://\<vállalat nevét árak\>.blackboard.com/**
    
    b. Kattintson a **Tovább** gombra
 
5. A **beállítás az egyszeri bejelentkezés Antik további tudnivalók a** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_05.png)

    egy. Kattintson a **metaadat-alapú letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.


6. Ha be van állítva az alkalmazás SSO, antik további támogatási csoport munkatársaitól kaphat, és küldje el nekik a következő:

    • A letöltött metaadatok


7. A klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.
    
    ![Azure Active Directory Single Sign-On][10]

8. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
 
    ![Azure Active Directory Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ebben a szakaszban a klasszikus portálon Britta Simon nevű próba felhasználó létrehozása.


![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_04.png) 

5. A **mondja el a felhasználóra vonatkozó** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:  ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-an-blackboard-learn-test-user"></a>Egy Antik további vizsgálat felhasználó létrehozása

Ebben a részben hozzon létre egy Britta Simon nevű Antik ismerje meg a felhasználót. 

Antik ismerje meg, hogy az alkalmazás támogatja a csak az idő felhasználói kiépítési. Győződjön meg arról, hogy állította be a jogcímalapú [Konfigurálása Azure AD az egyszeri bejelentkezés](#configuring-azure-ad-single-sign-on) **szakaszban leírt módon**

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ebben a részben engedélyezése Britta Simon Antik megtudhatja, hogy az access nyújtásával Azure egyszeri bejelentkezés használata.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Antik ismerje meg, végezze el az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Antik megtudhatja**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_50.png) 

3. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203]

4. A felhasználók listában jelölje ki a **Britta Simon**.

5. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]


### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a részben tesztelje az Azure Active Directory egyetlen bejelentkezés beállítási a Access panelen.

Antik ismerje meg, hogy alkalmazás támogatása, amikor az Access panel Antik további csempéjére szerezheti be automatikusan jelentkezett-on a Antik megtudhatja alkalmazásba.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_205.png
