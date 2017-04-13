<properties
    pageTitle="Oktatóprogram: Azure Active Directory integráció a Antik megtudhatja - Shibboleth |} Microsoft Azure"
    description="Egyszeri bejelentkezés között az Azure Active Directory és antik bemutatja, hogy – a Shibboleth konfigurálásának ismertetése."
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
    ms.date="09/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-blackboard-learn---shibboleth"></a>Oktatóprogram: Azure Active Directory integráció a Antik megtudhatja - Shibboleth

Ebből az oktatóanyagból megismerheti, hogyan integrálása Antik megtudhatja - Shibboleth az Azure Active Directory (Azure Active Directory).

Antik további tudnivalók a Shibboleth az Azure Active Directory - integrálása biztosít a következő előnyökkel jár:

- Az Azure Active Directory minderről Antik - Shibboleth hozzáféréssel rendelkező személyek megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on minderről Antik - Shibboleth (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a Antik megtudhatja - Shibboleth, van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy Antik ismerje meg, hogy – a Shibboleth egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni Azure AD az egyszeri bejelentkezés tesztkörnyezetben.

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. További tudnivalók a Antik - Shibboleth a gyűjteményből hozzáadása
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-blackboard-learn---shibboleth-from-the-gallery"></a>További tudnivalók a Antik - Shibboleth a gyűjteményből hozzáadása
További Antik – az Azure Active Directory, a Shibboleth integrációja konfigurálása kell hozzáadnia Antik tudnivalók – a gyűjteményből felügyelt szoftver alkalmazásai a Shibboleth.

**Ismerje meg, antik - hozzáadása a gyűjteményből Shibboleth tegye a következőket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory][1]
2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Antik megtudhatja - Shibboleth**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearnshibboleth_01.png)

7. Az eredmény ablaktáblában válassza a **További Antik - Shibboleth**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearnshibboleth_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ebben a részben konfigurálása és Azure Active Directory egyszeri bejelentkezés Antik további tudnivalók a tesztelése, – Shibboleth "Britta Simon" nevű vizsgálat felhasználón alapuló.

Az egyszeri bejelentkezési szeretne dolgozni Azure Active Directory kell, hogy a Antik megtudhatja, milyen a megfelelőjük a felhasználó - Shibboleth egy felhasználóhoz, az Azure Active Directory. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó Antik megtudhatja - hivatkozás viszonya Shibboleth kell létrehozni.

A hivatkozás kapcsolat jön létre a értéket a **felhasználó nevét** a **felhasználónév** Antik megtudhatja - Shibboleth értékként Azure Active Directory hozzárendelésével.

Beállítása és tesztelése Azure AD az egyszeri bejelentkezés Antik tudnivalók – a Shibboleth, szükséges a következő building blocks parancsot:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
3. **[Egy Antik megtudhatja - Shibboleth teszt felhasználó létrehozása](#creating-a-blackboard-learn-shibboleth-test-user)** – van-e egy megfelelője a Britta Simon Antik megtudhatja - Shibboleth, amelyen az Azure Active Directory ábrázolt csatolt.
4. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory egyszeri bejelentkezés beállítása

Ebben a részben Azure AD az egyszeri bejelentkezés a klasszikus portálon engedélyezése, és az egyszeri bejelentkezés beállítása a Antik megtudhatja - Shibboleth alkalmazást.


**Állítson be Azure AD az egyszeri bejelentkezés Antik megtudhatja - Shibboleth, hajtsa végre az alábbi lépéseket:**

1. Az alkalmazás integrációs **Antik megtudhatja - Shibboleth** lapján klasszikus-portálon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.
     
    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az antik megtudhatja - Shibboleth felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearnshibboleth_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearnshibboleth_04.png) 

    egy. **Bejelentkezési a URL-cím** mezőbe írja be a felhasználóknak, hogy bejelentkezés minderről a Antik - alkalmazást a következő mintát Shibboleth által használt URL-címe: **https://\<yourblackoardlearnserver\>.blackboardlearn.com/Shibboleth.sso/Login**
    
    b. Az **azonosító** mezőbe írja be az URL-cím használata a következő mintát: **https://\<yourblackoardlearnserver\>.blackboardlearn.com/shibboleth-sp**

    c billentyűkombinációt. **Válasz URL-cím** mezőbe írja be az URL-cím használata a következő mintát: **https://\<yourblackoardlearnserver\>.blackboardlearn.com/Shibboleth.sso/SAML2/POST**

    > [AZURE.NOTE] Lehetővé teszi a tudja megkeresni ezeket az értékeket a metaadat-alapú összevonás doucument Antik megtudhatja partnere által biztosított a.

    d. Kattintson a **Tovább** gombra
 
4. A **beállítás az egyszeri bejelentkezés Antik megtudhatja - Shibboleth a** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearnshibboleth_05.png)

    egy. Kattintson a **metaadat-alapú letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.


5. Ha be van állítva az alkalmazás SSO, a Antik megtudhatja - Shibboleth partnere kapcsolatba, és küldje el nekik a következő:

    • A letöltött **metaadatok**

    • **Kibocsátó URL-címe**

    • A **SAML egyszeri bejelentkezési URL-címe**

    • **Egyetlen kijelentkezési szolgáltatás URL-címe**

6. A klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.
    
    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
 
    ![Azure Active Directory Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ebben a szakaszban a klasszikus portálon Britta Simon nevű próba felhasználó létrehozása.


![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Az Azure Active Directory vizsgálat felhasználó létrehozása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:  ![vizsgálat Azure AD-felhasználó létrehozása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Az Azure Active Directory vizsgálat felhasználó létrehozása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   


### <a name="creating-an-blackboard-learn---shibboleth-test-user"></a>Egy Antik megtudhatja - Shibboleth teszt felhasználó létrehozása

Ebben a részben hozzon létre egy felhasználót Britta Simon nevű Antik megtudhatja - Shibboleth. Kérje a felhasználókat felvenni a Antik megtudhatja - Shibboleth platform Antik megtudhatja partnere.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ebben a részben Britta Simon használandó Azure egyszeri bejelentkezés a saját hozzáférést biztosít Antik megtudhatja - Shibboleth engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Antik megtudhatja - Shibboleth, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Alkalmazások listájában válassza a **További Antik - Shibboleth**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearnshibboleth_50.png) 

3. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203]

4. A felhasználók listában jelölje ki a **Britta Simon**.

5. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]


### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a részben tesztelje az Azure Active Directory egyetlen bejelentkezés beállítási a Access panelen.

A Antik megtudhatja – az Access panel Shibboleth csempére kattintva meg kell első automatikusan jelentkezett-on minderről a Antik - Shibboleth alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_205.png